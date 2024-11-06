# HR Dashboard - Power BI Implementation Documentation

## Table of Contents
- [Data Model](#data-model)
- [Dashboard Pages](#dashboard-pages)
  - [New Hires](#new-hires)
  - [Actives and Separations](#actives-and-separations)
  - [Bad Hires](#bad-hires)

## Data Model

The data model consists of the following tables and relationships:

### Core Tables
- **Employee**
  - Fields: `Age`, `AgeGroupID`, `BadHires`, `BU`
  - Central fact table connected to dimension tables

### Dimension Tables
- **AgeGroup**
  - Fields: `AgeGroup`, `AgeGroupID`
- **Date**
  - Fields: `Date`, `Day`, `Month`, `MonthEndDate`
- **Gender**
  - Fields: `Gender`, `ID`, `Sort`
- **PayType**
  - Fields: `PayType`, `PayTypeID`
- **SeparationReason**
  - Fields: `SeparationReason`, `SeparationTypeID`
- **Ethnicity**
  - Fields: `EthnicGroup`, `Ethnicity`
- **BU (Business Unit)**
  - Fields: `Region`, `RegionSeq`, `VP`, `Count of BU`

---

## Dashboard Pages

### New Hires Page

#### Visualizations
![New Hires Page](https://github.com/itsShrizon/HR-Dashboard-Power-BI/blob/main/HR%20Dashboard%20Page%201.jpg)

1. **Hires by Type (YoY)**
   - **Chart Type**: Line chart
   - **X-axis**: Months
   - **Y-axis**: Number of hires (logarithmic scale)
   - **Legend**: 
     - Full-Time (blue)
     - Part-Time (yellow)

2. **New Hires by AgeGroup and Gender**
   - **Chart Type**: Stacked bar chart
   - **Categories**: `<30`, `30-49`, `50+`
   - **Colors**: 
     - Male (blue)
     - Female (pink)

3. **Ethnicity and Region Matrix**
   - **Chart Type**: Table
   - **Columns**: Group B through Group G
   - **Rows**: Regions (e.g., North, Midwest, Northwest)
   - **Values**: New Hires and Actives counts

---

### Actives and Separations Page

#### Visualizations
![Actives and Separations Page](https://github.com/itsShrizon/HR-Dashboard-Power-BI/blob/main/HR%20Dashboard%20Page%202.jpg)

1. **Seps by SeparationReason**
   - **Chart Type**: Donut chart
   - **Categories**: Voluntary vs Involuntary

2. **Seps and Seps SPLY by Month and SeparationReason**
   - **Chart Type**: Stacked column chart with line
   - **X-axis**: Months
   - **Y-axis**: Separation counts
   - **Colors**: 
     - Voluntary (pink)
     - Involuntary (blue)

3. **Region Performance Matrix**
   - **Chart Type**: Table
   - **Columns**: Act SPLY, Actives, YoY, Involuntary Seps, etc.
   - **Rows**: Regions
   - **Conditional Formatting**: Based on YoY values

---

### Bad Hires Page

#### Visualizations
![Bad Hires Page](https://github.com/itsShrizon/HR-Dashboard-Power-BI/blob/main/HR%20Dashboard%20Page%202.jpg)

1. **BadHires by Gender**
   - **Chart Type**: Donut chart
   - **Categories**: Male vs Female

2. **Bad Hires YoY % Change**
   - **Chart Type**: Line chart by AgeGroup
   - **X-axis**: Months
   - **Y-axis**: Percentage change
   - **Lines**: 
     - `<30`
     - `30-49`
     - `50+`

3. **BadHire % of Actives by AgeGroup**
   - **Chart Type**: Horizontal bar chart
   - **Categories**: `<30`, `30-49`, `50+`
   - **Values**: Percentage of bad hires

4. **Region Matrix**
   - **Chart Type**: Table showing bad hire metrics by region and ethnic groups

---

## DAX Measures (Key Calculations)

```dax
// YoY Change
YoY Change = 
DIVIDE(
    [Current Value] - [SPLY Value],
    [SPLY Value],
    0
)

// Bad Hire Percentage
BadHire % of Actives = 
DIVIDE(
    [Total Bad Hires],
    [Total Actives],
    0
)

### Data Model Visualization

Here is a visualization of the relationships between the tables in the data model:

```mermaid
erDiagram
    Employee {
        int Age
        int AgeGroupID
        string BadHires
        string BU
    }
    
    AgeGroup {
        int AgeGroupID
        string AgeGroup
    }
    
    Date {
        date Date
        int Day
        string Month
        date MonthEndDate
    }
    
    Gender {
        int ID
        string Gender
        int Sort
    }
    
    PayType {
        int PayTypeID
        string PayType
    }
    
    SeparationReason {
        int SeparationTypeID
        string SeparationReason
    }
    
    Ethnicity {
        string EthnicGroup
        string Ethnicity
    }
    
    BU {
        string Region
        int RegionSeq
        string VP
        int CountOfBU
    }

    Employee ||--o{ AgeGroup : "AgeGroupID"
    Employee ||--o{ Date : "has"
    Employee ||--o{ Gender : "has"
    Employee ||--o{ PayType : "PayTypeID"
    Employee ||--o{ SeparationReason : "has"
    Employee ||--o{ Ethnicity : "has"
    Employee ||--o{ BU : "belongs_to"
