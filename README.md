# Conversational-Search-Query-Benchmark-Based-on-ESCI-S

This repository provides a complementary evaluation dataset derived from the **Amazon ESCI (Shopping Queries)** dataset and its structured-metadata extension **ESCI-S**. 

## Overview

We provide two files:

1) **Synthetic prices** for missing-price products in ESCI-S-derived US Cell Phones & Accessories subset  
2) **Conversation-style test queries** (including a label indicating whether each query is original ESCI text or an enhanced rewrite)

These files are designed to be used together with the original ESCI / ESCI-S datasets. Product metadata (title, category, rating etc.) should be taken from ESCI-S and joined using `asin`.


## Source Datasets

We leverage the **Amazon ESCI (Shopping Queries)** dataset, which contains query–product relevance annotations across a large catalog, and the extended version **ESCI-S**, which augments the catalog with structured metadata (e.g., price, ratings) for a large subset of products.

From ESCI-S, we select a domain-specific catalog of roughly **22K products** belonging to **Cell Phones & Accessories** (US locale). This catalog serves as the evaluation set for retrieval methods on attribute-rich queries.


## File 1: Synthetic prices for products with missing prices

**Filename:** `products_cell_phones_accessories.csv`

### Purpose
ESCI-S provides structured metadata including `price` for many products, but not all products have price values. For products with missing prices, we generate synthetic prices using an open-weight LLM based on product information (e.g., title, description) to improve coverage for evaluation scenarios that include price constraints.

### How to use
Join this file with ESCI-S product metadata via `asin`. Use `price_synthetic` where the original ESCI-S `price` is missing.

### Schema
| column | type | description |
|---|---:|---|
| `asin` | string | Product identifier shared with ESCI / ESCI-S |
| `price_synthetic` | float or empty | Synthetic price only for products whose original `price` is missing; empty otherwise |


## File 2: Conversation-style test queries with exact product matches

**Filename:** `queries_cell_phones_accessories.csv`

### Purpose
We create a challenging evaluation set by rewriting a subset of ESCI queries into more natural, conversation-style queries enriched with additional constraints (e.g., rating/review thresholds). These constraints deliberately narrow the set of products that satisfy each query.

### Construction summary
- We extract query–product pairs from ESCI that involve ~22K US Cell Phones & Accessories catalog.
- We keep pairs labeled as **exact matches** in ESCI.
- We randomly sample **151 queries** and rewrite most into enriched natural-language queries.

### Query counts
- **151 unique queries**
  - **146 enhanced** (rewritten with additional constraints)
  - **5 original ESCI queries** to test robustness across query complexity

### Schema
| column | type | description |
|---|---:|---|
| `example_id` | int | **ESCI** example identifier for the original query–product judgment used in our sample |
| `query_id` | int | **ESCI** query identifier |
| `product_id` | string | **ESCI** product identifier (ASIN) corresponding to an **exact-match** judgment for that query |
| `query` | string | The final query used for evaluation (enhanced rewrite or original) |
| `query_source` | string | `"enhanced"` or `"original_esci"` |


- The `query_source` label indicates whether the text is an **enhanced rewrite** or the **original ESCI query text**.


## Citations

This repository is a complementary dataset built on top of ESCI and ESCI-S:

- ESCI: Reddy et al., Shopping Queries Dataset: A Large-Scale ESCI Benchmark for Improving Product Search, arXiv:2206.06588 (2022).
- ESCI dataset release: Amazon Science, `amazon-science/esci-data`.
- ESCI-S metadata augmentation: `shuttie/esci-s`.


## License

This project is licensed under the Apache-2.0 License.
