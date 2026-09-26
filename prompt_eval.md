# Prompt Evaluation

## Overall Score

| Parameter | Score | Maximum |
|---|---:|---:|
| Prompt Clarity | 90 | 100 |
| Output Quality & Schema Guidance | 49 | 100 |
| Efficiency & Token Economy | 35 | 50 |
| **Total** | **174** | **250** |

## Executive Summary

This is a well-structured, unusually concrete operations prompt. It gives the model a role, a fixed budget and time window, identifiable rush periods, specific deliverables, and constraints against generic filler. Its strongest feature is that it ties the menu and production plan to unit economics and waste control rather than asking for an ungrounded menu brainstorm.

The main weaknesses are the request to show internal step-by-step reasoning, missing accounting definitions, and the lack of supplied or validated demand and cost data. Asking the model to state assumptions helps, but it does not prevent invented benchmarks or ensure the tables and ROI reconcile. There are no examples or explicit fallback rules for insufficient data. The prompt is efficient overall, though several constraints repeat and the scenario is not parameterized for reuse.

## Evaluated Prompt Analysis

- **Target:** The contents of `prmpt.txt`, a six-day college-canteen pilot brief with a ₹10,000 budget.
- **Approximate length:** About 500 words and roughly 700 tokens. Token count varies by tokenizer; this is an estimate, not a tokenizer measurement.
- **Structure:** Persona and objective; scenario; three planning questions; assumptions instruction; five numbered deliverables; output rules.
- **Core output requested:** Menu unit economics, capital allocation, daily quantities, rush and waste tactics, weekly financial projection, sensitivity to lower footfall, and a final prioritization statement.

## Detailed Parameter Breakdown

### 1. Prompt Clarity: 90/100

| Criterion | Score | Notes |
|---|---:|---|
| Role & Persona Definition | 19/20 | “Canteen Operations Strategist and Unit Economics Analyst” plus “think like a P&L owner” establishes a useful perspective. |
| Task Specificity & Negative Constraints | 22/25 | Fixed budget, six days, rush windows, item count, tables, and no-filler constraint are actionable. “Maximize net profit and student satisfaction” and “wastage near zero” do not define how to resolve tradeoffs. |
| Instruction Structure & Delimiters | 19/20 | Clear headings and numbered deliverables make the request scannable. Assumptions are requested before deliverables, but their exact format is not defined. |
| Tone, Style & Target Audience | 14/15 | “No filler/motivational language” and the price-sensitive student context establish tone and audience. |
| Unambiguous Language | 16/20 | Concrete constraints are strong, but “exactly 6 working days (Mon–Sat),” “net profit,” “COGS,” “ROI,” and “no-reorder-buffer window” can be interpreted in more than one way. |

**Strengths**

- The scenario is specific: “₹10,000 and exactly 6 working days (Mon–Sat)” and two defined rush windows.
- It requests assumptions explicitly: “do not silently invent numbers.”
- It limits the deliverable set and prescribes useful table fields, including cost price, selling price, margin, and prep time.

**Weaknesses**

- “Reason step-by-step through these questions internally (show this as a brief ‘Strategic Reasoning’ section)” conflates private reasoning with a user-facing rationale. Request a concise decision summary instead.
- “No-reorder-buffer window” does not say whether replenishment is prohibited or simply uncertain.
- “Maximize net profit and student satisfaction” gives no tie-break rule if the goals conflict.
- The prompt does not define whether buffer/reserve allocations count as expenses, how unused stock is valued, or what the ROI denominator is.

### 2. Output Quality & Schema Guidance: 49/100

| Criterion | Score | Notes |
|---|---:|---|
| Output Format & Schema Enforcement | 25/30 | Exact section headings and table columns are a strong start. It does not require budget totals, quantities, cost assumptions, formulas, and projections to reconcile across sections. |
| Few-Shot Examples & Demonstrations | 0/25 | No examples are provided. That is not essential for this analytical task, but examples could clarify expected calculation conventions. |
| Edge Cases & Fallback Instructions | 14/25 | Requires an unsold-stock cutoff and a 20%-lower-footfall sensitivity, but does not define behavior when inputs are unavailable, estimates are uncertain, demand is below the minimum batch, or production exceeds the budget. |
| Factuality & Hallucination Prevention | 10/20 | Explicit assumptions are a useful safeguard, but the prompt does not distinguish supplied facts from illustrative estimates or require unsupported local benchmarks to be labeled. |

**Strengths**

- The request for “Total revenue, COGS, net profit, ROI% for the week” and “Show the formula” encourages auditable calculations.
- Requiring a concrete mechanism and a same-day stock decision rule discourages generic advice.
- The 20% footfall sensitivity is a helpful, explicit stress case.

**Weaknesses**

- It does not define revenue as sold units only, distinguish sold-unit COGS from spoilage, or say how unused inventory and reserve cash affect the reported result.
- “CP/unit” and the separate packaging allocation could cause packaging to be counted twice or omitted from unit economics.
- The plan can invent footfall, prices, costs, and sales without stating that they are illustrative or showing how these values drive the quantities.
- No constraint explicitly checks that category allocations total ₹10,000, percentages total 100%, or planned production fits the purchased stock and rush capacity.

