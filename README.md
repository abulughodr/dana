# Dana — Jordan & Middle East Botanical Database

A React application for building a scientific botanical database focused on **Jordan and Middle East flora**.

The app searches for a plant and generates a complete, structured botanical profile. It combines curated botanical references, live taxonomic validation, and AI-assisted synthesis while enforcing strict source priority and explicit handling of missing data.

## What the app does

For each plant query, the app:

1. Resolves accepted naming and family data.
2. Injects regional context (Jordan/Middle East).
3. Generates a strict JSON profile in a fixed schema.
4. Renders the output in an interactive UI.
5. Preserves citations per field and supports export.

The generation pipeline is designed to avoid fabrication:

- Priority source ordering is enforced.
- Missing values should be marked as `DD` (data deficient), not guessed.
- Output is constrained to the expected JSON object format.

## How to use this program

> This repository currently documents the app behavior. Use the steps below inside your Dana React app workspace.

### 1) Configure required keys and data

Before running the app, make sure these are available in your environment or app settings:

- **Anthropic API key** (Claude generation)
- **POWO API access** (accepted name and family lookup)
- **Embedded Jordan Red List dataset** (909 species)
- **Internal botanical reference prompt/library** used by the app

### 2) Start the app

Run the React app in your normal development workflow (for example: install dependencies, then run your dev server), and open it in the browser.

### 3) Single-plant workflow

1. Enter one scientific or common plant name in the search input.
2. Submit search.
3. Review the generated 11-section profile.
4. Expand/collapse sections as needed.
5. Check badges (IUCN + Jordan native status).
6. Verify per-field APA citations.

### 4) Batch workflow (multiple plants)

1. Open batch/list mode.
2. Paste names as comma-separated or line-separated text.
3. Start processing (plants are processed sequentially).
4. Watch results appear in the left panel.
5. Click each plant to inspect the full profile on the right.
6. Reload prior work from **Saved sessions** when needed.

### 5) Export results

For any completed profile, use:

- **Word ↓** for `.docx` (zipped) report output.
- **HTML ↓** for print-ready A4 page (can be printed to PDF).
- **Copy JSON** for structured raw data.

### 6) Read missing values correctly

If a value appears as `DD`, that means **data deficient** (no reliable evidence found in the allowed source chain), not an error.

### 7) Quality checklist before using output

- Confirm accepted scientific name/family from POWO.
- Ensure Jordan/Middle East context is present.
- Confirm each populated field has citation support.
- Treat uncited/uncertain values as provisional.

## Data sources (priority order)

1. **Internal Reference Library**
   - Large embedded system prompt with curated data from major references, including:
     - *Flora Palaestina*
     - *Flora of Turkey*
     - *Flora of Iraq*
     - *Flora of Egypt*
     - PFAF
     - USDA
     - UF/IFAS
     - AUB Landscape Plants
     - and related botanical resources
2. **POWO (Plants of the World Online, Kew Gardens)**
   - Live API call for accepted name and family.
3. **Jordan Red List dataset**
   - Preloaded embedded dataset (Taifour & El-Oqlah, 2014).
   - Contains 909 plants with IUCN status, Arabic/English names, and Jordan distribution.
4. **Claude (Anthropic API)**
   - Fills unresolved fields using constrained prompt instructions and cross-referenced scientific context.
5. **No guessing policy**
   - Only approved resources and supplied references should be used.

## 11-section botanical profile schema

Each plant profile is generated with the following sections:

1. **Taxonomy & Nomenclature**
   - Scientific name, family, synonyms, common names (EN/AR), IUCN status, Jordan native status, phytogeographic region.
2. **Growth Form & Life History**
   - Growth form, life span.
3. **Vertical Structure**
   - Height, DBH, growth rate, longevity, stem architecture, branching.
4. **Crown Geometry & Canopy**
   - Crown diameter, shape, density, LAI, canopy porosity.
5. **Leaf Functional Traits**
   - Leaf type, arrangement, habit, thorns, pubescence, shade quality, color variation.
6. **Root System**
   - Root type, depth class, spread radius.
7. **Reproductive & Phenological**
   - Flowering/fruiting timing, germination ecology, age at maturity.
8. **Ecological Requirements**
   - Elevation, temperature range, stress tolerances, soil, pH, salinity, light.
9. **Geographic Distribution**
   - Wildlife value, Jordan distribution, habitat type.
10. **Landscape Use**
    - Landscape applications and notes.
11. **Historical & Cultural Use**
    - Edible, medicinal, construction, fuel, religious, and economic uses.

Every field should display an APA-style citation indicating the supporting source.

## Search modes

### 1) Single plant mode

- Enter one plant name.
- Receive the full 11-section profile.
- UI includes expandable/collapsible sections, IUCN badge, native status badge, and download actions.

### 2) Batch mode (list of plants)

- Paste comma-separated or line-separated plant names.
- Plants are processed sequentially with delay control to reduce rate-limit risk.
- Left panel populates incrementally with completed results.
- Clicking a plant opens the full profile in the right panel.

## Session persistence

Completed batch sessions are saved via `window.storage` (Cowork API integration).

Users can reopen prior sessions from the **Saved sessions** bar without rerunning generation.

## Export and data actions

- **Word ↓**
  - Creates a formatted `.docx` containing all 11 sections and citations, packaged as ZIP.
- **HTML ↓**
  - Creates a print-ready A4 HTML report with IUCN badge, tabular sections, and references list (suitable for browser print-to-PDF).
- **Copy JSON**
  - Copies the raw structured data object to clipboard.

## AI generation flow

Per plant, the app prompts Claude with:

- target plant name
- POWO context
- Jordan Red List context
- strict regional and schema constraints

Expected behavior:

- Return **only** the JSON object.
- Follow source-priority rules.
- Use `DD` for missing evidence.
- Avoid unsupported claims.

## Intended use

This project is intended for scientific and landscape-relevant botanical profiling, especially for Jordan and the broader Middle East ecological context.
