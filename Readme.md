# 🛒 Retail Data Engineering Pipeline (Medallion Architecture)

## 📌 Project Overview
This project implements an end-to-end **Retail Data Pipeline** using **Databricks Delta Live Tables (DLT)** following the **Medallion Architecture (Bronze → Silver → Gold)**.

The pipeline processes retail datasets such as:
- Customers
- Products
- Orders
- Order Items
- Inventory Events (Streaming)

---

## 🏗️ Architecture

```
Raw Data (S3 / ADLS)
        ↓
   Bronze Layer (Raw Ingestion)
        ↓
   Silver Layer (Cleaned & Transformed)
        ↓
   Gold Layer (Business Insights)
        ↓
   Dashboards / Analytics
```

---

## 🥉 Bronze Layer (Raw Data)
- Ingests raw data from cloud storage (CSV/JSON)
- Minimal transformation
- Adds metadata columns like:
  - `ingestion_timestamp`
  - `source_file`

### Example Tables:
- `customers_bronze`
- `products_bronze`
- `orders_bronze`
- `order_items_bronze`

---

## 🥈 Silver Layer (Cleaned Data)
- Data cleaning & standardization
- Removes duplicates
- Handles null values
- Applies transformations:
  - Lowercasing emails
  - Trimming strings
  - Data type casting

### Features:
- Data Quality Checks (`@dlt.expect`)
- Schema enforcement
- Slowly Changing Dimensions (SCD Type 2)

### Example Tables:
- `customers_silver`
- `products_silver`
- `orders_silver`

---

## 🥇 Gold Layer (Business Insights)
- Aggregated and analytics-ready data
- Used for dashboards & reporting

### Example Tables:
- `fact_sales`
- `dim_customer`
- `dim_product`

### Sample Metrics:
- Total Revenue
- Total Orders
- Average Order Value
- Category-wise Sales

---

## 🔄 Streaming (Real-Time Processing)
Supports real-time use cases like:
- Inventory tracking
- Low stock alerts

### SLA:
- ⏱️ Latency: < 2 minutes
- ✅ Exactly-once processing

---

## 🧰 Tech Stack
- Databricks
- Delta Live Tables (DLT)
- Apache Spark (PySpark)
- Delta Lake
- AWS S3 / ADLS
- SQL for Analytics

---

## 📊 Sample Queries

### Total Revenue
```sql
SELECT SUM(total_amount) AS total_revenue
FROM gold.fact_sales;
```

### Total Orders
```sql
SELECT COUNT(order_id) AS total_orders
FROM gold.fact_sales;
```

### Average Order Value
```sql
SELECT ROUND(AVG(total_amount), 2) AS avg_order_value
FROM gold.fact_sales;
```

### Category-wise Sales
```sql
SELECT category, SUM(total_amount) AS revenue
FROM gold.fact_sales
GROUP BY category;
```

---

## 📈 Dashboard
Built using **Databricks SQL Dashboard**:
- KPI Cards (Revenue, Orders)
- Bar Charts (Category Sales)
- Filters (Date, Category)

---

## ⚙️ How to Run the Pipeline

1. Upload raw data to cloud storage (S3/ADLS)
2. Create Delta Live Tables pipeline
3. Add notebooks for:
   - Bronze Layer
   - Silver Layer
   - Gold Layer
4. Configure:
   - Storage location
   - Target schema
5. Run pipeline

---

## 🚨 Data Quality Rules
Implemented using DLT expectations:
- `customer_id IS NOT NULL`
- `order_id IS NOT NULL`
- `price > 0`

---

## 📂 Project Structure

```
retail-data-pipeline/
│
├── bronze/
│   ├── customers_bronze.py
│   ├── products_bronze.py
│
├── silver/
│   ├── customers_silver.py
│   ├── products_silver.py
│
├── gold/
│   ├── fact_sales.sql
│   ├── dim_product.sql
│
├── dashboards/
│   └── retail_dashboard.sql
│
└── README.md
```

---

## 🚀 Key Features
- End-to-end pipeline (Batch + Streaming)
- Scalable architecture
- Data quality enforcement
- SCD Type 2 implementation
- Real-time analytics capability

---

## 📚 Learning Outcomes
- Medallion Architecture
- Delta Live Tables (DLT)
- Streaming with Spark
- Data Modeling (Fact & Dimension tables)
- Dashboard creation in Databricks

---

## 👩‍💻 Author
**Bhagavathi Vyshnavi**

---

## ⭐ Future Enhancements
- Add ML-based demand forecasting
- Implement CDC (Change Data Capture)
- Integrate alerting system