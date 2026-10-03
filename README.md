# Heart Disease Analysis Dashboard (Power BI)

An interactive Power BI dashboard that analyzes a dataset of 10,000 patients to understand how heart disease cases are distributed across age group, gender, smoking habit and diabetes status.

## <img width="572" height="311" alt="image" src="https://github.com/user-attachments/assets/a5633005-c3d2-45f3-ac09-96c7c3f8ca6e" />


## Objective

- Measure overall heart disease prevalence in the dataset
- Compare heart disease cases across age group, gender, smoking habit and diabetes status
- Provide slicers so users can filter the dashboard for any patient segment

## Dataset

| Item | Detail |
|---|---|
| File | `heart_disease.csv` |
| Rows | 10,000 patients |
| Columns | 21 health and lifestyle attributes |
| Target column | `Heart Disease Status` (Yes / No) |

Main columns used in this dashboard: Age, Gender, Smoking, Diabetes, Heart Disease Status.

## Data Preparation (Power Query)

- Loaded the CSV file and promoted the first row to headers
- Set correct data types for all columns
- Handled missing values: blank Gender, Smoking and Diabetes values were labeled as `Unknown`, and blank Age values were filled so that no patient rows were removed
- Created the calculated columns and measures listed below

## Data Model

**Calculated columns**

| Name | Logic |
|---|---|
| Heart Disease Flag | 1 if Heart Disease Status is Yes, otherwise 0 |
| Age Group | Young: below 30, Adult: 30 to 49, Senior: 50 and above |

**Measures (DAX)**

| Name | Purpose |
|---|---|
| Total Patients | Number of patient records |
| Heart Disease Patients | Number of patients with heart disease |
| Heart Disease % | Heart Disease Patients divided by Total Patients |
| Average Age | Average age of patients |

## Dashboard Components

| Visual | Type | What it shows |
|---|---|---|
| Total Patients, Heart Disease %, Heart Disease, Average Age | Cards | Key figures |
| Heart Disease Patients by Age Group | Column chart | Number of cases in each age group |
| Heart Disease Status by Age Group | Area chart | Patients with and without heart disease per age group |
| Heart Disease by Gender | Pie chart | Share of cases by gender |
| Heart Disease by Smoking Habit | Stacked column chart | Patients by smoking habit and disease status |
| Heart Disease by Diabetes Status | Donut chart | Share of cases by diabetes status |
| Gender, Age Group, Heart Disease Status, Diabetes Status | Slicers | Interactive filtering |

## Key Findings

- 2,000 out of 10,000 patients (20%) have heart disease, and the average patient age is 49
- Seniors account for the largest number of cases (969), followed by Adults (675) and Young patients (356). This is partly because the Senior group is the largest group in the data
- Heart disease rate is close to 20% in every group: Young 19.5%, Adult 20.9%, Senior 19.6%
- Gender split of cases is 51.5% female and 48.5% male, with rates of 20.7% and 19.4%
- Smokers (20.1%) and non-smokers (19.9%) show almost the same rate, and the same is true for diabetic (19.9%) and non-diabetic (20.1%) patients

## Project Files

```
Heart-Disease-PowerBI-Dashboard/
|-- heart_disease.csv               Source data
|-- Heart_Disease_Dashboard.pbix    Power BI report
|-- Dashboard_Screenshot.png        Dashboard preview
|-- POWERBI_GUIDE.md              
|-- README.md
```

## How to Open

1. Download or clone this repository
2. Open `Heart_Disease_Dashboard.pbix` in Power BI Desktop
3. If Power BI asks for the data location, point it to `heart_disease.csv`
4. Use the slicers or click on a chart element to explore the data

## Tools Used

- Power BI Desktop
- Power Query
- DAX
- CSV

## Limitations

- Only five of the 21 columns are used in the dashboard
- The dashboard shows counts and shares, so it describes the data but does not prove cause and effect
- Missing values were labeled or filled, which can slightly affect the Unknown category and the age average

## Author

**Wasim Akram**
GitHub: [akramwasim-in](https://github.com/akramwasim-in)
LinkedIn: [akramwasim-in](https://linkedin.com/in/akramwasim-in)
