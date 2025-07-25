# SQL Data Extraction and Processing Scripts
# Overview
This repository contains a collection of SQL scripts used to extract and process data from a variety of tables within the database. These scripts are designed to retrieve and aggregate data for different events and measurements across multiple time points. They are structured to handle duplicate entries, group data by unique identifiers (such as record_id), and produce clean, consolidated outputs that can be easily used for analysis.

# Purpose
Each script is designed to query specific tables and retrieve data for different events or measurements. The key operations include:

Event-Based Data Retrieval: Retrieving data for specified time points (e.g., baseline, week 10).

Handling Duplicates: Aggregating multiple entries for the same record by using functions like GROUP_CONCAT() or MAX().

Data Aggregation: Grouping data by a unique identifier (e.g., record_id) to ensure one row per identifier in the final output.

Common Fields Retrieved
Each query generally focuses on similar types of data, but may vary in terms of the specific fields and events being analyzed. Common fields include:

Measurement Fields: Such as fcc, dccs, flanker, listSort, pattComp, psm, or other task-specific metrics.

Event Fields: Data corresponding to different time events (e.g., Baseline, Week 10, Post-Treatment).

Identifiers: Typically, a unique record identifier like record_id.

# SQL Query Structure
# General Query Breakdown
Selection of Data:
Each query selects relevant columns from a specific table or dataset. The exact table and columns will depend on the data source and the required fields.

Event and Time Filtering:
A WHERE clause is used to filter rows based on specific time points (e.g., Baseline, Week 10). The exact time points are specified for each use case.

Handling Duplicates:
When multiple rows exist for the same record_id, the query ensures that duplicates are handled appropriately. Functions like GROUP_CONCAT() or MAX() are commonly used to merge or select values for each identifier.

Column Mapping:
Each event or measurement type is mapped to a separate column in the result set. For instance, baseline data will be assigned to baseline_fcc, baseline_dccs, etc., while follow-up data will go to corresponding columns like week10_fcc, week10_dccs, etc.

Example SQL Template:
```sql
SELECT 
    record_id,
    MAX(CASE WHEN time = 'Baseline' THEN vdate END) AS baseline_vdate,
    MAX(CASE WHEN time = 'Baseline' THEN fcc END) AS baseline_fcc,
    MAX(CASE WHEN time = 'Baseline' THEN dccs END) AS baseline_dccs,
    MAX(CASE WHEN time = 'Baseline' THEN flanker END) AS baseline_flanker,
    MAX(CASE WHEN time = 'Baseline' THEN listSort END) AS baseline_listSort,
    MAX(CASE WHEN time = 'Baseline' THEN pattComp END) AS baseline_pattComp,
    MAX(CASE WHEN time = 'Baseline' THEN psm END) AS baseline_psm,
    MAX(CASE WHEN time = 'Week10' THEN vdate END) AS week10_vdate,
    MAX(CASE WHEN time = 'Week10' THEN fcc END) AS week10_fcc,
    MAX(CASE WHEN time = 'Week10' THEN dccs END) AS week10_dccs,
    MAX(CASE WHEN time = 'Week10' THEN flanker END) AS week10_flanker,
    MAX(CASE WHEN time = 'Week10' THEN listSort END) AS week10_listSort,
    MAX(CASE WHEN time = 'Week10' THEN pattComp END) AS week10_pattComp,
    MAX(CASE WHEN time = 'Week10' THEN psm END) AS week10_psm
FROM your_table_name
WHERE time IN ('Baseline', 'Week10')
GROUP BY record_id;
```

# Key Notes:
Event Filtering: Adjust the WHERE clause to filter by the appropriate time events, such as 'Baseline', 'Week10', 'FollowUp', etc.

Duplicate Handling: Use GROUP_CONCAT() for cases where multiple values exist for the same event, or MAX() to select one value when duplicates should be discarded.

Grouping by ID: The query groups by record_id to ensure the data is returned as a single row per record identifier.

### Example Output

| record_id | baseline_vdate | baseline_fcc | baseline_dccs | baseline_flanker | baseline_listSort | baseline_pattComp | baseline_psm | week10_vdate | week10_fcc | week10_dccs | week10_flanker | week10_listSort | week10_pattComp | week10_psm |
|-----------|----------------|--------------|---------------|------------------|-------------------|------------------|--------------|--------------|------------|-------------|----------------|-----------------|-----------------|------------|
| 1         | 2025-02-05     | 5.1          | 3.2           | 7.4              | 8.1               | 6.3              | 5.5          | 2025-05-15   | 5.4        | 3.0         | 7.3            | 8.2             | 6.2             | 5.7        |
| 2         | 2025-02-06     | 5.0          | 3.3           | 7.5              | 8.0               | 6.4              | 5.6          | 2025-05-16   | 5.5        | 3.1         | 7.4            | 8.3             | 6.3             | 5.8        |


Changelog
Version 1.0: Initial script version to retrieve event-based data for Baseline and Week 10 events, with handling for duplicates and aggregation.

Version 1.x: Updates to handle additional time events (e.g., FollowUp), added support for more measurement types, and minor optimizations for data processing.

How to Use
Clone the Repository: First, clone the repository to your local machine or server.

```bash
git clone https://github.com/yourusername/sql-data-extraction.git
```
Set Up the Database Connection: Ensure that your SQL client is connected to the appropriate database that contains the required tables.

Run Queries: Select the appropriate SQL query depending on the event or measurement data you're interested in. Adjust the time field in the WHERE clause to match your needs (e.g., 'Baseline', 'Week10', 'FollowUp').

Modify the Query as Needed: If you need to adjust the fields or events, simply modify the SQL query by changing the SELECT fields, WHERE conditions, or GROUP BY clauses.
