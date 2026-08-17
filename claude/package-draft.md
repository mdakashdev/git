# Package:  @test/eats-uikit

## Overview

`@test/eats-uikit` হলো **Stencil.js** দিয়ে তৈরি একটি ওয়েব কম্পোনেন্ট লাইব্রেরি।
এই প্যাকেজের মূল উদ্দেশ্য হলো **eats component system-এর সঙ্গে UI Kit-এর কম্পোনেন্টগুলোকে সামঞ্জস্যপূর্ণ করা**, একই সঙ্গে eats-এর কম্পোনেন্টগুলোকে স্বাধীনভাবে ব্যবহার করার সুবিধা বজায় রাখা।
কোনো ব্যবহারকারী যখন `@test/eats-uikit` ইনস্টল করবেন, তখন তিনি এই প্যাকেজের মাধ্যমে উপলব্ধ **eats components**-এর পাশাপাশি **UI Kit-compatible components**-গুলোও ব্যবহার করতে পারবেন।

```text
আপনি যখন @test/eats-uikit প্যাকেজটি আপনার প্রজেক্টে install করবেন, তখন আপনি একই প্রজেক্টে দুই ধরনের component ব্যবহার করতে পারবেন:
eats components — eats system-এর নিজস্ব components।
UI Kit-compatible components — UI Kit-এর এমন components যেগুলো এই package-এর মাধ্যমে eats-এর সাথে কাজ করার উপযোগী করা হয়েছে।
সহজ উদাহরণ
ধরুন আপনার কাছে আগে থেকেই eats-এর একটা Button component আছে। এখন @test/eats-uikit install করার পর আপনি UI Kit-এর compatible Button, Input, Modal ইত্যাদিও ব্যবহার করতে পারবেন।
অর্থাৎ, এই package install করলে eats components বাদ যাবে না; বরং eats-এর সাথে UI Kit-এর compatible components-ও ব্যবহার করার সুযোগ যোগ হবে।
সংক্ষেপে:
@test/eats-uikit = eats components + UI Kit-compatible components একসাথে ব্যবহার করার সুবিধা।

```

## Package Goals

This project has two primary goals:
1. Make UI Kit components compatible with eats
2. Keep eats components independently usable

## Architecture

- `basic-structure`: monorepo root and catelog use korechi
- `packages`: all package and playground
- `eats-uikit`: eita actuall package ja ami develop kortechi - @test/eats-uikit
- `eslint`: eslint alada akta package ja @test/eats-uikit package a use korechi
- `playground`: eikhane amar package ta bivinno app a test kori.
- `nuxt-app`: nuxt application a demo hisabe use korechi.
- `uikit-app`: ekhane uikit package er sathe develop kora package compare kora hoyeche
- `uikit-app-test`: only uikit package er component gulo ekhane visible kore dekha hoyeche

## Commands

- `pnpm play:eats`: eita diye playground er nuxt-app ke run kore,
- `pnpm play:uikit`: eita diye playground er uikit-app ke run kore",
- `pnpm build:eats-uikit`: eita diye @test/eats-uikit package ke build kore,
- `pnpm test`: eita diye test run kore,

## Styling

When matching a UI Kit component:

- **UI Kit implementation**-এর সঙ্গে **eats implementation** তুলনা করতে হবে।
- প্রয়োজন অনুযায়ী বিদ্যমান **eats styles** এবং **design tokens** পুনরায় ব্যবহার করতে হবে। kintu compatible component gulor desing obossoi ui-kit component er moto design hote hobe.
- অপ্রয়োজনীয়ভাবে একই **styles** বারবার তৈরি করা এড়িয়ে চলতে হবে।
- বিদ্যমান **responsive behavior** বজায় রাখতে হবে।
- যেখানে প্রযোজ্য, **hover, focus, active, disabled, loading এবং error**-এর মতো বিভিন্ন state যাচাই করতে হবে।


## Important Notes


# Real Example 

# Package: @test/eats-uikit

## Overview

`@test/eats-uikit` is a web component library built with **Stencil.js**.

