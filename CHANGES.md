# Audit and refactor

The original notebook fits SMOTE before the split and fits the scaler again on test rows. The new workflow uses fold-local preprocessing and class weights; test observations are never resampled. Outcome is the original target name, but the dataset identity is unverified: confirm that it is genuinely a cancer dataset before interpreting it.

Original algorithms and historical outputs remain in legacy/. No production dataset was available during refactoring; new real-data metrics remain unmeasured. See README.md for the reproducible evaluation protocol.
