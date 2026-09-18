# Financial Freedom Dashboard & System Architecture

**Version:** 1.0  
**Planning Horizon:** 2026–2040  
**Status:** Architecture Foundation  
**Related Documents:**  
- `01_FINANCIAL_FREEDOM_BLUEPRINT_2026_2040.md`
- `02_FINANCIAL_FREEDOM_OPERATING_PLAN_2026_2030.md`
- `03_FINANCIAL_MODEL_AND_MILESTONE_ENGINE_2026_2040.md`

---

## 1. Purpose

This document defines the product and system architecture for the Financial Freedom OS dashboard.

The dashboard is not intended to be a decorative personal-finance tracker. It is a decision-support system that turns verified financial records and configurable assumptions into:

`INPUT → LEDGER → ENGINE → FORECAST → MILESTONES → DASHBOARD → REVIEW → RECALIBRATION`

The system must clearly distinguish historical/actual data from estimates, projections, and scenarios.

---

## 2. Product Objective

The system should answer, at minimum:

1. What is my current financial position?
2. How much cash flow and surplus do I generate?
3. How much capital am I actually accumulating?
4. How much progress have I made toward Rp1B, Rp3B, Rp5B, and Rp10B?
5. Under current assumptions, when could each milestone be reached?
6. Which assumptions have the largest effect on the path?
7. What changed since the previous monthly close?
8. What actions should be reviewed next?

The system must support informed decisions without presenting projections as guarantees.

---

## 3. System Architecture

### Layer 1 — Input

Sources of financial information:

- Manual transaction entry
- Monthly income records
- Expense records
- Asset records
- Liability records
- Business/productive asset records
- Investment contribution records
- Assumption configuration
- Optional future import/connectors

Input states:

- `ACTUAL`
- `ESTIMATED`
- `PROJECTED`
- `SCENARIO`

Actual financial records must remain distinguishable from model assumptions.

### Layer 2 — Financial Ledger

The ledger is the system of record.

Core records:

- Income
- Expense
- Transfer
- Contribution
- Withdrawal
- Asset valuation
- Liability balance
- Business asset valuation

Ledger principles:

- append-oriented transaction history
- corrections recorded transparently
- monthly closing snapshots
- source/date/category attached to records
- no silent rewriting of historical actuals

### Layer 3 — Calculation Engine

The engine consumes ledger data plus an assumption set.

Core calculations:

- monthly income
- monthly expenses
- monthly surplus
- contribution amount
- contribution rate
- liquid net worth
- invested capital
- productive/business assets
- liabilities
- total net worth
- reserve coverage
- milestone progress
- projected milestone crossing
- inflation-adjusted values
- scenario outputs

Primary flow:

`Income → Expenses → Surplus → Contributions → Capital → Compounding → Net Worth`

### Layer 4 — Scenario Engine

The scenario engine must support configurable assumption sets rather than hard-coded predictions.

Minimum scenarios:

- Conservative
- Base
- High-Growth

Configurable variables:

- income growth
- expense growth
- contribution rate
- business reinvestment
- investment return assumption
- inflation
- starting capital
- time horizon

Every projection must expose its assumption set.

### Layer 5 — Milestone Engine

Milestones:

| Milestone | Trigger |
|---|---:|
| Rp1B | Net worth ≥ Rp1,000,000,000 |
| Rp3B | Net worth ≥ Rp3,000,000,000 |
| Rp5B | Net worth ≥ Rp5,000,000,000 |
| Rp10B | Net worth ≥ Rp10,000,000,000 |

For every milestone, calculate:

- current value
- target value
- remaining gap
- percentage progress
- historical progress
- projected crossing period
- scenario-dependent crossing period
- last recalculation date

The system must not imply that a projected date is guaranteed.

---

## 4. Core Data Model

### UserProfile

- id
- currency
- planning_start_date
- planning_horizon
- financial_freedom_spending_target
- created_at
- updated_at

### Account

- id
- name
- type
- liquidity_class
- opening_balance
- active

### Transaction

- id
- date
- account_id
- type
- category
- amount
- description
- source
- status
- created_at

### IncomeSource

- id
- name
- type
- recurring
- monthly_baseline
- growth_assumption
- active

### Asset

- id
- name
- asset_type
- current_value
- liquidity_class
- valuation_date
- valuation_method
- included_in_net_worth

### Liability

- id
- name
- liability_type
- outstanding_balance
- interest_assumption
- due_profile

### Contribution

- id
- date
- amount
- destination
- source
- purpose

### BusinessAsset

- id
- name
- asset_type
- carrying_value
- planning_value
- conservative_value
- excluded_value
- valuation_date

Business asset value must remain separate from liquid/invested capital.

### AssumptionSet

