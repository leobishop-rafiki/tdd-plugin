---
name: implement-tdd
description: Implements a callable to satisfy an existing set of tests, defining its required behaviour, following TDD (tests as the spec). Use when the user asks to implement code, make existing/failing tests pass or invokes /implement-tdd.
---

# Implement Tests

## Definitions

- Callable: Any invocable code unit—such as a function, method, procedure, closure, lambda, or constructor—that may accept arguments, perform behavior or side effects, and may produce a result.
  - Use language specific terminology when communicating with a user.

## Purpose

Under TDD, tests are the explicit specification of a callable's expected behaviour, often written before the implementation to define the specification. 
This skill (re)-implements a callable so that every existing test case passes — it does not add, remove, or edit test cases, it does not change the spec.

## Local Guidance

Always check for coding standards in `AGENTS.MD`, `CLAUDE.MD`, existing skills, rules, etc... when available, follow their guidance.

## Tests are Readonly

**Never modify the tests**, this includes: renaming; deleting; "correcting" expected values; adding skips (`t.Skip`, `xit`, `@pytest.mark.skip`); loosening assertions; or restructuring the framework to make a case easier to satisfy.

## Workflow

1. **Identify the callable** to implement, including its full signature (receiver/parameters, return types, errors/exceptions). If it doesn't exist yet, ask the user to confirm the expected signature.
2. **Read every test case** covering the callable. For each case, extract: the inputs/setup, any expected dependency behaviour, and the exact expected output/error. Treat this full set as the complete behavioural contract.
3. **Check for contradictions or issues** across the cases before writing code:
  - Two cases that require different behavior for the same effective input.
  - An expectation that depends on internal behaviour no reasonable implementation could produce.
  - A case that looks like it's testing the wrong thing entirely.
  - If found, **stop here and raise it explicitly with the user**, quote the specific cases and explain why they looks problematic. You may propose a resolution.
4. **Implement the general behaviour, not the test data.** Write the logic that satisfies the *general contract* the tests describe — don't special-case on specific inputs just to match expected outputs. 
  - If the implementation reads like a lookup table of "if input == case1.input, return case1.want", it's likely overfit.
5. **Reuse or discard existing code at your discretion.** If a partial or stub implementation already exists you can reuse or discard it at your discretion. 
  - There's no obligation to preserve prior code for its own sake. The contract is the tests, not the existing implementation.
6. **Follow the codebase/user's existing conventions/standards**.
7. **Run the test suite** for this callable once implemented.
   - **All passing**: report this to the user, and confirm no test files were modified (e.g. via `git diff` on the test file(s), if available).
   - **Some failing**: iterate on the **implementation** only, re-running until they pass or until you hit a case that seems genuinely unsatisfiable, then **stop and raise it explicitly with the user**.
