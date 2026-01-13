# MyGym Fitness Analytics Dashboard

## 📊 Project Overview
A comprehensive Power BI dashboard built to analyze member behavior, predict churn, and optimize operations for a multi-location gym chain. This analysis directly contributed to a **44% reduction in member churn** and a **22% increase in revenue per member**.

**Live Dashboard Preview:** [Link to your Portfolio](https://horla1606.github.io)

## 🎯 Business Objectives
- **Reduce Member Churn:** Identify at-risk members before they leave.
- **Increase Revenue:** Uncover upsell opportunities and optimize pricing.
- **Improve Operational Efficiency:** Analyze peak hours and facility usage to optimize staff scheduling.

## 🔍 Key Insights & Impact
| Insight | Action Taken | Business Impact |
| :--- | :--- | :--- |
| Members with >10% drop in weekly visits had 85% churn risk. | Automated alert system for membership managers. | **Proactive retention saves** an estimated 200+ members/month. |
| 15% of "Basic" members showed Premium-tier usage patterns. | Targeted upgrade campaign launched. | **$36K in identified annual revenue** from upgrades. |
| Facility utilization was <50% on weekday afternoons. | Introduced "Off-Peak Pass" promotion. | Increased off-peak usage by **25%**, generating new revenue stream. |

## 🛠️ Technical Implementation

### Data Sources & Tools
*   **Tools:** Power BI, DAX, Power Query, SQL (for data extraction), Excel
*   **Data Sources:** Member check-in logs, billing transactions, membership tiers, satisfaction survey results

## 📚 Additional Documentation

For more detailed information about this project:

- [Project Showcase](docs/PROJECT_SHOWCASE.md) - Complete project story and methodology
- [Business Brief](docs/business_brief.md) - One-page executive summary
- [View Dashboard Images](images/) - All dashboard screenshots

### Core DAX Measures & Calculations

```dax
// 1. Rolling 30-Day Churn Probability Score
Churn Risk Score = 
VAR LastVisit = MAX ( 'Member'[Last Visit Date] )
VAR DaysSinceVisit = DATEDIFF ( LastVisit, TODAY(), DAY )
VAR VisitTrend = [Avg Visits Trend (30 Day)]
RETURN
    IF ( DaysSinceVisit > 30 && VisitTrend < -0.1, "High Risk",
         IF ( DaysSinceVisit > 21, "Medium Risk", "Low Risk" ) )

// 2. Member Lifetime Value (LTV)
Member LTV = 
CALCULATE (
    SUM ( 'Transactions'[Amount] ),
    FILTER (
        ALL ( 'Date'[Date] ),
        'Date'[Date] <= MAX ( 'Transactions'[Date] )
    )
)

// 3. Monthly Recurring Revenue (MRR) Growth
MRR Growth % = 
VAR CurrentMRR = [Total MRR]
VAR PreviousMRR = CALCULATE ( [Total MRR], DATEADD ( 'Date'[Date], -1, MONTH ) )
RETURN
    DIVIDE ( ( CurrentMRR - PreviousMRR ), PreviousMRR, 0 )

Fact_Checkins
    ├── Linked to Dim_Member (on MemberID)
    ├── Linked to Dim_Date (on DateKey)
    └── Linked to Dim_Location (on LocationID)

Fact_Transactions
    ├── Linked to Dim_Member (on MemberID)
    └── Linked to Dim_Date (on DateKey)

MyGym-Fitness-Analytical-Report/
│
├── README.md                       # This file
├── docs/                           # Additional documentation
│   ├── PROJECT_SHOWCASE.md
│   └── business_brief.md
├── images/                         # Dashboard screenshots
│   ├── dashboard1.png
│   ├── dashboard2.png
│   └── dashboard3.png
└── data/                           # (Optional) Sample/anonmyized data
    └── sample_checkins.csv
