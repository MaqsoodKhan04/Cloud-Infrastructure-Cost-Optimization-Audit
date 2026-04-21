# Cloud-Infrastructure-Cost-Optimization-Audit

PROJECT REPORT: Cloud Infrastructure FinOps & Cost Optimization Audit
Role: Lead Data Analyst

Tools Used: Python (Pandas), SQL (CTEs, Window Functions), Power BI, FinOps Principles

1. Executive Summary
The objective of this audit was to identify inefficiencies within our cloud infrastructure spending. By analyzing 90 days of raw billing logs, I identified that 5.2% of the total monthly spend was allocated to "Zombie Resources"—assets that were costing the company money but showing 0% utilization. Implementing the recommendations in this report would result in an estimated annual saving of $14,000+ without impacting system performance.

2. The Business Problem
In fast-growing tech environments, cloud costs often spiral because:

Lack of Accountability: Resources are created by engineering teams without proper "Department" or "Environment" tags.

Idle Assets: Developers spin up "Dev" or "Staging" environments for testing but forget to decommission them.

The "Black Box" Effect: Leadership knows the total bill but cannot see which specific projects are driving the costs.

3. Data Pipeline & Cleaning (The Technical Process)
I built a multi-stage ETL pipeline to move from raw, messy logs to actionable insights.

Phase A: Data Standardization (SQL & Python)
Missing Metadata: I discovered that roughly 10% of entries lacked department tags. I used a COALESCE logic in SQL to reclassify these as "Unallocated" to ensure every dollar was accounted for in the final report.

Date Normalization: Converted various string formats into a standard YYYY-MM-DD datetime object to allow for accurate "Daily Burn Rate" tracking.

Phase B: Feature Engineering
I created two custom metrics to better identify waste:

Waste Flag (Is_Waste): A boolean trigger for any resource where UsageAmount == 0 but Cost > 0.

Unit Cost: Calculated the efficiency of spend per service to identify which cloud products were "over-provisioned."

4. Key Insights & Findings
Finding 1: The "Zombie" Problem
Insight: I identified 52 unique Resource IDs categorized as "Zombie" assets.

Impact: These assets were primarily in the Database (RDS) category, suggesting that old test databases were left running indefinitely.

Recommendation: Immediate decommissioning of these 52 IDs.

Finding 2: The "Dev vs. Prod" Imbalance
Insight: Spending in the "Development" environment was nearly 85% of the cost of "Production."

Impact: Typically, Dev should be significantly cheaper. This indicated that Dev instances were likely over-specced (too powerful) for their requirements.

Finding 3: Departmental Accountability
Insight: The Engineering department accounted for 40% of the total spend but also 60% of the identified waste.

Impact: This highlights a need for a "Tagging Policy" specifically for the engineering team.

5. Visual Dashboard (Summary)
The final dashboard was designed for the CTO/CFO and included:

The Burn Rate Line Chart: A 90-day view showing a cost spike on Feb 14th, which I traced back to an unoptimized data migration.

Waste Heatmap: A breakdown of "Unused vs. Used" funds by department.

Slicers: Ability to filter by Environment (Prod/Dev/Staging) for granular auditing.

6. Final Recommendations
Automated Cleanup: Implement a script to auto-stop "Dev" instances outside of working hours (9 AM – 6 PM).

Right-Sizing: Review the top 10 most expensive EC2 instances; the data suggests we could downgrade these by one "tier" without affecting performance.

Governance: Enforce mandatory "Department" tagging at the point of resource creation.