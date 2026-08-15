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

## What was done

Every notebook in this folder was:
1. **Tested** — executed end-to-end with `jupyter nbconvert --execute` to find real errors (not just eyeballed)
2. **Fixed** — every crash, typo, deprecated API call, and logic bug resolved (see changelog below)
3. **Documented** — each notebook now opens with a markdown cell covering its title, problem statement, dataset, approach, and conclusion

**28 notebooks. 28 pass.**

## Changelog — what was actually broken and fixed

### 01-Python-Fundamentals/ (all 6 notebooks)
- Replaced all interactive `input()` calls with hardcoded demo values / simulated input sequences — the originals would hang indefinitely when run non-interactively (e.g. Kaggle's "Save & Run All")
- `02_loops.ipynb`: fixed `reverse.range(...)` (invalid syntax) → `reversed(range(...))`; rewrote a broken `total_sales()` function that never actually computed anything; fixed a `sum` variable shadowing the builtin `sum()`; wrapped an intentional `NameError` teaching example in try/except so it demonstrates the concept without halting execution
- `03_modules_and_packages.ipynb`: the custom `first.py` module it imported wasn't included — added a `%%writefile` cell so the module is created inline, making the notebook self-contained
- `01_intro_datatypes.ipynb`: fixed a `l3[1se][2]` typo (invalid variable name) → `l3[1][2]`
- `06_practice_loops_range.ipynb`: fixed an unfinished/broken exercise (`sales_data['store'][]` — invalid syntax) with a working implementation

### 04-Data-Visualization/histogram_piechart.ipynb
- `plt.title()` called with no argument (crashes) → added a label
- `.corr()` called on a DataFrame with a non-numeric column → added `numeric_only=True`

### 05-EDA-Descriptive-Stats/customer_profile_cardiofitness.ipynb
- `.corr()` on non-numeric columns → `numeric_only=True` (pandas 2.x+ compatibility)

### 06-Hypothesis-Testing/ (both notebooks)
- `01_hypothesis_testing_concepts.ipynb` never actually loaded its dataset — added the missing `pd.read_csv()` step, missing imports (`scipy.stats`, `LabelEncoder`), fixed an `insull()` typo → `isnull()`, fixed a `'smokers'` column-name typo → `'smoker'`, replaced deprecated `sns.distplot()` → `sns.histplot(..., kde=True)`
- Both notebooks: fixed a `LabelEncoder` batch-assignment pattern that raises a dtype error in current pandas — now encodes one column at a time

### 07-Regression/ (both notebooks)
- `linear_regression_autompg.ipynb`: the `horsepower` column was being median-imputed *before* being converted from string to numeric, causing a crash — reordered so type conversion happens first
- `logistic_regression_diabetes.ipynb`: fixed a `y_tr` typo → `y_test`; fixed a cell-ordering bug where a confusion matrix used `y_predict` before it was defined; fixed a cell that was accidentally saved as **raw text instead of code**, so it silently never ran

### 08-Classification-Models/supervised_learning_case_study_bank_marketing.ipynb
- `.corr()` on non-numeric columns → `numeric_only=True`
- `predict=model.predict(...)` referenced an undefined `model` → fixed to reference the actual trained `knn_model`
- F1-score formula had a missing parenthesis, silently computing the wrong value → fixed operator precedence

### 09-Clustering/hierarchical_clustering_customer_spend.ipynb
- `AgglomerativeClustering(affinity=...)` → scikit-learn renamed this parameter to `metric` (breaking change in 1.4+) — updated

### 10-Dimensionality-Reduction-PCA/ (both notebooks)
- `pca_case_study_vehicle.ipynb`: `.corr()` fix (as above); also fixed a real linear-algebra bug — eigenvectors were being sliced by row instead of by column, causing a matrix-multiplication shape mismatch
- `pca_with_regression_autompg.ipynb`: same horsepower type-conversion ordering fix as the regression notebook

### 11-Neural-Networks-Deep-Learning/
- `01_feed_forward_network_from_scratch.ipynb`: this was genuinely **unfinished** — a malformed label array (mixing a scalar with lists), two undefined-variable typos in the feedforward function, and **no training loop at all**. Fixed the bugs and added a complete backpropagation + gradient descent training loop so the network actually learns XOR.
- `02_cnn_image_classification.ipynb`: added a safe fallback for `nltk.download()` in case the Kaggle environment has internet access disabled
- `03_image_preprocessing_for_nn.ipynb`: fixed a one-character typo (`tensorflow.keras.application` → `.applications`)

### 12-Recommendation-Systems/movie_recommendation_system.ipynb
- The original `ratings.csv`, `movies.csv`, and `Market_Basket_Optimisation.csv` were missing from the uploaded project (already flagged as a known issue in the previous README) — generated small, realistically-shaped **synthetic replacements** so the notebook runs end-to-end. Swap in the real datasets for genuine results.
- Fixed a severe performance bug: `data.values` was being called ~150,000 times inside a nested loop instead of once, making the cell effectively hang
- The notebook stopped short after `from apyori import apriori` with no actual call — completed it with a working Apriori run and results printout
- Fixed an incomplete `import matplotlib.pyplot` (missing `as plt`)

## Known limitations to be aware of

- **`02_cnn_image_classification.ipynb`** contains three unrelated topics (CNN architecture, regex, NLP/LSTM) in one file, despite the filename. This is documented honestly in the notebook itself rather than hidden — treat it as a reference/cheatsheet notebook, not a single cohesive project.
- **`movie_recommendation_system.ipynb`** runs on synthetic data (see above) — the notebook's documentation cell says this explicitly.

## Suggested next steps

1. **GitHub**: `git init`, commit this folder, push to a new repo — the structure is ready as-is.
2. **Kaggle**: for each notebook you want to publish individually, create a new Kaggle Notebook, upload the relevant CSV(s) from `Datasets/` as a Kaggle Dataset, and paste in the notebook's code cells. Start with your strongest 2-3 (e.g. the diabetes logistic regression, the bank marketing case study, or the vehicle PCA case study) rather than publishing all 28 at once.
3. Consider swapping in the real MovieLens and market-basket datasets for the recommendation notebook if you still have them, to replace the synthetic placeholders with genuine results.