- id
- name
- income_growth_rate
- expense_growth_rate
- contribution_rate
- reinvestment_rate
- annual_return_rate
- inflation_rate
- horizon_years
- created_at

### ProjectionRun

- id
- assumption_set_id
- run_date
- starting_net_worth
- ending_net_worth
- milestone_results
- model_version

### Milestone

- id
- target_amount
- current_amount
- gap_amount
- progress_percent
- projected_period
- scenario
- status

### MonthlySnapshot

- id
- month
- income
- expenses
- surplus
- contributions
- liquid_net_worth
- invested_capital
- productive_assets
- liabilities
- total_net_worth
- reserve_months
- notes

---

## 5. Dashboard Information Architecture

### 5.1 Overview

The overview is the primary decision surface.

Show:

- Total Net Worth
- Liquid Net Worth
- Invested Capital
- Productive/Business Assets
- Liabilities
- Monthly Income
- Monthly Expenses
- Monthly Surplus
- Contribution Rate
- Reserve Coverage
- Current milestone
- Next milestone
- Projection status

The first screen should communicate position, momentum, and gaps without requiring the user to inspect charts.

### 5.2 Cash Flow

Views:

- income by month
- expenses by category
- surplus trend
- contribution trend
- recurring vs variable income
- recurring vs variable expenses

Key metric:

`Surplus Rate = Monthly Surplus / Monthly Income`

### 5.3 Net Worth

Show:

- historical net-worth timeline
- asset composition
- liabilities
- liquid vs illiquid capital
- invested vs productive capital

Every valuation should show its valuation date and status.

### 5.4 Milestones

Show four target cards:

- Rp1B
- Rp3B
- Rp5B
- Rp10B

Each card should contain:

- current amount
- remaining gap
- progress
- actual historical progress
- projected crossing period
- scenario selector

### 5.5 Forecast

Forecast view:

- monthly projected net worth
- yearly projected net worth
- contribution assumptions
- return assumptions
- inflation-adjusted view
- milestone crossing periods

Projection charts must visually distinguish actual history from future projections.

### 5.6 Scenarios

Allow switching between:

- Conservative
- Base
- High-Growth
- Custom

Users should be able to edit assumptions and immediately see how the model changes.

### 5.7 Sensitivity

Show how milestone timing changes when key variables change.

Minimum sensitivity dimensions:

- monthly contribution
- income growth
- investment return assumption
- expense growth
- business reinvestment

This is an analysis tool, not a prediction engine.

### 5.8 Monthly Close

Monthly close should answer:

- What happened?
- What was different from plan?
- What is the current net worth?
- How much surplus was generated?
- How much was contributed?
- Did reserve coverage improve?
- Did milestone progress improve?
- What assumptions need recalibration?
- What are the next month's priorities?

A month should be marked closed only after required actual inputs are confirmed.

### 5.9 Settings & Assumptions

Manage:

- currency
- planning horizon
- spending target
- milestone targets
- scenario assumptions
- asset valuation policy
- business asset inclusion policy
- inflation assumption
- return assumptions

Changes to assumptions should not rewrite historical actuals.

---

## 6. Dashboard KPI Definitions

### Net Worth

`Total Net Worth = Assets - Liabilities`

### Liquid Net Worth

`Liquid Net Worth = Liquid Assets - Relevant Liabilities`

### Monthly Surplus

`Monthly Surplus = Income - Expenses - Required Reserves`

### Contribution Rate

`Contribution Rate = Investable Contribution / Income`

### Reserve Coverage

`Reserve Months = Liquid Safety Reserve / Average Monthly Essential Expenses`

### Milestone Progress

`Progress = Current Net Worth / Target Net Worth`

Cap displayed progress at 100% for presentation while preserving the actual value internally.

---

## 7. Decision Layer

The dashboard should not merely display numbers.

It should surface factual changes such as:

- income increased/decreased
- expense ratio increased/decreased
- contribution rate changed
- reserve coverage changed
- net worth changed
- milestone gap widened/narrowed
- projection moved because an assumption changed
- actual performance diverged from the previous projection

Recommended action prompts should be generated from explicit rules and displayed as review items, not as guaranteed financial outcomes.

Example:

`IF surplus_rate declines for 3 consecutive months → flag "Cash-flow review"`

`IF reserve_months < configured_minimum → flag "Safety reserve review"`

`IF contribution_rate falls below configured_target → flag "Capital accumulation review"`

---

## 8. Data Integrity & Auditability

Rules:

1. Actual records must never be silently overwritten.
2. Every projection references a model version and assumption set.
3. Every asset valuation has a date and method.
4. Business valuation must be separable from liquid/invested capital.
5. Estimated data must never be displayed as actual.
6. Scenario outputs must never be stored as historical facts.
7. Monthly snapshots preserve historical dashboard state.
8. Calculations should be deterministic for identical inputs and model versions.
9. User corrections must leave an audit trail.
10. Exported data must preserve state labels.