The main purpose of this package is to make **UI Kit components compatible with the eats component system** while keeping eats components independently usable.

When a consumer installs `@test/eats-uikit`, they should be able to use the available eats components together with the UI Kit-compatible components provided by this package.

## Package Goals

This project has two primary goals:

1. Make UI Kit components compatible with eats
2. Keep eats components independently usable

## Architecture

This repository is structured as a monorepo.

- `basic-structure`: The monorepo root and main project structure. It contains the overall repository configuration and catalog setup.
- `packages`: Contains the packages and applications used by the project.
- `eats-uikit`: The main package (`@test/eats-uikit`) being developed. This package contains the UI Kit-compatible eats components and is the primary focus of development.
- `eslint`: A separate ESLint package/configuration used by `@test/eats-uikit`.
- `playground`: Used to test the package in different applications and environments.
- `nuxt-app`: A Nuxt application used to demonstrate and test `@test/eats-uikit` in a Nuxt environment.
- `uikit-app-test`: Used to display and test only the original UI Kit components.

## Commands

- `pnpm play:eats`: Runs the Nuxt app in the playground.
- `pnpm play:uikit`: Runs the UI Kit app in the playground.
- `pnpm build:eats-uikit`: Builds the `@test/eats-uikit` package.
- `pnpm test`: Runs the tests.

## Styling

When matching a UI Kit component:

- Compare the UI Kit implementation with the eats implementation.
- Reuse existing **eats styles** and **design tokens** where appropriate, while ensuring that the compatible component’s design matches the **UI Kit component**.
- Avoid unnecessarily duplicating the same **styles**.
- Preserve the existing **responsive behavior**.
- Where applicable, verify different states such as **hover, focus, active, disabled, loading, and error**.

# Reference

The `@test/uikit` package is the source of truth for the UI Kit components.

When implementing or modifying a component in `@test/eats`, always inspect the corresponding `@test/uikit` component first.

Do not assume the UI Kit behavior or API.


## UI Kit Compatibility

### Source of Truth

- The `@test/uikit` package is the primary reference implementation for all UI Kit components.
- The goal of this project is to make the required `@test/uikit` components compatible with `@test/eats`.
- When working on a component, always inspect the corresponding `@test/uikit` implementation before making changes.
- Do not implement a component based only on assumptions or memory.
- The existing `@test/eats` implementation should also be inspected before making changes.
- The goal is compatibility between the two systems, not simply copying the UI Kit implementation.

### Component Reference

- The list of components is maintained in: `docs/uikit-reference/components.md`
- The implementation status is maintained in: `docs/uikit-reference/status.md`
- The `@test/uikit` package itself is the source of truth for the actual component implementation.

### Component Workflow

- Work on components sequentially.
- Only one component can have the `running` status at a time.

For each component:

1. Select the next `pending` component.
2. Change its status to `running`.
3. Locate the corresponding component in `@test/uikit`.
4. Inspect the UI Kit implementation.
5. Inspect the corresponding `@test/eats` component.
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

A component must not be marked as `done` until it has been implemented and verified against the `@test/uikit` reference.

# Important Notes

## common
- Before starting the work, always check the status and update it in `docs/uikit-reference/status.md`. After completing the work, update the status file again.
- Do not commit or push automatically.
- Stay within the project context.
- Do not make unrelated changes.
- Ask before making significant architectural or behavioral changes.
- Test changes in the appropriate application before considering the task complete.

## uikit

## eats
- Do not break existing eats functionality unnecessarily.
- Keep eats components independently usable.


## package
- Before creating or modifying a component, always find and inspect the corresponding component in the **UI Kit package reference**. Then inspect the same component in the **eats package**, review its **API, properties, events, slots, and styling**, and develop accordingly.
- Once the component is successfully developed, use the corresponding component in the **playground Nuxt app** to visually verify it.
- When making a **UI Kit component** compatible with eats, consider its **Component API, Behavior, and Styling**.
- Follow the expected **UI Kit behavior**, integrate correctly with **eats**, preserve existing behavior, handle interactions consistently, and avoid unnecessary side effects.
