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

- aar CLAUDE.md file a sprint-2 er jonno akta paragraph diye dei. for sprint-2 rules

then kaj korar jonno simpley choto choto command diley hoye jai.
```

Command Example - 
```bash
go step 1 

step 1 approved
```


---
> Note: ami ei pattern a ami aro kichu application test korbo.



# Example : plan file 

# Sprint 2 plan — Charges, Bills, Payments (SRS RS-02, RS-03, RS-04)

We go through this plan **one step at a time**. After each step Claude stops, reports what was done and how it was checked, and waits for the user's go-ahead before starting the next step. A step's Sprint 1 files are only touched once the user approves that step.

Status legend: ⬜ not started · 🟡 in progress · ✅ done (approved by the user)

## Decisions so far

| Topic            | Decision                                                                                                                                                                                                              |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Charge types     | Fixed, Area-based (৳/sqft), Unit-type (by unit category), Custom (amount per unit), and **Member-wise** (amount × members of the unit), as in the prototype                                                           |
| Payer            | Owner or Occupant (tenant if the unit is let, else owner), from the existing `charge_templates.payer`                                                                                                                 |
| Rent             | **Tenant bills include the tenant's monthly rent** (from the unit's tenancy) as a "Rent" line                                                                                                                         |
| Payment reversal | No approval step: needs `payments.payments.reverse` and a reason; the original payment number is kept                                                                                                                 |
| Bill total       | A bill's total = its current lines + adjustments. Opening balance (unpaid earlier bills) and closing due (opening + this bill's balance) are shown on the bill. The existing seed invariant stays valid               |
| Design           | Follows `docs/CoBond.html` (Invoice & Bill group: Utilities, Configure Charges, Generate Invoice). Screens the prototype lacks (bills list, payments, receipts) reuse the same page, table, drawer and modal patterns |
| Money            | Decimal strings in JSON, calculated with `App\Support\Money`; never floats                                                                                                                                            |
| Late fees        | Added automatically to the payer's next bill as a "Late fee" line once the grace period has passed (answered 2026-11-03)                                                                                              |
| Allocation order | Verifying a payment pays the oldest bill first (oldest due date); the rest is kept as advance (answered 2026-11-03)                                                                                                   |
| Due & late fee   | Each charge has Deadline (day of month), Grace Period (days) and Late Fee (None / Fixed / %); no separate Penalty Date                                                                                                |
| Discounts        | Set per unit in Configure Charges only; no invoice-level discount. Published bills are corrected with adjustments                                                                                                     |
| Rent & payer     | Rent is read-only in Configure Charges (from the tenancy); no per-unit payer override, the charge's Payer decides                                                                                                     |
| Screens          | Configure Charges tabs "Occupant" / "Owner"; the payments page is called "Payments"                                                                                                                                   |
| Unit-type        | One unit type per charge; add one charge per type for different amounts                                                                                                                                               |
| Billing payer    | The person holding the role on the 1st of the month (else the first during the month); full month, no proration                                                                                                       |
| Adjustments      | Posted at once when the requester may approve; otherwise pending until an approver approves or rejects                                                                                                                |
| Advance          | Applied automatically to the payer's next published bill for the unit                                                                                                                                                 |
| Rent fund        | Rent goes to a restricted fund "Rent (held for owners)", code RENT, created automatically per organization (answered 2026-11-03)                                                                                      |
| Configured flag  | New table `unit_charge_configurations` (unit, effective from/to) records when a unit's charges were confirmed or reset                                                                                                |
| One-time once    | New table `charge_billings` (charge + unit unique, cascades with the invoice line) keeps a one-time charge to one bill per unit                                                                                       |
| Members          | Stored as a new `members` override type in `unit_charge_overrides` (value = member count)                                                                                                                             |

## Open questions (answered at the step that needs them)

1. ~~Which fund receives rent money?~~ Answered: a separate RENT fund (see Decisions)
2. ~~Late fees: added automatically to the next bill, or only shown as "late fee due"?~~ Answered: added automatically (see Decisions)
3. ~~Allocation order when verifying a payment?~~ Answered: oldest bill first (see Decisions)

---

## Step 1 — Stories ✅
Write the Sprint 2 stories in the same style as Sprint 1 (Given/When/Then, exact labels and messages):
- `stories/09-charges.md` (Utilities, Configure Charges, preview)
- `stories/10-bills.md` (generate, exceptions, publish, bill view, adjustments)
- `stories/11-payments.md` (record, verify, allocate, receipts, reversal)

**Sprint 1 files:** none. **Check:** the user reviews the stories; later steps implement them.

## Step 2 — Backend foundation ✅
- New migrations (Sprint 1 migrations are never edited):
    - charge type `per_member`
    - invoice line type `rent`
    - a unique rule so a one-time charge is billed only once per unit (`charge_billings`)
    - `unit_charge_configurations` (Configured / Not configured yet)
    - override type `members`
- `Services/Finance/RentFund` (finds or creates the RENT fund)
- `app/Http/Middleware/EnsureIdempotency` (`Idempotency-Key` header, uses the existing `idempotency_keys` table; next to the other middleware)
- `app/Services/Audit/AuditTrail` writer (uses the existing `audit_logs` table)
- `packages/types/src/billing.ts` (Charge, UnitCharge, Bill, BillLine, Payment, Allocation, Receipt, Adjustment)

**Sprint 1 files (need approval):** `apps/api/app/Providers/AppServiceProvider.php` (morph map: `billing_period`, `charge_template`, `unit_charge_override`) — approved 2026-11-03, `packages/types/src/index.ts` (export billing types) — approved 2026-11-03.
**Check:** migrations run on the test database; unit tests for the middleware.

## Step 3 — RS-02 Charges API 🟡
- `ChargeTemplateController`: list / add / edit / delete charges (Utilities)
- `UnitChargeController`: a unit's custom amount, members, discount (fixed / %), exemption, with effective dates (Configure Charges)
- `Services/Billing/ChargeCalculator`: works out every unit's amount for a month, with an explanation per line (e.g. "1,050 sqft × ৳ 4.50"), and skips charges outside their effective dates
- `GET /charges/preview?building_id=&period=`: the preview
- Feature tests: amounts per type, effective dates, discounts, exemptions, payer, rent, permissions, tenant isolation

**Sprint 1 files (need approval):** `apps/api/routes/api.php` (new routes).
**Check:** `php artisan test --filter=Billing`.

## Step 4 — RS-02 Utilities page ⬜
- New "Invoice & Bill" group in the sidebar (as in the prototype) with **Utilities**
- `pages/billing/utilities.vue`: table, filters, Add / Edit drawer, View modal, Delete
- `features/billing.css`: the prototype's billing classes (amber / blue / red badges, tabs, etc.)
- `useBillingActions` composable

**Sprint 1 files (need approval):** `apps/web/app/utils/constants.ts` (nav items, billing permissions), `apps/web/app/components/layout/Sidebar.vue` and `composables/useSidebar.ts` (second group), `apps/web/app/composables/useWorkspace.ts` (`BUILDING_PAGES`), `apps/web/app/assets/css/main.css` (one `@import`), `apps/web/tests/nuxt/fake-api.ts` (billing routes).
**Check:** web tests; the user tries the page in the browser.

## Step 5 — RS-02 Configure Charges page ⬜
- `pages/billing/charges.vue`: one row per unit with each charge, payer, rent and total; rows without a configuration are highlighted
- View modal (explanation of each amount = the preview) and Configure modal (custom amounts, members, discount, exemption, effective dates)

**Sprint 1 files:** none beyond Step 4. **Check:** web tests; preview matches the API calculation.

## Step 6 — RS-03 Bills API ⬜
- `Services/Billing/BillGenerator`: creates or refreshes **draft** bills for a building and month; idempotent per unit, month and payer; never touches published bills
- Exceptions: units with no payer, no charges, or a zero total
- Publish: numbers the bills (`DocumentNumber`), locks them, sets issue and due dates
- Adjustments: credit or debit on a published bill with a reason and a number; the bill itself is not edited
- Bill view: opening balance, lines, payments allocated, adjustments, closing due
- Feature tests, including "running generation twice creates no duplicates" and "published bills cannot be changed"

**Sprint 1 files:** `routes/api.php` (more routes). **Check:** `php artisan test --filter=Billing`.

## Step 7 — RS-03 Generate Invoice / Bills page ⬜
- `pages/billing/invoices.vue`: month picker, Generate drafts, exceptions panel, select and Publish, bill status, bill number
- Bill view modal (statement layout like the prototype's invoice) and Adjustment dialog

**Sprint 1 files:** `constants.ts` (nav item), `fake-api.ts`. **Check:** web tests; the user tries generate → publish → adjust.

## Step 8 — RS-04 Payments API ⬜
- Record a payment: cash, bank, cheque, MFS (provider and reference); duplicate references are warned about; `Idempotency-Key` protects against double posting
- Verify: allocates to unpaid bills, keeps the rest as advance (unallocated), posts to the fund ledger, issues a numbered receipt
- Reject (pending payments) and Reverse (verified payments: reason required, original number kept, allocations and ledger reversed)
- Feature tests: partial, advance and overpayments; receipt only after verification; reversal; double submit

**Sprint 1 files:** `routes/api.php`. **Check:** `php artisan test --filter=Payment`.

## Step 9 — RS-04 Payments page ⬜
- `pages/billing/payments.vue`: list with filters (method, status), Record Payment drawer, Verify, Reject, Reverse, Receipt view

**Sprint 1 files:** `constants.ts` (nav item), `fake-api.ts`. **Check:** web tests; the user tries record → verify → receipt → reverse.

## Step 10 — Wrap-up ⬜
- Full API and web test runs, lint (Pint, ESLint)
- Update the demo seeder only if the user approves (it is Sprint 1)
- Lock Sprint 2 in `CLAUDE.md` and tag `sprint-2`


# Example - CLAUDE.md file

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

## Current sprint: Sprint 2

Scope: SRS RS-02 (charges), RS-03 (bills), RS-04 (payments). The plan is `docs/sprint-2-plan.md`.

How we work:

1. **One step at a time.** Do only the step the user asked for. When it's finished, stop, report what was done and how it was checked, and wait for the user's go-ahead before starting the next step.
2. Update the step's status in `docs/sprint-2-plan.md` when it starts (🟡) and when the user approves it (✅). Record new decisions in its "Decisions so far" table.
3. Each step lists the Sprint 1 files it needs. Ask before touching them, even if they're listed in the plan.
4. If the plan needs to change, propose the change and wait for approval.

Keep this file up to date when a sprint is locked or started.
