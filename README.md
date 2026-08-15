# Data Science Notes — Organized, Fixed & Documented

A structured, topic-by-topic collection of data science and machine learning notebooks — covering Python fundamentals through deep learning — cleaned up, bug-fixed, and documented so every notebook runs end-to-end and is ready to publish (e.g. on Kaggle) or push to GitHub.

## Structure

| Folder | Contents |
|---|---|
| `01-Python-Fundamentals/` | Data types, loops, functions, modules/packages — core Python practice |
| `02-NumPy/` | Arrays, unique values, random sampling |
| `03-Pandas/` | Series/DataFrame basics, merge/join, apply/groupby |
| `04-Data-Visualization/` | Histograms, pie charts, box plots, heatmaps |
| `05-EDA-Descriptive-Stats/` | Descriptive analytics case study (CardioGoodFitness) |
| `06-Hypothesis-Testing/` | Concepts, then an applied case study (insurance data) |
| `07-Regression/` | Linear regression (auto-mpg), logistic regression (diabetes) |
| `08-Classification-Models/` | Decision tree, KNN, SVM, and a full supervised learning case study (bank marketing) |
| `09-Clustering/` | Hierarchical clustering, K-Means |
| `10-Dimensionality-Reduction-PCA/` | PCA + regression, PCA case study |
| `11-Neural-Networks-Deep-Learning/` | Feed-forward network from scratch, CNN architecture, image preprocessing |
| `12-Recommendation-Systems/` | Movie ratings analysis + market basket association rules |
| `Datasets/` | All CSV/image files used across the notebooks above — centralized here, not duplicated in each topic folder |

## Running a notebook locally
Datasets are kept in one central `Datasets/` folder rather than copied next to every notebook, to keep the repo size down. Since each notebook loads its data by filename alone (e.g. `pd.read_csv("insurance_data.csv")`), if you want to actually **run** a notebook rather than just read it, copy the relevant CSV from `Datasets/` into the same folder as that notebook first (or edit the path in the first code cell to point at `../Datasets/filename.csv`).


