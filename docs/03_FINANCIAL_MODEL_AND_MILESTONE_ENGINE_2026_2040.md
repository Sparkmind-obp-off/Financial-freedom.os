# Financial Model & Milestone Engine 2026–2040

## 1. Purpose

This document defines the calculation engine for translating the Financial Freedom OS into measurable capital targets.

The engine answers:
- Where is the current net-worth baseline?
- How much verified monthly surplus is available?
- What savings/investment rate is actually sustainable?
- How much capital is contributed each year?
- How does compounding affect the trajectory?
- Under explicit assumptions, when could the system cross Rp1B, Rp3B, Rp5B, and Rp10B?
- How sensitive is the trajectory to income growth, expenses, contribution rate, and investment returns?

**Milestone dates are model outputs, not promises or predictions.**

## 2. Core Model

**Income → Expenses → Surplus → Contributions → Compounding**

Surplus_t = Income_t - EssentialExpenses_t - OtherExpenses_t - RequiredReserves_t

InvestableContribution_t = Surplus_t × ContributionRate_t

EndingCapital_t = BeginningCapital_t × (1 + r_t) + Contribution_t

Where t is the period, r_t is the assumed investment return, Contribution_t is capital actually contributed, and EndingCapital_t is modeled ending value.

The engine must distinguish **modeled return** from **realized return**.

## 3. Required Inputs

### A. Starting Financial Position
| Input | Description |
|---|---|
| Starting net worth | Total assets minus liabilities at model start |
| Starting liquid assets | Cash and near-cash assets |
| Starting investment assets | Existing long-term investments |
| Starting productive/business assets | Assets expected to produce income |
| Total liabilities | Outstanding debt and obligations |
| Starting investable capital | Capital available for the modeled investment account |

### B. Monthly Cash Flow
| Input | Description |
|---|---|
| Gross monthly income | Total verified monthly inflow |
| Recurring income | Revenue/income with demonstrated recurrence |
| Essential expenses | Required living/operating costs |
| Variable expenses | Non-fixed spending |
| Tax/reserve allocation | Required reserves |
| Business reinvestment | Capital intentionally retained in the business |
| Monthly surplus | Calculated, not manually assumed |

### C. Growth Assumptions
| Input | Description |
|---|---|
| Income growth rate | Expected annual change in income |
| Expense growth rate | Expected annual change in expenses |
| Contribution-rate target | Portion of surplus allocated to capital |
| Business reinvestment rate | Portion retained for productive growth |
| Investment return assumption | Scenario assumption, not guaranteed |
| Inflation assumption | Used for real-value interpretation |
| Starting date | First modeled period |
| Horizon | Default 2026–2040 |

## 4. Capital Classification

### 4.1 Liquid Net Worth
Cash and assets that can reasonably be converted to cash without relying on uncertain business valuation.

### 4.2 Invested Capital
Long-term financial assets held for compounding.

### 4.3 Productive Business/Asset Value
Conservative estimate of business equity or other productive assets. This value must be separately displayed because it may not be immediately liquid.

### 4.4 Total Net Worth
NetWorth = TotalAssets - TotalLiabilities

The dashboard must never imply that total net worth equals spendable cash.

## 5. Milestone Definitions

| Milestone | Engine Trigger |
|---|---|
| Rp1B | Net worth >= Rp1,000,000,000 |
| Rp3B | Net worth >= Rp3,000,000,000 |
| Rp5B | Net worth >= Rp5,000,000,000 |
| Rp10B | Net worth >= Rp10,000,000,000 |

Each milestone should expose current progress, remaining gap, percentage complete, modeled crossing period, actual crossing period once achieved, capital composition at crossing, and assumptions used.

A milestone should be marked **Achieved** only from verified actual financial data.

## 6. Milestone Gap

For milestone M:
Gap = max(M - CurrentNetWorth, 0)

Progress = min(CurrentNetWorth / M, 1)

Example: if verified net worth is Rp250M, Rp1B progress = 25%, Rp3B = 8.33%, Rp5B = 5%, Rp10B = 2.5%. These are measurement outputs, not judgments.

## 7. Monthly Compounding Engine

For monthly modeling:
MonthlyRate = (1 + AnnualReturn)^(1/12) - 1

EndingBalance_m = BeginningBalance_m × (1 + MonthlyRate) + MonthlyContribution_m

