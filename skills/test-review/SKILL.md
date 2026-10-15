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

## Integration vs Unit tests

- Use integration tests whenever feasible.
- Use unit tests with mocks to fill in test coverage gaps and behavior that cannot be simulated with integration tests.
  - Do not reimplement the same test case in both integration and unit tests, always use integration tests where possible.

## Workflow

1. **Identify the callable under review**: the specific callable, including its signature (parameters, return type(s), errors/exceptions it can raise, side effects).
2. **Read the existing tests**: if they exist for that callable.
3. **Enumerate behavioural permutations** from the signature and implementation (if present):
  - Each parameter's nil/non-nil state (only where nil is actually a possible state per the type system).
  - Each conditional branch and loop edge case (zero iterations, one, many).
  - Each distinct return/error path.
  - Each distinct dependency call.
  - Plausable values (empty collections/strings, zero, expected).
  - Ordering/concurrency effects if relevant (e.g. idempotency, race conditions).
4. **Cross-reference** the enumerated permutations against the existing tests to find what's covered vs. missing.
5. **Identify the configuration required for each case**: the setup, inputs, output validation, etc...
6. **Identify mocking requirements**: identify if each test case requires mocking dependencies.
7. **Group the tests cases**: group by the configuration required for the group, with a secondary grouping by a coherent throughline linking the contained test cases.
  - Do not mix unit and integration tests within a group.
8. **Present findings** using the Output Format below. Do not write the missing tests unless the user explicitly asks — propose them first.

## Output Format

### Callable Purpose

Brief outline of the purpose of the method based on the signature, context and implementation (if present). 
Allows the user to verify understanding is correct.

### Covered

Brief bullet list of the behavioural permutations already tested. No need to be exhaustive, just enough to show the current coverage is understood.

### Gaps

A numbered list, for each group permutation:
- **Group**: the through line linking the group ([Unit|Integration])
  For each test case:
  - **Permutation**: the specific untested behaviour/input state
  If this is a new or updated test case define the following:
    - **Why it matters**: what could break silently without this test
    - **Proposed test case**: a short description or pseudocode/skeleton (name, setup, expected outcome)

## Anti-patterns to flag

- A single group per test case.
- A test file that validates the same field five times with different invalid values, i.e. fuzzing.
- Tests that only cover the happy path plus one generic failure case, missing other distinct failure paths the callable can encounter.
- Missing tests for zero-length/empty inputs when the function iterates over a collection.
