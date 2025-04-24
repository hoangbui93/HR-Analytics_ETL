# HR Analytics: Job Change of Data Scientists

## 📖 Project Overview
**Context and Content**

A company which is active in Big Data and Data Science wants to hire data scientists among people who successfully pass some courses which conduct by the company.

Many people signup for their training. Company wants to know which of these candidates are really wants to work for the company after training or looking for a new employment because **it helps to reduce the cost and time as well as the quality of training or planning the courses and categorization of candidates.**

Information related to demographics, education, experience are in hands from candidates signup and enrollment.

## 🗂 Data Sources

1. **Enrollies’ Demographics (Google Sheets)**  
   - **URL:** `https://docs.google.com/spreadsheets/d/1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI/edit?usp=sharing`  
   - **Fields:** `enrollee_id`, `full_name`, `city`, `gender`

2. **Education Profiles (Excel)**  
   - **URL:** `https://assets.swisscoding.edu.vn/company_course/enrollies_education.xlsx`  
   - **Fields:** `enrollee_id`, `enrolled_university`, `education_level`, `major_discipline`

3. **Work Experience (CSV)**  
   - **URL:** `https://assets.swisscoding.edu.vn/company_course/work_experience.csv`  
   - **Fields:** `enrollee_id`, `relevent_experience`, `experience`, `company_size`, `company_type`, `last_new_job`

4. **Training Hours (MySQL)**  
   - **Host:** `112.213.86.31:3360`  
   - **DB:** `company_course` → Table `training_hours`

5. **City Development Index (HTML)**  
   - **URL:** `https://sca-programming-school.github.io/city_development_index/index.html`

6. **Employment Status (MySQL)**  
   - **Same MySQL credentials**, Table `employment`

## ▶️ How to Run
From the project root:
`python hr_analytics_etl.py`

This will:
- Extract from Google Sheets, Excel, CSV, MySQL, HTML
- Transform each DataFrame (fill/mode imputations, type conversions)
- Load into a local SQLite “data warehouse” (HR_DataWarehouse.db)

## 🔍 ETL Steps & Design Decisions
- Google Sheet → Demographics:
  - Fill missing gender with "Unknown"
  - Cast city, gender to category

- Excel → Education:
 - Impute missing major_discipline as "Unknown"
 - Impute education_level and enrolled_university with mode

- CSV → Work Experience:
 - Standardize experience and last_new_job using mode
 - Fill missing company_size/company_type with "Unknown"

- MySQL → Training & Employment:
 - Pull full tables via SQLAlchemy
 - No further cleaning required

- HTML → City Development Index:
 - Read first table via pandas.read_html()
 - Keep columns city, city_development_index

- Refactoring:
All repeated “load” logic wrapped in utility functions: load_google_sheet(), load_excel(), load_csv(), load_html_table(), load_mysql_table(), save_to_sqlite()
