# UniSched — Academic Outcome Analysis

**Turn assessment spreadsheets into student-level outcome reports.**

The application currently included in this repository is a Python desktop tool for evaluating course and program outcomes. It combines student grades, assessment weights and outcome relationships, then exports per-student results to Excel.

Built with Tkinter, pandas and openpyxl, it provides a local, Turkish-language interface for loading tables, adding records, running calculations and saving reports.

> **Repository scope:** the current source is `lab/main.py`, an outcome-assessment application. Earlier documentation described a Django timetable scheduler, but that implementation is not present in the current repository. This README documents the code that is available here.

## User Workflow

1. Open the desktop application and choose the student-grade, course-outcome and program-outcome workbooks.
2. Inspect the loaded tables using the corresponding table buttons. The interface also supports creating tables and adding rows.
3. Enter a course name in **Ders Adı** to identify the exported reports.
4. Select **Hesaplamaları Yap** to calculate the intermediate and student-level result tables.
5. Select **Tabloları Kaydet (4-5)** to write the course-outcome and program-outcome reports.

This workflow keeps input inspection and report generation in one window. Numeric validation checks grades against a 0–100 range and relationship values against a 0–1 range. Non-numeric values are converted to missing values with a warning.

## Inputs and Outputs

| Input | Purpose | Expected layout |
| --- | --- | --- |
| Student grades | Scores for each assessment | First row contains headers; first column identifies the student; remaining columns contain grades |
| Course outcomes | Relationships between outcomes and assessments | First row supplies percentage weights after the first cell; second row contains headers; numeric relationship values are 0–1 |
| Program outcomes | Relationships between program and course outcomes | Second row contains headers; first column identifies the program outcome; remaining values are 0–1 |

The repository includes `lab/notlar.xlsx`, `lab/degerlendirmeler.xlsx` and `lab/progcikti.xlsx` as existing input files. Match the assessment and outcome ordering across the inputs: the calculations use column and row positions rather than joining tables by identifier.

Exports are written to the working directory:

- `<course-name>_tablo4.xlsx`: course-outcome results grouped by student.
- `<course-name>_tablo5.xlsx`: program-outcome results grouped by student.

If the course-name field is empty, the filename prefix is `dersadi`. Table 3 is an intermediate calculation held in memory rather than a separate exported workbook.

## Calculation Pipeline

```text
Assessment weights + course-outcome relationships
                      ↓
              Weighted outcome matrix (Table 3)
                      ↓ + student grades
              Course-outcome results (Table 4)
                      ↓ + program-outcome relationships
              Program-outcome results (Table 5)
```

The first stage weights each assessment-to-outcome relationship. The second combines those weighted values with each student's grades and calculates totals, maximums and achievement percentages. The final stage combines course-outcome achievement with program-outcome relationships to produce program-level achievement rates.

The implementation handles missing numeric values explicitly during accumulation. Some calculated percentages are truncated to two decimal places. Review the input alignment and generated reports before using the results for an assessment decision.

## Technical Structure

The application is contained in [lab/main.py](lab/main.py):

| Component | Responsibility |
| --- | --- |
| `DegerlendirmeIslemleri` | Workbook loading, numeric checks, DataFrame calculations and Excel exports |
| `DegerlendirmeApp` | Tkinter windows, file selection, table previews, row entry and user messages |
| pandas | Tabular data manipulation and Excel input |
| openpyxl | Workbook access and formatted result output |
| Tkinter / ttk | Desktop interface and table views |

There is no web server or database setup in this version. Input and output stay in local files.

## Run Locally

Install Python 3 with Tkinter support, then:

```sh
git clone https://github.com/Tunayblg/UniSched.git
cd UniSched
python -m venv .venv
```

Activate the environment:

```powershell
# Windows PowerShell
.\.venv\Scripts\Activate.ps1
```

```sh
# macOS / Linux
source .venv/bin/activate
```

Install the external dependencies and launch from the application directory:

```sh
python -m pip install pandas openpyxl
cd lab
python main.py
```

`python -m tkinter` can be used to check that Tkinter is available. Prefer `.xlsx` inputs; although the picker displays `.xls`, the course-outcome loader uses openpyxl, so legacy `.xls` files are not supported throughout the workflow.

## Working with Your Data

Use copies of input workbooks when experimenting: adding rows can write back to the selected input file. When a table is created without an existing file path, the application writes files such as `notlar_new.xlsx`, `degerlendirmeler_new.xlsx` and `progcikti_new.xlsx`.

Dependency versions are not pinned in this repository. The present application does not include timetable generation, classroom scheduling, user accounts or Django administration.
