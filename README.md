# Bagging: Random Forest

One decision tree is built entirely from its training rows, and swapping those rows gives a
genuinely different tree. This repository measures that instability, then removes most of it
the way every modern tree method does: build **many** trees on resampled data and combine
them.

It runs on two datasets. **1,030 laboratory mixes of concrete**, where the
target is the compressive strength the hardened sample withstood, carries notebooks 01 to 03
and 05. **45,222 census records**, where the target is whether a person earned more than
$50,000, appears in notebook 04 as the case where a forest out of the box is *worse* than a
single pruned tree. Both ship in `data/`, so every notebook runs on its own and offline.

```
01  one tree, two halves of the data, two different answers
        |
        v
02  bagging: average twenty trees, beat all twenty
        |
        v
03  random forests: take away features too, and the trees stop agreeing
        |
        v
04  out-of-bag: score a forest with the rows each tree never saw
        |
        v
05  the whole cycle, on your own, against the best single tree
```

## Learning Objectives

By the end of this repository, you should be able to:

- Measure how far a model's predictions move when its training rows change, and name that
  movement variance.
- Draw a bootstrap sample, and state what share of the original rows it contains and why.
- Build a bagged ensemble by hand, and explain why its average beats every model that went
  into it.
- Fit a random forest, and set `max_features` to trade each tree's accuracy against the
  ensemble's.
- Compute an out-of-bag score, and use it to choose settings without touching the test set.
- Compare a tuned forest with a single tree on error, on where that error falls, and on which
  features it relied upon.

## Learning Path

**Scope** marks the core path every student is expected to complete. Optional lessons stay in
the repository and are worth returning to, but the day does not depend on them. **Units** are
indicative pacing, where one unit is 45 minutes.

| File / Folder | Description | Scope | Units |
| --- | --- | --- | --- |
| [**1 - How Much of a Tree Is Luck?**](01_how_much_of_a_tree_is_luck.ipynb) | Two trees from disjoint halves of one experiment disagree by 7.57 MPa. Variance and bias named, the bootstrap sample introduced, and twenty trees measured. | Core | 1.5 |
| [**2 - Bagging**](02_bagging.ipynb) | Twenty trees that are each worse than one tree, averaging into something better than all of them. Why it works, how many trees, and the bias it cannot touch. | Core | 1.75 |
| [**3 - Random Forests**](03_random_forests.ipynb) | 182 of 200 bagged trees ask the same first question. `max_features` breaks that up, each tree gets worse, the forest gets better, and extra trees go one step further. | Core | 1.25 |
| [**4 - Out of the Bag**](04_out_of_bag.ipynb) | The 37% of rows each tree never saw, turned into a validation score. On the census data a default forest loses to one pruned tree, and the out-of-bag score finds the fix. | Core | 1.25 |
| [**5 - The Whole Cycle, On Your Own**](05_exercise_full_cycle.ipynb) | Baseline, forest, out-of-bag tuning, one test score, error analysis and importance. Seven exercises, no new material. | Core | 2.25 |

### Additional Folders and Files

| File / Folder | Description |
|---|---|
| [**Data**](data/) | `concrete.csv`: 1,030 mixes, seven ingredients and an age in days, with `compressive_strength` in MPa as the target. `census.csv`: 45,222 records, 8 categorical and 5 numeric columns, with `income` as the target. Both are modified from the UCI originals; see the references below for what changed. |
| [**Solutions**](solutions/) | Reference solutions for every notebook. |
| [**pyproject.toml**](pyproject.toml) | Project configuration and dependencies. |
| [**uv.lock**](uv.lock) | Dependency lock file. |

## Setup

> [!NOTE]
> Throughout these steps, text in angle brackets like `<repo-name>` is a **placeholder**. Replace it, including the `< >` brackets, with your own value. For example, `cd <repo-name>` becomes `cd amle-bagging-random-forest`.

### 1. Create the Repository from the Template

Click **Use this template** on GitHub.

When creating the repository:

- Set yourself as the **Owner**
- Choose a repository name
- Disable **Include all branches**
- Click **Create repository**

> [!IMPORTANT]
> If you are working in pairs or groups, only **one person** should complete this step.

---

### 2. Add Collaborators (Pairs/Groups Only)

If working with teammates:

1. Open the repository on GitHub
2. Go to **Settings → Collaborators**
3. Add your teammates as collaborators
4. Share the repository link with your team

Teammates should accept the invitation before continuing.

---

### 3. Clone the Repository

Copy the SSH URL from the **Code** button on GitHub, then run:

```bash
git clone <copied-ssh-url>
```

The copied SSH URL will look like `git@github.com:<your-username>/<repo-name>.git`.

---

### 4. Move into the Project Folder and Install Dependencies

This installs all dependencies and creates a virtual environment in `.venv/`.

```bash
cd <repo-name>
uv sync
```

---

### 5. Open the Notebook

> [!NOTE]
> Make sure you open VS Code from the project root so it automatically detects the environment created by `uv sync`.

Launch VS Code in the project root folder:

```bash
code .
```

Then open `01_how_much_of_a_tree_is_luck.ipynb`, selecting the Python environment created by `uv sync` as the kernel. Work through the notebooks in order. All five are the core path.

## References & Further Reading

- [**scikit-learn: Ensembles**](https://scikit-learn.org/stable/modules/ensemble.html): Bagging, forests and extra trees, with the parameters that matter and the reasoning behind them.
- [**RandomForestRegressor**](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html) and [**RandomForestClassifier**](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html): Note the different `max_features` defaults, which notebook 03 is about.
- [**Breiman, Random Forests (2001)**](https://doi.org/10.1023/A:1010933404324): The paper the method comes from, including the out-of-bag estimate.
- [**Geurts, Ernst and Wehenkel, Extremely Randomized Trees (2006)**](https://doi.org/10.1007/s10994-006-6226-1): Where the random thresholds of notebook 03 come from.
- [**scikit-learn: Permutation feature importance**](https://scikit-learn.org/stable/modules/permutation_importance.html): The measure notebook 05 finishes with.
- [**UCI: Concrete Compressive Strength**](https://archive.ics.uci.edu/dataset/165/concrete+compressive+strength): Collected by I-Cheng Yeh, licensed CC BY 4.0. **Modified here:** converted from the original `.xls` to CSV and the nine columns renamed to short snake_case names. No rows or values were changed.
- [**UCI: Adult**](https://archive.ics.uci.edu/dataset/2/adult): Extracted from the 1994 US census by Barry Becker and Ronny Kohavi, licensed CC BY 4.0. **Modified here:** the 48,842 original rows are reduced to the 45,222 with no missing values, two columns are renamed, and the leading spaces inside its category values have been stripped.
