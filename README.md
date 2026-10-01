Here is the complete breakdown of the **languages, frameworks, tools, and libraries** used across the **QuickBite** platform (`Core-Service` & `Order-Service`):

---

### 1. 💻 Programming Languages
* **TypeScript** (v5.x): Strict typing with decorators (`experimentalDecorators`, `emitDecoratorMetadata`) enabled for dependency injection and validation.
* **SQL (PostgreSQL dialect)**: Schema definitions, indexes, geospatial queries, and transactional locking (`FOR UPDATE SKIP LOCKED` / `FOR UPDATE`).

---

### 2. 🚀 Core Frameworks & Runtime
* **Node.js**: Asynchronous backend runtime.
* **Express.js (v5)**: Minimalist HTTP web framework for REST API routing and middleware pipelines.
* **Socket.IO (v4)**: Real-time, bi-directional, event-based WebSocket communication layer.

---

### 3. 🗄️ Databases, Query Builders & Storage
* **PostgreSQL (15+)**: Primary relational database:
  * Single database cluster for `Core-Service`.
  * Multi-region sharded clusters (`eg`, `ksa`, ...) + PostGIS extension for `Order-Service`.
* **Knex.js**: SQL query builder and schema migration management tool (strictly preferred over ORMs for full control over queries and transactions).
* **Redis (`ioredis`)**:
  * Cross-service read-through caching.
  * Fast 24-hour request idempotency store.
  * RabbitMQ event deduplication (`SETNX`).
  * Live delivery agent presence & geospatial proximity tracking (`GEOADD`, `GEORADIUS`).
  * Distributed pub/sub clustering adapter (`@socket.io/redis-adapter`) for WebSocket multi-worker fan-out.

---

### 4. 📨 Messaging & Asynchronous Event Processing
* **RabbitMQ (`amqplib`, `amqp-connection-manager`)**:
  * Topic exchange (`core.events`) for cross-service domain event broadcasting.
  * Durable consumer queues with auto-reconnection and Dead-Letter Exchange/Queue (`DLX`/`DLQ`) failover.
* **Transactional Outbox Pattern**: Implemented with `croner` scheduling the background outbox drainer in `Core-Service` using PostgreSQL `FOR UPDATE SKIP LOCKED` and publisher confirms.

---

### 5. 🏗️ Architecture, Dependency Injection & Validation
* **TSyringe (`tsyringe`, `reflect-metadata`)**: Lightweight inversion of control (IoC) / Dependency Injection container using token symbols (`TOKENS`).
* **Class Validator (`class-validator`)**: Declarative, decorator-based DTO request body validation.
* **Class Transformer (`class-transformer`)**: Object-to-class deserialization and nested DTO transformation.
* **Zod**: Type-safe runtime environment variable validation (`env.ts`).

---

### 6. 🔒 Authentication, Authorization & Security
* **JWT (`jsonwebtoken`)**: Stateless access & refresh token generation and verification.
* **Bcrypt (`bcrypt`)**: Cryptographic password hashing.
* **Helmet (`helmet`)**: HTTP security header hardening.
* **CORS (`cors`)**: Cross-Origin Resource Sharing control.
* **Cookie Parser (`cookie-parser`)**: HTTP cookie extraction for tokens.
* **Custom RBAC & Internal API-Key Guards**: Granular resource-action authorization middleware (`core:product:create`, etc.) and internal service-to-service communication keys.

---

### 7. 🌐 External Integrations & Services
* **Kashier v3 Payment Gateway**: Online payment session creation, hosted checkout redirects, and HMAC-signed webhook signature verification.
* **Mailjet (`node-mailjet`)**: Transactional email notifications and staff invitation dispatch.

---

### 8. 🛠️ Developer Tooling & Build Utilities
* **`tsx`**: Fast TypeScript execution and live reload watcher for development.
* **`ts-node`**: TypeScript execution runtime.
* **`uuid` (v4)**: Public, non-sequential client-facing IDs.
* **`dotenv`**: Environment variable file loader (`.env`).
