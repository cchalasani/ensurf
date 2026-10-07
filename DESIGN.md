# Design Guidelines

This document outlines the architectural principles, component design rules, coding standards, and operational guidelines for this repository. It serves as a single source of truth for maintainers, contributors, and automated development agents.

---

## 1. Architectural Philosophy

### 1.1 Core Principles
- **Separation of Concerns (SoC)**: Isolate business and domain logic from transport layers, I/O protocols, storage engines, and user interfaces.
- **High Cohesion & Loose Coupling**: Design modules that do one cohesive job well and interact with other modules via minimal, well-typed interfaces.
- **Explicit over Implicit**: Avoid hidden magic, implicit globals, or ambient state. Dependencies, configurations, and data flows should be explicit and traceable.
- **Defensive & Fail-Safe Defaults**: Secure by default, safe against unexpected inputs, and resilient against external dependency outages.
- **Simplicity & YAGNI**: Build for current, validated requirements with clean extension points, rather than building speculative generic abstractions.

### 1.2 Structural Organization (Hexagonal / Layered Archetype)
```text
┌────────────────────────────────────────────────────────┐
│                   Entrypoints / Adapters                │
│    (CLI, HTTP API, Event Consumers, Scripts, UI)        │
└──────────────────────────┬─────────────────────────────┘
                           │ uses
                           ▼
┌────────────────────────────────────────────────────────┐
│                   Core Application                     │
│         (Use cases, orchestrators, workflows)           │
└──────────────────────────┬─────────────────────────────┘
                           │ operates on
                           ▼
┌────────────────────────────────────────────────────────┐
│                     Domain Model                       │
│    (Entities, domain invariants, pure business logic)  │
└──────────────────────────▲─────────────────────────────┘
                           │ implements contracts of
┌──────────────────────────┴─────────────────────────────┐
│                 Infrastructure & Clients               │
│       (Databases, external APIs, file system, network) │
└────────────────────────────────────────────────────────┘
```

- **Domain Layer**: Contains pure business rules, validation, and domain types. Must have zero external dependencies or framework couplings.
- **Application Layer**: Coordinates use cases, dispatches commands, and sequences business operations.
- **Infrastructure Layer**: Implements adapters for external boundaries (database queries, network clients, file storage).
- **Presentation / Entrypoint Layer**: Translates incoming requests into application commands and serializes responses.

---

## 2. Component & Interface Design

### 2.1 Interface Contracts
- Design narrow interfaces focused on client needs (Interface Segregation Principle).
- Declare public APIs explicitly. Mark internal or unexported modules and functions clearly.
- Depend upon interfaces or abstract boundaries for external services to simplify unit testing and swapping implementations.

### 2.2 State Management & Immutability
- Prefer immutable data structures wherever possible.
- Avoid shared mutable state across asynchronous or concurrent contexts.
- Isolate mutations to designated boundary points or repositories.

### 2.3 Idempotency & Determinism
- Where operations mutate state or perform network actions, aim for idempotent APIs using idempotency keys or natural unique constraints.
- Pure functions should produce deterministic outputs for given inputs with no side effects.

---

## 3. Error Handling Strategy

### 3.1 Categorized Errors
Differentiate errors cleanly by their root origin:
- **Client / Domain Errors (4xx equivalent)**: Input validation failures, invariant violations, resource not found, unauthorized actions.
- **System / Infrastructure Errors (5xx equivalent)**: Network timeouts, storage connection dropouts, disk full, unexpected exceptions.

### 3.2 Error Principles
- **Never swallow errors silently**: Always either handle an error completely or wrap it with diagnostic context and propagate it upwards.
- **Fail Fast**: Validate preconditions and input arguments immediately at boundary entry points before initiating expensive operations.
- **Meaningful Diagnostics**: Error messages should clearly communicate *what* failed, *which identifier or entity* was involved, and *how to remediate* without leaking sensitive data (credentials, secrets, internal IPs).

---

## 4. Testing Strategy

### 4.1 The Testing Pyramid
```text
         ▲
        / \      End-to-End / Smoke (Verify end-to-end critical paths)
       /   \
      /-----\    Integration Tests (Verify persistence, adapters, APIs)
     /       \
    /---------\  Unit Tests (Fast, exhaustive coverage of pure logic)
```

1. **Unit Tests**:
   - Test domain logic, state machines, algorithmic transforms, and validations in isolation.
   - Run in milliseconds without requiring network, disk, or external database setups.
2. **Integration Tests**:
   - Verify that adapters correctly integrate with real or containerized infrastructure (databases, caches, third-party clients).
3. **End-to-End / Smoke Tests**:
   - Test high-priority end-to-end user journeys or CLI commands across integrated modules.

### 4.2 Test Hygiene
- Follow the **Arrange-Act-Assert** (AAA) pattern.
- Give test cases descriptive names that document behavior (e.g., `test_should_reject_invalid_token_with_unauthorized_error`).
- Tests must be independent and order-agnostic. Clean up created resources in teardown.
- Avoid over-mocking internal implementation details; assert on observable outputs and side-effects.

---

## 5. Configuration & Observability

### 5.1 Configuration Management
- Follow 12-Factor principles: read configurations from environment variables or dedicated configuration files.
- Validate configuration on boot/startup and crash early with actionable error messages if required configurations are missing or malformed.
- Separate environments (development, test, staging, production) cleanly without hardcoding environment branches deep in business code.

### 5.2 Logging & Tracing
- **Structured Logging**: Emit logs in machine-readable formats (JSON or structured key-value).
- **Log Levels**:
  - `DEBUG`: Verbose internal state for local debugging.
  - `INFO`: Significant lifecycle events (startup, completed jobs, connection established).
  - `WARN`: Recoverable anomalies or degraded operations.
  - `ERROR`: Unrecoverable operation failures that require investigation.
- **Correlation**: Include request or correlation IDs across log lines to enable distributed tracing.
- **Data Protection**: Never log passwords, tokens, API keys, or personally identifiable information (PII).

---

## 6. Code Quality & Contribution Standards

### 6.1 Coding Standards
- Enforce strict formatting and linting via automated repository tools.
- Keep functions and methods concise and focused on a single responsibility.
- Write self-documenting code with clear naming; add comments only to explain non-obvious *why*, not obvious *what*.
- Avoid circular dependencies between modules.

### 6.2 Pull Request & Review Checklist
Before merging changes, ensure:
- [ ] Code adheres to the architectural layering and boundaries defined above.
- [ ] All new logic is covered with appropriate unit or integration tests.
- [ ] All existing tests pass without regressions.
- [ ] Linters, formatters, and static analysis tools report zero errors or warnings.
- [ ] Public API docs or README updates are included where relevant.
- [ ] No secrets or sensitive test credentials are committed.

---

<!-- CUSTOMIZE: Customize below for project-specific language, frameworks, and tooling -->
## 7. Project Specifics

- **Language / Runtime**: Elixir (Erlang/OTP)
- **Package Manager / Build Tool**: Mix (`mix.exs`)
- **Application Model**: OTP Application with supervision tree (`Ensurf.Application`)
- **Default Test Runner**: ExUnit (`mix test`)
- **Code Formatter & Style**: `mix format`, `mix format --check-formatted`
- **Compiler**: `mix compile --warnings-as-errors`

