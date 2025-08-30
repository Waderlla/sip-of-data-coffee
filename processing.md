# Data Processing

This document describes the complete data preparation process for the project **“Sip of Data: Stories Over Coffee”**. The description is 100% consistent with the actual workflow – all steps in Power Query were performed via the **GUI** (menu). No DAX code is shown here (it lives in a separate file).

---

## 1) Country Cleaning (Python + Excel)

**Working files:**
- `notebooks/clean_and_map_countries.ipynb`
- `data/unique_countries.csv` – list of raw country names (extracted from source)
- `data/country_mapping.csv` – draft mapping (after first run)
- `data/country_mapping_final.csv` – final dictionary (after manual corrections and ISO completion)

**Steps:**
1. The notebook `clean_and_map_countries.ipynb` automatically processed `unique_countries.csv` using `pycountry`.  
   - It generated `country_mapping.csv` (draft) and then `country_mapping_final.csv` after manual corrections of names and ISO codes.  
   - These operations were performed fully inside the notebook – not manually in Excel.  
2. In Excel, I opened the original survey data file and **manually copied and pasted** two columns from `country_mapping_final.csv`:  
   - **Country = Cleaned Country**  
   - **ISO = ISO Code**  
3. The enriched dataset was saved as `coffee_survey_clean.xlsx` / `coffee_survey_clean.csv` and used in Power BI.

> Note: the **ISO** column was ultimately **not used** in the visuals – it remains for educational purposes and as an example of data enrichment.

---

## 2) Loading Data into Power BI

- The data was loaded into Power BI from **Excel** (`coffee_survey_clean.xlsx`) containing cleaned countries.  
- For reproducibility, a matching **CSV** file (`coffee_survey_clean.csv`) is included in the repository.

---

## 3) Transformations in Power Query (GUI)

All of the following steps were performed using Power Query’s GUI.

### 3.1 Add Index
- Added column **`Index`**, starting at **1** (*Add Column → Index Column → From 1*).  
- Purpose: unique record identifier and a stable key to join unpivoted tables.

### 3.2 Shorten / Standardize Labels
- **After loading into Power Query**, long responses were shortened using **Replace Values** (Transform → Replace Values), e.g.:  
  - “Taste and enjoyment – I simply like the taste of coffee” → **“Taste and enjoyment”**  
  - “No, I don’t experience any negative effects” → **“No effects”**  
- Wording was standardized, e.g.:  
  - **“Several times a week” → “A few times a week”**  
  - **“Several times a month” → “A few times a month”**  
- Purpose: cleaner labels on visuals and consistent slicers.

### 3.3 Sorting Indices (Conditional Columns)
- Created **sorting helper columns** (via *Conditional Column*) to enforce logical rather than alphabetical ordering:  
  - **`FirstCoffee_idx`** – order of first coffee times (from “Right after waking up” → … → “No fixed time / it varies”).  
  - **`How often drink index`** – frequency of drinking (1 = “Every day”, …, 5 = “Rarely/Occasionally”).  
  - **`relationship_index`** – coffee relationship scale (from “I’m a true coffee enthusiast” → “Coffee is just a drink to me”).  
  - **`CupsPerDay_label`** – numeric values converted to readable labels: “1 cup”, “2 cups”, “3-4 cups”, “5 or more”.  

### 3.4 Unpivot – Multiple Choice Questions
Unpivot was applied to **two** multiple-choice questions:  
- **Reasons for drinking coffee**  
- **Negative effects of drinking coffee**  

**Procedure (GUI + custom column):**
1. **Duplicated** the main table.  
2. Added a **Custom Column** replacing commas with a pipe `|` marker to prepare for splitting. Example (Reasons table):

   ```M
   = Table.AddColumn(#"Removed Columns", "Custom", each 
       Text.Replace(
       Text.Replace(
       Text.Replace(
       Text.Replace(
       Text.Replace(
           Text.From([#"What are your main reasons for drinking coffee? "]),
           ", R", "|R"),
           ", T", "|T"),
           ", E", "|E"),
           ", C", "|C"),
           ", S", "|S"))
   ```
   (similar transformation applied for the “Negative effects” column with other markers).  

