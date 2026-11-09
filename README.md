# StockWise: Multi-Branch Retail Inventory & Automated Reorder System

**Course Code & Name:** INSY 8311 & Database Development with PL/SQL  
**Instructor:** Eric Maniraguha  
**Student Name:** Rugira Gyslain  
**Student ID:** 20251IMA019  
**Academic Year:** 2026-2027 (Semester I)  
**Institution:** Adventist University of Central Africa (AUCA)  

---

## 1. Problem Statement
In developing economic contexts like Rwanda, the wholesale and retail trade sector accounts for 53.5% of all commercial establishments, yet suffers from extreme informality rates (between 87% and 91.9%) and heavy geographic concentration. Small and medium retail enterprises rely predominantly on manual record-keeping or static spreadsheets, leading to severe supply chain vulnerabilities. National business evaluations report that 50.9% of commercial enterprises experience operational capacity underutilization caused by unoptimized replenishment schedules and input supply bottlenecks. 

Without real-time relational inventory tracking, retail businesses face systemic operational failures:
* **Revenue Losses from Stockouts:** Out-of-stock events during peak demand windows cause immediate lost sales and customer dissatisfaction.
* **Tied-Up Working Capital:** Overstocking slow-moving or obsolete items locks up vital operating capital.
* **Human Error & Procurement Delays:** Manual tracking fails to evaluate supplier lead times ($L$) and demand variability ($\sigma_d$), resulting in delayed purchase requisitions.

StockWise addresses these inefficiencies by embedding deterministic inventory control equations directly into a server-side Oracle Database architecture powered by PL/SQL.

---
## 2. Objectives
### Main Objective
To design, implement, and deploy a centralized relational Oracle Pluggable Database (PDB) powered by automated PL/SQL stored logic to optimize inventory replenishment and eliminate retail stockouts across store branches[cite: 5, 7, 8].

### Specific Objectives
1. **Relational Data Normalization:** Transform flat transactional sales data into a Third Normal Form (3NF) database schema enforcing strict entity-relationship constraints[cite: 5].
2. **Automated Reorder Triggering:** Develop PL/SQL stored packages, functions, and triggers to dynamically track inventory balances and automatically generate pending purchase requisitions when stock drops below safety thresholds[cite: 5, 7, 8].
3. **Data Integrity & Audit Logging:** Implement database-level exception handlers and autonomous audit triggers to log all stock adjustments and prevent sales of out-of-stock items[cite: 5, 9].
4. **Interactive Innovation Layer:** Connect a real-time Power BI analytics dashboard to the Oracle Database to provide management with instant visibility into inventory health, stockout risks, and branch sales trends[cite: 7, 9].

