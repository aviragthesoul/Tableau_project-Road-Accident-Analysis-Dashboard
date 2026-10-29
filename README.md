UK Road Accident Analysis Dashboard

An interactive Tableau dashboard that analyzes UK road accidents from 2019 to 2022. The raw data was cleaned in Excel Power Query, then modeled and visualized in Tableau with year-over-year comparisons and a severity selector.

Dashboard preview

Live dashboard: View on Tableau Public

Table of contents
Project overview
Business questions
Dataset
Tools used
Workflow
Data cleaning in Power Query
Tableau build
Dashboard features
Key insights
Repository structure
How to use
Author
Project overview

Road accidents cost lives and put a heavy load on emergency services. This project turns a large accident record into a single-page dashboard that shows how many accidents and casualties happened, how severe they were, and where, when and under what conditions they occurred.

The dashboard compares a current year against a previous year, so changes in accidents and casualties are visible at a glance. A severity selector lets the viewer switch the lower charts between Fatal, Serious and Slight casualties.

Business questions
How many accidents and casualties happened in a given year, and how does that compare with the year before?
How many casualties were fatal, serious or slight?
Which vehicle types are involved in the most fatal casualties?
How do weather and road surface conditions affect fatal casualties?
Which road types are the most dangerous?
Where do the accidents happen geographically?
Dataset
Item	Detail
Source	YOUR_DATA_SOURCE (for example, Kaggle or the UK Department for Transport)
Period	1 January 2019 to 31 December 2022
Size	660,679 rows and 14 columns
Grain	One row per vehicle involved in an accident

Important: the Index column repeats, because each row is a vehicle, not a unique accident. For that reason, Total Accidents is a row count, and casualties are the sum of Number_of_Casualties. Duplicates were intentionally not removed.

Main columns: Index, Accident_Severity, Accident Date, Latitude, Longitude, Number_of_Casualties, Number_of_Vehicles, Road_Surface_Conditions, Road_Type, Weather_Conditions, Vehicle_Type, Light_Conditions, District Area, Urban_or_Rural_Area.

Tools used
Microsoft Excel (Power Query): data cleaning and preparation
Tableau Public: data modeling, calculated fields, parameters and dashboard design
GitHub: version control and project showcase
Workflow
Raw CSV  ->  Power Query cleaning  ->  Cleaned Excel file  ->  Tableau
        ->  Calculated fields and parameters  ->  Worksheets  ->  Dashboard
Data cleaning in Power Query
Imported the CSV with Data > From Text/CSV and opened it in the Power Query Editor.
Set data types: dates as Date (using the English (United Kingdom) locale, because dates are dd-mm-yyyy), latitude and longitude as Decimal Number, casualty and vehicle counts as Whole Number, and the remaining columns as Text.
Trimmed text columns to remove hidden spaces that would split one value into two.
Handled missing values:
Road_Surface_Conditions (726 blanks) labelled as Unknown
Weather_Conditions (about 14,000 blanks) grouped into Other
Road_Type blanks kept as Null, because they represent a real gap in the data
Rows with no latitude or longitude kept, since they still count in the totals and Tableau simply skips them on the map
Checked categories for spelling and consistency (for example Light_Conditions had no nulls or duplicates).
Removed unused columns to reduce file size.
Kept all duplicate Index values on purpose, as explained above.
Validated the result: 660,679 rows in total and 144,419 rows for 2022.
Loaded the table to a worksheet and saved it as accident_data_cleaned.xlsx.
Tableau build
Data preparation
Connected the cleaned Excel file as an extract.
Created Year (YEAR([Accident Date])) and Month (MONTH([Accident Date])) and converted both to dimensions.
Grouped vehicle types into Agricultural, Bus, Car, Truck, Bike and Other, and weather into Fine, Rain, Snow/Fog and Other.
Parameters
Parameter	Type	Default
Current Year	Integer list (2019 to 2022)	2022
Previous Year	Integer list (2019 to 2022)	2021
Select Accident Severity	String list (Fatal, Serious, Slight)	Fatal
Calculated fields

For each KPI there is a current-year value, a previous-year value and a year-over-year (YoY) percentage.

Current Year Accidents = SUM(IF [Year]=[Current Year] THEN 1 ELSE 0 END)
Previous Year Accident = SUM(IF [Year]=[Previous Year] THEN 1 ELSE 0 END)
YOY Accidents          = (ZN([Current Year Accidents]) - ZN([Previous Year Accident]))
                         / ZN([Previous Year Accident])

Current Year Fatal     = SUM(IF [Year]=[Current Year] AND [Accident Severity]="Fatal"
                             THEN [Number of Casualties] END)

Current Year Selected  = SUM(IF [Year]=[Current Year]
                             AND [Accident Severity]=[Select Accident Severity]
                             THEN [Number of Casualties] END)

The same pattern is used for total casualties, serious, slight and the selected severity. YoY values use the custom number format ▲0.00%;▼0.00% to show arrows.

Worksheets
Five KPI cards (Total Accidents, Total Casualties, Fatal, Serious, Slight), each with a sparkline (blue area for the current year, orange line for the previous year, on a dual axis)
Fatal casualties by vehicle type (shape-based icon tiles)
Fatal casualties by weather condition (donut)
Fatal casualties by road surface (donut)
Fatal casualties by road type (horizontal bar with value and percentage)
Fatal casualties by location (dark-themed map)
Dashboard design
Custom background and panels with a dark blue theme and orange accents
Floating sheets placed on the panels, with transparent backgrounds
Title bar with the parameter controls at the top right
Dashboard features
Year comparison: choose any current and previous year, and every KPI and YoY value updates
Severity selector: switches the vehicle, weather, road surface, road type and map views between Fatal, Serious and Slight
KPI cards with trends: monthly sparklines show seasonality at a glance
Geographic view: a map of accident locations across the UK
Clean visual theme: dark layout designed to match a professional analytics dashboard
Key insights

Using 2022 compared with 2021 (Fatal selected):

Metric	2022	YoY
Total accidents	144,419	down 11.70%
Total casualties	195,737	down 11.89%
Fatal casualties	2,855	down 26.40%
Serious casualties	27,045	down 16.30%
Slight casualties	165,837	down 10.82%
Fatal casualties fell sharply, much faster than total accidents.
Cars account for most fatal casualties (2,350 of 2,855), followed by trucks (239) and bikes (176).
Fatal casualties rose only in the "Other" vehicle group (33, up 50%), though from a small base.
Most fatal casualties happen in fine weather (85.01%), which suggests driver behaviour matters more than conditions.
About two thirds happen on dry roads (67.60%), with wet roads at roughly 27%.
Single carriageways are the most dangerous road type, with 74.01% of fatal casualties (2,113), followed by dual carriageways (23.71%).
Repository structure
Road-Accident-Analysis-Dashboard/
|-- README.md
|-- Road_Accident_Dashboard.twbx        Tableau packaged workbook
|-- data/
|   |-- accident_data_cleaned.xlsx      Cleaned dataset (or a sample)
|   `-- accident_data_sample.csv        First 1,000 rows of the raw data
|-- power_query/
|   `-- cleaning_steps.txt              Power Query (M) code
`-- images/
    `-- dashboard.png                   Dashboard screenshot
How to use
Open the Tableau Public link above to explore the dashboard online, or
Download Road_Accident_Dashboard.twbx and open it in Tableau Desktop or Tableau Public.
Use the Current Year, Previous Year and Select Accident Severity dropdowns at the top right.
Author

Avirag | GitHub

If you found this project useful, consider giving it a star.
