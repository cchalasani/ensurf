---
name: design-review
description: >-
  Audits and reviews code changes, pull requests, or architecture plans against the repository
  design guidelines and quality standards. Use when reviewing code or evaluating design decisions.
---

# Design & Architecture Review Skill

This skill guides the agent in conducting a systematic review of code changes against the repository's architectural principles and design guidelines.

---

## Review Procedure

### Step 1: Architectural Alignment
- Compare the code structure with [`DESIGN.md`](../../DESIGN.md).
- Verify separation of concerns:
  - Check whether domain/business logic has leaked into transport, controllers, or storage layers.
  - Verify that adapters depend inward on domain abstractions, not vice versa.

### Step 2: Interface & State Review
- Check API surface area: Are exported functions, classes, and types minimal and cohesive?
- Verify state mutations: Is state handled deterministically? Are concurrency or async hazards mitigated?
- Inspect configuration usage: Are configurations explicitly passed or loaded from validated schemas?

### Step 3: Error & Resilience Review
- Ensure errors are distinguished between domain/client errors and unexpected system failures.
- Check that errors provide actionable debugging context without exposing secrets.
- Verify that fallbacks or retry loops (if any) have sensible bounds and timeouts.

### Step 4: Test & Quality Review
- Ensure test coverage reflects the test pyramid: fast unit tests for logic, integration tests for boundaries.
- Verify absence of flaky or order-dependent assertions.

### Step 5: Deliver Feedback
Provide review feedback organized into:
1. **Critical Issues**: Architectural violations, security risks, error swallowing, or breaking changes.
2. **Improvements**: Simplification opportunities, performance optimizations, or missing tests.
3. **Praise / Approved Elements**: Clean patterns, good test coverage, or elegant designs.
