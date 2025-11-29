# Architecture & Layering Rules (Kotlin + Ktor)

You are working in a Kotlin backend project using Ktor.
Always follow a **controller → service → repository** architecture.

## Layers and Responsibilities

### Controllers (API layer)
- Map HTTP routes to controller functions.
- Parse input (path/query parameters, headers, request bodies).
- Call the appropriate service functions.
- Map service results to HTTP responses (status codes, DTOs).
- Handle only *light* validation and request shaping.
- **Do NOT** contain business logic or persistence logic.

### Services (Domain / business layer)
- Contain all business logic and application rules.
- Coordinate multiple repositories, other services, and external APIs.
- Enforce invariants, rules, and workflows.
- Perform input validation that is *business related*.
- Expose `suspend` functions when I/O or coroutines are involved.

### Repositories (Persistence / data access layer)
- Abstract access to the database or any persistence mechanism.
- Contain only data access code: queries, inserts, updates, deletes.
- Return domain models or simple data structures, not HTTP objects.
- Do not contain business logic.

## Allowed Dependencies

- Controllers may depend on:
    - Services
    - Mappers and DTOs
- Services may depend on:
    - Other services (carefully, avoid cycles)
    - Repositories
    - Domain models and value objects
- Repositories may depend on:
    - Database client / ORM / query builder
    - Data source configuration

**Forbidden dependencies:**

- Repositories must never depend on services or controllers.
- Services must never depend on controllers.
- Controllers must never depend directly on repositories for business operations.

If you detect a violation, refactor the code to respect this layering.

## Package & Naming Conventions

Use packages like:

- `com.example.app.controller`
- `com.example.app.service`
- `com.example.app.repository`
- `com.example.app.entity` (entities, value objects, request/response DTOs)
- `com.example.app.config` (wiring / DI / Ktor setup)

Naming patterns:

- Controllers: `UserController`, `AuthController`.
- Services: `UserService`, `AuthService`.
- Repositories: `UserRepository`, `OrderRepository`.

## Error Handling & Result Flow

- Services should communicate failures using:
    - domain-specific exceptions, or
    - sealed result types (e.g. `sealed class Result<out T>`).
- Controllers are responsible for:
    - converting errors to appropriate HTTP status codes,
    - returning useful error bodies (message, code, optional details).
- Do not throw raw low-level exceptions from repositories all the way to the controller without translating them.

## Cross-Cutting Concerns

- Logging, metrics, tracing, and security checks should be implemented:
    - via Ktor plugins/middleware where possible, or
    - in dedicated helpers/utilities.
- Avoid sprinkling cross-cutting logic inside business methods if it can be centralized.
- Prefer composition over inheritance for cross-cutting behaviors.

Always prefer **small, composable functions** and keep each layer focused on its responsibilities.
