---
name: kl-javascript_typescript
description: "Apply KL's JavaScript and TypeScript preferences when writing, reviewing, or refactoring scripts and applications: explicit types, semantic casing, practical classes, and concise implementation."
metadata:
  display-name: KL-javascript_typescript
  version: "1.0.1"
---

# JavaScript + TypeScript

- Use TypeScript for new projects almost always.
- Keep existing JavaScript when a migration would add unrelated work.
- Keep existing JavaScript if its only a simple script already made in Js.

If Javascript add JSDocs always. Prefer explicit JSDocs types, `any` type not allowed neither in Typescript nor in JavaScript.

## Types and implementation

- Prefer explicit parameter types, public return types, and visibility modifiers. Annotate locals when they clarify domain meaning; avoid mechanically repeating obvious types.
- Use strictest checking; prefer `noUncheckedIndexedAccess`, `noImplicitOverride`, and `noFallthroughCasesInSwitch` for new TypeScript configurations. Preserve the project's runtime and module settings.
- Use plain typed objects for data, classes for behavior and injected collaborators, and interfaces for replaceable IO contracts. Do not wrap every object in a class or create an interface per class.
- Use unions, enums, and named domain types for meaningful states and identifiers. Keep helpers concrete; introduce generics when the same operation actually needs multiple types.
- Use `Partial`, `Pick`, `Omit`, and `satisfies` when they express the intended shape directly. Prefer a named options object when several arguments would obscure a call.
- Prefer readable branches, early returns, and straightforward transformations over clever expressions or layers. Static helper methods are welcome for cohesive, stateless operations; simple standalone functions are also fine.
- non-null assertions are permitted if known to always be non-full due to logic business.

## Naming and layout

- Prefer PascalCase for public domain members, methods, classes, and exported helpers; camelCase for private/protected implementation, parameters, and locals. Use SCREAMING_SNAKE_CASE for global immutable configuration.
- Preserve framework/library names and external data shapes, including camelCase methods required by contracts. Do not rename existing APIs just to enforce casing.
- Use `I` for interface names and a project prefix for domain entities when useful (`IFileSystem`, `Player`). Do not spread `KL` into unrelated projects or naming.
- Prefer one main class per file, domain types near their feature, and tests beside the implementation. Follow existing filenames and exports; the reference commonly uses default exports for its main classes.
- Use short comments for intent and non-obvious invariants; add JSDoc with an example when it makes a utility's contract clearer.

## Tooling

- For a new setup, prefer Biome with tabs, double quotes, semicolons, and a 120-column limit. Follow existing tooling instead of installing a competing formatter.
- Prefer type-only imports for erased types. Keep value imports for classes used by decorator-based DI metadata; converting those imports to `import type` can break runtime resolution. Scope lint exceptions to that requirement.

For backend architecture, dependency injection, and testing strategy, read [KL-backend](../KL-backend/SKILL.md) when the task involves backend work.

USE BUN as PACKAGE MANAGER. NPM, PNPM, YARN not allowed for new projects unless they already use them in that case, follow project package manager convention.
