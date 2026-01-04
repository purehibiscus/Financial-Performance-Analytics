# Financial-Performance-Analytics
Budget vs Actuals | Time Intelligence | GAAP-Aligned Power BI Model

**Author:** Abbas Nadani

**Tools:** Power BI, DAX, Power Query (M), CSV

**Model Type:** Star Schema (Finance-grade)


**Executive Summary**

This project demonstrates the design and delivery of a production-ready financial analytics solution using Power BI.

It enables:

1. Budget vs Actual variance analysis
2. Time-intelligent financial reporting (MoM, YoY, Rolling)
3. GAAP-aligned P&L structure
4. Scenario extensibility (Actual / Budget / Forecast)
5. Executive-level storytelling and drill-down analysis

The solution is designed to scale across entities, time periods, and financial scenarios while maintaining semantic clarity and performance.

**Business Problem & Stakeholder Needs**

**Problems Identified**
1. Finance teams lacked a single source of truth
2. Budget vs Actual reconciliation was manual and slow
3. No standardized financial hierarchy
4. Limited visibility into variance drivers
5. Time comparisons required Excel-based workarounds

**Stakeholders**
1. CFO / Finance Director
2. FP&A
3. Operations Leaders
4. Executive Management

**Solution Overview**

**What I Built**

1. Star schema financial model
2. Separate Actuals and Budget fact tables
3. Shared conformed dimensions
4. Centralized DAX measure layer
5. Time intelligence via calculation groups
6. Executive dashboards (2 pages)

**Why This Works**

1. Prevents double counting
2. Supports financial semantics
3. Enables future forecasting
4. Matches enterprise BI best practices

**Data Sources & Tables**


Source	| Description
Fact_Actuals	| Monthly actual financial transactions
Fact_Budget	| Planned financial values
Dim_Date	| Time Intelligence
Dim_Account	| Chart of accounts with financial hierarchy
Account_Mapping	| P&L structure
Dim_Entity	| Business units / legal entites
Dim_Scenario	| Actual / Budget / Forecast
Dim_FX_Rates	| Currency handling
_Measures	| Centralized DAX layer

**Data Modeling (Star Schema)**

**Design Principles**
1. One-directional relationships
2. No fact-to-fact joins
3. No bi-directional filters
4. Measures isolated in _Measures
5. Financial logic handled in DAX, not visuals

**Model Diagram (Conceptual)**

yaml diagram


                 Dim_Date
                    |
                    |
Dim_Entity —— Fact_Actuals —— Dim_Account —— Account_Mapping
                    |
                    |
               Dim_Scenario

                 Dim_Date
                    |
                    |
Dim_Entity —— Fact_Budget  —— Dim_Account
                    |
                    |
               Dim_Scenario

          Dim_FX_Rates (used in measures)
          _Measures (no relationships)

The above layout ensures scalability, performance, and auditability.

**Financial Hierarchy Design**

**Embedded in Dim_Account**

Statement (P&L)
 └── Section (Revenue / Expense)
     └── Subsection (e.g. Subscription Revenue)
         └── Account

**Sort Order**

1. Controlled via SortOrder
2. Ensures correct financial statement layout
3. Prevents alphabetical misordering

**Measures & DAX Architecture**

**Measure Table Strategy**
1. _Measures table created manually
2. No relationships
3. All business logic centralized
4. Improves discoverability & governance

Core Measure

Actual Amount =
SUM ( Fact_Actuals[Amount] )

Budget Amount =
SUM ( Fact_Budget[BudgetAmount] )


Variance Measures
Variance =
[Actual Amount] - [Budget Amount]

Variance % =
DIVIDE ( [Variance], [Budget Amount] )


**Favorable / Unfavorable Logic**
Variance Status =
VAR AccountType = SELECTEDVALUE ( Dim_Account[AccountType] )
VAR Var = [Variance]
RETURN
SWITCH (
    TRUE(),
    AccountType = "Revenue" && Var > 0, "Favorable",
    AccountType = "Revenue" && Var < 0, "Unfavorable",
    AccountType = "Expense" && Var < 0, "Favorable",
    AccountType = "Expense" && Var > 0, "Unfavorable",
    "Neutral"
)


**Time Intelligence (Calculation Groups)**

**Implemented Metrics**
1. Month-over-Month (MoM)
2. Year-over-Year (YoY)
3. Rolling 3-Month
4. Rolling 12-Month
5. Prior Period comparisons


**Why Calculation Groups**

1. Reduces measure explosion
2. Keeps model clean
3. Enables reusable logic
4. Enterprise best practice

**Security & Governance**

**Row-Level Security**

1. Entity-based access
2. Easily extendable to email-based RLS
3. Applied to demonstrate data security awareness

**Dashboard Design**

**Dashboard Page 1 – Executive Financial Overview**

**Business Questions**

1. Are we ahead or behind budget?
2. Which areas drive variance?
3. Is performance improving over time?
4. Are expenses under control?

**Visuals**

1. KPI Cards:
  i.  Actual
  ii. Budget
  iii. Variance
  iv. Variance %

2. Line Chart:
  i. Actual vs Budget by Month

3. Waterfall Chart:
  i. Variance by Section

4. Financial Matrix:
  i. Rows: Statement → Section → Subsection
  ii. Columns: Actual | Budget | Variance | %

Stakeholders: CFO, Finance Leadership


**Dashboard Page 2 – Variance Drivers & Trends**

**Business Questions**

1. Which accounts are underperforming?
2. Which entities drive unfavorable variance?
3. Are trends improving MoM / YoY?
4. Where should management intervene?

**Visuals**

1. Bar Chart:
  i. Variance by Account

2. Column Chart:
  i. Variance by Entity

3. Line Chart:
  i. Rolling 12-Month Actuals

4. Heatmap / Conditional Matrix:
  i. Variance Status by Month

Stakeholders: FP&A, Operations Leaders


**Key Insights Enabled**

1. Rapid identification of cost overruns
2. Revenue shortfall detection
3. Time-based trend awareness
4. Actionable executive insights
5. Clean audit trail for finance

**Future Enhancements**

1. Forecast scenario
2. FX calculation group
3. Fabric pipelines (ADF / OneLake)
4. Commentary layer
5. Planning write-back


**Final Statement**

This project demonstrates end-to-end BI ownership, combining:

1. Technical depth
2. Financial literacy
3. Modeling discipline
4. Business storytelling
