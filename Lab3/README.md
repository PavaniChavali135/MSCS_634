# MSCS_634_Lab_3: Advanced Clustering Techniques
**Author:** Pavani Chavali
**Topic:** Partitioning Algorithms: K-Means vs. K-Medoids  

## Purpose of the Lab Work
The primary goal of this experiment is to explore and evaluate unsupervised machine learning techniques—specifically partitioning-based clustering algorithms. Utilizing the chemical profiles within the SciKit-Learn Wine Dataset, this project investigates how K-Means and K-Medoids algorithms group high-dimensional data without prior knowledge of class labels. By calculating performance benchmarks such as the Silhouette Score and Adjusted Rand Index, and utilizing dimensionality reduction for visualization, the lab aims to distinguish the geometric and computational differences between defining clusters via mathematical means versus real-world representative medoids.

## Key Insights from Clustering Results
1. **Performance Discrepancies:** K-Means slightly outperformed K-Medoids in both structural cohesion (Silhouette Score) and ground-truth alignment (ARI). This indicates that the Wine Dataset, once standardized, does not contain extreme structural outliers that would disrupt traditional arithmetic centroids, allowing K-Means to operate under ideal conditions.
2. **Geometric Tethering:** Visualizing the data through PCA revealed how K-Medoids restricts its center to an actual data point. While this creates a highly interpretable center (a real wine profile), it occasionally sacrifices optimal geometric boundary placement compared to K-Means, which can synthesize a point perfectly in the mathematical center of the data mass.
3. **The Necessity of Scaling:** Clustering without standardizing the 13 distinct features would have caused features with large nominal values to completely dictate the distance matrices, essentially silencing fractional attributes like chemical phenols.

## Challenges Faced and Decisions Made
A significant technical environmental challenge occurred during the implementation of the K-Medoids algorithm. The standard Python environment being used (a WebAssembly-based Jupyter kernel via `mambajs`) lacks the C-compilation support necessary to install standard extensions like `scikit-learn-extra`. 

Rather than abandoning the assignment or migrating environments, a programmatic decision was made to build a **custom, pure Python implementation of the Partitioning Around Medoids (PAM) algorithm**. By utilizing standard NumPy operations and SciKit-Learn's `pairwise_distances`, the custom class accurately replicates the K-Medoids training behavior entirely within native Python, bypassing the environmental limitation while successfully fulfilling the lab's core objectives.

Additionally, visualizing the cluster formations presented a challenge due to the 13-dimensional nature of the dataset. To map the results onto a 2D plot, Principal Component Analysis (PCA) was applied *only* at the final step to transform the coordinates of the data points and the derived centers for human-readable visualization.



## Analysis

Step 1: Data Preparation and Feature Scaling
The foundation of any distance-based machine learning algorithm is robust data preprocessing. In this phase, we import the Wine Dataset, which contains 13 continuous chemical attributes derived from three distinct wine cultivars. Because features like "Proline" naturally exist in the thousands while attributes like "Nonflavanoid phenols" are fractional, raw distance calculations would be severely disproportionate, heavily weighting features with larger magnitudes. To counteract this, z-score normalization is applied. This transformation centers the mean of each feature at zero with a standard deviation of one, projecting all dimensions into a uniform, comparable scale space necessary for accurate centroid and medoid formulation.

Step 2: K-Means Partitioning
The K-Means algorithm partitions the data by iteratively minimizing the within-cluster sum of squares. It generates synthetic points in the feature space (centroids) that represent the arithmetic mean of all data points assigned to a specific cluster. While computationally highly efficient and mathematically elegant for producing spherical clusters, its reliance on Euclidean means makes it highly susceptible to shifting when outliers are present.

Step 3: K-Medoids Partitioning (Custom Implementation)
In contrast to the synthetic centroids of K-Means, the K-Medoids algorithm designates actual existing data points as the cluster centers. By minimizing absolute distances to an actual representative instance, this technique gains substantial robustness against outliers and noise.

Because standard libraries for K-Medoids (like scikit-learn-extra) rely on C-extensions that are incompatible with browser-based WebAssembly environments (e.g., JupyterLite), a custom pure Python Partitioning Around Medoids (PAM) class has been engineered below. This ensures the algorithm can execute natively in any environment by utilizing standard Euclidean pairwise distance matrices.

Step 4: Dimensionality Reduction and Visualization
Because the standardized feature space is 13-dimensional, direct visualization is mathematically impossible. We utilize Principal Component Analysis (PCA) to project the scaled feature space down to two primary dimensions that capture the maximum possible variance. This allows us to map the high-dimensional clustering structures—and their respective centroids/medoids—onto a 2D Cartesian plane for visual comparative analysis.

Comparative Analysis
Which algorithm produced better-defined clusters?
Based on both visual inspection of the PCA scatter plots and the mathematical scoring, K-Means generated slightly better-defined cluster separations for this specific dataset. The Silhouette Score for K-Means marginally outperforms K-Medoids, indicating tighter intra-cluster cohesion and better inter-cluster separation. Additionally, the Adjusted Rand Index (ARI) demonstrates that the K-Means partition aligned more accurately with the true, underlying cultivar labels of the Wine Dataset.

Differences in shape and positioning:
K-Means naturally attempts to minimize variance around a central mathematical mean, resulting in tightly packed boundaries. Its synthetic centroids locate themselves perfectly in the geometric center of the local data densities. Conversely, K-Medoids shapes are slightly more irregular. Because K-Medoids must tether its center to a real, existing data point (the cyan diamond), the center is occasionally pulled slightly off the absolute mathematical center of mass. This subtle shift alters the assignment boundaries for the ambiguous points positioned along the peripheries between clusters.

Algorithmic Preferences:

K-Means Preference: This approach is optimal for datasets possessing continuous numerical variables lacking severe outliers, particularly when computational efficiency is required for massive datasets (Big Data contexts). It is the standard choice when expecting roughly spherical data distributions.

K-Medoids Preference: This technique is vastly superior when a dataset is contaminated with significant noise or extreme outliers that would falsely skew a mathematical mean. Furthermore, it is the only viable option when dealing with categorical or non-Euclidean data (like document similarities or DNA sequencing) where a mathematically averaged "synthetic" point cannot physically exist.