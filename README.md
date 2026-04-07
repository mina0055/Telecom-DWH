# Telecom Data Warehouse ETL Pipeline (SSIS)

## Project Overview
This project implements an automated ETL (Extract, Transform, Load) pipeline using **SQL Server Integration Services (SSIS)** to process telecom transaction logs. The pipeline handles data cleaning, identity resolution via Lookups, and automated file management across multiple batch sources.

---

## 🏗 Pipeline Architecture

### Control Flow Logic
The process is wrapped in a **Foreach Loop Container** to ensure the system scales automatically as new CSV files are added to the source directory.

![Control Flow Diagram](docs/architecture/control_flow.png)
> *Placeholder: Insert screenshot of your Foreach Loop with the File System Task.*



---

### Data Flow Logic
The core "Signal Processing" of the data includes a **Conditional Split**, **Lookup Transformation**, and **Data Conversion** bridge.

![Data Flow Diagram](docs/architecture/data_flow.png)
> *Placeholder: Insert your final green 1,770-row Data Flow screenshot here.*



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

![SQL Count Results](docs/architecture/sql_verify.png)
> *Placeholder: Insert a screenshot of your SQL query results showing the 1,770 rows.*

---

## How to Run
1. Execute `/sql/create_tables.sql` to initialize the database.
2. Open the `.sln` file in Visual Studio.
3. Update **Connection Managers** to point to your local directories.
4. Press `F5`.
