# Real-data evaluation — 2026-11-02

Input: `Cancer/cancer.csv`. Dataset SHA-256: `b6dc1c915648e11e2e15e35c8b64107d9fefd1c9079215eb769ae9abd0c45a36`.

Model selection used only training-fold CV. One final holdout was reserved before preprocessing. The full CV leaderboard, dependency versions, seed, feature schema and split sizes are in [experiments/report.json](experiments/report.json).

- Model: **LogisticRegression**
- Training rows: **455**; holdout rows: **114**
- Selection metric: `f1_macro`; selected CV score: **0.974320**
- Dummy baseline CV score on the same folds: **0.385135**
- Five CV folds; seed 42; tuning: **True**
- Identical raw predictor rows grouped: **False**

| Metric | Holdout value |
|---|---:|
| accuracy | 0.956140 |
| balanced_accuracy | 0.960317 |
| f1_macro | 0.953534 |
| roc_auc | 0.995370 |
| average_precision | 0.997065 |

Accuracy 95% Wilson interval: [0.9014, 0.9811]. This treats holdout observations as independent; grouped duplicates reduce the effective sample size, so the interval is descriptive rather than a complete uncertainty estimate.


## Dataset-specific interpretation

All 569 feature rows and Outcome labels exactly match sklearn.datasets.load_breast_cancer (Wisconsin diagnostic breast cancer). Label 0 = malignant; label 1 = benign. Thus the reported positive_class=1 ROC/AP concern the benign class; use class-0 recall when discussing malignancy detection. This confirms the data contents, not its source license or clinical validity.

## Reproduce

Activate an environment with the project's requirements, change to the repository root, and run:

```cmd
python -m mlproject train --data "D:\data\Cancer\cancer.csv" --output artifacts --folds 5 --seed 42 --tune
```

Results were obtained with the dependency versions recorded in the JSON report. Different versions can change results slightly. Negative MAE selection scores are higher-is-better. CV scores are selection estimates and optimistic; the holdout is the final internal result. Hyperparameter tuning is enabled on the smaller datasets and diabetes; for credit, soccer, student and churn the default bounded candidate comparison is used. Scores are not directly comparable across datasets or tasks.

Do not tune against the now-observed holdout or claim improvement over the legacy notebooks: leakage, split policies and preprocessing differ. Raw data, row-level predictions and trained pickle artifacts are not included in this evidence commit.
