# Zara Product Sales Intelligence

## Project Overview

This project analyzes a scraped Zara product catalog using SQL, PostgreSQL, Excel, and Power BI. It compares product-level sales volume indicators across product type, price band, product placement, and promotion status.

The goal is to turn imperfect raw data into clear business findings. Every assumption and limitation is documented.

## Dataset

·     Source: [https://www.kaggle.com/datasets/xontoloyo/data-penjualan-zara](https://www.kaggle.com/datasets/xontoloyo/data-penjualan-zara)

·     Records: 252 product records (252 unique product IDs, 194 unique product names)

·     Data type: Single snapshot of a product catalog. All 252 rows come from one scrape window on 19 Feb 2024.

·     Prices: USD, ranging from 7.99 to 439.00, with a median of 79.90

·     Primary metric: Sales Volume indicator, ranging from 529 to 2,989 per product

·     Mix: 218 products in the MAN section and 34 in the WOMAN section

## Data Limitations

Sales Volume is a proxy metric provided in the scraped dataset. It is not an audited transaction count. The dataset is one snapshot. It has no customer records, individual orders, transaction dates, regional information, or product costs.

This analysis cannot establish customer behavior, historical sales growth, regional performance, or actual profit margins.

Other issues found during validation:

·     58 product names repeat across different product IDs (for example, BOMBER JACKET appears three times with different volumes). Product name is not a unique key. Use product ID.

·     Some products carry a product type that does not match their name. SUIT JACKET IN 100% LINEN is tagged t-shirts, and BOUCLÉ TEXTURED JACKET appears under both jackets and t-shirts. Product type comes from the source terms field and was not corrected.

·     Jeans has only 8 products, and the over-200 price band has only 4. Averages for these groups are unstable.

·     The WOMAN section has 34 products against 218 for MAN, so gender comparisons are weak.

·     Two descriptions and one product name are missing.

## Tools Used

·     PostgreSQL 18

·     SQL

·     Microsoft Excel

·     Microsoft Power BI

## Project Workflow

1.   Inspect and validate the raw dataset.

2.   Import and clean the data using PostgreSQL.

3.   Analyze product-level patterns using SQL.

4.   Validate results against independent totals.

5.   Build an interactive Power BI dashboard.

6.   Document findings, limitations, and recommendations.

## Dashboard

The dashboard shows five headline metrics: 5 product types, 47.62% of products on promotion, 459,573 total sales volume, 252 products, and an average of 1.82K per product. Charts break sales volume down by product type, price band, and product position. A table lists the five highest-volume products. The pie chart filters by product type, while the promotion slicer filters by promotion status.

## SQL Scripts

|File|Purpose|
|---|---|
|sql/zara_sql.sql|Full workflow: staging table, validation checks, clean table, price bands, and analysis queries|

The script runs in this order:

1.   **Staging.** staging_zara stores every column as text, so the raw import never fails on a bad value.

2.   **Validation.** Row counts, null counts, duplicate product IDs, and distinct values for category, position, and seasonal flags.

3.   **Cleaning.** zara_products has typed columns. Yes/No flags become booleans. Price and sales volume are stripped of non-numeric characters and cast to numbers. Rows without a product ID are dropped.

4.   **Price bands.** A price_band column holds <50, 50-100, 100-150, 150-200, and >200.

5.   **Analysis.** Sales by product type, position, promotion, and price band. Top product per type. Top 10% of products by volume. Average volume by price band and promotion.

To run it:

1.   Start PostgreSQL and create a database.

2.   Run the CREATE TABLE staging_zara statement.

3.   Load data/Zara_formatted.csv into staging_zara. The CSV has extra columns (brand, url, sku, currency) that the staging table omits, so map columns by name when importing. terms loads into product_type and section loads into gender.

4.   Run the rest of the script in order and compare staging_rows with clean_rows (both should be 252).

## Business Report

[Reports/Zara_Product_Sales_Intelligence.md](https://github.com/JoshuaSebastinSantosh/zara-sales-intelligence/blob/554b3995a3b7f7d41239b9d1e733c6f3600aabe4/Reports/Zara_Product_Sales_Intelligence.md)

## Key Findings

All figures below were recalculated from Zara_formatted.csv and match the dashboard.

**1. Jackets lead total volume, and the catalog is full of jackets.** Jackets hold 259,468 units, or 56.5% of total volume. They make up 140 of 252 products (55.6%). The shares are almost equal, so the pie chart reflects catalog mix and not stronger per-product demand.

**2. Average volume per product is nearly flat across types.**

|Product type|Products|Total volume|Share|Avg per product|
|---|---|---|---|---|
|Shoes|31|57,906|12.6%|1,868|
|Jackets|140|259,468|56.5%|1,853|
|Sweaters|41|75,242|16.4%|1,835|
|T-shirts|32|53,637|11.7%|1,676|
|Jeans|8|13,320|2.9%|1,665|

The gap between best and worst type is about 200 units per product.

**3. The 50-100 price band sells the most in total and per product.** It holds 107 products, 200,553 units (43.6%), and the highest average at 1,874. Volume per product falls in the higher bands: 1,723 for 150-200 and 1,515 for over 200. The over-200 band has only 4 products, so treat that figure with care. The rank correlation between price and sales volume is -0.03, which means price has almost no linear link to volume.

**4. Promotion shows no clear volume lift.** Promoted products average 1,827 units and non-promoted products average 1,821. The 6-unit gap is noise. By type, sweaters average 2,043 on promotion against 1,672 without. T-shirts show the opposite, at 1,558 against 1,780. No single pattern holds across the catalog.

**5. Placement differences are small.** Aisle has the most total volume (177,396, or 38.6%) and the most products (97). Front of Store has the highest average (1,873) and the highest median (2,067) on the fewest products (69). End-cap has the lowest average (1,778).

**6. Sales are spread evenly across products.** The top 10% of products (26 items) deliver 16.3% of volume. There is no sharp concentration in a few hit products. The five highest are PLAID TIE DYE OVERSHIRT (2,989), SUIT JACKET IN 100% LINEN (2,985), RIB COLLAR JACKET (2,973), CONTRASTING COLLAR JACKET (2,968), and DOUBLE FACED JACKET (2,942). Together they total 14,857 units.

## Conclusion

Within this snapshot, the volume indicator is driven mostly by how many products Zara lists in each type, not by price, promotion, or placement. Jackets dominate the totals and the range. Per-product averages sit in a narrow band of about 1,500 to 1,900 across almost every cut.

The main recommendation is to avoid reading the headline pie chart as proof that jackets outperform other types. Compare types on average volume per product instead. The most consistent signal is that the 50-100 price band performs best, and it deserves a closer test with real sales data.

Three conclusions are not supported by this dataset: that promotions raise sales, that front-of-store placement raises sales, and that high-priced items sell worse. Each needs transaction-level data with dates, a promotion calendar, and store placement history to test properly.

## Repository Structure

zara/  
├── README.md  
├── data/  
│   ├── Zara.xlsx  
│   └── Zara_formatted.csv 
├── Reports/Zara_Product_Sales_Intelligence.md
├── sql/zara_sql.sql  
└── powerbi/ zara.pbix
