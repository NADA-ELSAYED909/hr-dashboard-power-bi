# HR Dashboard – Power BI Project

## 📌 Overview
This project was completed as part of the **Digital Egypt Pioneers Initiative (DEPI)**. I was given a raw HR dataset (split across three Excel sheets) and tasked with cleaning, modeling, and visualizing it in Power BI to answer key workforce questions — headcount, turnover, compensation, and data availability across departments.

## 🛠️ Tools Used
- **Power Query** – data cleaning and transformation
- **Power BI** – data modeling (relationships) and dashboard visualization

## 🗂️ Data Source & Structure
The raw data arrived as three separate sheets, which were kept as three related tables rather than merged into one flat table:

| Table | Key Fields | Role |
|---|---|---|
| `Employees` | ID, City, Salary, Performance Review, Overdue Vacation?, Last Promotion Date | Employee-level attributes |
| `Sheet1` | ID, Department, Employment Status, Gender, Hire Date, Education | Core employee record |
| `Department Bridge` | Department, Manager | Department → Manager mapping |

**Relationships:** `Employees` ↔ `Sheet1` on `ID`, and `Sheet1` ↔ `Department Bridge` on `Department`.

The `Department Bridge` table was kept separate instead of repeating the manager's name on every employee row. This avoids data redundancy (normalization) — if a manager changes, it's updated in one place instead of across every row belonging to that department.

## 🧹 Data Cleaning & Transformation (Power Query)
- **Changed Data Types** — corrected columns (dates, whole numbers, text) so calculations and relationships work correctly.
- **Trimmed Text** and **Capitalized Each Word** — applied to text columns (Employee, Department, Education, Position, etc.) to remove inconsistent spacing and casing from manual data entry. Without this, values like `"Sales"` and `"Sales "` would be treated as two different categories, silently breaking relationships and undercounting groups in the dashboard.
- **Nulls handled by context, not blanket-removed:**
  - `Last Promotion Date` nulls were **kept** — a blank here reflects a real event (a promotion) that simply hasn't happened yet, not a data problem.
  - `Availability = Unknown` (~20% of records) was **flagged, not ignored** — this reflects a genuine data collection gap rather than a real employee status (see Key Insights).

## 📊 Dashboard Features
- Filters: Department, Employment Status, Education
- KPIs: Total, Active, and Terminated Employees
- Headcount by Department | Terminations by Department
- Average Salary by Department
- Overdue Vacation Tracker
- Employee Availability Status
- Employee Details Breakdown table

## 💡 Key Insights
**1. Raw counts are misleading for turnover — rates tell the real story.**
Development has the highest number of terminations (11), which looks like the biggest problem at first glance. But Development is also the largest department (47 employees). Calculating the termination *rate* (terminations ÷ headcount) changes the picture: Sales (50%) and Strategy (44%) have proportionally higher turnover than Development (23%). Legal shows 100%, but with only 1 employee total, that figure isn't statistically reliable and shouldn't be treated as a pattern.

**2. Averages can hide small sample sizes.**
Marketing has the highest average salary (4,314), but it's based on only 4 employees — compared to 47 in Development. A small sample like this is more easily skewed by one or two individual salaries, so this figure should be read as a directional signal, not a reliable department-wide benchmark, without checking the underlying salary distribution.

**3. The "Unknown" availability status is a data quality issue, not an employee issue.**
~20% of employee records have an Unknown availability status — this reflects missing data at the recording stage, not an actual employee state. Breaking it down by department shows it isn't evenly spread: Development accounts for 12 of the 18 Unknown records (25.5% of its own headcount) — the highest rate of any department, even after adjusting for its larger size. This points to a specific data collection gap in how Development's records are maintained, worth investigating directly with HR.

## 🎓 What I'd Improve Next
- Investigate *why* Development has both the highest Unknown-data rate and largest headcount — is it a process gap specific to that department?
- Break down the salary averages by individual distribution (not just the mean) to confirm the Marketing figure isn't driven by one outlier.
- Add a calculated Termination Rate measure directly into the dashboard instead of only raw counts, so the more accurate insight is visible at a glance.
