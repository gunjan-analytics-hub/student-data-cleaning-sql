# Student Data Cleaning (SQL)

Cleaning a messy dataset of 10,000 student records using MySQL.

The raw data has repeated rows, spelling differences, blank cells, and impossible numbers. This project fixes these problems step by step with SQL and documents what each step does.

## Dataset

- **File:** `student_dataset_dirty.csv`
- **Size:** 10,000 rows and 15 columns
- **Columns:** Student_ID, Name, Age, Gender, City, Class, Attendance_Percentage, Study_Hours_per_day, Math_Score, Science_Score, English_Score, Total_Score, Parent_Education, Internet_Access, Result

### Problems in the raw data

| Problem | Example |
|---|---|
| Duplicate rows | 150 exact copies |
| Different spellings | Male, M, MALE, male |
| Mixed class formats | Six, 6th, 06, 6 |
| City typos | Dacca, Mymensign, Ctg, Barishal |
| Blank cells | Gender, City, Name, scores, and more |
| Impossible numbers | Attendance above 100, negative scores, Age 227 |
| Text instead of numbers | Age written as "fifteen" |

## What the SQL script does

The script is in `student_data_cleaning.sql` and has 19 steps.

1. **Prepare:** create the working table `student_dataset_v2` and remove duplicates (10,000 rows become 9,850).
2. **Standardize:** make Student_ID, Name, Gender, City, Class, Parent_Education, and Internet_Access use one clean format.
3. **Validate:** set impossible numbers to NULL (Age outside 0-18, attendance outside 0-100, study hours outside 0-24, scores outside 0-100).
4. **Fill blanks:** use the most common value (mode) for text columns and the middle value (median) for number columns.
5. **Derive:** recalculate Total_Score, rebuild Result (Pass if every subject is at least 33), and set Age from Class.
6. **Finalize:** change columns to the right data types (INT and FLOAT).

## How to run

1. Open MySQL Workbench (MySQL 8.0 or later).
2. Import `student_dataset_dirty.csv` into a table named `student_dataset_v1`.
3. Open `student_data_cleaning.sql` and run it from top to bottom.
4. Check the final table `student_dataset_v2`.

## Review findings

I checked the script against the real data and found some issues to improve:

- **Missing score fill:** Step 13 only has a comment for the median fill. Rows with a missing score get no Total_Score and are marked Fail (about 1,263 rows).
- **Internet access:** "Y" and "N" are not recognized, so about 857 "No" answers become "Yes".
- **Age:** Step 18 replaces most valid ages with a value taken from Class.
- **Result:** the new Pass/Fail rule matches the original Result only about 49% of the time.
- **Median calculation:** the median steps count NULL rows, which makes the value too low.
- **City names:** spelling variants such as Dacca, Ctg, and Barishal are not merged.
- **Names:** only the first word is capitalized.

## Report

`student_data_cleaning_report.pptx` is a 7-slide summary of the pipeline, the chart of dirty values, the issues above, and the recommended fixes.

## Project files

```
student-data-cleaning-sql/
├── student_dataset_dirty.csv
├── student_data_cleaning.sql
├── student_data_cleaning_report.pptx
└── README.md
```

## Skills

SQL, Data Cleaning, Data Quality
