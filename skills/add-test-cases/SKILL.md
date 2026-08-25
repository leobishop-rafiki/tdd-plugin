---
name: add-test-cases
description: Implements specific test cases for an existing callable in its existing test framework. Use when asked to implement test cases or /add-test-cases is invoked.
---

# Add Test Cases

## Definitions

- Callable: Any invocable code unit—such as a function, method, procedure, closure, lambda, or constructor—that may accept arguments, perform behavior or side effects, and may produce a result.
  - Use language specific terminology when communicating with a user.

## Purpose

Given a callable and a set of requested test cases, add those cases to the function's *existing* test framework. 
This skill does not scaffold new frameworks and does not invent test cases beyond what the user requested.
If no test scaffold exists guide the user to the `/scaffold-test` skill available in this plugin.

## Local Test Guidance

Always check for testing standards in `AGENTS.MD`, `CLAUDE.MD`, existing skills, rules, etc... when available, follow their guidance.

## Workflow

1. **Identify the callable.** Confirm the named callable exists (locate its definition, signature, receiver/parameters, return types, etc...). 
  - If it cannot be found, stop and ask the user to clarify its location.
2. **Identify the existing test framework.** Locate the test suite already covering this callable (e.g. its table-driven test, test class, or spec block).
  - If no test framework exists for this method/function, **stop** and tell the user to run `/scaffold-test` first. Do not create a framework yourself.
3. **Review the requested test cases against the existing framework.**
  - If the requested cases fit the existing struct/table/fixtures as-is, proceed to implementation.
  - If a requested case needs a framework change (new field, new mock, new fixture, structural change to the table/runner), **stop and raise the specific change with the user for discussion before making it.** Never modify the framework without explicit user approval.
4. **Implement the requested test cases** in the existing framework, following any local test guidance.
  - Add one case per distinct behaviour the user requested
  - Don't expand scope by adding cases the user didn't ask for.
  - Reuse existing fixtures/mocks/helpers; only add new shared fixture data if the requested cases need it.
    - Prefer extending existing helpers over inlining literals.
5. **Run the tests** and report the result. 
  - If the tests have failed identify **stop** and report this to the user.
