<div align="center">
<img src="http://googleusercontent.com/file_content/4" alt="Project Blueprint Banner">
</div>

🚀 Advanced Customer Segmentation for FinTech
This repository contains a Jupyter Notebook (Customer_Segmentation.ipynb) that performs an end-to-end customer segmentation analysis on a credit card dataset. The project leverages unsupervised machine learning to identify distinct customer personas and builds a predictive model to operationalize these insights.

📝 Project Overview
The primary goal is to partition a credit card customer base into meaningful, behaviorally-defined groups. This allows for the development of targeted marketing, product, and risk management strategies.

Dataset: CC_GENERAL.csv - Contains transactional and behavioral data for approximately 9,000 credit card holders.

🛠️ Key Analytical Steps
* Data Cleaning & EDA: The notebook starts with data loading, cleaning (imputing missing values), and a thorough exploratory data analysis to understand data distributions and correlations.

* Feature Engineering: New, insightful features are created to better capture customer behavior, such as credit utilization and payment ratios.

* Unsupervised Clustering: Three different clustering algorithms are compared to find the optimal segmentation:

* * K-Means

* DBSCAN

* Gaussian Mixture Models (GMM)

* Model Validation: The Silhouette Score is used to quantitatively determine the best-performing clustering model.

* Predictive Modeling: A RandomForestClassifier is trained on the resulting cluster labels to create a model that can assign new customers to a segment in real-time.

* Persona Analysis: The final segments are analyzed to build detailed customer personas, and feature importance is used to identify the key behavioral drivers.

⚙️ How to Use
Ensure you have Jupyter Notebook or a compatible environment (like Google Colab) installed.

Place the CC_GENERAL.csv file in the same directory as the notebook.

Run the cells in the Customer_Segmentation.ipynb notebook sequentially to reproduce the analysis.

📦 Dependencies
This project uses the following core Python libraries:

pandas
numpy
matplotlib
seaborn
scikit-learn
