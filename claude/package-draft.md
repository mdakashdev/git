# Package:  @rddm/bricks-uikit

## Overview

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

## Package Goals

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

## Commands

- `pnpm play:bricks`: eita diye playground er nuxt-app ke run kore,
- `pnpm play:uikit`: eita diye playground er uikit-app ke run kore",
- `pnpm build:bricks-uikit`: eita diye @rddm/bricks-uikit package ke build kore,
- `pnpm test`: eita diye test run kore,

## Styling

When matching a UI Kit component:

- **UI Kit implementation**-এর সঙ্গে **Bricks implementation** তুলনা করতে হবে।
- প্রয়োজন অনুযায়ী বিদ্যমান **Bricks styles** এবং **design tokens** পুনরায় ব্যবহার করতে হবে। kintu compatible component gulor desing obossoi ui-kit component er moto design hote hobe.
- অপ্রয়োজনীয়ভাবে একই **styles** বারবার তৈরি করা এড়িয়ে চলতে হবে।
- বিদ্যমান **responsive behavior** বজায় রাখতে হবে।
- যেখানে প্রযোজ্য, **hover, focus, active, disabled, loading এবং error**-এর মতো বিভিন্ন state যাচাই করতে হবে।


## Important Notes


# Real Example 

# Package: @rddm/bricks-uikit

## Overview

`@rddm/bricks-uikit` is a web component library built with **Stencil.js**.

The main purpose of this package is to make **UI Kit components compatible with the Bricks component system** while keeping Bricks components independently usable.

When a consumer installs `@rddm/bricks-uikit`, they should be able to use the available Bricks components together with the UI Kit-compatible components provided by this package.

## Package Goals

This project has two primary goals:

1. Make UI Kit components compatible with Bricks
2. Keep Bricks components independently usable

## Architecture

This repository is structured as a monorepo.

- `basic-structure`: The monorepo root and main project structure. It contains the overall repository configuration and catalog setup.
- `packages`: Contains the packages and applications used by the project.
- `bricks-uikit`: The main package (`@rddm/bricks-uikit`) being developed. This package contains the UI Kit-compatible Bricks components and is the primary focus of development.
- `eslint`: A separate ESLint package/configuration used by `@rddm/bricks-uikit`.
- `playground`: Used to test the package in different applications and environments.
- `nuxt-app`: A Nuxt application used to demonstrate and test `@rddm/bricks-uikit` in a Nuxt environment.
- `uikit-app-test`: Used to display and test only the original UI Kit components.

## Commands

- `pnpm play:bricks`: Runs the Nuxt app in the playground.
- `pnpm play:uikit`: Runs the UI Kit app in the playground.
- `pnpm build:bricks-uikit`: Builds the `@rddm/bricks-uikit` package.
- `pnpm test`: Runs the tests.

## Styling

When matching a UI Kit component:

- Compare the UI Kit implementation with the Bricks implementation.
- Reuse existing **Bricks styles** and **design tokens** where appropriate, while ensuring that the compatible component’s design matches the **UI Kit component**.
- Avoid unnecessarily duplicating the same **styles**.
- Preserve the existing **responsive behavior**.
- Where applicable, verify different states such as **hover, focus, active, disabled, loading, and error**.

# Reference

The `@rddm/uikit` package is the source of truth for the UI Kit components.

When implementing or modifying a component in `@nexus/bricks`, always inspect the corresponding `@rddm/uikit` component first.

Do not assume the UI Kit behavior or API.


## UI Kit Compatibility

### Source of Truth

- The `@rddm/uikit` package is the primary reference implementation for all UI Kit components.
- The goal of this project is to make the required `@rddm/uikit` components compatible with `@nexus/bricks`.
- When working on a component, always inspect the corresponding `@rddm/uikit` implementation before making changes.
- Do not implement a component based only on assumptions or memory.
- The existing `@nexus/bricks` implementation should also be inspected before making changes.
- The goal is compatibility between the two systems, not simply copying the UI Kit implementation.

### Component Reference

- The list of components is maintained in: `docs/uikit-reference/components.md`
- The implementation status is maintained in: `docs/uikit-reference/status.md`
- The `@rddm/uikit` package itself is the source of truth for the actual component implementation.

### Component Workflow

- Work on components sequentially.
- Only one component can have the `running` status at a time.

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

# Status Definitions

- `pending`: Work has not started.
- `running`: Currently being implemented.
- `done`: Implementation is complete and verified.

A component must not be marked as `done` until it has been implemented and verified against the `@rddm/uikit` reference.

# Important Notes

## common
- Before starting the work, always check the status and update it in `docs/uikit-reference/status.md`. After completing the work, update the status file again.
- Do not commit or push automatically.
- Stay within the project context.
- Do not make unrelated changes.
- Ask before making significant architectural or behavioral changes.
- Test changes in the appropriate application before considering the task complete.

## uikit

## bricks
- Do not break existing Bricks functionality unnecessarily.
- Keep Bricks components independently usable.


## package
- Before creating or modifying a component, always find and inspect the corresponding component in the **UI Kit package reference**. Then inspect the same component in the **Bricks package**, review its **API, properties, events, slots, and styling**, and develop accordingly.
- Once the component is successfully developed, use the corresponding component in the **playground Nuxt app** to visually verify it.
- When making a **UI Kit component** compatible with Bricks, consider its **Component API, Behavior, and Styling**.
- Follow the expected **UI Kit behavior**, integrate correctly with **Bricks**, preserve existing behavior, handle interactions consistently, and avoid unnecessary side effects.
