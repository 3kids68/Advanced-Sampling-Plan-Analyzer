# Advanced Sampling Plan Analyzer

A single‑page, front‑end web app for designing, looking up, and visualizing acceptance sampling plans.

### Features & Pages
- **Probability Distribution**: Interactive tool for exploring and comparing different probability distributions used in sampling plans.
- **AQL Plan Table Lookup (ANSI/ASQ Z1.4)**: Query `(n, c, code letter)` by lot size and inspection level. Supports Normal/Reduced/Tightened states and renders the OC curve.
- **Reverse Sampling Query**: Query sampling plan parameters in reverse to find optimal configurations.
- **C=0 Plan Lookup**: Zero‑acceptance plans (`c=0`) with OC curve. Supports Hypergeometric/Binomial/Poisson.
- **AQL–LTPD Balanced Plan**: Satisfy both AQL and LTPD with producer/consumer risk (α, β). The app searches/optimizes to recommend `(n, c)`, draws the OC curve, and provides an efficiency score plus improvement suggestions.
- **Multi‑plan Curve Comparison**: Compare OC/AOQ/ATI curves after exporting plans from other pages (efficiency scoring lives on the AQL–LTPD page).
- **Modern UI**: Glassmorphism styling, multiple light/dark themes, keyboard accessibility.

### One-click Help (per page)
- A consistent Help button is available in the top-right chart toolbar on every page.
- Clicking Help opens a unified modal with tabs:
  - Overview: What this page is for
  - Inputs: Meaning of inputs and when to use them
  - Outputs & Charts: What the outputs represent, chart axes/units
  - When to Use: Typical scenarios and guidance
- The Help content is contextual to the current page.

### Key Capabilities
- **Distributions**: Binomial, Poisson, Hypergeometric (recommended for small finite lots).
- **OC curves**: Real‑time rendering using the chosen distribution and `(n, c)`.
- **Efficiency score (AQL–LTPD)**: `E = 1 - |Pa_AQL - (1-α)| - |Pa_LTPD - β| - penalty`, with visual rating (🌟/✅/👍/⚠️/❌/🚫).
- **Suggestions**: Data‑driven hints on sample size, risk constraints, distribution choice, and AQL–LTPD ratio.
- **Export**: High‑resolution PNG and CSV (curve data + parameters). Some pages support JSON plan export.

### Project Structure (high‑level)
- `Advanced_Sampling_Plan_Analyzer_2510098.html`: Main app (HTML + styles + scripts).
- `master tables/`: Lookup data and helpers (`CodeLetterTable.js`, `normal.js`, `reduced.js`, `tightened.js`, `c=0 table.js`).
- `requirements/`: Page specs and algorithm details (`aql_ltpd_balanced_plan.md`, `aql_plan_table_lookup.md`, `c0_plan_table_lookup.md`, `efficiency_analysis_system.md`).
- `background_info/`: AOQ/ATI PDFs and related background materials.

See `requirements/ui_design_standards.md` for consolidated UI rules (left sidebar tabs, 4:3 chart sizing with fixed height, button sizing/radius, export button placement, and active-tab hover parity).

### Quick Start (Local)
This is a static front‑end app—no dependencies to install.
1. Open `Advanced_Sampling_Plan_Analyzer_2510098.html` directly in your browser, or
2. Serve locally to avoid file‑access restrictions:
   - Python: `python -m http.server 8000` → visit `http://localhost:8000/Advanced_Sampling_Plan_Analyzer_2510098.html`
   - Node (http‑server): `npx http-server -p 8000` → visit `http://localhost:8000/Advanced_Sampling_Plan_Analyzer_2510098.html`

### Usage Highlights
- **Probability Distribution**
  1) Explore and compare different probability distributions (Binomial, Poisson, Hypergeometric) used in sampling plans.
  2) Visualize distribution characteristics and understand their applications in sampling contexts.
- **AQL Table Lookup**
  1) Enter Lot Size and Inspection Level, choose an AQL.
  2) Get `code letter`, `n`, `c`. If the table value is `up/down`, a note is shown and the OC curve is not drawn.
- **Reverse Sampling Query**
  1) Query sampling plan parameters in reverse to find optimal configurations for specific requirements.
  2) Analyze existing plans to understand their characteristics and performance.
- **C=0 Plan**
  1) Enter Lot Size and AQL (or custom), choose a distribution.
  2) Lookup/compute `n` (with `c=0`), plot the OC curve; estimate LQ at Pa=10% if needed.
- **AQL–LTPD Balanced Plan**
  1) Enter AQL, LTPD, α/β, distribution, optional lot size N, and an optimization target.
  2) Click Calculate to get recommended `(n, c)`, Pa at AQL/LTPD, actual α/β, AOQL/ASN, the efficiency rating, suggestions, and the OC curve.
