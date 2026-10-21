---
name: kl-backend
description: "Apply KL's backend architecture and testing preferences when building, reviewing, or refactoring services, APIs, workers, and bots in any language or framework: explicit constructor injection, thin entry points, real service tests, and replaceable IO."
metadata:
  display-name: KL-backend
  version: "1.0.0"
---

# Backend

Apply these language- and framework-independent preferences using the project's native mechanisms. They come from KL's tastes. Adopt the patterns needed for the task.

## Architecture and dependency injection

- Make dependencies explicit through constructors or initializers. Pass databases, filesystem access, network clients, and other collaborators instead of locating them through globals inside business logic.
- Use the framework's native DI container when available. Keep resolution at composition roots and test setup; do not inject the container as a service locator or introduce a second container.
- Give replaceable external IO a small contract. Use concrete services directly when an extra interface adds no useful substitution boundary.
- Centralize dependency identifiers and wire production/test implementations separately. Resolve classes by their type when supported; use explicit tokens for interfaces, configured values, and ambiguous dependencies.
- Let the container construct the service graph, including in tests. Override the IO boundary or configuration that changes; do not manually rebuild every collaborator and register a parallel graph.
- Choose lifetimes deliberately: shared infrastructure and intentional queues may be singletons; per-execution context must stay isolated. Keep request-specific data out of singleton fields. A child container alone does not guarantee isolated state.
- A typed facade grouping related data services is welcome when it simplifies callers. It is an access surface, not an implied transaction or a mandatory layer for every project.

## Business logic and persistence

- Keep controllers, commands, handlers, and jobs as thin orchestrators. Put cohesive business operations in services and stateless calculations/formatting in testable helpers.
- Reuse an existing base class for common execution setup; otherwise use the framework's middleware, guards, or interceptors. Introduce inheritance only when shared lifecycle behavior justifies it.
- Keep database representation and domain data separate where they differ. Map names, nullability, booleans, and storage encodings at the persistence boundary rather than leaking conversions into callers.
- Inject the database connection into data services. Direct ORM-backed services are fine; do not add generic repositories or duplicate CRUD layers without a concrete need.
- Preserve the distinction between an expected absence/rejection and an unexpected failure. Follow the existing contract for empty results, optional values, booleans, or domain results; do not silently turn infrastructure failures into success.
- Validate untrusted input at entry points. Handle errors at a useful boundary with diagnostics and appropriate caller feedback; keep sensitive internals out of public responses.
- When an operation promises one state transition across several writes, use the datastore's transaction mechanism. A grouped service facade does not provide atomicity by itself.
- For interactive entry points, provide meaningful progress and success/failure feedback. Keep localization optional unless requested or already part of the application.

## Tests and isolation

- Keep tests beside the behavior they verify, using the project's runner. Cover the observable outcome and important failure paths, not private methods or an exact internal call sequence.
- For persistence-backed behavior, prefer real services and real queries against an isolated test database with the actual schema/migrations. Probably you could use SQLite in memory, not hand-written RAM repositories, ask if should use SQLite or proyect's database for realistic tests.
- Use an in-memory database only when its semantics fit the production datastore. Use an isolated instance of the actual engine when constraints, transactions, or dialect-specific behavior matter.
- Seed explicit fixtures through reusable domain builders and seed helpers. Reset mutable data before each test while retaining/reseeding static lookup data. A shared resettable database requires sequential execution or separate databases for parallel tests.
- Reuse a test composition setup that replaces the database and external IO. Resolve the subject through a child container or framework testing module; bind an already-resolved instance only when subject and assertions must share that exact instance.
- Fake filesystem, messaging, and remote APIs at their boundaries; keep the service under test and its relevant business collaborators real. Pure helpers with no injected dependencies can be instantiated directly.
- Drive workflows through their input boundary, then assert returned responses/events/messages and persisted state. Verify invalid input, missing data, rejection/cancellation, or permission failures where relevant to that workflow.
- Clean state through public lifecycle methods: clear queues, cancel timers, restore spies/clocks, and close resources. Do not reach into private collections to reset a test. Resetting database rows does not reset singleton services.

## Applying this in a framework

Use native provider registration, explicit token bindings, test-provider overrides, and lifecycle hooks. In a framework with testing modules, import the relevant module and override external providers before compilation; resolve the subject from that module. Keep request handling and authorization in their native extension points. These preferences do not require a particular language, ORM, DI library, test runner, or framework.

