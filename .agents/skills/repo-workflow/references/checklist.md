# Development & Quality Checklist

Use this checklist during development and before finalizing pull requests or code modifications.

---

## 1. Architecture & Design
- [ ] Does the change respect established module and layer boundaries?
- [ ] Are public interfaces minimal, explicit, and strongly typed?
- [ ] Is domain logic decoupled from external I/O and frameworks?
- [ ] Are dependencies injected rather than accessed via global state?

## 2. Robustness & Error Handling
- [ ] Are edge cases and boundary conditions handled?
- [ ] Are errors explicitly propagated or handled rather than silently swallowed?
- [ ] Are error messages informative and devoid of sensitive credentials or PII?

## 3. Testing
- [ ] Are new components covered by unit tests?
- [ ] Are integration tests added for new boundary adapters?
- [ ] Do all tests pass deterministically without order dependencies?
- [ ] Are mock boundaries placed at external services rather than internal business logic?

## 4. Code Quality & Cleanliness
- [ ] Is the code formatted according to repository style?
- [ ] Are unused imports, dead code, and temporary debugging statements removed?
- [ ] Are documentation and comments preserved and updated where necessary?
