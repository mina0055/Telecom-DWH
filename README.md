# Telecom Data Warehouse ETL Pipeline (SSIS)

## Project Overview
This project implements an automated ETL (Extract, Transform, Load) pipeline using **SQL Server Integration Services (SSIS)** to process telecom transaction logs. The pipeline handles data cleaning, identity resolution via Lookups, and automated file management across multiple batch sources.

---

## 🏗 Pipeline Architecture

### Control Flow Logic
The process is wrapped in a **Foreach Loop Container** to ensure the system scales automatically as new CSV files are added to the source directory.


<div align="center">
  <img src="images/Screenshot 2026-04-06 154233.png" alt="Control Flow Diagram" width="700px">
  <p><i>Foreach Loop and File System Task Automation</i></p>
</div>





---

### Data Flow Logic
The core "Signal Processing" of the data includes a **Conditional Split**, **Lookup Transformation**, and **Data Conversion** bridge.

<div align="center">
  <img src="images/Screenshot 2026-04-06 154503.png" alt="Data Flow Diagram" width="850px">
  <p><i>Finalized Data Flow showing 1,770 rows processed successfully</i></p>
</div>


---

## ⚡ Technical Challenges & Engineering Solutions

### 1. Metadata Corruption & "VS_ISBROKEN" Resolution
* **The Problem:** The package encountered internal XML LineageID conflicts within the Union All component, causing validation "short circuits."
* **The Solution:** Implemented a bypass by renaming columns to `Final_ID` and rebuilding the Union All component to clear the metadata cache and restore signal integrity.

### 2. Type Safety & Normalization (Unicode Mismatch)
* **The Problem:** Mismatched "Voltage levels" (Data Types) between CSV text and SQL `nvarchar` columns.
* **The Solution:** Integrated a **Data Conversion** block to act as a level shifter, forcing all inputs into `DT_WSTR` and `DT_DBTIMESTAMP` before the final load.

---

## 🛠 Tools & Environment
* **ETL Engine:** SQL Server Integration Services (SSIS)
* **Storage:** SQL Server (T-SQL)
* **Development:** Visual Studio (SSDT)

## 📊 Results & Verification
* **Total Records Processed:** 1,770 rows
* **Status:** 100% Success (All components Green)
* **Validation:** Verified via SQL Server Management Studio (SSMS).

<div align="center">
  <img src="docs/architecture/sql_verify.png" alt="SQL Count Results" width="400px">
  <p><i>SQL verification confirming total row count</i></p>
</div>
---

## How to Run
1. Execute `/sql/create_tables.sql` to initialize the database.
2. Open the `.sln` file in Visual Studio.
3. Update **Connection Managers** to point to your local directories.
4. Press `F5`.
