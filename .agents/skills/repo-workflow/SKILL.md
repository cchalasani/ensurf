---
name: repo-workflow
description: >-
  Standard development workflow for the repository. Use when building, testing,
  implementing features, refactoring, or running verification steps on the codebase.
---

# Repository Development Workflow

This skill guides the agent through the standard workflow for implementing changes, adding features, refactoring, and verifying code quality in this repository.

---

## Workflow Steps

### Step 1: Context & Discovery
1. Inspect the repository structure and active stack:
   - Identify configuration and manifest files (e.g., `package.json`, `pyproject.toml`, `Cargo.toml`, `mix.exs`, `go.mod`).
2. Review [`DESIGN.md`](../../DESIGN.md) to ensure the planned implementation aligns with the architectural layers and conventions.
3. Check existing tests to understand the testing patterns used across the project.

### Step 2: Implementation
1. Keep changes focused and minimal: implement only what is required by the task.
2. Maintain layer boundaries:
   - Keep domain logic pure and free of I/O dependencies.
   - Place external API calls, database operations, or file system access in infrastructure adapters.
3. Validate inputs at entry points and handle errors explicitly with diagnostic context.

### Step 3: Verification
1. Run the test suite:
   ```bash
   mix test
   ```
2. Check formatting:
   ```bash
   mix format --check-formatted
   ```
3. Compile with warnings as errors:
   ```bash
   mix compile --warnings-as-errors
   ```
4. Consult the [Development & Quality Checklist](./references/checklist.md) before concluding the task.


---

## References

- [Quality Checklist](./references/checklist.md): Pre-flight checklist for changes.
- [Design Guidelines](../../DESIGN.md): Repository architectural standards.
