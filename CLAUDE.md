# CLAUDE.md

This file guides Claude Code (CLI / VSCode extension) when working in this repository.

## What this is

The Kotlin/JVM implementation of Agentic CMS, part of the multi-stack family alongside
the Laravel, Django, TypeScript and Go builds. Spring Boot on a Java 21 toolchain,
auto-provisioned through the Foojay resolver so no local JDK setup is needed.

Cross-stack canon lives one directory up: `../FEATURE_MATRIX.md` for feature parity and
`../DESIGN_SYSTEM.md` for the visual language. Both are read-only from here.

## Stack

- Kotlin with the `kotlin-jvm`, `kotlin-spring` and `kotlin-jpa` compiler plugins, versions
  pinned through the Gradle version catalog (`libs.plugins.*`).
- Spring Boot: web, Thymeleaf, Data JPA, Security, OAuth2 client.
- Flyway for migrations, against PostgreSQL.
- Frontend assets built by `build.mjs` (Tailwind), invoked through `npm run build`.

## Commands

- `./gradlew check` — the gate. Compiles, runs the tests, applies the static checks.
- `./gradlew bootRun` — run locally.
- `npm run build` — build the frontend assets via `build.mjs`. Needed before the
  templates render correctly.
- `docker compose up` — local dependencies.

## Conventions specific to this stack

- Constructor injection only. No field injection, no `@Autowired` on properties.
- Repository interfaces for data access. This matches the Laravel side of the family and
  deliberately differs from the Django side, which adds a repository only where it
  removes duplication that already exists. The difference is a decision, not drift.
- Thin controllers, business logic in services.
- JPA entities are not `data class` and are not returned directly as API response
  bodies. Both cause real problems: generated `equals`/`hashCode` misbehave for managed
  entities, and exposing an entity leaks the schema and triggers lazy loading outside a
  transaction.

The reasoning behind those, and the N+1 traps in Spring Data JPA, are in the
`elman-kotlin-spring` and `elman-arch-conventions` skills.

## Test gate

`.claude/test-gate.sh` runs before every `git push`. Keep it green rather than bypassing
it. See `../../agentic-development-kit/docs/how-to-integrate.md` if it needs wiring.

<!-- ELMAN-KIT:BEGIN -->
General practices live in the `elman-practices` skills, loaded per task. This file holds what is specific to this project and wins where they disagree.
<!-- ELMAN-KIT:END -->
