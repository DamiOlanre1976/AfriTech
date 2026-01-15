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


### 1. Preview Dataset
```sql
select * from HealthcareTb;
```
```sql
select count(*) from HealthcareTb;
```

### 2. Check for Null Values
```sql
select * from HealthcareTb
where 
    Name is null
    or Age is null
    or Gender is null
    or BloodType is null	
    or MedicalCondition is null
    or DateofAdmission is null
    or Doctor is null
    or Hospital is null 
    or InsuranceProvider is null
    or BillingAmount is null
    or RoomNumber is null
    or AdmissionType is null
    or DischargeDate is null
    or Medication is null
    or TestResults is null;
```

### 3. Backup Table Before Cleaning
```sql
create table HealthcareTbBackupBeforeNameClean as
    select * from HealthcareTb;
```
```sql
select * from HealthcareTbBackupBeforeNameClean;
```

### 4. Clean the Name Column
```sql
update HealthcareTb
set name = initcap(trim(name));
```
```sql
select name from HealthcareTb;
```

### 5. Check for Duplicates
```sql
select 
    Name, Age, Gender, BloodType, MedicalCondition, DateofAdmission, Doctor, Hospital, InsuranceProvider, 
    BillingAmount, RoomNumber, AdmissionType, DischargeDate, Medication, TestResults,
    count(*)
from HealthcareTb
group by 
    Name, Age, Gender, BloodType, MedicalCondition, DateofAdmission, Doctor, Hospital, InsuranceProvider, 
    BillingAmount, RoomNumber, AdmissionType, DischargeDate, Medication, TestResults
having count(*) > 1;
```

### 6. Test One Duplicate Row (Confirmation)
```sql
with CteTestDuplicate as(
    select
        *,
        row_number() 
            over(partition by 
                Name, Age, Gender, BloodType, MedicalCondition, DateofAdmission, Doctor, Hospital, InsuranceProvider, 
                BillingAmount, RoomNumber, AdmissionType, DischargeDate, Medication, TestResults
            order by ctid)
    from HealthcareTb
    where Name = 'Abigail Young'
)
select * from CteTestDuplicate;
```

### 7. Backup Before Removing Duplicates
```sql
create table HealthcareTbBackupBeforeRemoveDuplicates as 
    select * from HealthcareTb;
```

### 8. Remove Exact Duplicates
```sql
with CteRemoveDuplicates as (
    select 
        ctid,
        row_number()
            over(partition by
                Name, Age, Gender, BloodType, MedicalCondition, DateofAdmission, Doctor, Hospital, InsuranceProvider, 
                BillingAmount, RoomNumber, AdmissionType, DischargeDate, Medication, TestResults
                order by ctid
            ) as row_num
    from HealthcareTb
)
delete from HealthcareTb 
where ctid in 
(
    select ctid from CteRemoveDuplicates where row_num > 1
);
```

### 9. Check Again After Removing Duplicates
```sql
select * from HealthcareTb where name = 'Abigail Young';
```

### 10. Duplicates Where Only Age is Inconsistent
```sql
select
    Name, Gender, BloodType, MedicalCondition, DateofAdmission, Doctor, Hospital, InsuranceProvider, 
    BillingAmount, RoomNumber, AdmissionType, DischargeDate, Medication, TestResults,
    count(distinct Age)
from HealthcareTb
group by 
    Name, Gender, BloodType, MedicalCondition, DateofAdmission, Doctor, Hospital, InsuranceProvider, 
    BillingAmount, RoomNumber, AdmissionType, DischargeDate, Medication, TestResults
having count(distinct Age) > 1;
```
```sql
select * from HealthcareTb where name = 'Aaron Archer';
```

### 11. Backup Before Removing Age-Inconsistent Duplicates
```sql
create table HealthcareTbBackUpBeforeAge as
    select * from HealthcareTb;
```
```sql
select * from HealthcareTbBackUpBeforeAge;
```

### 12. Remove Age-Inconsistent Duplicates
```sql
delete from HealthcareTb h2
using HealthcareTb h1
where h1.Age < h2.Age
and h1.Name = h2.Name
and h1.Gender = h2.Gender
and h1.BloodType = h2.BloodType
and h1.MedicalCondition = h2.MedicalCondition
and h1.DateofAdmission = h2.DateofAdmission
and h1.Doctor = h2.Doctor 
and h1.Hospital = h2.Hospital 
and h1.InsuranceProvider = h2.InsuranceProvider 
and h1.BillingAmount = h2.BillingAmount
and h1.RoomNumber = h2.RoomNumber
and h1.AdmissionType = h2.AdmissionType
and h2.DischargeDate = h2.DischargeDate
and h1.Medication = h2.Medication
and h1.TestResults = h2.TestResults;
```