---

## 9. Privacy & Security

Financial information is sensitive.

Minimum requirements:

- HTTPS in production
- encrypted storage where supported
- server-side secrets only
- least-privilege access
- authenticated access before persistent financial data is exposed
- no third-party sharing by default
- audit events for sensitive changes
- export capability
- deletion capability
- safe error messages without financial-data leakage

The production architecture should remain compatible with Cloudflare-based deployment.

---

## 10. Technical Architecture

Recommended initial structure:

`Frontend`
→ dashboard UI

`Application Layer`
→ authentication  
→ validation  
→ business rules  
→ API handlers

`Domain Layer`
→ ledger services  
→ calculation engine  
→ milestone engine  
→ scenario engine  
→ monthly close engine

`Persistence Layer`
→ relational database  
→ migrations  
→ audit records

`Observability Layer`
→ application logs  
→ calculation-run metadata  
→ error tracking  
→ audit trail

The domain/calculation layer should remain independent from UI components so the financial engine can later support:

- web dashboard
- mobile interface
- voice operator
- automated monthly reports
- external integrations

---

## 11. MVP Scope

### MVP must include

- manual financial input
- income/expense ledger
- asset/liability records
- monthly snapshot
- net-worth calculation
- surplus calculation
- contribution tracking
- Rp1B/Rp3B/Rp5B/Rp10B milestones
- configurable assumptions
- Base scenario
- forecast
- monthly close
- clear Actual vs Projected distinction

### Defer from MVP

- bank synchronization
- broker synchronization
- automatic market pricing
- complex tax engine
- multi-user household collaboration
- AI financial advice
- automated external trading
- complex portfolio optimization

The system should prove the core operating loop before adding integrations.

---

## 12. Production Evolution

### Phase A — Foundation

- schema
- domain types
- calculation functions
- validation
- test fixtures

### Phase B — MVP Dashboard

- overview
- cash flow
- net worth
- milestones
- settings

### Phase C — Forecast & Scenarios

- projection runs
- scenario engine
- sensitivity analysis
- inflation-adjusted view

### Phase D — Monthly Operating System

- monthly close
- variance analysis
- recalibration
- historical snapshots

### Phase E — Production Hardening

- authentication
- authorization
- database security
- audit logs
- observability
- backups
- export/delete
- Cloudflare deployment

### Phase F — Operator Layer

Potential future capabilities:

`Voice/Input → Financial Context → Calculation → Scenario → Review → Action`

This layer should only be added after the deterministic financial core is reliable.

---

## 13. Acceptance Criteria

The architecture is considered implemented when:

- [ ] Financial records can be entered and persisted.
- [ ] Net worth is reproducibly calculated.
- [ ] Monthly surplus is reproducibly calculated.
- [ ] Contribution rate is calculated.
- [ ] Assets and liabilities are separated correctly.
- [ ] Business assets can be included/excluded according to policy.
- [ ] Rp1B/Rp3B/Rp5B/Rp10B progress is calculated.
- [ ] Projection runs reference explicit assumptions.
- [ ] Actual and projected values are visually separated.
- [ ] Scenario changes produce deterministic model changes.
- [ ] Monthly snapshots can be closed and retained.
- [ ] Historical actuals are not silently rewritten.
- [ ] Sensitive financial data is protected by authentication/authorization in production.
- [ ] The calculation engine has automated tests.
- [ ] The system can be deployed independently of the development sandbox.

---

## 14. Operating Loop

The final operating loop is:

`1. CAPTURE → 2. CLOSE → 3. CALCULATE → 4. FORECAST → 5. COMPARE → 6. REVIEW → 7. RECALIBRATE → 8. EXECUTE`

### Capture
Record actual financial activity.

### Close
Finalize the month.

### Calculate
Update financial position.

### Forecast
Run current assumptions.

### Compare
Compare actual vs previous plan.

### Review
Identify material deviations.

### Recalibrate
Update assumptions based on evidence.

### Execute
Carry the reviewed priorities into the next operating period.

This loop is the core behavior of Financial Freedom OS.

---

## 15. Next Build Artifact

The next implementation artifact should be:

`05_GENSPARK_IMPLEMENTATION_PROMPT.md`

Its purpose is to translate this architecture and the preceding blueprint/model documents into a deterministic implementation instruction for Genspark, including:

- repository inspection
- stack detection
- architecture constraints
- implementation sequence
- database/schema requirements
- calculation engine requirements
- UI requirements
- test requirements
- security requirements
- acceptance gates
- Git workflow
- deployment constraints

No production implementation should bypass the documented acceptance criteria.
