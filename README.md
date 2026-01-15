# AfriTech Electronics Ltd. — Reputation Monitoring & SQL Data Cleaning/EDA Project

## Company Overview
**Company Name:** AfriTech Electronics Limited (Los Angeles, U.S.A.)  
**Founded:** 2017  
**Industry:** Consumer Electronics (Smartphones, Tablets, Wearables)  
**Employees:** 200  

AfriTech Electronics Ltd. is a leading player in the global consumer electronics industry, widely recognized for innovation in smartphones, tablets, and wearable technology. Headquartered in the United States, the company blends cutting-edge technology with superior quality, pushing boundaries in product excellence and user experience.

---

## Growth Journey & Milestones
- **2017:** Launched its first flagship smartphone series, setting new standards in design and performance.
- **2019:** Expanded into wearable technology.
- **2021:** Introduced AI-driven features across product categories.
- **2022:** Achieved **$2 million** in annual revenue.
- **2025:** Recognized among top innovators in smart device integration.

---

## Market Presence & Customer Base
AfriTech maintains a strong foothold in the U.S. market, with growing expansion across **Europe and Africa**. Customers include tech enthusiasts, professionals, and consumers who prioritize innovation, performance, and reliability.

---

## Problem Statement
AfriTech Electronics Ltd. has faced growing challenges affecting its brand reputation, including:
- Negative customer reviews
- Product recalls
- Public relations crises
- Declining customer trust and market share

---

## Key Challenges
1. **Negative Social Media Buzz:** Increased negative sentiment around products and customer service.  
2. **Customer Complaints:** Product defects, support delays, billing issues.  
3. **Product Recalls:** High media attention and stakeholder concern.  
4. **Competitive Pressure:** Rivals gaining market share using AfriTech’s reputation struggles.

---

## Why This Project Matters
Brand reputation is a critical asset in consumer electronics. Social media heavily influences customer decisions. Understanding evolving sentiment and engagement enables:
- Proactive reputation management
- Faster customer issue resolution
- Better crisis communication
- Stronger customer trust and loyalty

---

## Project Objectives
This project supports AfriTech’s decision-making by enabling:
- **Monitoring social media conversations** about the brand and products
- **Sentiment analysis** to spot positive/negative trends
- **Prioritizing customer issues** for faster resolution
- **Early crisis detection** through warning signals

---

## Tools & Skills Used
- SQL (PostgreSQL-style syntax)
- Data Cleaning Techniques
- Duplicate Handling Strategies
- Exploratory Data Analysis (EDA)
- Window Functions (ROW_NUMBER, DENSE_RANK, LEAD)
- Data Backup Best Practices

---

## Repository Contents
- `docs/` → project background + data dictionary
- `sql/` → full SQL script including:
  - null checks
  - name cleaning
  - duplicate handling
  - adding patient/visit identifiers
  - EDA queries and insights

---

## How to Use
1. Load your dataset into a SQL database (PostgreSQL recommended).
2. Run the script in `sql/healthcare_sql_analysis.sql`.
3. Review EDA outputs and adapt insights for reporting.

---

## Author
**Damilola Lasode**  

---

# Project Background — AfriTech Electronics Ltd.

## About AfriTech Electronics Ltd.
AfriTech Electronics Limited is a U.S.-based consumer electronics company founded in 2017 and headquartered in Los Angeles. The company produces premium smartphones, tablets, and wearable devices, with strong innovation across product lines.

## Business Challenge
In recent years, AfriTech has faced brand reputation issues due to:
- rising customer complaints
- negative online reviews
- product recalls
- PR crises

This has contributed to decreased customer trust, reduced sales, and intensified competition.

## Practical Implications
- **Proactive Reputation Monitoring:** Detect PR risks early before escalation.
- **Customer Engagement:** Improve trust by responding quickly and transparently.
- **Crisis Communication:** Create structured communication workflows for recalls and controversies.

## Real-World Alignment
Companies like Samsung and Apple monitor sentiment and feedback actively to protect brand trust and drive long-term loyalty.

---

# Data Dictionary (HealthcareTb)

| Column Name          | Description |
|---------------------|-------------|
| Name                | Patient name |
| Age                 | Patient age |
| Gender              | Patient gender |
| BloodType           | Patient blood type |
| MedicalCondition    | Diagnosis/condition |
| DateofAdmission     | Admission date |
| DischargeDate       | Discharge date |
| Doctor              | Assigned doctor |
| Hospital            | Hospital name |
| InsuranceProvider   | Insurance provider |
| BillingAmount       | Total billing cost |
| RoomNumber          | Room number assigned |
| AdmissionType       | Emergency/Urgent/Elective |
| Medication          | Medication prescribed |
| TestResults         | Test outcome |
| Patient_id          | Generated patient identifier |
| VisitId             | Generated visit identifier |
