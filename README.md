### Project Based Virtual Intern: Kimia Farma x Rakamin Academy

# **Kimia Farma - Big Data Analytics**

## About Company
Kimia Farma is the first pharmaceutical industry company in Indonesia, established by the Dutch East Indies government in 1817. In 1958, the Government of the Republic of Indonesia merged several pharmaceutical companies into PNF (Perusahaan Negara Farmasi) Bhinneka Kimia Farma. Then on August 16, 1971, the legal form of PNF was changed to Perseroan Terbatas, thus changing the company's name to PT Kimia Farma (Persero).

Kimia Farma's main business areas include Pharmaceutical Manufacturing supported by Research and Development; Distribution and Trading; Marketing; Pharmacy Retail; Clinical Laboratories and Health Clinics. Almost all therapy classes are accommodated by the company's products, consisting of more than 385 product items marketed throughout Indonesia and exported to several countries.

## About Project
Analyzing product sales insights at Kimia Farma is the task of a Data Analyst. In this project, I am tasked with delving into the available data and evaluating the sales of Kimia Farma's products. The data provided for analysis includes transaction data, inventory data, branch office data, and product data. From this analysis, the aim is to evaluate Kimia Farma's business performance and enhance product sales.

## Importing Dataset to BigQuery
![image](https://github.com/kevinhaposan/About-Me/assets/156397084/ab876d85-eb38-41e9-a812-82cbc2239ff2) <br>
In this section, I named the project segment as Rakamin-KF-Analytics. As for the dataset section, I labeled it as 'kimia_farma'. Following the dataset naming, I imported the four provided tables for analysis into BigQuery, and the results are now accessible in the results section.

## Analysis Table
![image](https://github.com/kevinhaposan/About-Me/assets/156397084/078a3fc9-730c-4741-82e1-0037e1b83268) <br>
In a schema, there is a **fact table** that acts as the main table and **dimensional tables** that support the fact table. In this project, **kf_final_transaction acts as the fact table**. The tables acting as **dimensional tables are kf_kantor_cabang, kf_inventory, and kf_product**.

In kf_final_transaction, there is a column named transaction_id, which acts as the identifier for rows in the table. The kf_final_transaction table has branch_id and product_id columns, which will function as the join keys to the dimensional tables. The branch_id column in kf_final_transaction will join with the branch_id column in the kf_kantor_cabang table. The product_id column in the kf_final_transaction table will join with the product_id column in both the kf_product and kf_inventory tables.

This data clearly originated from duplicates and null values

## **BigQuery Syntax**
```sql
WITH tmp1 AS (
  SELECT ft.transaction_id,
         ft.price,
         ft.discount_percentage,
         CASE
          WHEN ft.price <= 50000 THEN 0.05
          WHEN ft.price <= 100000 THEN 0.15
          WHEN ft.price <= 300000 THEN 0.20
          WHEN ft.price <= 500000 THEN 0.25
          ELSE 0.30
        END AS persentase_gross_laba,
        (ft.price - (ft.price * ft.discount_percentage)) AS nett_sales,
  FROM `kimia_farma.kf_final_transaction`AS ft
),

tmp2 AS (
  SELECT tmp1.transaction_id,
         tmp1.nett_sales,
         tmp1.persentase_gross_laba,
         (tmp1.nett_sales*(tmp1.persentase_gross_laba)) AS nett_profit
  FROM tmp1
)

SELECT ft.transaction_id,
       ft.date,
       ft.branch_id,
       kc.branch_name,
       kc.kota,
       kc.provinsi,
       kc.rating AS rating_cabang,
       ft.customer_name,
       ft.product_id,
       inv.product_name,
       pro.price AS actual_price,
       ft.discount_percentage,
       tmp1.persentase_gross_laba,
       tmp1.nett_sales,
       tmp2.nett_profit,
       ft.rating AS rating_transaksi
FROM `kimia_farma.kf_final_transaction`AS ft
LEFT JOIN `kimia_farma.kf_kantor_cabang`AS kc
      ON ft.branch_id = kc.branch_id
LEFT JOIN `kimia_farma.kf_inventory`AS inv
      ON  ft.product_id = inv.product_id
LEFT JOIN kimia_farma.kf_product AS pro
      ON ft.product_id = pro.product_id
LEFT JOIN tmp1
      ON ft.transaction_id = tmp1.transaction_id
LEFT JOIN tmp2
      ON ft.transaction_id = tmp2.transaction_id
```

This script utilizes Common Table Expressions (CTEs). In the first CTE, the 'transaction_id' column serves as the join key to the main query. Subsequently, I segmented the price and stored this segmentation as 'persentase_gross_laba'. The calculation for 'nett_sales' is based on the following equation:
ft.price - (ft.price * ft.discount_percentage)

In the second CTE, I also implemented a CTE using the 'transaction_id' column as the join key to the main query. Following this, I computed 'nett_profit' using the equation:
tmp1.nett_sales * tmp1.persentase_gross_laba

After creating the two CTEs, a primary query was formulated to extract the necessary columns for business analysis. These columns include 'transaction_id', 'date', 'branch_id', 'branch_name', 'kota', etc. (as specified in the SELECT section of the SQL script). The 'kf_final_transaction' serves as the main table and fact table, while three additional tables and two CTEs are joined using the specified join keys.

This structure enables comprehensive analysis of business data
