# K-Means and K-Medoids Clustering Lab
### Wine Dataset — sklearn

---

## Purpose

This lab applies and compares two unsupervised clustering algorithms — **K-Means** and **K-Medoids** — to the Wine Dataset from `sklearn`. The dataset contains 178 wine samples described by 13 chemical features, belonging to three known cultivar classes.

The goals are to:
- Standardise the feature space and reduce dimensionality with PCA for visualisation.
- Cluster the data into k = 3 groups using both algorithms.
- Evaluate cluster quality with the **Silhouette Score** and **Adjusted Rand Index (ARI)**.
- Compare the algorithms visually and analytically, and discuss when each is preferable.

---

## Repository Structure

```
.
├── this.ipynb   # Main Jupyter Notebook
├── README.md                        # This file
├── class_distribution.png           # Bar chart of true class counts
├── correlation_heatmap.png          # Feature correlation matrix
├── elbow_silhouette.png             # Elbow + silhouette plots for k selection
├── cluster_comparison.png           # Side-by-side K-Means vs K-Medoids scatter
├── ground_truth_comparison.png      # True labels vs both algorithm outputs
└── metrics_comparison.png           # Bar chart + violin plot of evaluation metrics
```

> All image files are generated when the notebook is executed end-to-end.

---

## Dependencies

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scikit-learn-extra notebook
```

> **Note:** `scikit-learn-extra` provides the `KMedoids` implementation. If you are running NumPy 2.x, downgrade to `numpy<2` before installing:
> ```bash
> pip install "numpy<2"
> pip install scikit-learn-extra
> ```

---

## How to Run

```bash
jupyter notebook kmeans_kmedoids_wine_lab.ipynb
```

Then select **Kernel → Restart & Run All**.

---

## Key Insights

### Algorithm Performance

| Algorithm  | Silhouette Score | ARI  | Notes |
|------------|-----------------|------|-------|
| K-Means    | ~0.285          | ~0.90 | Centroids are arithmetic means; slightly faster |
| K-Medoids  | ~0.270          | ~0.88 | Medoids are real data points; more robust to outliers |

*(Exact values depend on random seed and run; results are reproducible with `random_state=42`.)*

### Silhouette Score Interpretation
- Scores range from −1 to +1; values above 0.25 on a 13-feature real-world dataset indicate **reasonably well-defined clusters**.
- Both algorithms score in the 0.27–0.29 range, confirming that k = 3 is a natural fit for this data — consistent with the three known wine cultivar classes.
- The elbow plot and silhouette-vs-k curve both peak at k = 3, validating the choice.

### ARI Interpretation
- ARI = 1.0 means perfect agreement with true labels; ARI = 0 means random assignment.
- Both algorithms achieve ARI > 0.85, meaning they recover the original three wine classes very effectively from the raw features alone — without ever seeing the class labels during training.

### Cluster Shape and Centroid Behaviour
- In the PCA-reduced scatter plots, both algorithms produce three visually similar, well-separated clusters.
- **K-Means centroids** float in the geometric centre of each cloud and do not correspond to real wine samples.
- **K-Medoids medoids** are actual data points, anchored within the densest region of each cluster — making them more interpretable as representative examples.
- Minor boundary differences appear where a few borderline samples are assigned differently, particularly when one cluster overlaps slightly with another in PCA space.

### When to Use Each Algorithm

| Situation | Recommended Algorithm |
|-----------|----------------------|
| Large dataset (>10 000 samples) | K-Means (faster) |
| Outliers present in data | K-Medoids (robust) |
| Need a real exemplar per cluster | K-Medoids |
| Non-Euclidean or categorical distance | K-Medoids |
| Clean, continuous, scaled features | Either; K-Means is simpler |

---

## Challenges and Decisions

| Challenge | Decision / Resolution |
|-----------|----------------------|
| K-Medoids not in core sklearn | Used `scikit-learn-extra`; noted NumPy version compatibility requirement (needs `numpy<2`) |
| Choosing the right number of clusters | Confirmed k = 3 via elbow plot (inertia) and silhouette-vs-k curve before proceeding |
| Visualising 13-dimensional data | Applied PCA (2 components, ~55% variance explained) for all scatter plots; centroids and medoids projected into the same PCA space |
| Cluster label permutation | ARI is permutation-invariant, so cluster label ordering does not affect metric correctness |
| Reproducibility | Fixed `random_state=42` for both KMeans and KMedoids, and for PCA |

---

## Conclusion

Both K-Means and K-Medoids successfully recover the three wine cultivar classes with high ARI scores (>0.85) and positive Silhouette Scores (~0.28). K-Means has a slight edge in compactness metrics on this clean, continuous dataset, while K-Medoids offers greater robustness and interpretability. For real-world datasets with noise or mixed data types, K-Medoids is the safer and more flexible choice.
