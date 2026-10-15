# Zara Product Sales Intelligence

Business Report \| 252 products, single catalog snapshot scraped 19 Feb 2024

## Executive Summary

This report compares the Sales Volume indicator for 252 Zara products across product type, price band, promotion status, and shelf position. The main finding is that catalog mix explains most of the pattern. Jackets lead total volume at 56.5% and make up 55.6% of products. Average volume per product stays between 1,665 and 1,868 for every product type, and the other comparisons show small gaps too.

**These are descriptive patterns in one snapshot. They are not evidence that the patterns will persist. Confidence in the direction of the findings is moderate. Confidence in any claim about cause is low.**

## Key Findings

| **Comparison**    | **Result**                                                                                                                                      | **Reading**                                                                                                                              |
|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| **Product type**  | Jackets: 259,468 units (56.5%) from 140 products. Average per product: shoes 1,868, jackets 1,853, sweaters 1,835, t-shirts 1,676, jeans 1,665. | Totals follow catalog size. Types perform alike per product.                                                                             |
| **Price band**    | 50-100: 107 products, 200,553 units (43.6%), average 1,874. Over 200: 4 products, average 1,515. Price and volume rank correlation: -0.03.      | The 50-100 band beats the catalog average (1,824) by about 3%. Price shows no real link to volume.                                       |
| **Promotion**     | Promoted (120 products) average 1,827. Not promoted (132) average 1,821.                                                                        | No clear overall difference. The 6-unit gap cannot establish a causal effect. By type the direction flips: sweaters +371, t-shirts -222. |
| **Position**      | Front of Store average 1,873 (69 products). Aisle 1,829 (97). End-cap 1,778 (86). Aisle holds the most total volume (177,396).                  | Best and worst average differ by about 5%. Aisle has the highest total volume, largely alongside its higher product count.               |
| **Concentration** | The top 26 products (10%) deliver 16.3% of volume. The top five sit between 2,942 and 2,989.                                                    | Volume is not heavily concentrated in the top 10% of products.                                                                           |

## Business Recommendation

Do not change pricing, promotions, or placement on this evidence. Make the 50-100 price band the first segment to test with real sales data. It is the only comparison where the same group leads on both total volume (43.6%) and average per product (1,874). The lead is small, so treat it as a priority to test and not as proof. Compare the band against the others on actual unit sales and margin through the next season, and do not assume the proxy predicts them.

## Data Limitations
- **Single snapshot.** All rows come from one scrape on 19 Feb 2024. The data shows no trends, growth, or before-and-after effects.
- **Sales Volume is a proxy.** It is a scraped indicator, not an audited transaction count.
- **Fields absent from the source.** The source never contained orders, customers, regions, or product costs. Profit and margin cannot be calculated.
- **Missing values.** Two descriptions and one product name are blank. This is minor and does not affect the volume comparisons.
- **Classification issues.** Product type comes from the source field and was not corrected. SUIT JACKET IN 100% LINEN sits under t-shirts, and 58 product names repeat across different IDs.
- **Small groups.** Jeans has 8 products, the over-200 band has 4, and the WOMAN section has 34 of 252.

## Next Steps
- **Obtain** transaction-level data with dates, units, and store or region, to replace the proxy.
- **Add** product cost, to compare margin by price band.
- **Add** a promotion calendar and price history, to compare the same products before and during promotion.
- **Collect** placement history, to test Front of Store with a controlled comparison.
- **Review** the product-type labels, correct them, and rerun the type comparison.
- **Repeat** the snapshot at intervals to see whether the 50-100 lead holds.

## Methodology and Tools

PostgreSQL and SQL staged, cleaned, and analyzed the data. Excel checked totals independently. Power BI built the dashboard. Total Sales Volume (459,573) matched across all three. The full workflow is documented in the README.