## 3. Data Source
* **Dataset Name:** Online Retail Transactional Dataset
* **Source / Platform:** UCI Machine Learning Repository[cite: 7]
* **Dataset Link:** [UCI Machine Learning Repository - Online Retail](https://archive.ics.uci.edu/dataset/352/online+retail)
* **Record Count & Scope:** Contains over 500,000 line-item transactions capturing product SKUs, descriptions, quantities, timestamps, unit prices, customer IDs, and geographic locations.
* **Licensing:** Public Domain / Open Educational & Research License (No PII)[cite: 7].

## 4. Innovation Layer
* **Option Chosen:** Analytics Dashboard (Power BI / Excel Power Query)[cite: 9]
* **Implementation Plan:** 
  The project builds a live **Power BI Analytics Dashboard** connected directly to the Oracle Database instance via Oracle Native Client / ODBC[cite: 5, 9]. 
* **Key Features:**
  * **Real-time Low-Stock Alerts:** Displays items whose current stock level is below the reorder threshold[cite: 7, 9].
  * **Pending Reorder Status:** Visualizes active purchase requests generated automatically by PL/SQL triggers[cite: 5, 7, 8].
  * **Sales & Turnover Metrics:** Tracks top-selling product categories and daily sales revenue by customer location[cite: 5].

## 5. Planned Database Schema (3NF Tables)
1. **`CUSTOMERS`**: Stores customer identity and country metrics (`Customer_ID`, `Customer_Name`, `Country_Name`, `Created_Date`)[cite: 5].
2. **`SUPPLIERS`**: Stores procurement parameters and vendor fulfillment metrics (`Supplier_ID`, `Supplier_Name`, `Contact_Email`, `Lead_Time_Days`, `Is_Active`)[cite: 5].
3. **`PRODUCTS`**: Stores inventory master records (`Product_ID`, `Supplier_ID`, `Stock_Code`, `Description`, `Unit_Price`, `Current_Stock_Level`, `Reorder_Level`, `Safety_Stock`, `Is_Active`)[cite: 5].
4. **`ORDERS`**: Captures header details for customer sales transactions (`Order_ID`, `Customer_ID`, `Invoice_Number`, `Order_Date`, `Total_Amount`, `Order_Status`)[cite: 5].
5. **`ORDER_ITEMS`**: Captures line-item transaction records (`OrderItem_ID`, `Order_ID`, `Product_ID`, `Quantity`, `Unit_Price`, `Subtotal`)[cite: 5].
6. **`REORDER_REQUESTS`**: Manages automated purchase requisitions (`Reorder_ID`, `Product_ID`, `Supplier_ID`, `Quantity_Requested`, `Request_Date`, `Approved_Date`, `Status`)[cite: 5].
7. **`INVENTORY_AUDIT_LOG`**: Tracks historical stock modifications and price updates (`Audit_ID`, `Product_ID`, `Old_Quantity`, `New_Quantity`, `Modified_By`, `Timestamp`)[cite: 5].

## 6. Planned PL/SQL Features
* **Stored Package (`INVENTORY_MGMT_PKG`):** Groups transactional procedures and functions to handle sales execution, restock validation, and dynamic inventory calculations within the database engine[cite: 5, 6, 9].
* **Functions:**
  * `FN_CALCULATE_REORDER_POINT`: Evaluates 30-day average daily sales, supplier lead times, and safety stock levels to calculate dynamic reorder points[cite: 8, 9].
  * `FN_GET_TOTAL_SALES`: Calculates cumulative sales revenue for specific items or date ranges[cite: 5].
* **Procedures:**
  * `PROCESS_SALE`: Accepts bulk order item arrays using `FORALL` execution to process sales, update stock balances, and write invoice headers[cite: 5, 9].
  * `REPLENISH_STOCK`: Increases product inventory balances upon shipment receipt and updates pending reorder requests to 'FULFILLED'[cite: 5].
* **Triggers:**
  * `TRG_BEFORE_ORDER_ITEM_INSERT`: A `BEFORE INSERT` trigger that checks available stock and raises custom exception `EXC_INSUFFICIENT_STOCK` if requested quantity exceeds current inventory[cite: 5].
  * `TRG_CHECK_REORDER_LEVEL`: An `AFTER UPDATE` autonomous trigger on `PRODUCTS` that automatically inserts a row into `REORDER_REQUESTS` when stock drops below `Reorder_Level`[cite: 5, 7, 8, 9].
  * `TRG_AUDIT_STOCK_CHANGE`: Intercepts modifications to inventory balances and records user ID and execution timestamps in `INVENTORY_AUDIT_LOG`[cite: 5].
* **Explicit Cursors:** Implements explicit cursors (`c_pending_reorders`) with `%FOUND`, `%ROWCOUNT`, and `%NOTFOUND` attributes for batch reorder processing[cite: 9].

## 7. References (APA)
* Dania, A., & Adediran, A. (2025). Genetic Algorithm Optimization for Inventory Control of Deteriorating Retail Goods. *Pesquisa Operacional*, 45, 1-18[cite: 4].
* Mishra, A., & Mohapatra, M. (2026). POSventory: An Integrated Inventory Management and Point of Sale Platform. *International Journal of Sciences and Innovation Engineering*, 3(4), 1775-1772[cite: 8].
* National Institute of Statistics of Rwanda (NISR). (2024). *Integrated Business Enterprise Survey (IBES) 2023 Report*. NISR Publications[cite: 3].
* Oracle Corporation. (2026). *Oracle Database PL/SQL Language Reference, 19c (E96448-09)*. Oracle Help Center[cite: 6].
* UCI Machine Learning Repository. (2015). *Online Retail Transactional Dataset*. UCI ML Repository.
* Zoho Corporation. (2026). *Zoho Inventory System Features and Architecture Documentation*. Zoho Official Documentation[cite: 11].