This avoids simply dividing an annual return by 12. If the model uses another convention, the convention must be explicitly recorded.

## 8. Income Growth Engine

Income_(y+1) = Income_y × (1 + IncomeGrowthRate_y)

Support flat income, step-up income, percentage growth, and custom annual income paths. Prefer the custom path once real historical data exists.

## 9. Expense Growth Engine

Model essential and discretionary expenses separately:
EssentialExpense_(y+1) = EssentialExpense_y × (1 + EssentialExpenseGrowth)
DiscretionaryExpense_(y+1) = DiscretionaryExpense_y × (1 + DiscretionaryExpenseGrowth)

This prevents lifestyle inflation from being hidden inside one opaque expense number.

## 10. Contribution Engine

Contribution must be based on **verified surplus**, not desired income.

Surplus = Income - Expenses - RequiredReserves
InvestmentContribution = max(Surplus × InvestmentAllocationRate, 0)
BusinessReinvestment = max(Surplus × BusinessAllocationRate, 0)

Validation rule:
InvestmentAllocation + BusinessAllocation + OtherCapitalAllocation <= 100%

The engine must prevent total allocations from exceeding available surplus.

## 11. Scenario Engine

Support at least three scenarios:

### Scenario A — Conservative
Uses lower income growth and/or lower modeled investment return with controlled expenses.

### Scenario B — Base
Uses the user's current best-supported assumptions.

### Scenario C — High-Growth
Uses stronger income growth and higher contributions.

The high-growth scenario is a planning case, not an expected outcome.

The UI must never label scenarios as guaranteed, certain, safe profit, or promised return.

## 12. Scenario Parameters

| Parameter | Conservative | Base | High-Growth |
|---|---:|---:|---:|
| Income growth | Input | Input | Input |
| Expense growth | Input | Input | Input |
| Contribution rate | Input | Input | Input |
| Investment return | Input | Input | Input |
| Business reinvestment | Input | Input | Input |

Do not hard-code universal percentages into the product. The user must be able to change assumptions.

## 13. Milestone Crossing Algorithm

For each modeled period:
1. Calculate income.
2. Calculate expenses.
3. Calculate surplus.
4. Calculate capital contributions.
5. Apply modeled investment return.
6. Update productive/business asset values using separately documented assumptions.
7. Update liabilities.
8. Calculate total net worth.
9. Compare net worth with each milestone.
10. Record the first period where the threshold is crossed.

Flow: **Input → Cash Flow → Allocation → Asset Growth → Liabilities → Net Worth → Milestone Check**

The engine must retain the assumptions used for every projection run.

## 14. Business Asset Valuation

Business value should not automatically be treated as liquid investment capital.

Support three states:
- **Conservative Valuation:** verified realizable value.
- **Planning Valuation:** documented internal estimate for strategic planning.
- **Excluded Valuation:** uncertain business value excluded from the primary milestone calculation.

Recommended dashboard presentation:
- **Primary Net Worth:** conservative/verified basis
- **Extended Net Worth:** includes documented business valuation

This prevents an uncertain valuation from looking like cash.

## 15. Inflation-Adjusted View

Provide both nominal milestone value and real purchasing-power equivalent.

RealValue = NominalValue / (1 + Inflation)^n

The nominal milestone remains the official target unless the user explicitly changes the target framework.

## 16. Sensitivity Analysis

Show how results change when one major assumption changes.

Minimum dimensions:
- Income growth
- Monthly contribution
- Contribution rate
- Investment return
- Expense growth
- Starting capital

Example questions include: what happens if monthly contribution increases by Rp1M, income growth is lower, expenses rise faster than income, investment return is lower, or starting capital changes?

Sensitivity outputs should be presented as ranges or alternate scenarios, not certainty.

## 17. Reverse Target Calculator

Given target milestone, starting capital, time horizon, assumed return, and current contribution, calculate the approximate additional contribution required.

FV = PV(1+r)^n + PMT × [((1+r)^n - 1)/r]

Therefore:
PMT = (FV - PV(1+r)^n) × r / ((1+r)^n - 1)

When r = 0:
PMT = (FV - PV) / n

This calculator is for planning only and must not be presented as a guaranteed path.

## 18. Target Date Calculator

Calculate the first modeled period where NetWorth >= Milestone.

If the milestone is not reached within the selected horizon:
**Status = Outside modeled horizon**

Do not extrapolate indefinitely without showing the assumption set.

