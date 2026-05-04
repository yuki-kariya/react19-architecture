# 2. Adopt Codebase Conventions

Date: 2026-05-31

## Status

Accepted

## Context

Over time, it becomes difficult to trace the dependency flow between modules and components, which increases maintenance costs.

One reason is that, when examining the codebase, it is not immediately clear where specific logic is implemented, so it takes time to reach the part of the code we actually need to inspect.

## Decision

Document the rules.

Architects and engineers must understand these rules before writing code.

These rules will also be loaded into AI agents such as Codex and Claude Code as a dedicated skill.

This document will also be managed according to the following rules:

- Create `doc/architecture/fe.md`.
- When creating files or directories, and when deciding what data definitions or logic should be placed in them, the contents documented in `fe.md` must always be treated as the source of truth.
- On the other hand, the actual coding style (for example, always declaring React components as functions and using arrow functions) should be enforced only by linters and formatters.

## Consequences

By understanding the documented rules in advance, authors can logically break large processes into smaller units and write code that is easier to maintain, while readers can reach the code they truly care about more quickly.

When AI agents generate code automatically, they will follow the same unified rules, which improves both code quality and the quality of reviews between humans and AI.

Documenting the rules in advance also helps prevent conflicts between engineers over coding style and structure.
