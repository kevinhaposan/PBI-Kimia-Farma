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
[Script SQL](
https://github.com/kevinhaposan/PBI-Kimia-Farma/blob/ab246fa6331c12192817250828bfb522f9a710cd/Query%20SQL%20Final%20Task.txt)
