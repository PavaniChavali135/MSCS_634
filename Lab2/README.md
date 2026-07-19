# MSCS_634_Lab_2: Advanced Big Data and Data Mining
**Author:** Pavani Chavali  
**Topic:** K-Nearest Neighbors (KNN) vs. Radius Neighbors (RNN) Classifiers  

## Purpose of the Lab Work
The objective of this laboratory assignment is to practically evaluate and compare the architectural differences between K-Nearest Neighbors (KNN) and Radius Neighbors (RNN) algorithms. Utilizing the SciKit-Learn Wine Dataset, this project explores how varying hyperparameter configurations—specifically the neighbor count (*k*) and the spatial threshold (*radius*)—affect the predictive accuracy of lazy learning classifiers. This exploration builds a foundational understanding of model tuning, spatial density sensitivity, and hyperparameter optimization in multi-class machine learning tasks.

## Key Insights from Accuracy Trends and Observations
1. **KNN Stability:** The KNN model maintained relatively robust accuracy across the lower tested configurations (k=1 to k=11). However, as *k* increased toward 21, the model's accuracy began to fluctuate and decay. This demonstrates the phenomenon of over-smoothing, where excessively large neighbor boundaries cause the classifier to ignore distinct local patterns and default to broader dataset majorities.
2. **RNN Sensitivity to Scale:** The Radius Neighbors algorithm exhibited flat or drastically shifting accuracy trends across the radius values (350 to 600). Because it relies on a fixed spatial boundary, RNN is intensely influenced by the magnitudes of unscaled features. As the radius increases, the likelihood of encompassing points from competing classes rises, muddying the decision boundary.
3. **Density Adaptability:** KNN adapts dynamically to sparse regions by expanding its implicit search radius until *k* neighbors are found, whereas RNN fails to adapt in sparse regions, highlighting their fundamental algorithmic divergence.

## Challenges Faced and Decisions Made
A significant technical challenge encountered during the implementation of the RNN algorithm was the presence of data sparsity and varying feature magnitudes. Because the raw Wine Dataset contains unscaled variables with substantial numeric ranges, certain test instances lacked any neighboring points within the tighter radius thresholds. 

To resolve this issue without altering the dataset's native scales (which was required to utilize the specific high-value radius constraints detailed in the assignment instructions), a strategic decision was made to configure the `outlier_label` parameter within the `RadiusNeighborsClassifier`. Setting this parameter to `'most_frequent'` ensured that isolated data points were gracefully handled by defaulting to the most common class label, preventing fatal compilation errors and allowing the experiment to run to completion.



Step 1: Load and Prepare the Dataset
The initial phase of this experiment involves acquiring and preparing the Wine Dataset, a classic multi-class classification problem containing various chemical constituents of wines derived from three different cultivars. Distance-based classification algorithms, such as nearest neighbor variants, are highly sensitive to the spatial distribution of the feature space. Therefore, performing basic exploratory data analysis and properly partitioning the dataset into training and testing subsets is crucial for ensuring that the model evaluates unseen data fairly, preventing data leakage and overfitting.

Step 2: Implement K-Nearest Neighbors (KNN)
The K-Nearest Neighbors algorithm operates as a non-parametric, lazy learning method. Rather than constructing a generalized internal model during the training phase, it retains the training instances and performs classification based on spatial proximity during inference. The parameter k defines the exact number of neighboring instances used to vote on the class label of a new query point. A lower k captures fine-grained, localized patterns but is highly susceptible to noise, whereas a larger k smooths the decision boundaries, potentially ignoring minority class structures.

Step 3: Implement Radius Neighbors (RNN)
While KNN selects a fixed number of neighbors regardless of their distance, the Radius Neighbors classifier selects all neighbors that fall within a specified spatial radius, ignoring points outside of this bound. This allows the model to adapt dynamically to the varying density of the dataset; dense regions will aggregate many votes, while sparse regions will rely on fewer points. Given that this specific assignment utilizes unscaled data (with features like "proline" extending into the thousands), the radius values must be sufficiently large to capture neighboring points across these expansive scales. Furthermore, an outlier handling mechanism is required in case a test point finds zero training points within its fixed radius.

Step 4: Visualize and Compare Results
By projecting the accuracy metrics onto a line plot, we can visually decipher the relationship between hyperparameter tuning and model performance for both algorithms. Understanding these trajectories is fundamental for model selection and algorithmic optimization.

Model Comparison and Discussion:
Based on the experimental output, KNN and RNN demonstrate distinct behavioral paradigms due to their underlying geometric assumptions.

KNN enforces a strict neighbor count limit. This guarantees that every testing instance receives exactly k votes, circumventing the issue of "empty neighborhoods." As a result, its accuracy trajectory is often more stable, though it degrades when k becomes excessively large, causing the model to overgeneralize and allow majority classes to overpower localized minority boundaries.

Conversely, RNN enforces a strict spatial limit. This approach is highly sensitive to the scale and density of the dataset. Because the features in the Wine Dataset possess drastically different variance scales (e.g., alcohol content vs. proline concentrations), Euclidean distances are heavily skewed by large-magnitude variables. In unscaled environments, an improperly tuned radius can either engulf the entire dataset (resulting in blanket predictions of the majority class) or fail to capture any points at all (triggering outlier exception handling).

When to prefer which model:
KNN is generally preferable when working with datasets containing uniform feature spaces or varying densities, as its adaptive boundary prevents isolated points from being left without a prediction. It serves as an excellent baseline due to its lack of reliance on strict spatial bounds. RNN, on the other hand, is highly effective in anomaly detection or environments where feature scaling has been meticulously applied and uniform density is guaranteed. RNN is preferable when one wishes to actively penalize or ignore points that are geographically distant in the feature space, thereby ensuring that only genuinely similar instances contribute to the classification vote.