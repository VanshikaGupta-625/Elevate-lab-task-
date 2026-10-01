# Elevate-lab-task-
Repository for daily data analytics tasks and projects during the Elevates Labs Internship.
# Elevate Labs Data Analyst Internship - Task 1 Submission
**Submitted by:** Vanshika Gupta | **Role:** Data Analyst Intern

---

## 1. Project Overview & Code Architecture
This repository features the complete data preprocessing pipeline and analytics suite for the Medical Appointment No Shows dataset. Below is the production-ready Python cleaning pipeline implemented for this project:

```python
import pandas as pd
import numpy as np

def run_data_cleaning():
    # Load the raw dataset source containing initial data records
    source_file = 'KaggleV2-May-2016.csv'
    df = pd.read_csv(source_file)
    
    # 1. Deduplication Pipeline
    df.drop_duplicates(inplace=True)

    # 2. Identification columns formatting (PatientId and AppointmentID)
    df['PatientId'] = df['PatientId'].astype(np.int64).astype(str)
    df['AppointmentID'] = df['AppointmentID'].astype(str)

    # 3. Categorical styling and text normalization
    df['Gender'] = df['Gender'].str.strip().str.upper() 
    df['Neighbourhood'] = df['Neighbourhood'].str.strip().str.title() 

    # 4. Temporal structuring and logical validation
    df['ScheduledDay'] = pd.to_datetime(df['ScheduledDay']).dt.date
    df['AppointmentDay'] = pd.to_datetime(df['AppointmentDay']).dt.date

    # Logical Check: ScheduledDay must occur strictly before or on the AppointmentDay
    df = df[df['ScheduledDay'] <= df['AppointmentDay']]

    # 5. Numerical range validation and outlier treatment (Age boundary)
    df = df[(df['Age'] >= 0) & (df['Age'] <= 115)]

    # 6. Binary metric regularization
    binary_indicators = ['Scholarship', 'Hipertension', 'Diabetes', 'Alcoholism', 'SMS_received']
    for column in binary_indicators:
        df[column] = pd.to_numeric(df[column], errors='coerce').fillna(0).astype(int)
        
    # 7. Anomaly treatment for Handicap metric
    df['Handcap'] = pd.to_numeric(df['Handcap'], errors='coerce').fillna(0).astype(int)
    df.loc[df['Handcap'] > 1, 'Handcap'] = 1

    # 8. Target variable refactoring (No-show consistency check)
    df['No-show'] = df['No-show'].str.strip().str.title()
    df = df[df['No-show'].isin(['Yes', 'No'])]

    # 9. Final structural missing value substitution
    df.fillna('Unknown', inplace=True)
    
    # Save the pristine verified dataset
    df.to_csv('cleaned_medical_data.csv', index=False)
    print("Advanced Data Cleaning complete.")

if __name__ == "__main__":
    run_data_cleaning()
```

---

## 2. Process Documentation (What I Did & How I Did It)
Investigated the raw directory tracking 110,527 entries and successfully regularized the attributes across all 14 metrics using the following data engineering operations:

* **Handling Logical Chronological Faults:** I cross-referenced the scheduling logs against the actual hospital execution dates. I identified entries where `ScheduledDay` occurred after the `AppointmentDay` had passed and filtered them out as logical system entry errors.
* **Outlier & Range Regularization:** Scanned numerical traits inside the `Age` column and deleted negative row variables. Adjusted scale variations within the `Handcap` factor by capping values greater than 1 down to boolean parameters.
* **Purging Unbiased Statistics:** Located absolute identical multi-column repetitions across the matrix and executed deduplication steps to secure genuine record sets.
* **Visual Analytical Asset Generation:** Generated an automated browser report asset (`medical_attendance_dashboard.html`) displaying structural verification charts including attendance distributions and pathology traits.

---

## 3. Technical Interview Answers

1. **What are missing values and how do you handle them?**
Missing fields are unrecorded data voids. I manage them using controlled drop logic if minimal, or domain imputation to fill blanks safely.

2. **How do you treat duplicate records?**
Duplicates artificial warp core statistics. I identify them using multi-column sorting tools and discard identical matrices.

3. **Difference between dropna() and fillna() in Pandas?**
`dropna()` cuts structural rows holding null variables completely. `fillna()` overwrites blank spots safely with custom values.

4. **What is outlier treatment and why is it important?**
Outliers are extreme value spikes (e.g., Age 115+). Capping them ensures visualizations remain logically consistent.

5. **Explain the process of standardizing data.**
Standardizing applies baseline rules across parameters: cleaning types, dates, string cases, and casing formats uniformly.

6. **How do you handle inconsistent data formats (e.g., date/time)?**
I structure conflicting inputs into standardized, clean native objects using type-casting parameters like `pd.to_datetime()`.

7. **What are common data cleaning challenges?**
Major hurdles include parsing messy dates, preserving raw tracking statistics, and addressing broken text strings.

8. **How can you check data quality?**
Quality is measured by running range validations, evaluating raw matrix forms, and checking row null calculations.
