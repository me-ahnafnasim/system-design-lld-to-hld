# Example — WorldConnect (Global Social Platform, React.js)

End-to-end trace of the 13 levels using one product: **WorldConnect** — connect 1B users with realtime translation.

Use this as the worked example after reading `docs/ARCHITECTURE-MAP.md`. Each section maps to its exact folder (01 → 13).

---

## 01 Business — what do we do?
- Capabilities: identity, content sharing, realtime messaging, AI translation, payments (premium), analytics/ads
- Roadmap: Y1 core (profiles/posts/messaging) → Y2 video/AI/marketplace → Y3 business accounts
- Governance: GDPR/CCPA, multi-region residency, budget 40 infra / 30 dev / 20 security / 10 innovation

## 02 Enterprise — portfolio fit
- Portfolio: Web, CRM, Identity, Data Warehouse, Ad platform, Email infra
- Standards: OAuth2/JWT, Kafka events, Postgres ownership for identity, warehouse for analytics

## 03 Solution — solve one problem
- Problem: global launch with realtime feed + translation
- Systems: React web → BFF → User/Content/Messaging/Translation services → Kafka → moderation/search/analytics

## 04 System — one system, microservices
```text
API Gateway (auth, rate-limit)
 ├── User Service │ Content Service │ Messaging Service │ Translation Service
 └── Event Bus (Kafka)
```
- Quality: 10M concurrent, 99.99%, <200ms API, <1s page load
- CAP: feed = AP (eventual), payments = CP (strong). DBs: Postgres (users, ACID), Cassandra (feeds, write throughput), Redis (cache/sessions), MongoDB (content metadata)

## 05 Application — inside User Service (Clean)
```text
Presentation (controllers/DTOs) → Application (use-cases) → Domain (entities/V.O.) → Infrastructure (DB/APIs)
```
- Bounded contexts: Identity, Social, Content, Messaging
- Domain entity `User` + value object `Email` (validation in constructor); `UserRepository` interface in domain, Postgres impl in infra; `UpdateUserProfileUseCase` injected (DI).

## 06 Integration — how services talk
- REST for CRUD, GraphQL for React data fetching, WebSockets for messaging/notifications, gRPC inter-service, Kafka for async (`UserPostCreated → notify/analytics/moderation/search`).
- Circuit breaker on Translation Service with cached fallback; CQRS for feed (write DB, read cache).

## 07 Data — ownership + movement
- Owner: User Service owns users; Content owns posts; feeds derived
- Operational DBs → ETL → Warehouse → BI; Redis cache-aside; Cassandra sharded for feeds

## 08 Infrastructure / Deployment
- Docker + K8s, frontend ×5 / gateway ×3 autoscale on CPU>70%, blue-green + canary 5% + feature flags.

## 09 Component / Service — inside the system
- Gateway, BFF (`React → Node BFF → microservices` for aggregation/SSR), circuit breaker, CQRS, event-driven fan-out (see 06).

## 10 Package / Module — inside User Service
```text
src/{domain/{entities,value-objects,repositories,events},
     application/{use-cases,services,dtos},
     infrastructure/{database,external-apis,config},
     presentation/{controllers,middleware,routes}, shared/}
```
- Rule: `domain ← application ← infrastructure ← presentation`. ADP/SDP/CCP enforced, no cycles.

## 11 OOP Foundations — atoms in WorldConnect
- **Class/Object:** `User`, `Profile`, `Email` (value object with validation), `Avatar` (composed), `Post[]` (aggregated in `UserFeed`).
- **Interface vs abstract:** `DataSource` / `StorageService` / `UserRepository` interfaces; `BaseComponent` abstract for shared render helpers; `PostgresUserRepository` implements the port.
- **Relationships:** `UserProfile` *composes* `Avatar`; `UserFeed` *aggregates* `Post[]`; `UpdateUserProfileUseCase` *depends on* `UserRepository` abstraction; `UserManager` *encapsulates* private `users` map.
- **Static/instance, this/super:** `Logger.getInstance()` static access point vs per-request use-case instances; `UserCard extends BaseComponent` uses `super` for shared behavior, `this` for instance state.
- **Coupling/cohesion:** split god `UserProfile`/`MegaComponent` into `UserInfo/UserPosts/UserFriends`, `Header/Feed/Sidebar/Footer`; Context over prop-drilling.

## 12 Class / Object — React + TypeScript highlights
- **SRP:** split `UserProfile` into `UserInfo/UserPosts/UserFriends` with `useQuery`.
- **OCP:** `Button variant={primary|secondary|danger}` extensible without edits.
- **LSP:** `RestDataSource` / `GraphQLDataSource` both satisfy `DataSource`.
- **ISP:** split fat `UserActions` into `Auth/Profile/SocialActions`.
- **DIP:** `useStorage(storage: StorageService)` works with Local or Session impl.
- **Observer:** WebSocket → `setNotifications` in `NotificationBell`.
- **Compound:** `Tabs/TabList/Tab/TabPanel` via context.
- **Hook:** `useFriendship(friendId)` encapsulates request/accept.
- **Anti-patterns:** god component → split to `Header/Feed/Sidebar/Footer`; prop-drilling → Context. (Atoms — relations, encapsulation, coupling fixes — now live in Level 11 above; here is the SOLID/GoF judgment on top.)

## 13 Function / Algorithm — single behavior
- `isValidEmail()`, feed ranking, `calculateScholarship`-style pure functions: note complexity, validate inputs, handle errors, prefer pure/testable.

---

Each level constrains the one below — enterprise standards limit solution options, system style limits app structure, package rules limit class design. Practice by re-tracing WorldConnect top-down without looking.
