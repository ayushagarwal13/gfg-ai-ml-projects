# Unlocking Customer Personas with AI 🛍️

## Overview
This project demonstrates how Unsupervised Machine Learning can be used to identify and analyze customer personas from mall customer data. Using clustering techniques such as K-Means Clustering, the project groups customers based on attributes like age, annual income, and spending score.

The goal is to help businesses better understand customer behavior and make data-driven marketing decisions.

---

# Project Objective
The primary objective of this project is to:

- Perform customer segmentation using clustering algorithms.
- Identify distinct customer groups/personas.
- Analyze spending patterns and customer behavior.
- Generate actionable business insights.
- Understand how unsupervised learning works in real-world scenarios.

---

# Dataset Information
The project uses the **Mall Customers Dataset** containing:

| Feature | Description |
|---|---|
| CustomerID | Unique ID for customers |
| Gender | Male/Female |
| Age | Customer age |
| Annual Income (k$) | Annual income in thousand dollars |
| Spending Score (1-100) | Customer spending score assigned by the mall |

---

# Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- Scikit-learn
- SciPy
- Jupyter Notebook / Google Colab

---

# Machine Learning Concepts Covered

## Unsupervised Learning
Unlike supervised learning, unsupervised learning does not use labeled target variables. The algorithm identifies hidden patterns directly from the dataset.

## K-Means Clustering
K-Means clustering groups similar customers together into clusters.

### Workflow:
1. Choose the number of clusters (K).
2. Initialize centroids.
3. Assign data points to nearest centroid.
4. Update centroid positions.
5. Repeat until convergence.

---

# Project Workflow

## Step 1: Import Libraries
Libraries for data analysis, visualization, and machine learning are imported.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
```

---

## Step 2: Load Dataset
The Mall Customers dataset is loaded and explored.

```python
df = pd.read_csv('Mall_Customers.csv')
```

---

## Step 3: Exploratory Data Analysis (EDA)
The project performs:

- Dataset inspection
- Missing value checking
- Statistical analysis
- Feature distribution analysis
- Correlation analysis
- Customer behavior visualization

### Visualizations Included:
- Histograms
- Scatter plots
- Pair plots
- Cluster visualizations
- Distribution charts

---

## Step 4: Data Preprocessing
Data preprocessing includes:

- Removing unnecessary columns
- Feature scaling using StandardScaler
- Preparing features for clustering

---

## Step 5: Finding Optimal Number of Clusters
The Elbow Method is used to determine the best value of K.

```python
kmeans = KMeans(n_clusters=5)
```

---

## Step 6: Customer Segmentation
Customers are grouped into different clusters based on purchasing behavior and income patterns.

Example customer groups:

- High Income – High Spending
- High Income – Low Spending
- Low Income – High Spending
- Low Income – Low Spending
- Average Customers

---

## Step 7: Visualization of Clusters
The clustered customer groups are visualized using scatter plots for better understanding and business interpretation.

---

# Business Insights
The project helps businesses:

- Identify valuable customers.
- Improve targeted marketing campaigns.
- Increase customer retention.
- Personalize offers and recommendations.
- Understand customer spending behavior.

---

# Key Learnings
Through this project, the following concepts were learned:

- Unsupervised Learning
- Clustering Algorithms
- K-Means Clustering
- Exploratory Data Analysis
- Feature Scaling
- Customer Segmentation
- Data Visualization
- Business Analytics

---

# Project Structure

```bash
Unlocking_Customer_Personas_with_AI/
│
├── Unlocking_Customer_Personas_with_AI.ipynb
├── Mall_Customers.csv
├── README.md
└── images/
```

---

# How to Run the Project

## Clone the Repository

```bash
git clone <repository-link>
```

## Install Dependencies

```bash
pip install pandas numpy matplotlib seaborn plotly scikit-learn scipy
```

## Run the Notebook

Open the notebook using:

- Jupyter Notebook
- VS Code
- Google Colab

---

# Sample Output
The final output includes:

- Clustered customer groups
- Visual cluster plots
- Customer behavior insights
- Business recommendations

---

# Future Improvements
Possible enhancements:

- Deploy as a web application
- Add real-time customer prediction
- Use advanced clustering algorithms
- Integrate recommendation systems
- Add dashboard visualizations

---

# Conclusion
This project demonstrates how Artificial Intelligence and Machine Learning can help businesses discover meaningful customer personas through clustering techniques. By understanding customer groups, organizations can make smarter marketing and business decisions.

---

# Author
Ayush Agarwal

---

# Connect
If you found this project useful, feel free to star the repository and connect on GitHub.


