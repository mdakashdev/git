# Summary
- `package overview` - A one-sentence description of the entire package/project that provides a clear overall understanding of its purpose and scope.
  (done)
- `package goal` - To state the purpose clearly and specifically - (done)



- উদ্দেশ্যটা **point-by-point** বা **সংক্ষেপে নির্দিষ্ট করে বলা**।


# Package:  @rddm/bricks-uikit

`@rddm/bricks-uikit` হলো **Stencil.js** দিয়ে তৈরি একটি ওয়েব কম্পোনেন্ট লাইব্রেরি।
এই প্যাকেজের মূল উদ্দেশ্য হলো **Bricks component system-এর সঙ্গে UI Kit-এর কম্পোনেন্টগুলোকে সামঞ্জস্যপূর্ণ করা**, একই সঙ্গে Bricks-এর কম্পোনেন্টগুলোকে স্বাধীনভাবে ব্যবহার করার সুবিধা বজায় রাখা।
কোনো ব্যবহারকারী যখন `@rddm/bricks-uikit` ইনস্টল করবেন, তখন তিনি এই প্যাকেজের মাধ্যমে উপলব্ধ **Bricks components**-এর পাশাপাশি **UI Kit-compatible components**-গুলোও ব্যবহার করতে পারবেন।

```text
আপনি যখন @rddm/bricks-uikit প্যাকেজটি আপনার প্রজেক্টে install করবেন, তখন আপনি একই প্রজেক্টে দুই ধরনের component ব্যবহার করতে পারবেন:
Bricks components — Bricks system-এর নিজস্ব components।
UI Kit-compatible components — UI Kit-এর এমন components যেগুলো এই package-এর মাধ্যমে Bricks-এর সাথে কাজ করার উপযোগী করা হয়েছে।
সহজ উদাহরণ
ধরুন আপনার কাছে আগে থেকেই Bricks-এর একটা Button component আছে। এখন @rddm/bricks-uikit install করার পর আপনি UI Kit-এর compatible Button, Input, Modal ইত্যাদিও ব্যবহার করতে পারবেন।
অর্থাৎ, এই package install করলে Bricks components বাদ যাবে না; বরং Bricks-এর সাথে UI Kit-এর compatible components-ও ব্যবহার করার সুযোগ যোগ হবে।
সংক্ষেপে:
@rddm/bricks-uikit = Bricks components + UI Kit-compatible components একসাথে ব্যবহার করার সুবিধা।

```

This project has two primary goals:
1. Make UI Kit components compatible with Bricks
2. Keep Bricks components independently usable

## Architecture

- `basic-structure`: monorepo root and catelog use korechi
- `packages`: all package and playground
- `bricks-uikit`: eita actuall package ja ami develop kortechi - @rddm/bricks-uikit
- `eslint`: eslint alada akta package ja @rddm/bricks-uikit package a use korechi
- `playground`: eikhane amar package ta bivinno app a test kori.
- `nuxt-app`: nuxt application a demo hisabe use korechi.
- `uikit-app`: ekhane uikit package er sathe develop kora package compare kora hoyeche
- `uikit-app-test`: only uikit package er component gulo ekhane visible kore dekha hoyeche


## Important Notes

- context er baire kichu korbe na.
- nije theke commit or push korbe na
- must follow existing package architecture
-

`@rddm/uikit` package 20-21 ta component ke `@nexus/bricks` er component er sathe compatible korbo, so amake `@rddm/uikit` component gulo reference hisabe dite cai
seta kivabe dibo, akta folder kore sekhane component name onujai rakhbo, naki onno vabe aar clude.md te kivabe referecne dibo.

arek ta jinish cai, jei component a kaj korbo sekhane status maintain korte cai, jeita running colbe running baki gulo pending and jei gulo hoye jabe seigulo
done

akta compnent ses hole

refactor policy ki hobe


---
# Package: `@rddm/bricks-uikit`

`@rddm/bricks-uikit` is a web component library built with **Stencil.js**.

The primary goal of this package is to make the components from the UI Kit compatible with the components provided by the Bricks package.

When a user installs `@rddm/bricks-uikit`, they should be able to use all Bricks components along with the UI Kit-compatible components provided by this package.

## Goals

This package has two main goals:

1. **Make UI Kit components compatible with Bricks components**

    * Adapt and align UI Kit components so that they work consistently with the Bricks component system.
    * Maintain compatibility with the existing Bricks architecture and APIs where applicable.

2. **Allow Bricks components to be used independently**

    * Bricks components should remain independently usable.
    * Consumers should be able to use Bricks components without being required to use the UI Kit components.

