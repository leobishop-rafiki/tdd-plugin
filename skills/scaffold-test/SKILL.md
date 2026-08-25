---
name: scaffold-test
description: Scaffolds a table-driven test framework (struct, table, runner) for a specific callable, without implementing any test cases.
disable-model-invocation: true
---

# Scaffold Table-Driven Test

## Definitions

- Callable: Any invocable code unit—such as a function, method, procedure, closure, lambda, or constructor—that may accept arguments, perform behavior or side effects, and may produce a result.
  - Use language specific terminology when communicating with a user.

## Purpose

Generate the boilerplate table-driven test framework for the specified callable. Covering the test case struct, table and runner.
Do not write any actual test cases. This skill produces the skeleton for the user to fill in.

## Local Test Guidance

Always check for testing standards in `AGENTS.MD`, `CLAUDE.MD`, existing skills, rules, etc... when available, follow their guidance.

## Workflow

1. **Locate/create the test file**: if it doesn't exist, create it.
2. **Read the callable signature** parameter names/types, return types, and any errors it can return.
3. **Derive input fields**, one per parameter.
4. If the callable is a method **inspect the receiver's fields** to identify additional setup fields that may be required.
  1. If the reciever has dependencies **ask the user** to clarify if these should be mocked, integrated, etc...
    - Only include mocks/setup for dependencies actually excercised by the callable, otherwise dependencies can be left unspecified, reducing bloat.
  2. If the receiver has any initialised or stateful values ask the user to clarify what values the test should seed.
    - If the value is unused by the callable it should be left unspecified.
5. **Derive expected-output fields** from the return signature. For each return value, choose exact-match or a flexible assertion based on the type:
  - Deterministic value, e.g. no timestamps, generated IDs, pointers, etc..., should be exact-matched.
  - Non-deterministic value may require a flexible assertion, following existing guidance, language or codebase standards.
6. **Assemble the skeleton**: test case specification struct; table (empty with a single `// TODO: add test cases` marker); runner setting up the test, executing the callable and validating the response and any covered dependency calls.
7. **Do not implement any test cases.** Leave the table empty, with the `// TODO` marker.
8. **Present the generated framework** to the user for review.

## Notes

- Keep fixture/helper data (e.g. a shared `paramAFixture()`) as a follow-up for when cases are added — don't invent fixtures/helpers while scaffolding.
- A framework should only use one of exact-match or flexible-match output checks, avoid mixing the two.
