# My practical experience 

ami akta SRS dekhe akta portion pojjonto AI ke diye banaichi ja amar starting ta fullfill kore. 

then ami SRS er baki kaj gulo korte cai, kaj ta onek boro at a time bose AI ke diye korano kothin. 

tai ami akta workflow te niye aste cai. sei jonno ami AI ke promt diye boli 

```
Lock all the work completed so far as Sprint 1. Do not make any changes to Sprint 1 unless I explicitly ask you to do so.
If you think any changes are necessary, ask for my permission before making them.
We will work on Sprint 2 next.
To ensure these instructions are followed, create and maintain a CLAUDE.md file with these guidelines if necessary.
```

then akta CLAUDE.md file root a create kore. ami ja caichilam setar jonno akta rule kora hoyeche. 

ja ami each session load hobar somay pabo. or AI mone rakhbe.

```
# CLAUDE.md

## Sprint 1 is locked

Sprint 1 is everything in the repository up to and including commit `7a6ec7e` (git tag `sprint-1`). It covers:

- Stories `stories/01-navigation.md` … `stories/08-shared-behaviour.md` (and `stories/draft.md`)
- The code that implements them: `apps/api` (registry: buildings, blocks, floors, units, residences, occupancies, import, settings / PIN), `apps/web` (pages, components, composables, CSS), `packages/types`, `packages/ui`
- Migrations, seeders and tests that exist at that commit, and the docs in `docs/`

Rules:

1. **Do not change Sprint 1 unless the user explicitly asks for that specific change.** This includes code, stories, migrations, seeders, tests and docs that belong to Sprint 1.
2. **If a Sprint 1 change looks necessary** (a bug, a conflict with Sprint 2 work, a refactor or a shared helper), stop and ask the user first. Explain what you would change, where, and why, then wait for approval. Don't make the change as a side effect of Sprint 2 work.
3. **Approval covers only the change that was approved.** It doesn't unlock the rest of Sprint 1.
4. **Never edit an existing migration.** Schema or data changes go in a new migration, and only when it's Sprint 2 work or an approved Sprint 1 change.
5. Sprint 2 work goes in new files where possible. If Sprint 2 needs to extend a Sprint 1 file (for example a new route, a new nav item or a new field on a shared type), that counts as a Sprint 1 change: ask first.
6. To see whether a file is Sprint 1, run `git diff sprint-1 -- <path>` or `git log sprint-1 -- <path>`.
```


then ami sprint 2 niye kaj korar jonno ami AI ke boli 

now i want to work sprint-2 @docs/srs.pdf , [ekhane rs-02,03,04 ei paragraph gulo pest korechilam], 

ami bolechilam je. ami kaj ta workflow maintain kore korte cai. step by step. 

```text
then se amake docs/sprint-2-plan.md akta file sob kichu step by step kore dei.

then kaj korar jonno simpley choto choto command diley hoye jai.
```

```bash
go step 1 

step 1 approved
```


---
> Note: ami ei pattern a ami aro kichu application test korbo.