## Architecture

This project is organized as a monorepo with the following structure:

* **`basic-structure`**

    * The monorepo root.
    * Contains the overall project structure and catalog configuration.

* **`packages`**

    * Contains all packages and playground applications used in the project.

* **`bricks-uikit`**

    * The main package currently being developed.
    * Published as `@rddm/bricks-uikit`.
    * Contains the UI Kit-compatible Bricks components.

* **`eslint`**

    * A separate ESLint package/configuration.
    * Used by the `@rddm/bricks-uikit` package for linting and code-quality checks.

* **`playground`**

    * Used to test and validate the package in different applications and environments.

* **`nuxt-app`**

    * A Nuxt application used to demonstrate and test the package in a Nuxt environment.

* **`uikit-app`**

    * Used to compare the original UI Kit components with the components developed in `@rddm/bricks-uikit`.

* **`uikit-app-test`**

    * Used to display and test only the original UI Kit components.
    * This application is intended to provide a reference for comparing UI Kit behavior and appearance.

## Important Notes

* **Stay within the project context.**

    * Do not make changes that are unrelated to the current task or project requirements.
    * Do not introduce unnecessary dependencies, architectural changes, or refactoring unless explicitly requested.

* **Do not commit or push changes automatically.**

    * Never create a Git commit on your own.
    * Never push changes to a remote repository unless explicitly instructed to do so.

* **Preserve existing behavior.**

    * Avoid breaking existing Bricks or UI Kit functionality.
    * Prefer minimal and focused changes.

* **Follow the existing project conventions.**

    * Before introducing a new pattern, check how similar functionality is implemented elsewhere in the repository.
    * Reuse existing components, utilities, types, and patterns whenever appropriate.

* **Keep Bricks components independently usable.**

    * Changes made for UI Kit compatibility should not unnecessarily prevent Bricks components from being used independently.

এটা চাইলে আমি আরও **Claude Code–optimized `CLAUDE.md`** বানিয়ে দিতে পারি—যেখানে `Development Rules`, `Component Guidelines`, `Testing`, `Do/Don't`, এবং **কোনো component modify করার আগে Claude কী কী check করবে**—এসবও structured করে দেওয়া থাকবে।

---
অবশ্যই। নিচের version-টা আমি **Claude Code-এর জন্য আরও actionable** করে সাজিয়েছি—যাতে শুধু project context না, বরং component তৈরি/modify করার সময় Claude কীভাবে কাজ করবে সেটাও পরিষ্কার থাকে।

# `@rddm/bricks-uikit`

## Overview

`@rddm/bricks-uikit` is a web component library built with **Stencil.js**.

The main purpose of this package is to make **UI Kit components compatible with the Bricks component system** while keeping Bricks components independently usable.

When a consumer installs `@rddm/bricks-uikit`, they should be able to use the available Bricks components together with the UI Kit-compatible components provided by this package.

---

## Project Goals

This project has two primary goals:

### 1. Make UI Kit components compatible with Bricks

UI Kit components should be adapted to work correctly within the Bricks ecosystem.

This includes:

* Matching the behavior of existing Bricks components.
* Maintaining consistent APIs and component usage patterns.
* Preserving the expected UI Kit appearance and behavior where applicable.
* Reusing existing Bricks functionality instead of unnecessarily duplicating it.
* Ensuring the resulting components work correctly as web components.

### 2. Keep Bricks components independently usable

Bricks components must remain independently usable.

The implementation should not introduce unnecessary dependencies between Bricks components and UI Kit components.

Consumers should be able to use Bricks components without being required to use the UI Kit.

---

# Architecture

This repository is structured as a monorepo.

## `basic-structure`

The monorepo root and main project structure.

It contains the overall repository configuration and catalog setup.

## `packages`

Contains the packages and applications used by the project.

### `bricks-uikit`

The main package being developed.

Package name:

```text
@rddm/bricks-uikit
```

This package contains the UI Kit-compatible Bricks components and is the primary focus of development.

### `eslint`

A separate ESLint package/configuration used by `@rddm/bricks-uikit`.

Use the existing ESLint configuration and project conventions instead of introducing a new linting setup.

---

# Applications

## `playground`

Used to test the package in different applications and environments.

Use this application when you need to manually verify component behavior during development.

## `nuxt-app`

A Nuxt application used to demonstrate and test `@rddm/bricks-uikit` in a Nuxt environment.

Use this application when validating Nuxt-specific integration.

## `uikit-app`

Used to compare:

* Original UI Kit components
* Components implemented in `@rddm/bricks-uikit`

This application is especially useful when implementing or modifying a component that needs to visually or behaviorally match the UI Kit.

