# 🧬 DA5401 A5 — Visualizing Data Veracity in Multi-Label Classification (Yeast Dataset)

#### Name: Aditya Thakur &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Roll No: DA25M004

---

## 🧾 Abstract

This study explores **manifold learning techniques** — **t-SNE** and **Isomap** — to visualize and analyze the **veracity challenges** in multi-label yeast gene expression data.  
The dataset exhibits **strong class imbalance**, **noise**, and **overlapping functional categories**, making it ideal for studying nonlinear manifolds.  

Through systematic **preprocessing (outlier removal, scaling, PCA denoising)** and **evaluation via trustworthiness and continuity metrics**,  
we assess how each algorithm preserves local vs. global structures.  

Results show that:
- **t-SNE** excels at **local compactness**, revealing fine-grained subclusters.  
- **Isomap** preserves **global continuity**, unfolding the underlying curved manifold.  
Together, they provide complementary insights into the biological structure of gene expression data.

---

## 🔹 1. Preprocessing Pipeline

### Step 1️⃣ — Data Loading & Integrity Check
- Loaded `yeast.arff`, converted byte-encoded columns → strings.
- Verified dataset → **no missing values** and **no duplicates** detected.

### Step 2️⃣ — Outlier Detection (Z-Score)
- Applied Z-score filtering ($|z| > 3.5$) to remove extreme points.
- **Without removal:** t-SNE/Isomap produced scattered, structureless embeddings.  
- **After removal:** clear and separable clusters emerged.

$$
z = \frac{x - \mu}{\sigma}
$$

### Step 3️⃣ — Feature Scaling
- Standardized all features:
  $$
  X' = \frac{X - \mu}{\sigma}
  $$
- Prevented features with large ranges from dominating distance computations.

### Step 4️⃣ — PCA Denoising
- Performed PCA after scaling (≈95% variance retained).  
- Reduced redundancy and noise, improving both **continuity** and **trustworthiness** in later embeddings.

---

## 🔹 2. t-SNE Analysis

### Parameters Tested
Perplexity ∈ [5, 10, 20, 30, 40, 50, 60, 70, 80]

| Metric | Range | Interpretation |
|:-------|:------|:---------------|
| **Trustworthiness** | 0.93–0.94 | Excellent local structure preservation |
| **Continuity** | 0.34–0.36 | Moderate global consistency |
| **KL Divergence ↓** | 1.95 → 1.61 | Stabilizes at higher perplexity |

**Best Setting:** `perplexity = 60` → High trustworthiness (0.93), lowest KL (1.57)

### Observations
- 🟤 **MostFreqCombo**: compact, dense clusters (dominant pattern).  
- 🟢 **Top1_label_0**: sparse; limited samples hinder cluster formation.  
- ⚪ **Other**: overlaps, forming background distribution.

**Interpretation**
- *Noisy Labels:* single-label points inside mixed clusters suggest ambiguous or correlated gene functions.  
- *Outliers:* isolated samples indicate rare expression signatures.  
- *Hard-to-Learn Zones:* regions with color mixing imply nonlinear boundaries.

🧭 **Conclusion:**  
t-SNE effectively preserves **local manifolds** and reveals fine substructure,  
but global relationships are partially lost — typical for t-SNE embeddings.

---

## 🔹 3. Isomap Analysis

### Parameters Tested
Neighborhood size \(k ∈ [5, 10, 15, 20, 30, 40, 50, 70]\)

| k | Recon. Error | Trust. | Cont. | Observation |
|:--:|:-------------:|:------:|:------:|:-------------|
| 5 | 263.2 | 0.73 | 0.06 | Fragmented neighborhoods |
| 20–30 | 125–107 | 0.74 | 0.05 | Smooth, continuous manifold |
| 70 | 89.6 | 0.75 | 0.05 | Over-flattened, global merging |

**Best Setting:** `k = 30` → Low reconstruction error (107.3), stable global structure.

### Observations
- 🟤 **MostFreqCombo**: forms two connected lobes (continuous surface).  
- 🟢 **Top1_label_0**: sparse and scattered.  
- ⚪ **Other**: fills gaps, ensuring global connectivity.

**Interpretation**
- *Trustworthiness ↓ (~0.74):* local detail traded for global coherence.  
- *Continuity (~0.05):* stable across k, reflecting consistent unfolding.  
- *Recon. Error ↓:* confirms accurate manifold recovery.

🧭 **Conclusion:**  
Isomap captures the **global curvature** and **smooth transitions** between clusters,  
though it sacrifices fine local precision — aligning with its global-preserving nature.

---

## 🔹 4. Comparative Evaluation — t-SNE vs. Isomap

| Method | Param | Trust. | Cont. | KL / Recon | Focus |
|:-------|:------|:------:|:------:|:-------------:|:------|
| **t-SNE** | Perplexity = 60 | 0.93 | 0.35 | 1.58 | Local clusters |
| **Isomap** | k = 30 | 0.75 | 0.05 | 107.3 | Global structure |

### Objectives
- **t-SNE minimizes:**
  $$
  \text{KL}(P||Q) = \sum_i \sum_j p_{ij} \log \frac{p_{ij}}{q_{ij}}
  $$
- **Isomap minimizes:**
  $$
  E = \sum_{i<j} (d^{(\text{geo})}_{ij} - d^{(\text{low})}_{ij})^2
  $$

### Comparative Interpretation
| Aspect | t-SNE | Isomap |
|:-------|:------|:--------|
| **Preserves** | Local clusters | Global manifold curvature |
| **Trustworthiness** | High (0.93) | Moderate (0.75) |
| **Continuity** | Moderate (0.35) | Low but stable (0.05) |
| **Cluster Form** | Compact, discrete | Continuous, stretched |
| **Use Case** | Local structure analysis | Global geometry analysis |

$$
\text{t-SNE} \Rightarrow \text{Local Compactness}, \quad
\text{Isomap} \Rightarrow \text{Global Continuity.}
$$

---

## ✅ Final Takeaways
- **Outlier removal + PCA** greatly improved manifold interpretability.  
- **t-SNE:** high local accuracy, excellent for sub-cluster discovery.  
- **Isomap:** preserves large-scale manifold structure and curvature.  
- Combined, they provide a **complete representation** — local faithfulness + global coherence —  
  essential for understanding the **veracity and complexity** of yeast gene expression data.

---
