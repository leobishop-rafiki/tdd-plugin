---
name: test-review
description: Reviews a test suite to find gaps in coverage of distinct functional behaviour permutations. Use when asked to review tests, check test coverage, or invokes /test-review.
---

# Test Review

## Definitions

- Callable: Any invocable code unit—such as a function, method, procedure, closure, lambda, or constructor—that may accept arguments, perform behavior or side effects, and may produce a result.
  - Use language specific terminology when communicating with a user.

## Purpose

Under TDD, tests are the explicit specification of a callable's expected behaviour, often written before the implementation to define the specification. 
This skill reviews the tests for a given callable, finds input/behavioral permutations that aren't explicitly covered, and proposes concrete additional test cases to close the gaps.

## Scope of "sufficient coverage"

Cover distinct *behaviours*, not every possible input combination.

- **Do** cover: each distinct code path/branch, each distinct return class, boundary values (empty, zero, expected), each explicit error condition the callable can raise.
- **Do** cover nil/null handling exhaustively: every input parameter and dependency return value that can be nil/null/undefined/None needs to be tested for both its nil and non-nil state.
- **Don't** fuzz every field for validation.
- **Don't** duplicate tests that exercise the same code path with trivially different data.

## Workflow

1. **Identify the unit under review**: the specific callable, including its signature (parameters, return type(s), errors/exceptions it can raise, side effects).
2. **Read the existing tests** for that callable.
3. **Enumerate behavioural permutations** from the signature and implementation (if present):
  - Each parameter's nil/non-nil state (only where nil is actually a possible state per the type system).
  - Each conditional branch and loop edge case (zero iterations, one, many).
  - Each distinct return/error path.
  - Each distinct dependency call.
  - Plausable values (empty collections/strings, zero, expected).
  - Ordering/concurrency effects if relevant (e.g. idempotency, race conditions).
4. **Cross-reference** the enumerated permutations against the existing tests to find what's covered vs. missing.
5. **Present findings** using the Output Format below. Do not write the missing tests unless the user explicitly asks — propose them first.

## Output Format

### Covered

Brief bullet list of the behavioural permutations already tested. No need to be exhaustive, just enough to show the current coverage is understood.

### Gaps

A numbered list, for each missing permutation:

- **Permutation**: the specific untested behaviour/input state
- **Why it matters**: what could break silently without this test
- **Proposed test case**: a short description or pseudocode/skeleton (name, setup, expected outcome)

Group all nil/null-handling gaps together and call them out explicitly, since they're required coverage.

## Anti-patterns to flag

- A test file that validates the same field five times with different invalid values, i.e. fuzzing.
- Tests that only cover the happy path plus one generic failure case, missing other distinct failure paths the callable can encounter.
- Missing tests for zero-length/empty inputs when the function iterates over a collection.