## `uikit-app-test`

Used to display and test only the original UI Kit components.

Treat this application as a reference implementation when comparing the behavior, structure, and appearance of UI Kit components.

---

# Development Guidelines

## Understand Before Changing

Before modifying or creating a component:

1. Find the corresponding UI Kit component.
2. Find the corresponding Bricks component, if one exists.
3. Review how similar components are implemented in the repository.
4. Check the existing component API, properties, events, slots, and styling.
5. Check the relevant test/demo application.
6. Make the smallest change necessary to achieve the requested behavior.

Do not start implementing based only on assumptions.

---

# Component Compatibility

When making a UI Kit component compatible with Bricks, consider the following:

### Component API

Check and preserve, where appropriate:

* Component name
* Properties
* Attributes
* Events
* Methods
* Slots
* CSS parts
* CSS custom properties
* Public types/interfaces

Do not rename or remove an existing API unless explicitly requested.

### Behavior

The component should:

* Follow the expected UI Kit behavior.
* Integrate correctly with the Bricks component system.
* Preserve existing Bricks behavior where applicable.
* Handle user interactions consistently.
* Avoid introducing unnecessary side effects.

### Styling

When matching a UI Kit component:

* Compare the UI Kit implementation with the Bricks implementation.
* Reuse existing Bricks styles and design tokens where appropriate.
* Avoid duplicating styles unnecessarily.
* Preserve responsive behavior.
* Verify states such as hover, focus, active, disabled, loading, and error where applicable.

---

# Reuse Existing Components

Prefer existing Bricks components and utilities over creating duplicate implementations.

Before creating a new component, check whether an equivalent or similar Bricks component already exists.

For example:

```text
Existing Bricks component
        ↓
Can it satisfy the UI Kit requirement?
        ↓
Yes → Reuse/adapt it
No  → Implement the required functionality
```

Do not create duplicate components when existing functionality can be reused safely.

---

# UI Kit vs Bricks

When there is a difference between the UI Kit implementation and the Bricks implementation:

1. Identify the actual difference.
2. Determine whether the difference is intentional.
3. Check existing patterns in the repository.
4. Prefer compatibility with the Bricks architecture.
5. Preserve the required UI Kit behavior and visual appearance.
6. Avoid changing unrelated Bricks behavior.

Do not blindly copy the UI Kit implementation into Bricks.

The goal is **compatibility**, not duplication.

---

# Testing

After making a component change, verify the component in the appropriate application.

Depending on the change, use:

* `uikit-app` for UI Kit vs Bricks comparison.
* `uikit-app-test` for reference behavior of the original UI Kit component.
* `playground` for general integration testing.
* `nuxt-app` for Nuxt-specific integration.

When possible, verify:

* Rendering
* Props
* Events
* Slots
* User interactions
* Styling
* Responsive behavior
* Disabled/loading states
* Accessibility behavior
* TypeScript/build compatibility

Do not consider a component complete only because it compiles.

---

# Code Quality

Follow the existing code style and architecture.

Before adding new code:

* Look for existing patterns.
* Reuse existing utilities.
* Reuse existing types.
* Follow existing naming conventions.
* Keep components focused.
* Avoid unnecessary abstractions.
* Avoid unnecessary dependencies.
* Avoid unrelated refactoring.

Prefer simple, maintainable implementations.

---

# Changes and Scope

Stay strictly within the scope of the requested task.

Do not:

* Refactor unrelated components.
* Rename unrelated files.
* Change project architecture without permission.
* Add unnecessary dependencies.
* Modify unrelated configuration.
* Remove existing functionality without a clear requirement.
* Change public APIs without explicit instruction.

If a larger architectural change appears necessary, explain the reason before making it.

---

# Git Rules

Claude must **never automatically commit or push changes**.

Do not:

```text
git commit
git push
```

unless the user explicitly asks for it.

Claude may inspect Git status, history, or diffs when necessary to understand the current state of the project.

---

# Dependency Rules

Do not add a new dependency unless it is necessary for the requested task.

Before adding a dependency:

1. Check whether the repository already has a dependency that solves the problem.
2. Check whether the functionality can be implemented using existing project utilities.
3. Prefer existing dependencies and project conventions.
4. Add a new dependency only when there is a clear technical reason.

---

# File Modification Rules

Only modify files that are relevant to the requested task.

Before modifying a file:

* Understand its purpose.
* Check whether it is used elsewhere.
* Consider whether the change affects the public API.
* Check for related tests or demo applications.

Avoid broad changes when a focused change is sufficient.

---

# When Something Is Unclear

