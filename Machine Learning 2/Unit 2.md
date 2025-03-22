## Machine Learning Clustering Algorithms II: Notes

**1. Finding Unusual Events (Anomaly Detection):**

* Anomaly detection aims to identify unusual or rare data points that deviate significantly from the majority of the data.
* These anomalies can represent outliers, errors, or interesting events.
* Applications include fraud detection, network security, and system health monitoring.

**2. Gaussian (Normal) Distribution:**

* A common assumption in anomaly detection is that the data follows a Gaussian (normal) distribution.
* The Gaussian distribution is characterized by its mean (μ) and standard deviation (σ).
* Anomaly detection algorithms often use the probability density function of the Gaussian distribution to determine the likelihood of a data point being an anomaly.  Points with low probability density are flagged as potential anomalies.

**3. Anomaly Detection Algorithm:**

* **Density-based anomaly detection:**  Identifies anomalies as points in regions of low data density.
* **Clustering-based anomaly detection:**  Uses clustering algorithms to group similar data points.  Points that do not belong to any cluster or belong to very small clusters are considered anomalies.
* **One-Class SVM:**  Trains a model to represent the "normal" data and identifies deviations from this model as anomalies.
* **Gaussian Mixture Models (GMMs):**  Models the data as a mixture of multiple Gaussian distributions. Points with low probability under the overall mixture model are considered anomalies.

**4. Anomaly Detection vs. Supervised Learning:**

* **Anomaly Detection (Unsupervised):**  Typically uses unlabeled data and learns the "normal" patterns from the data itself.  Suitable when anomalies are rare and difficult to label.
* **Supervised Learning (Classification):**  Requires labeled data with examples of both normal and anomalous instances.  Suitable when sufficient labeled data is available and the types of anomalies are known in advance.

**5. Choosing What Features to Use (PCA):**

* Feature selection and engineering are crucial for effective anomaly detection.
* **Principal Component Analysis (PCA):** A dimensionality reduction technique that can be used to identify the most important features that contribute to the variance in the data.  Reducing the number of features can improve the performance of anomaly detection algorithms and reduce computational complexity.  PCA can also help visualize high-dimensional data.

**6. Organizing Clusters as a Hierarchical Tree (Hierarchical Clustering):**

* Hierarchical clustering builds a hierarchy of clusters, represented as a tree (dendrogram).
* **Agglomerative Clustering (bottom-up):** Starts with each data point as a separate cluster and iteratively merges the closest clusters until a single cluster remains.
* **Divisive Clustering (top-down):** Starts with all data points in a single cluster and recursively splits the cluster into smaller clusters until each data point forms its own cluster.

**7. Agglomerative Clustering:**

* Uses a linkage criterion to determine the distance between clusters (e.g., single linkage, complete linkage, average linkage, Ward's method).
* The dendrogram visualizes the hierarchy of clusters and the order in which they were merged.
* Allows for different levels of granularity in the clustering by cutting the dendrogram at different heights.

**8. DBSCAN Clustering (Density-Based Spatial Clustering of Applications with Noise):**

* A density-based clustering algorithm that groups data points based on their density and identifies outliers (noise points).
* Defines clusters as dense regions separated by regions of low density.
* Two key parameters: `epsilon` (radius of neighborhood) and `minPts` (minimum number of points within the neighborhood to form a dense region).
* Advantages: Can discover clusters of arbitrary shapes, robust to outliers, does not require specifying the number of clusters.


These notes provide a detailed overview of the key concepts related to anomaly detection, dimensionality reduction, and hierarchical and density-based clustering.  Remember to supplement these notes with practical examples and visualizations to solidify your understanding.  Consider implementing these algorithms using libraries like scikit-learn in Python.
