# E-Commerce Product Data Cleaning & Title Optimisation
Excel-based data cleaning pipeline for 3,540 e-commerce product records: duplicate detection, outlier capping, missing value analysis, and short_title feature engineering reducing average title length by 54%. HNG Internship Stage 1.
Cleaned 3,540 raw product records from a multi-category e-commerce dataset, resolved data quality issues across five dimensions, and engineered a `short_title` feature reducing average title length by 54% for improved SEO and readability — all in Excel.

**[Cleaned Dataset]()** · **[Technical Report]()**

## The Problem
The raw dataset had 3,540 product listings and a title problem nobody had quantified yet. Titles averaged 84 characters — the longest ran to 401. They were stuffed with keywords, model codes, colour variants, and filler phrases like "set of," "includes," and "features." Useless for SEO. Impossible to scan.

Beyond titles, the data had four other issues that would have broken any downstream analysis:
- **38.5% of bullet_points were blank** — 1,362 rows with no feature descriptions
- **53.5% of descriptions were missing** — 1,895 rows with no product copy
- **4 duplicate titles** creating identical records for different product IDs
- **Outlier product lengths** — the raw `product_length` column had a maximum value of 96,000, distorting any length-based analysis or modelling
No data team had mapped these issues. No short title existed. That's what this project built.
## What I Did
This was an HNG Internship Stage 1 task — the brief gave a dataset, a tool constraint (Excel only), and a problem to solve. I worked through it end to end: explored the structure, profiled every column, documented every quality issue, applied fixes, and engineered the `short_title` feature from scratch using Excel formulas.

**Tools & Concepts**
`Microsoft Excel` · `Data Cleaning` · `Feature Engineering` · `Outlier Detection & Capping` · `Missing Value Analysis` · `Duplicate Detection` · `SEO Title Optimisation` · `Descriptive Statistics` · `Data Quality Documentation`
## The Cleaning Decisions That Mattered

### Duplicate Detection — Title vs. Product ID
The dataset had zero duplicate `product_id` values, which looked clean at first. But 4 rows had identical `title` values pointing to different product IDs — the same product listed twice under different identifiers. Removing the title rather than ID was the right call. Removing the ID alone would have left the problem in place.

### Outlier Capping on `product_length`
The raw `product_length` column had a mean of 1,155 and a max of 96,000. One value was sitting 35× above the 75th percentile. Rather than dropping those rows (losing valid product data) or leaving them (distorting any length analysis), I applied a cap at 2,552 — a value chosen to contain the realistic upper range without stripping information from the rest of the dataset. 231 rows were affected. The capped values live in `product_length_capp`, so the original data is preserved, and the choice is auditable.

### Missing Values — Document, Don't Impute
`bullet_points` (38.5% missing) and `description` (53.5% missing) are content fields. Filling them with placeholder text or averages would have been worse than useless — it would have introduced false data into a marketing dataset. The right call was to flag the gaps clearly in the report and leave the fields blank, so that any downstream analyst knows exactly what they have.

## The `short_title` Feature
The core deliverable was a new column: a concise, scannable version of each product title, limited to 30–50 characters.

The methodology stripped four categories of noise from every title:
- **Redundant phrases** — "includes," "set of," "features," "with"
- **Model codes embedded mid-title** — SKU strings like `T86_2561C_Navy_Mix`
- **Repeated attribute lists** — dimensions, voltages, and specs already in the description
- **Filler qualifiers** — "high quality," "best seller," "professional grade"

What stayed: brand name, product category, one key differentiator (colour, size, or pack count), and enough specificity to distinguish the listing.
| Original Title | Short Title |
|---|---|
| ArtzFolio Tulip Flowers Blackout Curtain for Door Window & Room Eyelets & Tie Back Canvas Fabric Width 4.5feet 54inch Height 5 feet 60 inch 2 PCS | ArtzFolio Tulip Blackout Curtain - 2 PCS |
| Marks & Spencer Girls' Pyjama Sets T86_2561C_Navy Mix_9-10Y | Marks & Spencer Girls' Pyjama Sets - 9-10Y |
| HOMEIDEAS 100% Blackout Curtains 52 X 63 Inch Length 2 Panels Light Grey Total Room Darkening Curtains for Bedroom Thermal Grommet Double Layers Thick Soundproof Curtains Energy Saving | HOMEIDEAS 100% Blackout Curtains (2 Panels) |

**Result:** average title length dropped from 84 characters to 39 — a 54% reduction. Every short title fits within the 50-character ceiling. All 3,540 rows have a populated `short_title`.

## Dataset Overview: Before and After
| Metric | Before | After |
|---|---|---|
| Total rows | 3,540 | 3,536 |
| Duplicate titles | 4 | 0 |
| Average title length | 84 characters | 39 characters |
| Max title length | 401 characters | 50 characters |
| `product_length` outliers (>2,552) | 231 rows | Capped, not removed |
| `short_title` column | Not present | 100% populated |

## Files in This Repo
| File | What's inside |
|---|---|
| [title_optimization_final.xlsx]() | Cleaned dataset with `short_title` feature — Excel formulas retained |
| [Technical_Report_Product_Data_Cleaning.docx]() | Full technical report — methodology, cleaning steps, examples, and before/after statistics |

## What This Enables
A clean version of this dataset, with short titles, supports:
- **SEO analysis** — short, structured titles that search algorithms can parse cleanly
- **Product catalogue management** — scannable titles that humans can read at speed
- **Length-based modelling** — capped `product_length_capp` is now usable as a feature without outlier distortion
- **Content gap analysis** — documented missing fields tell the marketing team exactly where copy needs to be written

This was built as part of the HNG14 Internship. This is the Stage 1 Task, April 2026. The constraint of working entirely in Excel — no Python, no SQL — forced a disciplined approach to what formulas alone can accomplish when the logic is sound.

← [Back to main portfolio]
