# F1 Pace & Grid as Simple Predictors of Race Outcomes (Math 261A — Project 1)

**Author:** Suneel Chandra Vanamu
**Course:** MATH 261A
**Repo:** [https://github.com/Suneel1508/MATH261A-project1](https://github.com/Suneel1508/MATH261A-project1)

## Overview

This project asks a practical question: **How do a driver’s on-day pace and starting position relate to where they finish?** Using the public **F1DB** CSV release, I build simple one-predictor regression models on the **2014–present hybrid era** to quantify:

* **Model A:** Best-lap pace deficit → **finishing position**
* **Model B:** Starting grid position → **finishing position**

I also include quick bivariate EDA and standard residual checks. The goal is a clean, interpretable read in **positions per second** (pace) and **positions per grid place** (track position).

## Data

* **Primary source:** F1DB (Formula 1 Database) — CSV release
  GitHub: [https://github.com/f1db/f1db](https://github.com/f1db/f1db)
  I use the race results, fastest laps, pit stops (as needed), and race metadata to assemble a **driver–race** table for 2014+.
  Dataset license: F1DB is released under CC BY 4.0. See the F1DB repo for details.

* **Files expected in this repo (place in `data/`):**

  * `f1db-races.csv`
  * `f1db-races-race-results.csv`
  * `f1db-races-fastest-laps.csv`

## Research Questions

1. **Does being slower on your best lap (vs the race’s fastest lap) predict finishing further back?**
2. **Does starting farther back on the grid predict finishing further back?**

Both are framed as **simple linear regressions** with a single, interpretable predictor.

## Repository Structure

```
MATH261A-project1/
├─ README.md # Project overview, how to run, licensing & credits
├─ MATH261A-project1.Rproj 
├─ .gitignore

├─ analysis/ 
│ └─ Math261A_Project1.Rmd # R Markdown with code

├─ paper/ # Final paper assets
│ ├─ Math261_Project_Report_v1.qmd # Main Quarto source for the paper
│ ├─ references.bib # Bibliography used by the QMD
│ └─ Math261_Project_Report_v1.pdf # Rendered PDF 

├─ data/ # Data 
│ ├─ F1db-csv/ # Full F1DB CSV release (raw, unchanged)
│ │ ├─ f1db-races.csv
│ │ ├─ f1db-races-race-results.csv
│ │ ├─ f1db-races-fastest-laps.csv
│ │ └─ ... (other F1DB tables)
│ └─ F1_Datasets_for_Project/ # Subset/derived files used in this project
│ ├─ f1db-races-race-results.csv
│ ├─ f1db-races-fastest-laps.csv
│ └─ ... (any cleaned/merged CSVs)
```

* **Project\_Report.qmd** knits to PDF and contains the full analysis, figures, and write-up.
* Paths inside the `.qmd` assume CSVs live in `data/`.

## Reproducibility & Setup

1. **Requirements**

   * R (4.3+ recommended)
   * Quarto (or R Markdown)
   * Packages: `tidyverse` (`readr`, `dplyr`, `tidyr`, `stringr`), `ggplot2`, `broom`, `janitor`, `knitr`, `patchwork` (optional)

2. **Install packages**

   ```r
   install.packages(c("tidyverse","broom","janitor","patchwork","knitr"))
   ```

3. **Render the report**

   ```bash
   # from the repo root
   quarto render Project_Report.qmd
   # output: Project_Report.pdf
   ```

> If knitting from RStudio, just open `Math261A_Project_Report_v3.qmd` and click **Render**.

## How to Run the Analysis (quick start)

* Mention the path of F1DB CSVs in `data_dir in the .rmd or .qmd file`
* Open `Math261A_Project_Report_v3.qmd`
* Knit to PDF; the report:

  * Loads/cleans data (2014+)
  * Builds variables: **finish position**, **best-lap deficit (sec)**, **grid position**
  * Shows **EDA** (univariate + points-only bivariate)
  * Fits **Model A** and **Model B**
  * Reports **R² / adj-R²**, **Residual SE (in positions)**, **slope + 95% CI**
  * Includes **residual vs fitted** and **Q–Q** diagnostics

## Results (at a glance)

* **Grid → Finish** shows a **stronger, steadier** relationship (higher R²; smaller error in positions).
* **Pace deficit → Finish** is **meaningful but noisier**—race dynamics (safety cars, traffic, strategy) add spread.
* Both effects are stated in plain units (**positions / grid place**; **positions / second**).

(Exact values are reported in the PDF under *Results*.)

## External Resources

For preliminary research and troubleshooting, I consulted:

* **F1DB** documentation and CSV release: [https://github.com/f1db/f1db](https://github.com/f1db/f1db)
* **LLM-based assistants** (e.g., **ChatGPT Edu**) for brainstorming and draft phrasing
* **Online forums** (e.g., **Stack Overflow**) for minor R/ggplot tips


## Acknowledgments

* **Project template:** adapted from **Peter Gao’s** MATH 261A template
  [https://github.com/peteragao/MATH261A-project-template/tree/main](https://github.com/peteragao/MATH261A-project-template/tree/main)

* **Documentation inspiration:** **Sai Pranav Sripathi**
  [https://github.com/SaipranavSripathi/Math261-LinearRegression-AsiaCupCricketAnalysis](https://github.com/SaipranavSripathi/Math261-LinearRegression-AsiaCupCricketAnalysis)

* **Dataset:** thanks to the **F1DB** community for maintaining a high-quality public dataset.

## Citation

Please cite the data and tools if you reuse this work. A `references.bib` is included; add citations in the `.qmd` using `@f1db-github`, `@R-base-2024`, `@ggplot2-book-2016`, `@dplyr-2023`, etc.

## Notes & Limitations

* Analysis uses the **hybrid era (2014+)** for comparability across rules and tracks.
* Models are **one-predictor** by design for interpretability; many unmodeled race factors remain.
* Finishing position is an **integer rank** and bounded (1..N), so linear fits are an approximation; diagnostics are included.