## 19. Financial Freedom Threshold

Milestone wealth and financial freedom are related but not identical.

AnnualRequiredSpending = MonthlyRequiredSpending × 12
CoverageRatio = SustainableNonLaborIncome / AnnualRequiredSpending

Interpretation:
- Below 1.0: modeled income does not cover full required spending.
- Around 1.0: modeled income approximately covers required spending.
- Above 1.0: modeled income exceeds required spending under the stated assumptions.

A safety margin should be defined separately rather than assuming exactly 1.0 is sufficient.

## 20. Data Integrity Rules

1. No negative asset values unless explicitly supported.
2. Liabilities must be included in net-worth calculation.
3. Business revenue is not personal wealth.
4. Gross revenue is not equivalent to investable surplus.
5. Unrealized investment gains must be identified as unrealized.
6. Uncertain business valuation must be separated from liquid assets.
7. Contributions must come from actual available cash flow.
8. Projection assumptions must be versioned.
9. Actual results must override projections for completed periods.
10. Every projection must retain a timestamp and assumption set.

## 21. Projection vs Actual

Every metric should have one of these states:
- **Actual** — verified historical/current data
- **Estimated** — user-entered estimate
- **Projected** — model-generated future value
- **Scenario** — alternate assumption set

The UI must visually distinguish these states. A projected Rp1B crossing is not the same as an achieved Rp1B crossing.

## 22. Recalibration Loop

At each month-end:
**Actual Data → Recalculate → Compare → Diagnose → Adjust Assumptions → New Projection**

The system should not preserve an old projection simply because it was previously generated. Projection history remains available for audit while the active forecast uses the latest verified inputs.

## 23. Minimum Engine Outputs

### Current State
- Net worth
- Liquid net worth
- Investment capital
- Productive/business asset value
- Total liabilities
- Monthly income
- Monthly expenses
- Monthly surplus
- Savings/contribution rate

### Milestones
For Rp1B / Rp3B / Rp5B / Rp10B:
- Current amount
- Gap
- Progress %
- Modeled crossing period
- Actual status

### Forecast
- Annual net-worth path
- Annual contributions
- Investment growth
- Business/productive asset growth
- Liability trajectory

### Scenarios
- Conservative
- Base
- High-Growth

### Sensitivity
- Contribution sensitivity
- Income-growth sensitivity
- Return sensitivity
- Expense-growth sensitivity

## 24. Example Data Schema

```json
{
  "model_version": "1.0",
  "start_date": "2026-01-01",
  "horizon_year": 2040,
  "currency": "IDR",
  "starting_net_worth": 0,
  "starting_liquid_assets": 0,
  "starting_investments": 0,
  "starting_business_assets": 0,
  "starting_liabilities": 0,
  "monthly_income": 0,
  "monthly_essential_expenses": 0,
  "monthly_other_expenses": 0,
  "income_growth_rate": 0,
  "expense_growth_rate": 0,
  "investment_allocation_rate": 0,
  "business_reinvestment_rate": 0,
  "annual_return_assumption": 0,
  "inflation_assumption": 0,
  "milestones": [1000000000, 3000000000, 5000000000, 10000000000]
}
```

Zero values are placeholders. Production must require verified user inputs before producing a personalized projection.

## 25. Acceptance Criteria

- [ ] Starting net worth can be entered.
- [ ] Income and expenses can be entered.
- [ ] Surplus is calculated automatically.
- [ ] Contribution rate is configurable.
- [ ] Investment return assumption is configurable.
- [ ] Income growth is configurable.
- [ ] Expense growth is configurable.
- [ ] Monthly compounding works correctly.
- [ ] Rp1B/Rp3B/Rp5B/Rp10B milestones are tracked.
- [ ] Actual and projected values are separated.
- [ ] Scenario comparison works.
- [ ] Sensitivity analysis works.
- [ ] Business value is separated from liquid capital.
- [ ] Liabilities affect net worth.
- [ ] Projection assumptions are stored/versioned.
- [ ] Forecast recalculates from updated actual data.
- [ ] The system never represents projected returns as guaranteed.

## 26. Next System Layer

Once this engine is approved, the next artifact is:

**Financial Freedom Dashboard & System Architecture**

It should translate the model into:
**Input → Engine → Ledger → Forecast → Milestones → Dashboard → Monthly Review → Recalibration**

The dashboard should prioritize decision-relevant information rather than visual complexity.