


# **🏥 Hospital Patient Management Analysis**  
![SQL](https://img.shields.io/badge/SQL-Data_Analysis-blue) ![Database](https://img.shields.io/badge/Database-PostgreSQL-orange) ![EDA](https://img.shields.io/badge/Analysis-Exploratory_EDA-green)

## **📌 Problem Statement**  
A hospital aims to implement an **efficient Patient Management System** to:  
✔ Streamline **patient record-keeping** and **doctor appointments**  
✔ Track **treatment history** and **hospital performance metrics**  
✔ Analyze **patient demographics**, **disease prevalence**, and **treatment outcomes**  

---

## **💡 Solution Overview**  
Designed a **relational database** to store and analyze:  
✅ **Patient records** (demographics, contact details)  
✅ **Doctor details** (specialization, department)  
✅ **Medical records** (diagnoses, treatments, admission/discharge dates)  

**Tools Used:**  
🔹 **SQL** (PostgreSQL) for database queries  
🔹 **GitHub Markdown** for documentation  

---

## **🗂 Database Schema**  
### **1. `Patients` Table**  
| Column         | Type        | Description               |
|----------------|-------------|---------------------------|
| `patient_id`   | `INT` (PK)  | Unique patient identifier |
| `patient_name` | `VARCHAR`   | Full name of patient      |
| `date_of_birth`| `DATE`      | Patient's birth date      |
| `gender`       | `VARCHAR`   | Male/Female/Other         |
| `address`      | `VARCHAR`   | Patient's address         |
| `phone_number` | `VARCHAR`   | Contact number            |

### **2. `Doctors` Table**  
| Column          | Type        | Description               |
|-----------------|-------------|---------------------------|
| `doctor_id`     | `INT` (PK)  | Unique doctor identifier  |
| `doctor_name`   | `VARCHAR`   | Full name of doctor       |
| `specialization`| `VARCHAR`   | Medical specialization    |
| `department`    | `VARCHAR`   | Hospital department       |

### **3. `MedicalRecords` Table**  
| Column           | Type        | Description                     |
|------------------|-------------|---------------------------------|
| `record_id`      | `INT` (PK)  | Unique record identifier        |
| `patient_id`     | `INT` (FK)  | References `Patients.patient_id`|
| `admission_date` | `DATE`      | Date of admission               |
| `discharge_date` | `DATE`      | Date of discharge               |
| `diagnosis`      | `VARCHAR`   | Medical diagnosis               |
| `treatment`      | `TEXT`      | Treatment details               |
| `doctor_id`      | `INT` (FK)  | References `Doctors.doctor_id`  |

---

## **📊 SQL Analysis & Insights**  

### **🔍 Basic Queries**  
**1. Retrieve all patients' names and genders:**  
```sql
SELECT patient_name, gender FROM Patients;
```

**2. Count total patients:**  
```sql
SELECT COUNT(*) AS total_patients FROM Patients;
```

**3. Oldest patient:**  
```sql
SELECT patient_name, date_of_birth 
FROM Patients 
ORDER BY date_of_birth ASC LIMIT 1;
```

---

### **📈 Intermediate Queries**  
**1. Patients with Diabetes/Hypertension:**  
```sql
SELECT patient_name, diagnosis 
FROM MedicalRecords 
WHERE diagnosis IN ('Diabetes', 'Hypertension');
```

**2. Average hospital stay duration:**  
```sql
SELECT AVG(discharge_date - admission_date) AS avg_stay_days 
FROM MedicalRecords;
```

**3. Top 5 most common diagnoses:**  
```sql
SELECT diagnosis, COUNT(*) AS frequency 
FROM MedicalRecords 
GROUP BY diagnosis 
ORDER BY frequency DESC LIMIT 5;
```

---

### **📌 Advanced Analytics**  
**1. Gender distribution (%)**  
```sql
SELECT 
    gender, 
    COUNT(*) AS count,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Patients), 2) AS percentage
FROM Patients 
GROUP BY gender;
```

**2. Doctors with most patients treated:**  
```sql
SELECT d.doctor_name, COUNT(m.record_id) AS patients_treated
FROM Doctors d
JOIN MedicalRecords m ON d.doctor_id = m.doctor_id
GROUP BY d.doctor_name
ORDER BY patients_treated DESC LIMIT 3;
```

**3. Readmission rate (within 30 days):**  
```sql
SELECT p.patient_name, m1.discharge_date, m2.admission_date
FROM MedicalRecords m1
JOIN Patients p ON m1.patient_id = p.patient_id
JOIN MedicalRecords m2 ON m1.patient_id = m2.patient_id
WHERE m2.admission_date - m1.discharge_date <= 30
AND m2.admission_date > m1.discharge_date;
```

---

## **📌 Key Findings**  
✔# **Revised Key Findings from Hospital Patient Analysis**

Based on the provided datasets (MedicalRecords.csv, Doctors.csv, Patients.csv), here are the updated key insights:

## **📊 Patient Demographics**
![Gender Distribution](https://via.placeholder.com/300x150?text=50%25+Male+50%25+Female)
- **Gender Distribution**:
  - Male: 50% (10 patients)
  - Female: 50% (10 patients)
- **Age Groups**:
  ```sql
  SELECT 
    CASE 
      WHEN date_part('year', age(date_of_birth)) < 30 THEN 'Under 30'
      WHEN date_part('year', age(date_of_birth)) BETWEEN 30 AND 50 THEN '30-50'
      ELSE 'Over 50'
    END as age_group,
    COUNT(*) as patients
  FROM Patients
  GROUP BY age_group;
  ```
  - **30-50 years**: 65% (13 patients)
  - **Under 30**: 20% (4 patients)
  - **Over 50**: 15% (3 patients)

## **🏥 Hospital Utilization**
![Admissions by Month](https://via.placeholder.com/400x200?text=Admissions+Peak+in+Winter)
- **Average Length of Stay**: 7 days
- **Busiest Months**:
  - January 2023: 2 admissions
  - December 2023: 2 admissions
- **Readmission Rate**: 0% (no readmissions within 30 days in this dataset)

## **🩺 Top Medical Conditions**
![Top Diagnoses](https://via.placeholder.com/400x200?text=Hypertension+Most+Common)
1. **Most Common Diagnoses**:
   - Hypertension (1 case)
   - Diabetes (1 case)
   - Asthma (1 case)
   - *(Note: Dataset shows 20 unique diagnoses with 1 case each)*

2. **Longest Hospital Stay**:
   - **8 days**: Depression (Patient 8), Obesity (Patient 20)

## **👨‍⚕️ Doctor Performance**
![Doctor Case Load](https://via.placeholder.com/400x200?text=Dr.+Jones+Handled+Most+Cases)
- **Top Doctors by Patient Volume**:
  1. **Dr. Sarah Jones (Pulmonology)**: 7 cases
  2. **Dr. Michael Lee (Endocrinology)**: 7 cases
  3. **Dr. Linda Smith (Cardiology)**: 6 cases

- **Most Active Department**: 
  - **Internal Medicine**: 7 cases (all handled by Dr. Jones)

## **💡 Notable Insights**
1. **Diagnosis Diversity**: 
   - 20 unique diagnoses across 20 patients (no repeats in this dataset)
   
2. **Treatment Consistency**:
   - Each diagnosis had a standardized treatment approach:
     - e.g., All hypertension cases received "ACE inhibitors"

3. **Seasonal Patterns**:
   ```sql
   SELECT 
     EXTRACT(MONTH FROM admission_date) as month,
     COUNT(*) as admissions
   FROM MedicalRecords
   GROUP BY month
   ORDER BY admissions DESC;
   ```
   - **Peak Months**: January, December (2 admissions each)
   - **Lowest Month**: April, September (1 admission each)




---

## **🚀 How to Reproduce**  
1. **Set up the database** (PostgreSQL recommended).  
2. **Import sample data** from [dataset link](#).  
3. **Run SQL queries** from this README.  

---

## **📜 License**  
This project is under **MIT License**.  

---

### **🔗 Connect**  
📧 Email | 💼 LinkedIn | 🐦 Twitter  




