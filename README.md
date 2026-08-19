# GlobalPluse

# World Bank API Data Collection

## Overview

This project uses the **World Bank Open Data API** to programmatically collect economic, social, demographic, and development indicators for countries around the world.

The Python script dynamically fetches multiple indicators, handles API pagination, processes the returned JSON data, and organizes the resulting datasets by indicator category.

---

## API Used

**World Bank Open Data API**

The API endpoint follows this structure:

```text
https://api.worldbank.org/countries/all/indicators/{indicator_code}?format=json&per_page=1000&page={page}
```

### API Parameters

| Parameter        | Description                                     |
|------------------|-------------------------------------------------|
| `indicator_code` | Unique World Bank code identifying an indicator |
| `format=json`    | Requests the response in JSON format            |
| `per_page=1000`  | Retrieves up to 1,000 records per API request   |
| `page`           | Specifies the page of results to retrieve       |

---

## Data Collection Approach

The script uses a hierarchical structure to organize the API requests:

```text
Indicator Groups
│
├── Category 1
│   ├── Indicator 1
│   ├── Indicator 2
│   └── Indicator 3
│
├── Category 2
│   ├── Indicator 4
│   └── Indicator 5
│
└── Category 3
    └── Indicator 6
```

For every category, the script loops through its associated World Bank indicators and retrieves the available records.

---

## Pagination Handling

The World Bank API may return data across multiple pages. The script uses a `while` loop to continuously request pages until all available records are retrieved.

The API response contains metadata such as:

- Current page  
- Total number of pages  
- Number of records per page  
- Total number of records  

The script reads the total number of pages from the API response and uses it to control the data collection process.

This ensures that large datasets are not limited to the first 1,000 records.

---

## JSON Response Processing

The World Bank API returns a JSON structure containing two primary components:

```text
API Response
│
├── Metadata
│   ├── Page
│   ├── Total Pages
│   ├── Total Records
│   └── Records Per Page
│
└── Data Records
    ├── Country
    ├── Country Code
    ├── Year
    ├── Indicator
    ├── Indicator Code
    └── Value
```

The script extracts the actual records from:

```python
data[1]
```

while API metadata is accessed through:

```python
data[0]
```

---

## Category-Based Data Organization

The script initializes a dictionary:

```python
category_dataframes = {}
```

This allows the processed datasets to be organized according to their respective categories.

Conceptually, the final structure is:

```text
category_dataframes
│
├── Economy → DataFrame
├── Health → DataFrame
├── Education → DataFrame
└── Population → DataFrame
```

This organization makes the data easier to process, analyze, and visualize in tools such as **Power BI**.

---

## Error Handling

The script checks the HTTP response status before processing the returned data.

```python
if response.status_code != 200:
```

A status code of `200` indicates a successful API request.

The script also validates the response structure:

```python
if len(data) < 2:
```

This prevents processing incomplete or invalid API responses.

---

## Key Features

- Automated data extraction from the World Bank API  
- Multiple indicator support through indicator groups  
- Pagination handling for large datasets  
- JSON response parsing  
- HTTP error handling  
- Category-wise data organization  
- Scalable structure for adding new indicators  
- Data preparation for analysis and visualization  

---

## Technologies Used

- Python  
- `requests` — API requests and data retrieval  
- `pandas` — Data processing and DataFrame creation  
- World Bank Open Data API  
- Power BI — Data visualization and dashboard development  

---

## Data Pipeline

```text
World Bank API
      ↓
Indicator Groups
      ↓
API Requests
      ↓
Pagination Handling
      ↓
JSON Response
      ↓
Data Extraction
      ↓
Pandas DataFrames
      ↓
Category-wise Datasets
      ↓
Data Analysis
      ↓
Power BI Dashboard
```

---

## Purpose

The primary objective of this pipeline is to automate the collection of World Bank indicators instead of manually downloading individual datasets.

The resulting structured datasets can be used to identify **economic, demographic, social, and development trends across countries and over time**, providing a reliable data foundation for further analysis and dashboard development.



## Key Findings

- **5 headline KPI metrics** summarize the dashboard across economic, health, environmental, trade, and technology dimensions: GDP per capita, GDP growth, trade value, health expenditure, forest area, and internet usage. The KPI cards are configured as **averages**, making them useful for comparing typical country/region performance rather than totals.

- **Regional differences are a central driver of the analysis.** A **single-select Regions slicer** is connected to the dashboard, allowing the same KPIs and charts to be examined by geographic region. This makes it possible to compare how economic, health, environmental, and digital indicators vary across regions.

- **Economic performance is represented by 2 complementary measures:** GDP per capita and GDP growth. Looking at both helps distinguish between countries/regions with high income levels and those experiencing stronger current growth.

- **Health spending is explicitly compared across regions.** The dashboard uses **Current health expenditure (% of GDP)** and displays it as an average by region, helping identify differences in the share of economic output devoted to health.

- **Internet access is used as a cross-domain indicator.** The dashboard directly tests its relationship with **2 social outcomes**: childhood immunization and youth unemployment. This moves the analysis beyond simple KPI reporting into relationship analysis.

- **The immunization analysis uses a time-aware scatter plot**, with **year** used both as the category and as the play-axis. This supports observing how the relationship between internet penetration and immunization changes over time rather than relying on a single static snapshot.

- **Youth employment is assessed through the same internet-penetration lens.** The second scatter plot compares **youth unemployment rate** with internet usage, providing a way to identify whether higher digital penetration coincides with different youth-labour-market outcomes across countries and years.

- **Health performance is ranked across regions.** The regional health chart sorts average health expenditure in descending order, making the highest- and lowest-spending regions immediately visible.

- **The dashboard spans at least 8 named indicators**: forest area, GDP growth, GDP per capita, health expenditure, trade value, internet usage, childhood immunization, and youth unemployment. This provides a multi-dimensional view of development rather than focusing on a single economic measure.

##Dashboard
<p align="center">
  <img src="Screenshot 2026-08-19 230458.png" alt="Power BI Dashboard" width="100%">
</p>
## Additional Insights Visible from the Dashboard Design

- The dashboard combines **5 KPI cards + 2 scatter plots + 1 regional comparison chart + 1 multi-year trend chart + 1 regional slicer**, creating **10 major analytical elements** on a single page.

- The analysis intentionally links **economic development, public health, environment, trade, digital adoption, and labour markets**, allowing users to look for cross-domain patterns rather than evaluating indicators independently.

- The multi-year trend visual is designed to compare indicator trajectories over time and uses **average values** for the plotted indicator series. This makes it appropriate for identifying broad regional/global direction and changes in indicator levels.

- The dashboard is particularly useful for identifying **correlation-style patterns**, but the visual relationships should not be interpreted as causal effects. For example, higher internet penetration may coincide with higher immunization or different youth unemployment levels without proving that internet access caused those outcomes.

## Metrics Covered

| Area | Metric |
|---|---|
| Economy | GDP per capita (current US$) |
| Economy | GDP growth (annual %) |
| Trade | Average trade value |
| Health | Current health expenditure (% of GDP) |
| Environment | Forest area (% of land area) |
| Technology | Individuals using the Internet (% of population) |
| Health / Social | Immunization for kids |
| Labour | Youth unemployment rate |
"""
