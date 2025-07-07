
# 🏥 Hospital Patient Management Analysis: A PostgreSQL-driven Solution

This project outlines the design, implementation, and analysis of a **relational database system** aimed at optimizing patient management within a hospital. Leveraging **PostgreSQL**, this solution provides a robust framework for streamlining patient record-keeping, tracking doctor appointments, and analyzing critical hospital performance metrics.

-----

## 📖 Table of Contents

  * [📌 Problem Statement](https://www.google.com/search?q=%23-problem-statement)
  * [💡 Solution Overview](https://www.google.com/search?q=%23-solution-overview)
  * [🗂 Database Schema](https://www.google.com/search?q=%23-database-schema)
  * [📊 SQL Analysis & Insights](https://www.google.com/search?q=%23-sql-analysis--insights)
      * [🔍 Basic Queries](https://www.google.com/search?q=%23-basic-queries)
      * [📈 Intermediate Queries](https://www.google.com/search?q=%23-intermediate-queries)
      * [📌 Advanced Analytics](https://www.google.com/search?q=%23-advanced-analytics)
  * [✅ Key Findings & Observations](https://www.google.com/search?q=%23-key-findings--observations)
      * [Patient Demographics](https://www.google.com/search?q=%23-patient-demographics)
      * [Hospital Utilization](https://www.google.com/search?q=%23-hospital-utilization)
      * [Top Medical Conditions](https://www.google.com/search?q=%23-top-medical-conditions)
      * [Doctor Performance](https://www.google.com/search?q=%23-doctor-performance)
      * [Notable Insights](https://www.google.com/search?q=%23-notable-insights)
  * [🚀 How to Reproduce](https://www.google.com/search?q=%23-how-to-reproduce)
  * [📜 License](https://www.google.com/search?q=%23-license)

-----

## 📌 Problem Statement

A hospital faces challenges in efficiently managing patient data, doctor appointments, and historical treatment records. The objective is to implement an **efficient Patient Management System** that can:

  * Streamline **patient record-keeping** and **doctor appointments**.
  * Track comprehensive **treatment history** and evaluate **hospital performance metrics**.
  * Facilitate detailed analysis of **patient demographics**, **disease prevalence**, and **treatment outcomes**.

-----

## 💡 Solution Overview

To address these challenges, I designed and implemented a **relational database** in **PostgreSQL**. This system is structured to meticulously store and enable analysis across key hospital entities:

  * **Patient records:** Capturing essential demographic and contact information.
  * **Doctor details:** Including their specialization and departmental affiliations.
  * **Medical records:** Documenting diagnoses, prescribed treatments, and critical admission/discharge dates for each patient.

**Tools Used:**

  * **SQL (PostgreSQL):** For database creation, management, and complex data querying.
  * **GitHub Markdown:** For clear and comprehensive project documentation.

-----

## 🗂 Database Schema

The database is composed of three interconnected tables, designed to ensure data integrity and facilitate efficient querying:

### **1. `Patients` Table**

| Column         | Type        | Description                   |
| :------------- | :---------- | :---------------------------- |
| `patient_id`   | `INT` (PK)  | Unique patient identifier     |
| `patient_name` | `VARCHAR`   | Full name of patient          |
| `date_of_birth`| `DATE`      | Patient's birth date          |
| `gender`       | `VARCHAR`   | Male/Female/Other             |
| `address`      | `VARCHAR`   | Patient's address             |
| `phone_number` | `VARCHAR`   | Contact number                |

### **2. `Doctors` Table**

| Column           | Type        | Description                 |
| :--------------- | :---------- | :-------------------------- |
| `doctor_id`      | `INT` (PK)  | Unique doctor identifier    |
| `doctor_name`    | `VARCHAR`   | Full name of doctor         |
| `specialization` | `VARCHAR`   | Medical specialization      |
| `department`     | `VARCHAR`   | Hospital department         |

### **3. `MedicalRecords` Table**

| Column           | Type        | Description                       |
| :--------------- | :---------- | :-------------------------------- |
| `record_id`      | `INT` (PK)  | Unique record identifier          |
| `patient_id`     | `INT` (FK)  | References `Patients.patient_id`  |
| `admission_date` | `DATE`      | Date of admission                 |
| `discharge_date` | `DATE`      | Date of discharge                 |
| `diagnosis`      | `VARCHAR`   | Medical diagnosis                 |
| `treatment`      | `TEXT`      | Detailed treatment information    |
| `doctor_id`      | `INT` (FK)  | References `Doctors.doctor_id`    |

-----

## 📊 SQL Analysis & Insights

The following SQL queries demonstrate the analytical capabilities of the database, ranging from basic data retrieval to complex performance metrics.

### 🔍 Basic Queries

1.  **Retrieve all patients' names and genders:**
    ```sql
    SELECT patient_name, gender FROM Patients;
    ```
2.  **Count total number of patients in the system:**
    ```sql
    SELECT COUNT(*) AS total_patients FROM Patients;
    ```
3.  **Identify the oldest patient in the database:**
    ```sql
    SELECT patient_name, date_of_birth
    FROM Patients
    ORDER BY date_of_birth ASC LIMIT 1;
    ```

### 📈 Intermediate Queries

1.  **Find patients diagnosed with specific conditions (e.g., Diabetes or Hypertension):**
    ```sql
    SELECT p.patient_name, mr.diagnosis
    FROM MedicalRecords mr
    JOIN Patients p ON mr.patient_id = p.patient_id
    WHERE mr.diagnosis IN ('Diabetes', 'Hypertension');
    ```
2.  **Calculate the average duration of hospital stays for all patients:**
    ```sql
    SELECT AVG(discharge_date - admission_date) AS avg_stay_days
    FROM MedicalRecords;
    ```
3.  **Determine the top 5 most common diagnoses across all medical records:**
    ```sql
    SELECT diagnosis, COUNT(*) AS frequency
    FROM MedicalRecords
    GROUP BY diagnosis
    ORDER BY frequency DESC LIMIT 5;
    ```

### 📌 Advanced Analytics

1.  **Analyze the gender distribution of patients by percentage:**
    ```sql
    SELECT
        gender,
        COUNT(*) AS count,
        ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Patients), 2) AS percentage
    FROM Patients
    GROUP BY gender;
    ```
2.  **Identify the top 3 doctors who have treated the most patients:**
    ```sql
    SELECT d.doctor_name, COUNT(m.record_id) AS patients_treated
    FROM Doctors d
    JOIN MedicalRecords m ON d.doctor_id = m.doctor_id
    GROUP BY d.doctor_name
    ORDER BY patients_treated DESC LIMIT 3;
    ```
3.  **Calculate the readmission rate for patients within a 30-day window post-discharge:**
    ```sql
    SELECT p.patient_name, m1.discharge_date, m2.admission_date
    FROM MedicalRecords m1
    JOIN Patients p ON m1.patient_id = p.patient_id
    JOIN MedicalRecords m2 ON m1.patient_id = m2.patient_id
    WHERE m2.admission_date - m1.discharge_date <= 30
    AND m2.admission_date > m1.discharge_date;
    ```

-----

## ✅ Key Findings & Observations

Based on the analysis of the provided dataset, here are the key insights into hospital operations and patient demographics:

### 📊 Patient Demographics

  * **Gender Distribution**: The patient dataset shows an equal distribution, with **50% Male (10 patients)** and **50% Female (10 patients)**.
  * **Age Groups**:
      * The largest patient group falls within the **30-50 years** bracket, accounting for **65% (13 patients)**.
      * Patients **Under 30** comprise **20% (4 patients)**.
      * Patients **Over 50** represent **15% (3 patients)**.

### 🏥 Hospital Utilization

  * **Average Length of Stay**: Patients, on average, have a hospital stay duration of **7 days**.
  * **Busiest Months for Admissions**:
      * **January 2023** and **December 2023** were the peak months, each recording **2 admissions**. This suggests potential seasonal patterns in patient intake.
  * **Readmission Rate**: The analysis revealed a **0% readmission rate** within 30 days in this particular dataset, indicating effective short-term patient management or discharge planning for the recorded cases.

### 🩺 Top Medical Conditions

  * **Most Common Diagnoses**: The dataset exhibits a high diversity in diagnoses. Among the recorded cases, conditions like **Hypertension, Diabetes, and Asthma** each appeared once, indicating a broad spectrum of medical conditions treated. (Note: The provided data shows 20 unique diagnoses, each with 1 case, within the 20 patients).
  * **Longest Hospital Stays**: The longest recorded hospital stays were **8 days**, associated with diagnoses such as **Depression (Patient 8)** and **Obesity (Patient 20)**.

### 👨‍⚕️ Doctor Performance

  * **Top Doctors by Patient Volume**:
    1.  **Dr. Sarah Jones (Pulmonology)**: Handled **7 cases**.
    2.  **Dr. Michael Lee (Endocrinology)**: Also managed **7 cases**.
    3.  **Dr. Linda Smith (Cardiology)**: Treated **6 cases**.
  * **Most Active Department**: **Internal Medicine** emerged as the most active department, managing **7 cases**, all handled by Dr. Jones.

### 💡 Notable Insights

  * **Diagnosis Diversity**: The dataset shows a unique diagnosis for each of the 20 patients, highlighting the varied medical needs addressed by the hospital.
  * **Treatment Consistency**: For the diagnoses present, a standardized treatment approach was observed (e.g., all hypertension cases received "ACE inhibitors"), suggesting established protocols.
  * **Seasonal Patterns**: Admissions show a slight peak during winter months (January and December), while months like April and September recorded the lowest admissions (1 each). This could inform resource planning and seasonal staffing adjustments.

-----

## 🚀 How to Reproduce

To replicate the database setup and analysis:

1.  **Set up PostgreSQL:** Ensure you have a PostgreSQL server running on your local machine or a remote host.
2.  **Create the Database:** Use SQL commands (e.g., `CREATE DATABASE hospital_db;`) to set up your database.
3.  **Create Tables:** Execute the `CREATE TABLE` statements based on the [Database Schema](https://www.google.com/search?q=%23-database-schema) section.
4.  **Import Sample Data:** Import the provided `.csv` files (e.g., `Patients.csv`, `Doctors.csv`, `MedicalRecords.csv`) into their respective tables using PostgreSQL's import functionalities (e.g., `\copy` command in psql or through a GUI tool like pgAdmin).
      * (Consider adding a placeholder link for where the user can find these CSV files in your repository, e.g., `[dataset link](path/to/your/data/folder)`).
5.  **Run SQL Queries:** Execute the SQL queries provided in the [SQL Analysis & Insights](https://www.google.com/search?q=%23-sql-analysis--insights) section to perform the analysis.

-----

## 📜 License

This project is open-sourced under the **MIT License**. See the `LICENSE` file in the repository for more details.

-----