If the requested behavior is ambiguous:

1. Inspect existing implementations first.
2. Check the UI Kit implementation.
3. Check the Bricks implementation.
4. Check similar components.
5. Use existing project conventions to determine the most appropriate approach.

If the ambiguity could lead to a significant architectural or behavioral change, ask the user before proceeding.

Do not make large assumptions.

---

# Definition of Done

A component change is considered complete when:

* The requested functionality is implemented.
* UI Kit and Bricks compatibility requirements are satisfied.
* Existing Bricks behavior is not unnecessarily broken.
* The component follows existing project conventions.
* Relevant applications have been used for verification.
* No unnecessary dependencies or unrelated changes were introduced.
* The code passes the project's existing lint/build/test checks where applicable.
* No Git commit or push has been performed unless explicitly requested.

---

# Important Rules

These rules have the highest priority when working on this project:

1. **Stay within the project context.**
2. **Do not make unrelated changes.**
3. **Do not commit or push automatically.**
4. **Do not break existing Bricks functionality unnecessarily.**
5. **Keep Bricks components independently usable.**
6. **Prefer reusing existing components and utilities.**
7. **Follow existing project patterns and conventions.**
8. **Compare with the original UI Kit implementation when working on UI Kit compatibility.**
9. **Test changes in the appropriate application before considering the task complete.**
10. **Ask before making significant architectural or behavioral changes.**

---
হ্যাঁ—এটার জন্য আমি **একটা dedicated reference directory + per-component status file** রাখার পরামর্শ দেব। এতে Claude প্রতিবার UI Kit component-এর reference খুঁজে পাবে, আর কোন component `pending/running/done` সেটা পরিষ্কার থাকবে।

তোমার ক্ষেত্রে সবচেয়ে clean structure হবে:

```text
project-root/
├── CLAUDE.md
├── packages/
│   ├── bricks-uikit/
│   └── ...
│
└── docs/
    └── uikit-reference/
        ├── README.md
        ├── components/
        │   ├── button/
        │   │   └── README.md
        │   ├── input/
        │   │   └── README.md
        │   ├── select/
        │   │   └── README.md
        │   └── ...
        │
        └── status.md
```

তবে **20–21টা component-এর জন্য আলাদা আলাদা README বানানো বাধ্যতামূলক না**।

যদি তোমার মূল উদ্দেশ্য হয় Claude-কে original UI Kit component-এর reference দেওয়া, তাহলে আরও ভালো approach হতে পারে:

```text
docs/
└── uikit-reference/
    ├── button/
    │   ├── component.tsx
    │   ├── styles.css
    │   └── README.md
    ├── input/
    │   ├── component.tsx
    │   ├── styles.css
    │   └── README.md
    └── ...
```

### সবচেয়ে গুরুত্বপূর্ণ বিষয়

যদি `@rddm/uikit` package **already repository-এর মধ্যে থাকে**, তাহলে তার code copy করে `docs/uikit-reference`-এ রাখা দরকার নেই।

বরং `CLAUDE.md`-তে সরাসরি বলো:

> `@rddm/uikit` is the source of truth/reference implementation for UI Kit behavior.

এবং package-এর exact location Claude-কে বলে দাও।


যেমন:

```markdown
## UI Kit Reference

The `@rddm/uikit` package is the source of truth for the UI Kit components.

When implementing or modifying a component in `@nexus/bricks`, always inspect the corresponding `@rddm/uikit` component first.

Do not assume the UI Kit behavior or API.

For every component:

1. Find the corresponding component in `@rddm/uikit`.
2. Review its implementation, props, events, slots, styles, and behavior.
3. Compare it with the existing Bricks component.
4. Implement the required compatibility in `@nexus/bricks`.
5. Verify the result against the UI Kit reference.
```
---

## Status tracking

এখানে আমি **একটা central `status.md`** রাখার পরামর্শ দেব।

যেমন:

```text
docs/
└── uikit-reference/
    └── status.md
```

তার মধ্যে:

```markdown
# UI Kit Component Compatibility Status

| Component | Status |
|---|---|
| Button | done |
| Input | done |
| Select | running |
| Checkbox | pending |
| Radio | pending |
| Switch | pending |
| Modal | pending |
| Tooltip | pending |
```

তোমার requirement অনুযায়ী status হবে:

* 🟡 `pending` → এখনো কাজ শুরু হয়নি
* 🔵 `running` → বর্তমানে এই component নিয়ে কাজ চলছে
* 🟢 `done` → compatibility সম্পন্ন হয়েছে

### একটা গুরুত্বপূর্ণ rule

