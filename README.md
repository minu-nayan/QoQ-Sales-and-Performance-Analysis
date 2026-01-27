# QoQ Sales & Performance Analysis (Q1-Q2 2025)

## Table of Contents

1. [Project Overview](#project-overview)
2. [Business Context](#business-context)
3. [Data Model & Architecture](#Data-Model--Architecture)
4. [Key DAX Measures & Calculations](#key-dax-measures--calculations)
5. [Dashboard Pages & Analytical Focus](#Dashboard-Pages--Analytical-Focus)
6. [Key Analytical Capabilities](#Key-Analytical-Capabilities)
7. [Data Preparation & Transformations](#Data-Preparation--Transformations)
8. [Key Strategic Insights](#StrategicInsights)
9. [Conclusion](#Conclusion)

## Project Overview

This project delivers a Quarter-over-Quarter (QoQ) Sales and Performance Analysis for a B2B SaaS services organization, focusing on revenue performance across service verticals, AI/ML sub-verticals, and Business Development (BD) teams for Q1 and Q2 of 2025.

The dashboards are designed to support revenue leadership, sales operations, and business analytics teams by enabling granular performance comparisons, revenue attribution, and trend analysis across time, people, and service offerings.

The solution is built end-to-end in Power BI, with a strong emphasis on data modeling, DAX-driven metrics, and analytical storytelling, rather than static reporting.

## Business Context

Organization type: SaaS / Service-based B2B company
Revenue source: Projected contract value derived from B2B service contracts secured by the Business Development team.

#### Service verticals:
- Data Engineering
- Full Stack Development
- Artificial Intelligence (AI/ML)
- Others

#### AI/ML sub-verticals:
- Machine Learning
- AI Agents
- AI Workflow Automation

Each deal represents projected contract value won by a BD within a specific vertical and time period.

## Data Model & Architecture

### Core Tables

#### Sales (Fact Table)
- Date Won
- BD Name
- Client Name
- Geography (City, State, Country)
- Industry
- Project Vertical
- AI/ML Sub-Vertical
- Projected Contract Value

#### DateTable (Calendar Dimension)
- Date
- Month Name
- Month Number
- Quarter Name
- Year

Created using:

````
DateTable =
CALENDAR (
    MIN('Sales'[Date Won]),
    MAX('Sales'[Date Won])
)
````

Time Grain (Disconnected Parameter Table) Used to dynamically switch between Month-level and Quarter-level analysis.
````
Time Grain = {
    ("MonthName", NAMEOF('DateTable'[MonthName]), 0),
    ("QuarterName", NAMEOF('DateTable'[QuarterName]), 1)
}
````

Relationships

- Sales[Date Won] → DateTable[Date] (Many-to-One)
- Time Grain table is intentionally disconnected to enable dynamic axis control

## Key DAX Measures & Calculations

#### Revenue Measures
````
Total Revenue =
SUM('Sales'[Projected Contract Value])

Q1 Revenue =
CALCULATE(
    [Total Revenue],
    'DateTable'[QuarterName] = "Q1"
)

Q2 Revenue =
CALCULATE(
    [Total Revenue],
    'DateTable'[QuarterName] = "Q2"
)

Total Revenue Q1 Q2 =
[Q1 Revenue] + [Q2 Revenue]
````

#### QoQ Growth Calculation
````
QoQ Growth % =
DIVIDE(
    [Q2 Revenue] - [Q1 Revenue],
    [Q1 Revenue]
)
````
Displayed using conditional formatting for directional indicators (▲ / ▼).

#### Vertical & Sub-Vertical Attribution
Revenue is sliced using Project Vertical and AI/ML Sub-Vertical fields directly in visuals, avoiding hard-coded logic and preserving model flexibility.

This enables:
- Vertical-wise revenue contribution
- Sub-vertical performance inside AI/ML
- BD-level attribution across service lines

#### KPI Text Construction (Concatenated Measures)
To avoid redundant visuals and conserve layout space, concatenated text measures are used for KPI cards.
````
Vertical Revenue Summary =
"Full Stack Q1 & Q2 Revenue - " & FORMAT([Full Stack Revenue], "$#,##0") & UNICHAR(10) &
"Data Engineering Q1 & Q2 Revenue - " & FORMAT([Data Engineering Revenue], "$#,##0") & UNICHAR(10) &
"Others Q1 & Q2 Revenue - " & FORMAT([Others Revenue], "$#,##0") & UNICHAR(10) &
"AI/ML Q1 & Q2 Revenue - " & FORMAT([AI ML Revenue], "$#,##0")
````
This approach replaces legends and secondary tables with information-dense KPI cards.

## Dashboard Pages & Analytical Focus

1. Sales Analytics (Q1–Q2 2025)
- Total revenue, cost, and profit KPIs
- Vertical-wise revenue contribution
- Monthly revenue trends
- Industry-wise revenue distribution
- AI/ML sub-vertical breakdown
Purpose: Executive-level revenue visibility with drill-down capability

![Dashboard Preview](https://github.com/minu-nayan/QoQ-Sales-and-Performance-Analysis/blob/main/Sales%20Analytics%20SS%201.png)
  
2. Performance Analytics (Q1–Q2 2025)
- QoQ growth by BD
- Quarterly revenue comparison (Q1 vs Q2)
- Leads won per BD
- Revenue by BD across verticals and sub-verticals
Purpose: BD performance benchmarking and contribution analysis

![Dashboard Preview](https://github.com/minu-nayan/QoQ-Sales-and-Performance-Analysis/blob/main/Sales%20Performance%20Analytics%20SS%201.png)

## Key Analytical Capabilities

- Quarter-over-Quarter growth analysis
- BD-level performance attribution
- Vertical and sub-vertical revenue decomposition
- Dynamic time grain switching (Month ↔ Quarter)
- Legend-free, label-driven visual design
- High-density KPI storytelling using DAX

## Data Preparation & Transformations

- Calendar table generated using DAX (no auto date/time)
- Explicit quarter and month mapping
- Clean categorical separation between verticals and sub-verticals
- Standardized revenue formatting and display units
No assumptions are embedded directly in visuals; all business logic resides in measures.

## Strategic Insights

- Sales Momentum Strengthened in Q2 (+66% QoQ):
  Qoq growth indicates improved conversion efficiency, stronger deal closures, or enhanced market positioning entering Q2.

- Full Stack & Data Engineering Drive Majority of Revenue:
  These two verticals collectively form the largest revenue contribution and remain the dominant core offerings with the highest close value per deal.

- AI/ML Sub-Verticals Show High Upside Potential:
  Machine Learning represents the strongest sub-vertical, followed by AI Agents and Workflow Automation - suggesting a growing tech trend that could be scaled further in BD outreach.

- BD Performance is Uneven but Q2 Lift Visible Across All Reps:
  Each BD improved QoQ revenue performance, with BD-A delivering the highest revenue and BD-B/B-C showing competitive growth, reducing performance disparity across the team.

- Industry Spread Shows Deep Penetration in PropTech & EdTech:
  Top two industries indicate where buyer demand is currently highest; additional vertical diversification is visible but smaller in scale.

- Deal Velocity Improved Monthly (Q2 Slope Stronger than Q1):
  Monthly close rate spiked notably in April, signaling the presence of seasonality or successful campaigns in targeted periods.

- Cost Structure Light Relative to Revenue:
  With Cost = $3K vs. $250K total revenue, margins are extremely favorable, reinforcing the scalability of service-based SaaS sales operations.

- Sales Strategy Implication:
  Double-down on ML/AI & Full Stack verticals, replicate Q2 BD execution patterns, and expand industry penetration beyond PropTech/EdTech using insights from high-performing quarters.

The dashboards are optimized for decision support, not just reporting-enabling leadership to quickly identify growth drivers, performance gaps, and revenue concentration across people, services, and time.

## Conclusion

This project demonstrates a business-first analytics approach powered by:
- Robust dimensional modeling
- Explicit DAX-driven metric design
- Thoughtful visual composition
- Strong alignment with SaaS B2B revenue workflows

Built with: Power BI • DAX • Dimensional Modeling • Business Analytics

## 🔗 Connect With Me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/minu-nayan/)
[![Email](https://img.shields.io/badge/Email-Contact-red?logo=gmail)](mailto:minunayan08@gmail.com)
