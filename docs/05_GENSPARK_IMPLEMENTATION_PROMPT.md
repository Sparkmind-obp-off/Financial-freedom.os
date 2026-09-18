# Genspark Implementation Prompt — Financial Freedom OS

**Version:** 1.0  
**Target:** Genspark AI coding/implementation agent  
**Repository:** `Sparkmind-obp-off/Financial-freedom.os`  
**Branch:** `main`

## 1. Mission

Implement the Financial Freedom OS as a production-oriented financial operating system based on the repository documentation.

The implementation must follow this chain:

`CAPTURE → CLOSE → CALCULATE → FORECAST → COMPARE → REVIEW → RECALIBRATE → EXECUTE`

Do not replace the documented financial model with generic budgeting logic. Do not invent personal financial data.

## 2. Mandatory Pre-Implementation Inspection

Before writing application code:

1. Inspect the entire repository.
2. Read:
   - `README.md`
   - `docs/01_FINANCIAL_FREEDOM_BLUEPRINT_2026_2040.md`
   - `docs/02_FINANCIAL_FREEDOM_OPERATING_PLAN_2026_2030.md`
   - `docs/03_FINANCIAL_MODEL_AND_MILESTONE_ENGINE_2026_2040.md`
   - `docs/04_FINANCIAL_FREEDOM_DASHBOARD_AND_SYSTEM_ARCHITECTURE.md`
3. Detect the existing stack, package manager, framework, database configuration, scripts, and deployment configuration.
4. Reuse the existing stack when viable.
5. Do not migrate frameworks merely for preference.
6. Identify missing infrastructure before implementing it.
7. Preserve existing working functionality.

If the repository is still documentation-only, scaffold the smallest maintainable production-ready application appropriate for Cloudflare-compatible deployment.

## 3. Non-Negotiable Architecture

Use clear separation between:

`UI → Application/API → Domain Engine → Persistence`

The financial calculation engine must be independent of UI components.

Required domain modules:

- ledger
- cash-flow calculation
- net-worth calculation
- contribution calculation
- milestone calculation
- projection/forecast engine
- scenario engine
- sensitivity analysis
- monthly close
- audit/version metadata

Do not place financial formulas directly inside presentation components.

## 4. Financial Model

Implement the documented model exactly.

### Surplus

`Surplus = Income - EssentialExpenses - OtherExpenses - RequiredReserves`

### Investable Contribution

`InvestableContribution = Surplus × ContributionRate`

### Capital Compounding

`EndingCapital = BeginningCapital × (1 + ReturnRate) + Contribution`

For monthly compounding:

`MonthlyRate = (1 + AnnualReturn)^(1/12) - 1`

`EndingBalance = BeginningBalance × (1 + MonthlyRate) + MonthlyContribution`

### Net Worth

`NetWorth = Assets - Liabilities`

### Liquid Net Worth

`LiquidNetWorth = LiquidAssets - RelevantLiabilities`

### Contribution Rate

`ContributionRate = InvestableContribution / Income`

### Reserve Coverage

`ReserveMonths = LiquidSafetyReserve / AverageMonthlyEssentialExpenses`

Handle zero denominators safely.

## 5. Milestone Engine

Implement these configurable default milestones:

- Rp1,000,000,000
- Rp3,000,000,000
- Rp5,000,000,000
- Rp10,000,000,000

For each milestone calculate:

- current value
- target value
- gap
- progress percentage
- historical status
- projected crossing period
- scenario
- model version
- calculation timestamp

Never present a projected milestone date as guaranteed.

## 6. Scenario Engine

Support:

- Conservative
- Base
- High-Growth
- Custom

Variables must be configurable:

- income growth
- expense growth
- contribution rate
- business reinvestment
- annual return assumption
- inflation
- starting capital
- horizon

Do not hard-code investment returns as facts.

All forecast output must retain a reference to the assumption set that generated it.

## 7. Business Asset Treatment

Separate:

1. Liquid assets
2. Investment assets
3. Productive/business assets
4. Liabilities

Business assets must support at least:

- conservative value
- planning value
- excluded value

Do not silently treat speculative business valuation as liquid wealth.

## 8. Required Data Model

Implement equivalents of:

