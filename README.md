# ****Wholesale Customer Segmentation using Unsupervised Learning****

Segmenting 440 wholesale clients by annual spend with PCA, K-Means and Hierarchical (Ward) clustering, then translating the segments into practical business actions.

## **Business problem**

A wholesale distributor serves very different buyers (hotels, restaurants, cafes and retail shops) with one standard approach. Can customers be grouped by how they actually spend, so that delivery, pricing and promotions can be tailored per group?

## **Dataset**

**Source:** UCI Machine Learning Repository, Wholesale Customers dataset

**Size:** 440 customers, no missing values, no duplicates

**Features used for clustering:** annual spend on Fresh, Milk, Grocery, Frozen, Detergents_Paper, Delicassen

**Held out on purpose:** Channel (Horeca / Retail) and Region. They were not used to cluster and were used afterwards to validate the segments.


## **Approach**

- **EDA:** spend is heavily right-skewed (skewness 2.56 to 11.15), with a few very large buyers.

- **Preprocessing:** log1p transform and standardisation. 18 extreme customers (4.1%) were flagged but kept, since big buyers are real, valuable customers.

- **PCA:** PC1 + PC2 explain **71.3%** of the variance (4 components reach 90%).
PC1 = household goods axis (Milk, Grocery, Detergents_Paper)
PC2 = fresh / perishables axis (Fresh, Frozen, Delicassen)

- **K-Means:** K chosen with **Elbow, Silhouette, Calinski-Harabasz and Davies-Bouldin.**

- **Hierarchical clustering:** Ward linkage, dendrogram, cophenetic correlation across linkage methods.

- **Validation:** method comparison, subsample stability, per-customer silhouette, and an external check against Channel.
Profiling: segment medians, spend index vs overall median, share of customers vs share of revenue.

## **Key results**

- Metric	K-Means (K = 2)	Hierarchical (Ward)
- Silhouette	0.290	0.258
- Calinski-Harabasz	189.05	134.62
- Davies-Bouldin (lower is better)	1.352	1.600
- K-Means and Ward agree moderately (Adjusted Rand Index = 0.64).
K-Means is highly stable: mean ARI 0.967 +/- 0.038 across 50 random 80% subsamples.
The Ward dendrogram shows one dominant top-level split (merge distance about 35, versus about 24 for the next merge), supporting two main groups, with smaller nternal splits that hint at sub-segments.


## Customer Segments

| **Segment** | **Segment 0: Fresh and Frozen Buyers** | **Segment 1: Grocery and Household Buyers** |
|---|---:|---:|
| **Customers** | 252 (57.3%) | 188 (42.7%) |
| **Share of Total Spend** | 42.3% | 57.7% |
| **Strongest Categories** | Frozen, Fresh | Detergents_Paper, Grocery, Milk |
| **Weakest Categories** | Detergents_Paper, Grocery, Milk | Fresh, Frozen |
| **Channel** | 97% Horeca | 71% Retail, 29% Horeca |

## **Business recommendations**

#### **Segment 0 (hotel / restaurant / cafe type):**
-  prioritise delivery frequency, cold-chain reliability and freshness guarantees;
-   offer fresh and frozen bundles;
-   test cross-selling dairy, grocery and hygiene products (low spend suggests untapped wallet share).

#### **Segment 1 (retail type):** 
- offer volume or contract pricing and loyalty terms; 
- guarantee stock on grocery, dairy and cleaning lines; 
- assign account management, because 58% of revenue comes from 43% of customers.

- Target by spending behaviour and channel, not by region.