- **Multiple Plan Comparison**
  1) Import plans from other pages or manually enter plan parameters.
  2) Compare OC/AOQ/ATI curves across multiple sampling plans to evaluate performance differences.
  3) Use Help for definitions of AOQ/ATI and curve toggles.

### Export
- Chart toolbar provides `Export PNG` and `Export Data (CSV)`.
- Typical CSV columns: `x_defect_rate_percent,y_acceptance_prob,n,c,N,aql,ltpd,alpha,beta,distribution,label` (varies by page).

### Glossary
- **AQL**: Acceptable Quality Level.
- **LTPD**: Lot Tolerance Percent Defective.
- **OC curve**: Operating Characteristic (acceptance probability vs. defect rate).
- **α / β**: Producer’s / Consumer’s risk.
- **AOQL / ASN / ATI**: Average Outgoing Quality Limit / Average Sample Number / Average Total Inspection.

### Compatibility & Accessibility
- Works on modern browsers (Chrome/Edge/Firefox/Safari).
- Keyboard navigation, high‑contrast themes, and helpful tooltips are supported.

### Changelog (highlights)
- 2025‑01: Dynamic efficiency formula, tutorial integration, numerical stability and search convergence improvements.
- 2025‑10: AQL table page adds Normal/Reduced/Tightened states, dual cursor interactions, and high‑quality export refinements.
 - 2025‑10‑08: Unified theme bindings for all charts (legend, titles, ticks, grid, axis borders). Fixed light/dark inconsistencies on C=0 and AQL‑LTPD pages by updating the global theme updater to include `c0Chart` and `aqlLtpdChart`. Added UI standards §12.8.2/§12.8.3 and light‑theme tab contrast rule §9.6.3.
 - 2025‑10‑09: Align Actions button height with Parameters controls; set global `.btn` padding to `10px 12px` for consistent vertical sizing across sections. Add unified Help button and contextual modal on every page.

### License / Use
For educational and research use only. For commercial use or derivative works, please contact the author first.

### Rebuild Guide (from requirements + master tables)

This app can be fully reconstructed using only the specs in `requirements/` and data in `master tables/`. Follow this mapping:

1) UI & Layout
- Source: `requirements/ui_design_standards.md`
- Key directives:
  - Left sidebar navigation (`sidebar`) with vertical `.tab` buttons (width 100%, centered, no-wrap)
  - Main content width unchanged; expand outer container to include sidebar
  - Chart area keeps fixed height; width adapts to maintain 4:3 and centers horizontally
  - Buttons: height aligned with inputs (`.btn` padding `10px 12px`), small radius (8px); `.tab.active:hover` = `.tab:hover`

2) Probability Distribution page
- Interactive tool for exploring and comparing different probability distributions
- Visualizes distribution characteristics and applications in sampling contexts

3) AQL Plan Table Lookup (ANSI/ASQ Z1.4)
- Spec: `requirements/aql_plan_table_lookup.md`
- Data: `master tables/CodeLetterTable.js`, `normal.js`, `reduced.js`, `tightened.js`

4) Reverse Sampling Query
- Spec: `requirements/reverse_sampling_query.md`
- Query sampling plan parameters in reverse to find optimal configurations

5) C=0 Plan Lookup
- Spec: `requirements/c0_plan_table_lookup.md`
- Data: `master tables/c=0 table.js`

6) AQL–LTPD Balanced Plan page
- Spec: `requirements/aql_ltpd_balanced_plan.md`
- Implements optimization to recommend `(n, c)` with efficiency scoring, AOQL/ASN, and OC curve

7) Multiple Plan Comparison
- Spec: `requirements/multiple_plan_comparison.md`
- Accepts exports from other pages and renders OC/AOQ/ATI curves

8) Efficiency Analysis System
- Spec: `requirements/efficiency_analysis_system.md`

9) Charting
- Library: Chart.js (CDN). Use `maintainAspectRatio: false`; layout ratio is controlled by CSS (see 1).

10) Exports
- PNG/CSV per page spec; typical CSV headers are listed in each page’s requirements file.

Bootstrapping steps:
1. Create base HTML skeleton with `sidebar` + `pages` layout as in UI standards.
2. Implement each page per its `requirements/*.md` file, wiring inputs, outputs, and event handlers.
3. Load data sources from `master tables/` and build lookup functions.
4. Initialize charts with theme-aware colors; set CSS for 4:3 chart width with fixed height.
5. Wire exports (PNG/CSV) and plan export to comparison page.