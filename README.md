# DecodeLabs Data Analytics Internship - Project 1: Data Cleaning & Preparation

## 📌 Project Overview
This project represents the foundational phase of the DecodeLabs Data Analytics program. The main objective was to take a raw, unverified transactional dataset (`Dataset for Data Analytics`) and transform it into a highly reliable, error-free "source of truth." 

By executing a structured data integrity audit, data quality anomalies such as duplicates, loose text formats, missing codes, and misaligned data types were resolved using **Power BI (Power Query Editor)**.

---

## 🛠️ Data Cleaning Architecture & Methodology

Every transformation step was recorded in Power Query to ensure reproducibility. Below is the documentation of the sequential cleaning pipeline:

### 1. Unique Record Integrity Audit
* **Issue:** Potential risk of inflated transactional metrics due to duplicated unique identifiers.
* **Action:** Performed a global deduplication strictly targeted on the primary key column: `OrderID`.
* **Power Query Logic:** `Table.Distinct(#"Changed Type", {"OrderID"})`
* **Result:** Verified that every row represents a single, non-repeatable transaction, yielding a 0% error rate on primary identifiers.

### 2. Temporal Standardization
* **Issue:** Date tracking entries were recognized as plain text or loose formats, which stops time-series analytics.
* **Action:** Converted the `Date` column datatype to standard **Date**.
* **Power Query Logic:** Changed type to `Date` ensuring the output strictly adheres to clean chronological formatting.

### 3. Missing Value Strategy (Strategic Imputation)
* **Issue:** Blank or `null` data structures present in the `CouponCode` column.
* **Action:** Instead of deleting records (which reduces statistical power), missing coupon values were replaced with a placeholder to indicate organic sales.
* **Power Query Logic:** Replaced `null` values with `"NO_COUPON"` (or structural defaults like `"Direct"` for referral sources where applicable).

### 4. Text Whitespace Normalization
* **Issue:** Trailing and leading invisible whitespaces in text columns (e.g., `"Phone "` vs `"Phone"`), causing errors in future categorical groupings.
* **Action:** Applied a strict text-trim routine across structural categorical columns like `Product` and `PaymentMethod`.
* **Power Query Logic:** `Table.TransformColumns(..., {{"Product", Text.Trim, type text}})`
* **Result:** Removed all hidden formatting anomalies.

### 5. Automated Calculation Verification
* **Issue:** Need to mathematically prove that financial reporting parameters are absolutely accurate before moving to the analytics phase.
* **Action:** Created a validation layer by implementing a **Custom Column** (`Validation_Total_Price`) to mathematically test the dataset's calculation consistency.
* **Verification Formula:** $$\text{Calculated Gross Total} = \text{UnitPrice} \times \text{Quantity}$$
* **Result:** Confirmed total transaction validity against the pre-existing `TotalPrice` field, locking down mathematical correctness across all database entries.
