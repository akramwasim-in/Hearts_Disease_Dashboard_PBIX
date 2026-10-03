# Power BI Build Guide: Heart Disease Dashboard

This guide explains how the dashboard is built, step by step: data loading, Power Query cleaning, calculated columns, measures, visuals and interactions.

Note: the DAX below reproduces the numbers shown in the dashboard (Total 10,000, Heart Disease 2,000, 20%, Senior 969, Adult 675, Young 356). Compare it with the formulas in your own .pbix (Report view, click a column or measure, and read the formula bar).

---

## Step 1. Load the data

1. Open Power BI Desktop
2. Home > Get Data > Text/CSV
3. Select `heart_disease.csv` and click Transform Data (this opens Power Query)

## Step 2. Clean the data in Power Query

1. Home > Use First Row as Headers (if not applied automatically)
2. Check data types: Age, Blood Pressure, Cholesterol Level, BMI and similar columns should be numbers. Gender, Smoking, Diabetes and Heart Disease Status should be text
3. Check missing values. In this file, Age, Gender, Smoking and Diabetes each have between 19 and 30 blank rows
4. Replace blanks in text columns:
   - Select Gender, Smoking and Diabetes
   - Transform > Replace Values
   - Value to find: `null`, Replace with: `Unknown`
5. Replace blanks in Age (example: with the median or average age) so the patient count stays at 10,000
6. Close and Apply

## Step 3. Create calculated columns

A calculated column adds a new column to the table, one value per row. Use Table tools > New column.

**Heart Disease Flag** converts Yes/No into 1/0 so it can be summed:

```DAX
Heart Disease Flag =
IF ( heart_disease[Heart Disease Status] = "Yes", 1, 0 )
```

**Age Group** groups ages into three categories:

```DAX
Age Group =
IF (
    heart_disease[Age] < 30, "Young",
    IF ( heart_disease[Age] < 50, "Adult", "Senior" )
)
```

**Age Group Order** (recommended) so charts sort as Young, Adult, Senior instead of alphabetically or by value:

```DAX
Age Group Order =
SWITCH ( heart_disease[Age Group], "Young", 1, "Adult", 2, "Senior", 3 )
```

Then select the Age Group column > Column tools > Sort by column > Age Group Order.

## Step 4. Create measures

A measure is a calculation that recalculates based on the current filters (slicers and chart clicks). Use Home > New measure.

```DAX
Total Patients =
COUNTROWS ( heart_disease )
```

```DAX
Heart Disease Patients =
SUM ( heart_disease[Heart Disease Flag] )
```

```DAX
Heart Disease % =
DIVIDE ( [Heart Disease Patients], [Total Patients] )
```

Set the format of Heart Disease % to Percentage.

```DAX
Average Age =
AVERAGE ( heart_disease[Age] )
```

Optional measure for rate comparison across groups:

```DAX
Heart Disease Rate =
DIVIDE ( SUM ( heart_disease[Heart Disease Flag] ), COUNTROWS ( heart_disease ) )
```

## Step 5. Build the visuals

| Visual | Fields |
|---|---|
| Card: Total Patients | Measure: Total Patients |
| Card: Heart Disease % | Measure: Heart Disease % |
| Card: Heart Disease | Measure: Heart Disease Patients |
| Card: Average Age | Measure: Average Age |
| Clustered column chart | X-axis: Age Group, Y-axis: Heart Disease Patients |
| Area chart | X-axis: Age Group, Y-axis: Total Patients, Legend: Heart Disease Status |
| Pie chart | Legend: Gender, Values: Heart Disease Patients |
| Stacked column chart | X-axis: Smoking, Legend: Heart Disease Status, Y-axis: Total Patients |
| Donut chart | Legend: Diabetes, Values: Heart Disease Patients |
| Slicers (4) | Gender, Age Group, Heart Disease Status, Diabetes |


## Step 6. Interactions: filter, highlight and none

Power BI has three interaction modes between visuals:

| Mode | What happens to other visuals |
|---|---|
| Filter | Other categories disappear and only the selected data remains |
| Highlight | All categories stay visible, the selected part is shown in full color and the rest is dimmed |
| None | The visual ignores the selection |

4. Select the column chart > Format > Columns > Colors > fx > Format style: Field value > choose Bar Color

The selected group is shown in color and the others in grey, and all bars remain visible.

## Step 7. Issues found in the current report and how to fix them

| Issue | Fix |
|---|---|
| Area chart title says "by Age" but it is by Age Group | Rename the title to "Heart Disease Status by Age Group" |
| Age Group order is Senior, Adult, Young | Add Age Group Order and use Sort by column (Step 3) |
| Total Patients card uses Count of Age | Use the measure Total Patients (COUNTROWS) |
| Pie, donut and smoking charts have visual-level filters that remove Unknown and blank values, but the slicers still show Unknown | Remove those filters from the Filters pane, or remove Unknown from the slicers. Choose one approach so the visuals stay consistent |
| Pie and donut show the share of cases, not the rate | Rename the titles to "Share of Heart Disease Cases by Gender" and so on, or add Heart Disease Rate |
| Card label "Heart Disease" is unclear | Rename to "Heart Disease Patients" |
| "Average Age" is calculated by a measure named Heart Disease Avg | Rename the measure to Average Age and confirm whether it covers all patients or only patients with heart disease |
| Pie label is cut off (48.47...) | Reduce decimals to 1 and widen the visual, or move the label position to outside |
| Zoom sliders are visible on the column and area charts | Turn off Format > Zoom slider |
| Repository name is Hearts_Disease_Dashboard_PBIX | Rename to Heart-Disease-PowerBI-Dashboard. Then fix the link on LinkedIn |

## Step 8. Suggested insights to present

- Prevalence is 20% (2,000 of 10,000 patients)
- Seniors have the most cases (969), mainly because they are the largest group (4,944 patients)
- Rates by group are close: Young 19.5%, Adult 20.9%, Senior 19.6%
- Smoking and diabetes show almost no difference in rate (about 20% each)

