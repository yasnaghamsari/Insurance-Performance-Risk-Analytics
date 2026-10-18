# 🏥 Insurance Portfolio Analytics — Power BI Dashboard

A professional 4-page Power BI dashboard built on a synthetic Indian insurance dataset, covering executive KPIs, claims analysis, agent performance, and risk & retention insights.

---

## 📊 Dashboard Pages

### Page 1 — Executive Overview
- Total Premium: ₹566.94M
- Total Claims Amount: ₹603.46M
- Loss Ratio: 106.4% ⚠️ (claims exceed premiums — underwriting loss signal)
- Active Policies: 6,018
- Premium Trend line chart (Jan 2022 – Apr 2025)
- Premium breakdown by Policy Type and Region

### Page 2 — Claims Analysis
- Total Claims Count: 4,000
- Claim Approval Rate: 54.80%
- Avg Settlement Days: 25.2
- Claims by Status (Approved / Pending / Rejected) — Donut chart
- Claims by Policy Type with status breakdown — Stacked Bar chart
- Average Claim Amount: ₹1,50,865.74

### Page 3 — Agent Performance
- Top 10 Agents by Total Premium
- Agent Loss Ratio ranking — identifies high-risk agents
- Conditional formatted table: Green (low risk) → Red (high risk)
- Shlok Banik — Best performer at 64.4% Loss Ratio
- Ishaan Mahajan — Highest risk at 129.9% Loss Ratio

### Page 4 — Risk & Retention
- Policy Lapse Rate: 14.6% (1,463 lapsed policies)
- Premium vs Claims by Region — North highest premium at ₹181M
- Loss Ratio Heatmap by Policy Type × Region
  - Highest risk: Health in West at 134.8%
  - Only profitable segment: Life in West at 87.5%
- Premium vs Claims Trend (2022–2025)

---

## 🗂️ Data Model

**Star Schema** with 6 tables:

### Dimension Tables
| Table | Key Columns |
|---|---|
| Dim_Customer | CustomerID, Name, Gender, Age, City, State, Segment |
| Dim_Agent | AgentID, AgentName, Channel, Region |
| Dim_Policy | PolicyID, PolicyType, CoverageType, PolicyTerm, SumAssured |
| DateTable | Date, Month, MonthName, Quarter, WeekDay, Year |

### Fact Tables
| Table | Key Columns |
|---|---|
| Fact_Policies | PolicyNumber, PolicyID, CustomerID, AgentID, StartDate, EndDate, Status |
| Fact_Premiums | PolicyNumber, Date, PremiumAmount, PaymentMode |
| Fact_Claims | ClaimID, PolicyNumber, ClaimDate, ClaimAmount, ApprovedAmount, ClaimStatus, SettlementDays |

### Relationships
- Dim_Customer → Fact_Policies (1:N via CustomerID)
- Dim_Agent → Fact_Policies (1:N via AgentID)
- Dim_Policy → Fact_Policies (1:N via PolicyID)
- Fact_Policies → Fact_Premiums (1:N via PolicyNumber)
- Fact_Policies → Fact_Claims (1:N via PolicyNumber)
- DateTable → Fact_Premiums (1:N via Date)
- DateTable → Fact_Claims (1:N via ClaimDate)

---

## 📐 DAX Measures

```dax
-- Policy Counts
Total Policies = COUNTROWS(Fact_Policies)
Active Policies = CALCULATE(COUNTROWS(Fact_Policies), Fact_Policies[Status] = "Active")
Lapsed Policies = CALCULATE(COUNTROWS(Fact_Policies), Fact_Policies[Status] = "Lapsed")

-- Premium
Total Premium = SUM(Fact_Premiums[PremiumAmount])

-- Claims
Total Claims Amount = SUM(Fact_Claims[ClaimAmount])
Total Claims Count = COUNTROWS(Fact_Claims)
Approved Claims Count = CALCULATE(COUNTROWS(Fact_Claims), Fact_Claims[ClaimStatus] = "Approved")
Claim Approval Rate % = DIVIDE([Approved Claims Count], [Total Claims Count])
Average Claim Amount = AVERAGE(Fact_Claims[ClaimAmount])
Avg Settlement Days = CALCULATE(AVERAGE(Fact_Claims[SettlementDays]), Fact_Claims[ClaimStatus] = "Approved")

-- Risk
Loss Ratio % = DIVIDE([Total Claims Amount], [Total Premium])
Policy Lapse Rate % = DIVIDE([Lapsed Policies], [Total Policies])

-- Agent
Claims per Agent = DIVIDE([Total Claims Count], DISTINCTCOUNT(Fact_Policies[AgentID]))
```

---

## 🔍 Key Business Insights

1. **Loss Ratio of 106.4%** — The portfolio is operating at an underwriting loss. Claims (₹603M) exceed premiums collected (₹567M), signaling the need for premium repricing or stricter underwriting controls.

2. **Only 54.8% claim approval rate** — Nearly half of all claims are rejected or pending, indicating potential process inefficiencies or fraudulent claim patterns worth investigating.

3. **14.6% policy lapse rate** — 1,463 policies have lapsed, representing significant lost revenue. Retention strategies should target high-lapse segments.

4. **West region — highest risk** — Health insurance in the West region carries a 134.8% Loss Ratio, making it the most unprofitable segment in the portfolio.

5. **Life insurance in West — only profitable segment** — At 87.5% Loss Ratio, Life insurance in the West is the only segment operating below 100%, suggesting it is cross-subsidizing other lines.

6. **Agent performance gap** — Top agent (Shlok Banik) operates at 64.4% Loss Ratio vs worst agent at 229%, indicating major underwriting quality differences across the sales force.

7. **Premium peaked in April 2024** — The trend analysis reveals premiums peaked at ₹21M/month in mid-2024 before declining — a signal for management to investigate churn or market saturation.

---

## 🛠️ Tools & Technologies

| Tool | Usage |
|---|---|
| Microsoft Power BI Desktop | Dashboard development |
| DAX | Calculated measures and KPIs |
| Power Query | Data transformation and cleaning |
| Microsoft Excel (.xlsx) | Source data |
| Star Schema | Data modeling approach |

---

## 🎨 Design Choices

- **Color theme**: Dark navy `#1E2D4E` headers with white canvas
- **Risk signals**: Red color on Loss Ratio KPI to flag underwriting concern
- **Conditional formatting**: Green → Red scale on agent Loss Ratio table
- **Consistent layout**: Same header style and KPI card position across all 4 pages
- **Indian Rupee (₹)** formatting throughout for regional accuracy

---

## 📁 Project Structure

```
Insurance-PowerBI-Dashboard/
│
├── Insurance_PowerBI_Portfolio_Data.xlsx    # Raw dataset (6 sheets)
├── Insurance_Dashboard.pbix                # Power BI file
├── README.md                               # Project documentation
└── Screenshots/
    ├── Page1_Executive_Overview.png
    ├── Page2_Claims_Analysis.png
    ├── Page3_Agent_Performance.png
    └── Page4_Risk_Retention.png
```

---

## 👤 Author

**Akhil Akkineni**  
Aspiring Data Analyst | Power BI | SQL | Python  
[LinkedIn](https://www.linkedin.com/in/) | [GitHub](https://github.com/)

---

*This project was built as a portfolio piece to demonstrate data modeling, DAX, and business intelligence skills in the insurance domain.*
