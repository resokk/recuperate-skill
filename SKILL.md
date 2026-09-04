---
name: recuperate-skill
description: Builds a cached, comprehensive understanding of a project — its internal file dependencies, external libraries, full-text search index, public API surface, per-line logic, and domain ecosystem context — as the foundation for creating an implementation roadmap. Use when asked to plan or build a roadmap/phase structure for a project, or to gather deep codebase context before a large-scale refactor.
---

## Workflow

1. **Run the crawler team.** All six crawlers — `dependency-builder`, `domain-researcher`, `ext-lib-inspector`, `fulltext-search-builder`, `logic-describer`, `public-api-writer` — must run in parallel, with none started before or after another. Launch all six in the same batch: use your runtime's agent-team feature if one is available (a persistent parallel team), otherwise send all six as Agent tool calls in a single message. Launching even one of them separately — before, after, or staggered relative to the rest — is not parallel and defeats the point of this step.
   - **They don't interfere with each other by construction**: each crawler reads the same project tree but writes to its own disjoint path under `.cache/recuperate/` (`dependency/`, `ext-lib.md`, `fulltext.db`+`fulltext.md`, `public-api/`, `logic/`, `domains/<slug>.md`) — no shared output file, so no write conflicts regardless of run order or timing.
   - **`domain-researcher` needs a domain name(s) to research**, not a file scope — it studies the outside ecosystem, not this project's own code. Before launching the team, determine the domain(s) from the project's own description (README, package/manifest description) if that's enough to name one or more concrete domains; ask the user only if it genuinely can't be inferred. The other five crawlers default to the whole project and need no extra input.
   - Give every crawler its whole default scope (whole project) unless the user has already narrowed it.
2. **Wait for every crawler to finish** before treating this stage as done — a partial `.cache/recuperate/` (e.g. a dependency map but no public API doc) is not a valid base for the roadmap steps that come after.