### 13. Backup Before Adding Patient ID Column
```sql
create table HealthcareTbBeforePatientIDAdd as
    select * from HealthcareTb;
```

### 14. Add Patient_id Column
```sql
alter table HealthcareTb
add column if not exists Patient_id int;
```

### 15. Assign Patient_id
```sql
with AssignRowNumber as(
    select
        Name, 
        Age,
        Gender,
        BloodType,
        row_number() over(order by Name, Age, Gender, BloodType) as row_num
    from HealthcareTb
),
GroupPatients as (
    select
        c1.Name,
        c1.Age,
        c1.Gender,
        c1.BloodType,
        min(c2.row_num) as group_id
    from AssignRowNumber c1
    left join AssignRowNumber c2
        on c1.Name = c2.Name
        and c1.Gender = c2.Gender
        and c1.BloodType = c2.BloodType
        and c2.Age between c1.Age - 6 and c1.Age + 6
    group by c1.Name, c1.Age, c1.Gender, c1.BloodType
),
finalAssignPatientID as(
    select
        Name,
        Age, 
        Gender,
        BloodType,
        dense_rank() over(order by group_id) as Patient_id
    from GroupPatients
)
update HealthcareTb h 
set Patient_id = f.Patient_id
from finalAssignPatientID f
where h.Name = f.Name
and   h.Age = f.Age
and   h.Gender = f.Gender
and   h.BloodType = f.BloodType;
```

### 16. Confirm Patient ID Assignment
```sql
select 
    Patient_id, 
    count(*) 
from HealthcareTb
group by 1
having count(*) > 1;
```

### 17. Sample Patients Within ±6 Years Rule
```sql
select * from HealthcareTb where Patient_id = 34354;
```
```sql
select * from HealthcareTb where Patient_id = 1595;
```

### 18. Visit ID Creation
### Backup Before Adding Visit ID
```sql
create table HealthcareTbBackUpBeforeVisitID as
    select * from HealthcareTb;
```
```sql
select * from HealthcareTbBackUpBeforeVisitID;
```

### 19. Add VisitId Column
```sql
alter table HealthcareTb
add column if not exists VisitId int;
```
```sql
select * from HealthcareTb;
```

### 20. Assign VisitId
```sql
with AddVisitID as(
    select 
        Name, Age, Gender, BloodType, MedicalCondition, DateofAdmission, Doctor, 
        Hospital, InsuranceProvider, BillingAmount, RoomNumber, AdmissionType, 
        DischargeDate, Medication, TestResults, Patient_id,
        row_number() over(order by DateofAdmission) as VisitId
    from HealthcareTb
)
update HealthcareTb h
set VisitId = a.VisitId
from AddVisitID a
where h.Name = a.Name
and   h.Age = a.Age
and   h.Gender = a.Gender
and   h.BloodType = a.BloodType
and   h.MedicalCondition = a.MedicalCondition
and   h.DateofAdmission = a.DateofAdmission
and   h.Doctor = a.Doctor
and   h.Hospital = a.Hospital
and   h.InsuranceProvider = a.InsuranceProvider
and   h.BillingAmount = a.BillingAmount
and   h.RoomNumber = a.RoomNumber
and   h.AdmissionType = a.AdmissionType
and   h.DischargeDate = a.DischargeDate
and   h.Medication = a.Medication
and   h.TestResults = a.TestResults
and   h.Patient_id = a.Patient_id;
```

### Exploratory Data Analysis (EDA)

### 22. Which hospital has the most visits?
```sql
select 
    Hospital,
    count(VisitId) as number_of_visits
from HealthcareTb
group by 1
order by count(VisitId) desc;
```

### 23. Which patients visited the most?
```sql
select
    Patient_id,
    Name,
    count(*) as visit_count
from HealthcareTb
group by 1, 2
order by count(*) desc;
```

### 24. What medical conditions are treated?
```sql
select distinct MedicalCondition
from HealthcareTb;
```

### 25. Highest single medical bill per patient
```sql
with cte as 
(
    select
        Patient_id,
        Name,
        BillingAmount,
        row_number() over(partition by Name order by BillingAmount desc) as ranking
    from HealthcareTb
)
select
    Patient_id,
    Name,
    BillingAmount,
    ranking
from cte
where ranking = 1;
```

