# Project 3: Machine Learning Foundations

Predicting whether a person earns more than $50,000 a year from 1994 U.S. Census data (UCI Adult Income dataset), using a supervised binary classification pipeline built with scikit-learn, followed by a fairness audit across sex and race.

**Headline result:** a tuned random forest reaches F1 0.702 and ROC-AUC 0.918 on a held-out test set (a majority-class baseline gets 76.1% accuracy and F1 0). The fairness audit finds the model catches 85.6% of high-earning men but only 65.7% of high-earning women, and removing sex and race as inputs does not close that gap.

## Repository contents

| Path | What it is |
|---|---|
| `modeling.ipynb` | The full analysis: problem definition, cleaning, EDA, preprocessing, model training and tuning, evaluation, fairness audit, summary. Runs top to bottom. |
| `adult.csv` | The dataset (48,842 rows x 15 columns), built from the UCI `adult.data` and `adult.test` files with a header row added. Raw values are kept as published (`?` for missing, trailing `.` on test labels); the notebook cleans them. |
| `requirements.txt` | Exact package versions (`pip freeze`) used to produce the results. |
| `module_summary.pdf` / `.docx` | The written APA-style report. |
| `figures/` | The 10 figures saved by the notebook (regenerated on every run). |
| `data_source/adult.names` | The original UCI dataset description. |
| `Project 3 — Machine Learning Foundations.docx`, `APA style.docx`, `apa-7-template-word-doc.docx` | Assignment brief and APA materials. |

## Requirements

- **Python 3.13.** The pinned versions (numpy 2.5, pandas 3.1, scikit-learn 1.9) need a recent Python. Check with `python3 --version`.
- macOS or Linux. Windows works too; see the Windows notes below.
- About 1 GB of disk for the virtual environment.

If you don't have Python 3.13, install it from [python.org](https://www.python.org/downloads/) or with Homebrew (`brew install python@3.13`).

## Setup (one time)

Run these from the project folder.

1. Create a virtual environment:

   ```bash
   python3.13 -m venv .venv
   ```

2. Activate it (do this in every new terminal before working on the project):

   ```bash
   source .venv/bin/activate
   ```

3. Install the exact package versions:

   ```bash
   python -m pip install --upgrade pip
   ```

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Check that nothing conflicts (should print `No broken requirements found.`):

   ```bash
   python -m pip check
   ```

5. Register the environment as a Jupyter kernel. The notebook expects a kernel named `project3-ml`:

   ```bash
   python -m ipykernel install --user --name project3-ml --display-name "Python (project3-ml)"
   ```

## Running the notebook

### Interactively

```bash
jupyter lab modeling.ipynb
```

Make sure the kernel in the top-right corner says **Python (project3-ml)**, then choose **Run > Restart Kernel and Run All Cells**. The whole notebook takes about 1–2 minutes; the random forest grid search (60 fits) is the slowest step, around 40 seconds.

The first code cell prints the library versions. They should match `requirements.txt` (for example, `scikit-learn 1.9.1`). If they don't, the kernel is pointing at the wrong Python; see Troubleshooting.

### Headless (no browser)

To execute every cell and save the outputs back into the notebook:

```bash
jupyter nbconvert --to notebook --execute --inplace modeling.ipynb
```

Either way, the figures in `figures/` are rewritten. All randomness uses `RANDOM_STATE = 42`, so the numbers should match the report.

## Troubleshooting

**Library versions in the first cell don't match `requirements.txt`.** The `project3-ml` kernel is using a different Python (often Anaconda's). Re-run setup step 5 with the virtual environment activated. On macOS you can check which Python it uses:

```bash
cat ~/Library/Jupyter/kernels/project3-ml/kernel.json
```

The first `argv` entry should be this project's `.venv/bin/python`.

**`pip install` fails on numpy, pandas or scikit-learn.** The virtual environment was probably created with an older Python. Delete `.venv` and redo setup with Python 3.13.

**`FileNotFoundError: adult.csv`.** Jupyter was started from a different folder. Start it from the project folder, since the notebook reads `adult.csv` by relative path.

**Windows.** Use `py -3.13 -m venv .venv` to create the environment and `.venv\Scripts\activate` to activate it. `appnope` in `requirements.txt` is macOS-only; if it fails to install, remove that line and retry.

## Updating dependencies

If you install or upgrade a package, regenerate the pinned list with the environment active:

```bash
python -m pip freeze > requirements.txt
```

## Data source

Becker, B., & Kohavi, R. (1996). *Adult* [Data set]. UCI Machine Learning Repository. https://doi.org/10.24432/C5XW20 (licensed CC BY 4.0).
