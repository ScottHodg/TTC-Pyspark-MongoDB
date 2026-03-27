# **TTC Delay Analysis: Big Data Pipeline using Spark & MongoDB**

**Project Scope:** Individual Project

## **Overview**

This project implements a simulated industry-style Big Data pipeline to analyze public transit efficiency in Toronto. As urban populations grow, the Toronto Transit Commission (TTC) faces significant challenges in maintaining schedule adherence. This pipeline addresses the problem of identifying high-frequency delay patterns and "recidivist" routes to support better resource allocation and commuter transparency.

## ---

**Tech Stack**

* **Runtime:** Java 11 (OpenJDK)  
* **Processing Engine:** Apache Spark 3.x / PySpark  
* **Storage:** MongoDB Atlas (NoSQL Cloud)  
* **Language:** Python 3.10  
* **Dataset:** TTC Bus Delay Data (2025–Present) via City of Toronto Open Data Portal

## ---

**Project Workflow (ETL)**

The pipeline follows a strict **Extract, Transform, Load** workflow:

1. **Data Ingestion:** Large-scale transit logs (\~69,000 records) are ingested into Apache Spark using schema inference.

2. **Cleaning & Preprocessing:** \* Standardized date formats using to\_date().

   * Removed redundant dimensions like the 'Bound' column.

   * Filtered out null records and 0-minute delays to ensure statistical accuracy.  
3. **Advanced Analytics:**  
   * **Window Functions:** Applied Window.partitionBy("Line").orderBy("Date", "Time") to sort events chronologically per route.

   * **Recidivism Detection:** Utilized the lag() function to identify "streaks" where a route or vehicle was delayed 3 or more times in succession.

4. **Distributed Storage:** Processed insights are pushed into a schema-less MongoDB database, making them ready for real-world web dashboards or mobile applications.

## ---

**Key Tasks & Analysis**

The notebook executes four primary analytical tasks:

| Task | Objective | Key Insight |
| :---- | :---- | :---- |
| **A: Daily Trends** | Avg delay per day of the week. | Sunday has the highest average delay (25.82 mins). |
| **B: Vehicle Recidivism** | Identify specific buses with repeat offenses. | Many delays are attributed to "Vehicle 0," suggesting a need for better ID tracking. |
| **C: Line Recidivism** | Identify "problem routes" with high-frequency streaks. |  **32 Eglinton West** is the most problematic route (1,640+ streaks). |
| **D: Root Cause** | Map cryptic codes to human-readable descriptions. |  **'EFO' (Other)** and **'MFSH' (Used as Shuttle)** are the top causes. |

## ---

**Findings & Insights**

* **The "Eglinton Effect":** Statistical analysis confirms that the **32 Eglinton West** route is heavily impacted by LRT construction and traffic congestion, leading to constant bus bunching.

* **Data Integrity:** A high volume of delays are labeled as "Other," suggesting the TTC could improve their tracking process to record more specific reasons for service gaps.

* **Shuttle Usage:** Route **999** shows high recidivism almost exclusively due to shuttle bus usage, indicating it acts more as a relief line than a standard service line.

## **How to Run**

1. **Environment:** Ensure Java 11 and Spark 3.x are installed.  
2. **Dependencies:** Install pyspark and the mongo-spark-connector.  
3. **Database:** Replace the mongo\_uri in the notebook with your MongoDB Atlas connection string.  
4. **Execution:** Run the Jupyter Notebook cells sequentially to process the TTC Bus Delay Data since 2025.csv and Code Descriptions.csv files.