### 26. Smallest total bill amount (by patient)
```sql
select
    Patient_id,
    Name,
    sum(BillingAmount) as Total_Bill_Amount
from HealthcareTb
group by 1,2 
order by sum(BillingAmount);
```

### 27. Average medical bill
```sql
select avg(BillingAmount) from HealthcareTb;
```

### 28. Doctors working across multiple hospitals
```sql
select
    Doctor,
    count(distinct Hospital) as no_of_hospitals
from HealthcareTb
group by 1
having count(distinct Hospital) > 1
order by count(distinct Hospital) desc;
```

### 29. Doctors with the most patient visits
```sql
select
    Doctor,
    count(Patient_id) as no_of_patient_visits
from HealthcareTb
group by 1
order by 2 desc;
```

### 30. Number of times each condition is treated
```sql
select
    MedicalCondition,
    count(*) as no_of_times
from HealthcareTb
group by 1;
```

### 31. Visits by year (2019–2024)
```sql
select
    extract(year from DateofAdmission) as year,
    count(*) as no_of_visits
from HealthcareTb
group by 1
order by 2 desc;
```

### 32. Most prescribed medication for asthma
```sql
select
    Medication,
    count(*)
from HealthcareTb
where MedicalCondition = 'Asthma'
group by 1
order by 2 desc
limit 1;
```

### 33. Patient distribution by gender
```sql
select 
    Gender,
    count(Patient_id)
from HealthcareTb
group by 1;
```

### 34. Distribution of admission types
```sql
select 
    AdmissionType,
    count(*)
from HealthcareTb
group by 1;
```

### 35. Emergency visits by medical condition
```sql
select
    MedicalCondition,
    count(*) as no_of_emergency_visits
from HealthcareTb
where AdmissionType = 'Emergency'
group by 1;
```

### 36. Most common blood types
```sql
select
    BloodType,
    count(*) 
from HealthcareTb
group by 1
order by 2 desc;
```

### 37. Most common insurance providers
```sql
select 
    InsuranceProvider,
    count(Patient_id)
from HealthcareTb
group by 1
order by 2 desc;
```

### 38. Average billing amount by medical condition 
```sql
select
    MedicalCondition,
    round(avg(BillingAmount), 2) as Avg_Billing_Amount
from HealthcareTb
group by 1;
```

### 39. Hospitals with the highest total billing amount
```sql
select
    Hospital,
    sum(BillingAmount) as Total_Billing_Amount
from HealthcareTb
group by 1
order by 2 desc;
```

### 40. Average length of stay by medical condition
```sql
select
    MedicalCondition,
    round(avg(DischargeDate - DateofAdmission), 0) as average_length_of_stay
from HealthcareTb
group by 1;
```

### 41. Monthly trend of visits over time
```sql
select
    date_trunc('Month', DateofAdmission)::date, 
    count(VisitId)
from HealthcareTb
group by 1
order by 1;
```

### 42. Patients with the highest total billing amount
```sql
select
    Patient_id,
    Name,
    sum(BillingAmount) as Total_Billing_Amount
from HealthcareTb
group by 1, 2
order by 3 desc;
```

### 43. Doctors treating the most diverse conditions
```sql
select 
    Doctor,
    count(distinct MedicalCondition) as medical_conditions
from HealthcareTb
group by 1
having count(distinct MedicalCondition) > 1
order by 2 desc;
```

### 44. Patients admitted for multiple conditions
```sql
select
    Patient_id,
    Name,
    count(distinct MedicalCondition)
from HealthcareTb
group by 1, 2
having count(distinct MedicalCondition) > 1
order by 3 desc;
```

### 45. Patients readmitted within 30 days
```sql
with cte as 
(
    select 
        Patient_id,
        Name,
        DateofAdmission,
        lead(DateofAdmission) over(partition by Name order by DateofAdmission) as Next_Admission,
        (lead(DateofAdmission) over(partition by Name order by DateofAdmission) - DateofAdmission) as no_of_days
    from HealthcareTb
)
select
    Patient_id,
    Name,
    no_of_days
from cte
where no_of_days >= 30;
```

### 46. Conditions treated for patients with 3 different conditions
```sql
select distinct MedicalCondition 
from HealthcareTb
where Patient_id in
(
    select
        Patient_id
    from HealthcareTb
    group by 1
    having count(distinct MedicalCondition) = 3
);
```



