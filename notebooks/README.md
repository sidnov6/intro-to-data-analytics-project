# Notebooks

Number notebooks in execution order and give each one a single purpose. A likely structure after topic selection is:

1. `01_data_quality_and_cleaning.ipynb`
2. `02_exploratory_analysis.ipynb`
3. `03_baseline_and_main_analysis.ipynb`
4. `04_evaluation_and_robustness.ipynb`

Notebook rules:

- explain the question before showing code;
- run top-to-bottom from a fresh kernel;
- import reusable work from `src/`;
- set random seeds where randomness is used;
- show units, sample sizes, and important quality checks;
- do not hide errors, exclusions, or unsuccessful comparisons;
- avoid absolute paths tied to one teammate's computer; and
- keep the final saved version readable enough to support the report and Q&A.

The exact notebook list will be decided after the use case and evaluation plan are locked.