- UserProfile
- Account
- Transaction
- IncomeSource
- Asset
- Liability
- Contribution
- BusinessAsset
- AssumptionSet
- ProjectionRun
- Milestone
- MonthlySnapshot

Use stable IDs, timestamps, validation, and appropriate relationships.

## 9. Data State Model

Every financial value should be traceable to a state where relevant:

- ACTUAL
- ESTIMATED
- PROJECTED
- SCENARIO

The UI must clearly distinguish these states.

Historical actuals must not be overwritten silently.

## 10. Required MVP Screens

Implement:

### Overview
Display:

- total net worth
- liquid net worth
- invested capital
- productive/business assets
- liabilities
- monthly income
- monthly expenses
- monthly surplus
- contribution rate
- reserve months
- milestone progress
- projection status

### Cash Flow
Include:

- income trend
- expense trend/category
- surplus trend
- contribution trend
- recurring vs variable breakdown

### Net Worth
Include:

- historical net worth
- asset composition
- liabilities
- liquidity classification

### Milestones
Show Rp1B/Rp3B/Rp5B/Rp10B with:

- current amount
- gap
- progress
- projected period
- scenario

### Forecast
Show:

- monthly forecast
- annual forecast
- assumptions
- inflation-adjusted view
- milestone crossing periods

### Scenarios
Allow scenario selection and assumption editing.

### Monthly Close
Support:

- actual input confirmation
- plan-vs-actual comparison
- variance review
- snapshot creation
- recalibration notes

### Settings
Support:

- currency
- horizon
- spending target
- milestones
- scenario assumptions
- valuation policy

## 11. UX Requirements

The UI should be:

- decision-first
- simple
- readable
- mobile-friendly
- responsive
- explicit about uncertainty
- clear about actual vs projection
- free from false precision

Do not build a dashboard overloaded with decorative charts.

Numbers must be understandable before charts are inspected.

Use consistent formatting for Indonesian Rupiah where currency is configured as IDR.

## 12. Decision Flags

Implement deterministic review flags.

Examples:

`IF surplus_rate declines for 3 consecutive months → cash-flow review flag`

`IF reserve_months < configured minimum → safety reserve review flag`

`IF contribution_rate < configured target → capital accumulation review flag`

Flags are review prompts, not financial guarantees or personalized investment advice.

## 13. Monthly Close

A monthly close should:

1. collect actual records
2. validate required inputs
3. calculate income
4. calculate expenses
5. calculate surplus
6. calculate contributions
7. update net worth
8. update reserve coverage
9. update milestone progress
10. compare against prior projection
11. record variance
12. optionally recalibrate assumptions
13. create immutable/traceable monthly snapshot

Do not allow a closed month to be silently rewritten.

## 14. Projection Engine Requirements

Projection runs must be reproducible.

Store:

- run ID
- model version
- assumption set
- starting values
- horizon
- generated timestamp
- resulting milestone periods
- ending values

Identical inputs + identical model version must produce identical results.

## 15. Sensitivity Analysis

Provide a simple sensitivity layer for:

- monthly contribution
- income growth
- return assumption
- expense growth
- business reinvestment

Show how changing one variable affects projected milestone timing or ending capital.

Clearly label sensitivity output as model analysis.

## 16. Security

Treat financial data as sensitive.

Implement, where applicable to the chosen stack:

- authenticated access
- authorization
- server-side secrets
- secure environment variables
- input validation
- output sanitization
- least privilege
- audit events
- safe error handling
- no sensitive data in logs
- export/delete capability

Never expose secrets in client-side code.

## 17. Testing

Create automated tests for:

### Unit tests
- surplus
- contribution
- compounding
- net worth
- liquid net worth
- reserve months
- milestone progress
- milestone crossing
- scenario calculations
- inflation adjustment
- zero-denominator cases

### Integration tests
- ledger → calculation engine
- calculation engine → dashboard data
- assumption set → projection run
- monthly close → snapshot

### Acceptance tests
Verify all acceptance criteria in document 04.

Use deterministic fixtures.

Include edge cases:

- zero income
- zero contribution
- negative monthly surplus
- no assets
- liabilities greater than assets
- zero return
- very long horizon
- milestone already achieved
- missing optional valuation
- business asset excluded