একসাথে **শুধু একটি component `running` থাকবে**।

Claude-কে এটা explicitly বলে দেওয়া ভালো:

```markdown
## Component Status Rules

Each component has one of the following statuses:

- `pending` — Work has not started.
- `running` — Currently being implemented.
- `done` — Implementation and verification are complete.

Only one component may have the `running` status at a time.

Before starting work on a component:

1. Change its status from `pending` to `running`.
2. Complete the implementation.
3. Verify the component against the `@rddm/uikit` reference.
4. Run the relevant checks.
5. Change its status from `running` to `done`.
6. Move to the next `pending` component only after the current component is `done`.
```

---

# আমি হলে আরও একটা জিনিস করতাম

`CLAUDE.md`-তে 20–21টা component-এর নাম hardcode না করে **একটা component registry** রাখতাম।

যেমন:

```text
docs/
└── uikit-reference/
    ├── README.md
    ├── components.md
    └── status.md
```

`components.md`:

```markdown
# UI Kit Components

The following components need to be made compatible with `@nexus/bricks`.

1. Button
2. Input
3. Select
4. Checkbox
5. Radio
6. Switch
7. Modal
8. Tooltip
9. Dropdown
10. ...
```

আর `status.md`:

```markdown
# Compatibility Status

| # | Component | Status |
|---|---|---|
| 1 | Button | done |
| 2 | Input | done |
| 3 | Select | running |
| 4 | Checkbox | pending |
| 5 | Radio | pending |
| 6 | Switch | pending |
| 7 | Modal | pending |
| 8 | Tooltip | pending |
| 9 | Dropdown | pending |
```

এতে `CLAUDE.md` অনেক ছোট এবং maintainable থাকবে।

---

# `CLAUDE.md`-তে Reference section

তোমার existing `CLAUDE.md`-তে আমি এই sectionটা add করতাম:

```markdown
# UI Kit Compatibility

## Source of Truth

The `@rddm/uikit` package is the primary reference implementation for all UI Kit components.

The goal of this project is to make the required `@rddm/uikit` components compatible with `@nexus/bricks`.

When working on a component, always inspect the corresponding `@rddm/uikit` implementation before making changes.

Do not implement a component based only on assumptions or memory.

For each component, compare:

- Component API
- Props
- Attributes
- Events
- Methods
- Slots
- CSS/classes
- Styling
- States
- Accessibility behavior
- User interactions
- Responsive behavior

The existing `@nexus/bricks` implementation should also be inspected before making changes.

The goal is compatibility between the two systems, not simply copying the UI Kit implementation.

## Component Reference

The list of components is maintained in:

`docs/uikit-reference/components.md`

The implementation status is maintained in:

`docs/uikit-reference/status.md`

The `@rddm/uikit` package itself is the source of truth for the actual component implementation.

---

# Component Workflow

Work on components sequentially.

Only one component can have the `running` status at a time.

For each component:

1. Select the next `pending` component.
2. Change its status to `running`.
3. Locate the corresponding component in `@rddm/uikit`.
4. Inspect the UI Kit implementation.
5. Inspect the corresponding `@nexus/bricks` component.
6. Compare their API, behavior, styling, and structure.
7. Implement the required compatibility.
8. Test the component.
9. Compare the result against the UI Kit reference.
10. Fix any issues found during verification.
11. Change the component status to `done`.
12. Continue with the next `pending` component.

Do not start another component while a component is marked as `running`.

---

# Status Definitions

- `pending`: Work has not started.
- `running`: Currently being implemented.
- `done`: Implementation is complete and verified.

A component must not be marked as `done` until it has been implemented and verified against the `@rddm/uikit` reference.
```

## আমার recommendation

তোমার use case অনুযায়ী আমি **এই structure-টাই ব্যবহার করতাম**:

```text
project-root/
│
├── CLAUDE.md
│
├── docs/
│   └── uikit-reference/
│       ├── README.md
│       ├── components.md
│       └── status.md
│
├── packages/
│   ├── bricks/
│   ├── uikit/
│   └── ...
```

**`@rddm/uikit` component-এর code আলাদা করে copy করে রাখার দরকার নেই**, যদি Claude সরাসরি package source দেখতে পারে। এতে duplicate reference code maintain করতে হবে না।

আর `status.md` হবে Claude-এর **task queue**—সে `pending → running → done` করে sequentially এগোবে।

চাইলে আমি তোমার জন্য এখন **পুরো production-ready `CLAUDE.md` + `components.md` + `status.md` তিনটা file একসাথে** তৈরি করে দিতে পারি, যেখানে 20–21টা component-এর জায়গাও থাকবে।