3. **Split Column by Delimiter** (`|`) into several columns.  
4. **Unpivot Columns** – transformed wide structure into long (1 row = 1 selected answer).  
5. Removed the original multi-select column and renamed **Custom** back to the original question name.  
6. Left only: **`Index`** + the new long-format answer column.  
7. Created relationships between:  
   - `coffee_survey_clean (1)` → `Unpivot_reasons (*)`  
   - `coffee_survey_clean (1)` → `Unpivot_experience (*)`  
   joined on **`Index`**, with **both-directional filtering** enabled for cross-filtering in visuals.  

> Label shortening (Replace Values) was applied **after unpivot**, directly on the new long-format tables.

---

## 4) Data Model (Power BI)

- **Main table:** `coffee_survey_clean` – contains individual survey responses (with shortened labels and sorting indices).  
- **Supporting tables:**  
  - `Unpivot_reasons` – long format for reasons to drink coffee (1 row = 1 reason).  
  - `Unpivot_experience` – long format for negative effects (1 row = 1 effect).  
- **Relationships:** `coffee_survey_clean (1)` → `Unpivot_reasons (*)` and `coffee_survey_clean (1)` → `Unpivot_experience (*)`, joined on **`Index`**, both set to **bidirectional filtering**.  

---

## 5) DAX Measures (overview)

- Built a set of **percentage and count measures** for single-choice questions.  
- Implemented a **disconnected slicer** for *cups per day* (labels: “1 cup”, “2 cups”, “3-4 cups”, “5 or more”).  
- Full DAX code is stored separately in **[`dax_measures.txt`](./dax_measures.txt)**.

---

## 6) Summary of Process

1. **Country cleaning** (Python + manual copy/paste in Excel) with Country/ISO enrichment (ISO retained for educational use).  
2. **Load into Power BI** (Excel; CSV copy included for reproducibility).  
3. **Shorten/standardize labels** in Power Query (Replace Values).  
4. **Sorting indices** for ordered categories (Conditional Columns).  
5. **Unpivot** for two multi‑select questions using custom column + delimiter split, linked 1:* by `Index` with both‑directional filtering.  
6. **DAX measures** for single-choice questions and interactivity (separate file).  

> The pipeline was designed to produce a clear, interactive, and maintainable dashboard – and reflects the actual GUI‑based workflow in Power Query and Power BI.

---

## Manually Written M Code

Most transformations were done via the Power Query GUI (auto‑generated M).  
However, the following helper columns were **written manually in M** to ensure correct, reproducible sorting across visuals.  
These fragments reflect the final, shortened labels used in the model.

**How often drink index**
```M
if [#"How often do you drink coffee?"] = "Every day" then 1
else if [#"How often do you drink coffee?"] = "A few times a week" then 2
else if [#"How often do you drink coffee?"] = "Once a week" then 3
else if [#"How often do you drink coffee?"] = "A few times a month" then 4
else if [#"How often do you drink coffee?"] = "Rarely/Occasionally" then 5
else null
```

**FirstCoffee_idx**
```M
let v = [#"At what time do you usually have your first coffee of the day?"] in
if v = "Right after waking up" then 1
else if v = "Within the first hour after waking" then 2
else if v = "Late morning" then 3
else if v = "Early afternoon" then 4
else if v = "Late afternoon" then 5
else if v = "Evening" then 6
else if v = "No fixed time / it varies" then 7
else null
```

**relationship_index**
```M
let r = [#"Which of the following statements best describes your relationship with coffee? "] in
if r = "I’m a true coffee enthusiast" then 1
else if r = "I can’t imagine a day without coffee" then 2
else if r = "Coffee is an important part of my day" then 3
else if r = "I like coffee, but I could do without it" then 4
else if r = "Coffee is just a drink to me" then 5
else null
```

**CupsPerDay_label** (created via Conditional Column to label numeric answers)
```M
if [#"How many cups of coffee do you usually drink per day?"] = "1" then "1 cup"
else if [#"How many cups of coffee do you usually drink per day?"] = "2" then "2 cups"
else if [#"How many cups of coffee do you usually drink per day?"] = "3–4" then "3-4 cups"
else if [#"How many cups of coffee do you usually drink per day?"] = "5 or more" then "5 or more"
else null
```