### 3. Efficiency & Token Economy: 35/50

| Criterion | Score | Notes |
|---|---:|---|
| Conciseness & Fluff Elimination | 12/15 | Most instructions are decision-oriented; some requirements recur across the planning questions and deliverables. |
| Token Economy & Context Footprint | 12/15 | The specificity is valuable, though the three “how to think” questions repeat requirements later requested in the deliverables. |
| Dynamic Parameterization | 3/10 | The fixed pilot makes the brief immediately usable, but budget, dates, footfall, costs, and rush windows are not reusable input slots. |
| Signal-to-Noise Ratio | 8/10 | Information is ordered logically. The request for visible internal reasoning spends tokens on a form of explanation that should instead be a concise rationale. |

## Actionable Recommendations

1. Replace the instruction to show step-by-step internal reasoning with a short, user-facing rationale covering the chosen menu mix, the main risk, and the selected profit lever.
2. Define the operating assumptions: whether replenishment is allowed, what costs count as COGS versus operating expense, how waste and remaining inventory are treated, and the denominator for ROI.
3. Require a reconciliation check: budget categories sum to ₹10,000; percentages sum to 100%; production, available stock, sales, waste, and financial totals agree.
4. Set a data policy: use user-supplied figures when available; otherwise label every assumed price, cost, and demand value as illustrative and avoid presenting it as a local fact.
5. Add a fallback for missing or contradictory inputs: state the minimum assumptions needed, then give a clearly labeled provisional plan rather than implying certainty.
6. Consider named input slots for budget, pilot dates, rush windows, footfall, available equipment, and local costs if this prompt will be reused.

## Optimized Prompt Rewrite (Production-Ready)

```text
<role>
You are a canteen operations strategist and unit-economics analyst experienced with high-footfall college food service in India. Optimize for profitable, fast service and low spoilage, while keeping prices appropriate for students.
</role>

<scenario>
Plan a six-day pilot, Monday through Saturday, with an opening budget of ₹10,000. The two known rush windows are 10:00–11:00 AM and 1:00–2:00 PM. Assume no replenishment after the pilot begins unless the user explicitly says otherwise.
</scenario>

<data_policy>
Use user-provided data as facts. If footfall, prices, costs, equipment, or staffing are missing, state concise, clearly labeled illustrative assumptions; do not present estimates as verified local benchmarks. If an input is contradictory, identify the conflict and use the least-risk provisional assumption. Keep all calculations traceable to stated inputs.
</data_policy>

<analysis>
Do not provide private chain-of-thought or hidden step-by-step reasoning. Provide a concise “Strategic Rationale” of 3–5 lines summarizing the menu choice, the larger financial risk (undersupply or oversupply), and the main profit lever.
</analysis>

<deliverables>
Begin with an “Assumptions” box. Then use these exact headings:

### 1. Menu & Unit Economics
Choose 3–4 items and justify non-obvious choices briefly. Give a table with item, cost per unit (including a note on packaging treatment), selling price, gross margin in ₹ and %, and prep time per unit. State whether costs are estimates.

### 2. Capital Allocation (₹10,000)
Give a table with category, ₹ amount, percent of opening budget, and rationale. Use Raw Materials, Operational Buffer (gas/labour/misc), Packaging, and Emergency Reserve. Amounts must total ₹10,000 and percentages 100%. Explain the reserve based on the stated risk.

### 3. Daily Production Plan
For Monday–Saturday, show units per item for each rush window and off-peak. Identify deliberate test days and explain the demand signal being tested. Ensure planned production is supportable by the upfront stock and stated capacity.

### 4. Demand & Waste Management
Design one concrete rush-hour mechanism. Give a same-day unsold-stock decision rule with quantities or trigger, cutoff time, and food-safety limits. Do not recommend carrying food forward unless it is safe and compliant.

### 5. Financial Projection & ROI
Show weekly units sold, revenue, COGS for units sold, spoilage/write-off cost, operating expenses, net operating profit, and ROI with formulas. Define ROI as net operating profit divided by the ₹10,000 opening budget. Distinguish unspent reserve and usable ending inventory from expenses; do not count the same packaging or cost twice. Include a 20% below-assumption footfall sensitivity and state what changes in the calculation.
</deliverables>

<validation>
Before answering, check that the allocation sums to ₹10,000; percentages sum to 100%; daily production and sales do not exceed available stock; revenue, COGS, waste, and profit reconcile; and the sensitivity case uses the same assumptions except for footfall. If data is insufficient for a reliable figure, label the result provisional and say which input would change it most.
</validation>

<output_rules>
Use tables for comparisons. Keep the answer concise and decision-focused; avoid motivational filler. End with exactly two lines in this form:
If I only had one more rupee to spend, I'd spend it on [item or action],
because [specific expected operational or financial benefit].
</output_rules>
```