## 18. Seed Data Policy

If demo data is needed:

- label it clearly as DEMO
- do not mix it with actual user data
- use synthetic values
- make it removable/resettable

Never fabricate the user's personal financial baseline.

## 19. Cloudflare / Production Constraint

The application must be designed so production deployment can be separated from the Genspark sandbox.

Prefer architecture compatible with Cloudflare Pages/Workers and an external relational persistence layer where required by the chosen stack.

Do not make the Genspark sandbox a production dependency.

## 20. Implementation Sequence

Execute in this order:

### Phase 0 — Repository Audit
- inspect repo
- identify stack
- confirm docs
- identify gaps

### Phase 1 — Foundation
- project structure
- types
- schema
- validation
- domain utilities
- test infrastructure

### Phase 2 — Financial Engine
- ledger calculations
- net worth
- surplus
- contributions
- milestones
- projection engine
- scenario engine

### Phase 3 — Persistence
- database
- migrations
- repositories/services
- monthly snapshots
- audit records

### Phase 4 — Dashboard MVP
- Overview
- Cash Flow
- Net Worth
- Milestones
- Forecast
- Settings

### Phase 5 — Operating Loop
- Monthly Close
- variance analysis
- recalibration
- sensitivity

### Phase 6 — Hardening
- auth
- authorization
- security
- observability
- backup/export/delete
- production configuration

Do not skip earlier phases to build advanced UI.

## 21. Git Workflow

Work incrementally.

Use meaningful commits such as:

- `feat: scaffold financial freedom os`
- `feat: implement financial domain engine`
- `feat: add financial persistence`
- `feat: add dashboard mvp`
- `feat: add forecast and scenarios`
- `feat: add monthly close workflow`
- `test: add financial engine coverage`
- `chore: harden production configuration`

Do not commit secrets, credentials, personal financial data, or generated private keys.

## 22. Definition of Done

The implementation is not complete merely because the UI renders.

It is complete only when:

- [ ] documented architecture is implemented
- [ ] financial formulas are tested
- [ ] actual/projected/scenario states are distinct
- [ ] ledger data persists correctly
- [ ] net worth is reproducible
- [ ] surplus is reproducible
- [ ] milestones work
- [ ] forecasts reference assumptions
- [ ] scenarios work
- [ ] monthly close works
- [ ] historical snapshots remain traceable
- [ ] security baseline is implemented
- [ ] automated tests pass
- [ ] production configuration is documented
- [ ] no user financial data was invented
- [ ] acceptance criteria from docs 03 and 04 are satisfied

## 23. Agent Behavior Rules

When implementing:

1. Inspect before changing.
2. Prefer existing repository conventions.
3. Make the smallest correct change.
4. Keep domain logic deterministic.
5. Never invent financial data.
6. Never hide assumptions.
7. Never represent projections as facts.
8. Never expose secrets.
9. Test every financial formula.
10. Preserve documentation alignment.
11. If a requirement conflicts with existing code, document the conflict and choose the least destructive compatible implementation.
12. Do not add unnecessary integrations before the core financial operating loop works.

## 24. Final Verification Report

At the end of implementation, produce a concise report containing:

- detected stack
- implemented phases
- files changed
- database/schema changes
- financial formulas implemented
- tests executed and results
- security checks
- remaining known limitations
- deployment instructions
- exact commit SHA(s)

The final report must distinguish completed work from planned work.

## 25. Source of Truth

Priority order:

1. `docs/03_FINANCIAL_MODEL_AND_MILESTONE_ENGINE_2026_2040.md`
2. `docs/04_FINANCIAL_FREEDOM_DASHBOARD_AND_SYSTEM_ARCHITECTURE.md`
3. `docs/02_FINANCIAL_FREEDOM_OPERATING_PLAN_2026_2030.md`
4. `docs/01_FINANCIAL_FREEDOM_BLUEPRINT_2026_2040.md`
5. `README.md`
6. existing implementation conventions

If implementation details are unspecified, choose a simple, maintainable, testable solution and document the decision.

**START NOW: inspect the repository and documentation first. Then implement Phase 0 and proceed sequentially through the highest safe phase supported by the repository state. Do not claim completion without verification.**
