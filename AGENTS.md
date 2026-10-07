# Agent Guidelines & Repository Rules

These guidelines instruct AI agents and pair-programming assistants working in the `ensurf` repository. All contributors and agents must follow these instructions.

---

## 1. Core Operating Principles

1. **Adhere to Design Guidelines**: Before designing, modifying, or refactoring code, review and comply with [`DESIGN.md`](./DESIGN.md). Respect architectural layers, modular boundaries, and error handling protocols.
2. **Preserve Documentation Integrity**: Always preserve existing docstrings, explanatory comments, and architectural notes unrelated to your changes.
3. **No Phantom Changes**: Do not perform speculative refactoring outside the scope of the user's specific request. Focus on delivering clean, minimal, working implementations.
4. **Safety First**: Do not execute destructive commands (e.g., `git reset --hard`, `rm -rf`, force pushes) without explicit instruction. Never commit secrets, credentials, or `.env` files.

---

## 2. Code Modification Workflow

When fulfilling a coding task:
1. **Analyze**: Inspect existing project files, dependencies, conventions, and test patterns first.
2. **Plan**: Formulate a concise plan ensuring changes align with [`DESIGN.md`](./DESIGN.md).
3. **Implement**:
   - Write clean, well-typed code following language idioms.
   - Separate pure domain logic from side-effect-heavy I/O.
   - Use explicit dependencies instead of ambient or global state.
4. **Verify**:
   - Run available test suites, linters, and type checkers.
   - Ensure zero regressions in existing tests.
   - If introducing new functionality, write corresponding unit or integration tests.
5. **Report**: Succinctly summarize changes made, files touched, and verification results.

---

## 3. Skill & Tool Usage

- Workspace skills are located in [`.agents/skills/`](./.agents/skills/).
- Available repository skills:
  - [`repo-workflow`](./.agents/skills/repo-workflow/SKILL.md): Standard developer workflow for feature additions, testing, and verification.
  - [`design-review`](./.agents/skills/design-review/SKILL.md): Review and audit checklist to verify compliance with architectural guidelines.
- Always check `.agents/skills/` for relevant playbooks before executing complex multi-step workflows.

---

## 4. Testing & Verification Standards

- **Unit tests**: Must be fast, hermetic, and deterministic.
- **Integration tests**: Must cleanly spin up and tear down any transient fixtures or resources.
- If a test fails, diagnose the root cause instead of disabling, skipping, or modifying tests to match erroneous behavior.
