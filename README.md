# HNG Stage 1 — E-Commerce Product Data Cleaning & Title Optimisation
Cleaned 3,847 raw product records from a multi-category e-commerce dataset, removed 306 duplicate entries, resolved data quality issues across six dimensions, and engineered a `short_title` feature reducing average title length by 55% for improved SEO and readability — all in Excel.

**[Cleaned Dataset]()** · **[Technical Report]()**
## The Problem
The raw dataset had 3,847 product listings and no usable title column. Titles averaged 86 characters — the longest ran to 403. They were stuffed with keywords, model codes, colour variants, and filler phrases like "set of," "includes," and "features." Useless for SEO. Impossible to scan.

Beyond titles, the data had five other issues that would have broken any downstream analysis:
- **306 duplicate rows** — every duplicate product_id had a matching duplicate title, pointing to systematic re-entry rather than legitimate product variants
- **41.4% of BULLET_POINTS were blank** — 1,591 rows with no feature descriptions
- **55.7% of DESCRIPTION was missing** — 2,144 rows with no product copy
- **Inconsistent column naming** — `PRODUCTID`, `TITLE`, and `BULLET_POINTS` in uppercase; `ProductLength` in mixed case; no standard convention applied across the file
- **Outlier product lengths** — `ProductLength` peaked at 96,000 against a 75th percentile of 1,024, distorting any length-based analysis

No data team had mapped these issues. No short title existed. That's what this project built.

## What I Did
This was an HNG Internship Stage 1 task — the brief gave a dataset, a tool constraint (Excel only), and a problem to solve. I worked through it end to end: explored the structure, profiled every column, documented every quality issue, applied fixes, and engineered the `short_title` feature from scratch using Excel formulas.

## Tools & Concepts
`Microsoft Excel` · `Data Cleaning` · `Feature Engineering` · `Outlier Detection & Capping` · `Missing Value Analysis` · `Duplicate Detection` · `Column Standardisation` · `SEO Title Optimisation` · `Descriptive Statistics` · `Data Quality Documentation`

## The Cleaning Decisions That Mattered
### Duplicate Detection — 306 Rows, Not a Sampling Issue
The dataset had 306 duplicate `PRODUCTID` values — and every one of them had a matching duplicate `TITLE`. That pattern rules out variant listings or accidental ID reuse. It points to a systematic re-entry problem: the same batch of products was imported twice. Removing on `PRODUCTID` was the correct call. Final row count after deduplication: **3,541**.

### Column Naming — Standardised Before Any Formula Work
The raw file had no consistent naming convention. `PRODUCTID`, `TITLE`, `BULLET_POINTS`, and `DESCRIPTION` were uppercase; `ProductLength` was mixed case; `PRODUCTTYPEID` was all-caps but multi-word. Before building any formulas, column names were standardised to lowercase (`product_id`, `title`, `bullet_points`, `description`, `product_type_id`, `product_length`) — the convention expected by SQL and Python pipelines downstream.

### Missing Values — Document, Don't Impute
`BULLET_POINTS` (41.4% missing) and `DESCRIPTION` (55.7% missing) are marketing content fields. There were blank spaces in them, so I put a placeholder text, which is the standard for blank space, `NaN`.
### Outlier Capping on `ProductLength`
The raw `ProductLength` column had a mean of 1,151 and a max of 96,000. One value sat 35× above the 75th percentile. Rather than dropping those rows or leaving them in, a cap was applied at 2,552 — a value that contains the realistic physical upper range without stripping information from the rest of the dataset. The original column is preserved so the decision is fully auditable.

## The `short_title` Feature
The core deliverable was a new column: a concise, scannable version of each product title, limited to 30–50 characters. The methodology stripped four categories of noise from every title:
- **Redundant phrases** — "includes," "set of," "features," "with"
- **Model codes embedded mid-title** — SKU strings like `T86_2561C_Navy_Mix`
- **Repeated attribute lists** — dimensions, voltages, and specs already in the description
- **Filler qualifiers** — "high quality," "best seller," "professional grade"
What stayed: brand name, product category, one key differentiator (colour, size, or pack count).

| Original Title | Short Title |
|---|---|
| ArtzFolio Tulip Flowers Blackout Curtain for Door Window & Room Eyelets & Tie Back Canvas Fabric Width 4.5feet 54inch Height 5 feet 60 inch 2 PCS | ArtzFolio Tulip Blackout Curtain - 2 PCS |
| Marks & Spencer Girls' Pyjama Sets T86_2561C_Navy Mix_9-10Y | Marks & Spencer Girls' Pyjama Sets - 9-10Y |
| HOMEIDEAS 100% Blackout Curtains 52 X 63 Inch Length 2 Panels Light Grey Total Room Darkening Curtains for Bedroom Thermal Grommet Double Layers Thick Soundproof | HOMEIDEAS 100% Blackout Curtains (2 Panels) |

**Result:** average title length dropped from 86 characters to 39 — a 55% reduction. Every short title fits within the 50-character ceiling. All 3,541 rows have a populated `short_title`.

## Dataset Overview: Before and After
| Metric | Before | After |
|---|---|---|
| Total rows | 3,847 | 3,541 |
| Duplicate rows | 306 | 0 |
| Average title length | 86 characters | 39 characters |
| Max title length | 403 characters | 50 characters |
| `short_title` column | Not present | 100% populated |
| `ProductLength` max value | 96,000 | 2,552 (capped) |
| Missing BULLET_POINTS | 1,591 (41.4%) | 1,591 — documented |
| Missing DESCRIPTION | 2,144 (55.7%) | 2,144 — documented |
| Missing PRODUCTTYPEID / ProductLength | 178 rows each (4.6%) | Flagged as batch import failure |

## Files in This Repo
| File | What's inside |
|---|---|
| [productdata] | Cleaned dataset with `short_title` feature — Excel formulas retained |
| [Technical_Report_Product_Data_Cleaning] | Full technical report — methodology, cleaning steps, examples, and before/after statistics |

## What This Enables
A clean version of this dataset, with short titles, supports:
- **SEO analysis** — short, structured titles that search algorithms can parse cleanly
- **Product catalogue management** — scannable titles that humans can read at speed
- **Length-based modelling** — capped `product_length` is now usable as a feature without outlier distortion
- **Content gap analysis** — documented missing fields tell the marketing team exactly where copy needs to be written

This was built as an HNG14 Internship Stage 1 task, April 2026. The constraint of working entirely in Excel — no Python, no SQL — forced a disciplined approach to what formulas alone can accomplish when the logic is sound.

← [Back to main portfolio]
