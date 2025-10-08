# Advanced Sampling Plan Analyzer

A single‑page, front‑end web app for designing, looking up, and visualizing acceptance sampling plans.

### Features & Pages
- **AQL–LTPD Balanced Plan**: Satisfy both AQL and LTPD with producer/consumer risk (α, β). The app searches/optimizes to recommend `(n, c)`, draws the OC curve, and provides an efficiency score plus improvement suggestions.
- **AQL Plan Table Lookup (ANSI/ASQ Z1.4)**: Query `(n, c, code letter)` by lot size and inspection level. Supports Normal/Reduced/Tightened states and renders the OC curve.
- **C=0 Plan Lookup**: Zero‑acceptance plans (`c=0`) with OC curve. Supports Hypergeometric/Binomial/Poisson.
- **Multi‑plan Curve Comparison**: Compare OC/AOQ/ATI curves after exporting plans from other pages (efficiency scoring lives on the AQL–LTPD page).
- **Modern UI**: Glassmorphism styling, multiple light/dark themes, keyboard accessibility.

### Key Capabilities
- **Distributions**: Binomial, Poisson, Hypergeometric (recommended for small finite lots).
- **OC curves**: Real‑time rendering using the chosen distribution and `(n, c)`.
- **Efficiency score (AQL–LTPD)**: `E = 1 - |Pa_AQL - (1-α)| - |Pa_LTPD - β| - penalty`, with visual rating (🌟/✅/👍/⚠️/❌/🚫).
- **Suggestions**: Data‑driven hints on sample size, risk constraints, distribution choice, and AQL–LTPD ratio.
- **Export**: High‑resolution PNG and CSV (curve data + parameters). Some pages support JSON plan export.

### Project Structure (high‑level)
- `Advanced_Sampling_Plan_Analyzer_251008.html`: Main app (HTML + styles + scripts).
- `master tables/`: Lookup data and helpers (`CodeLetterTable.js`, `normal.js`, `reduced.js`, `tightened.js`, `c=0 table.js`).
- `requirements/`: Page specs and algorithm details (`aql_ltpd_balanced_plan.md`, `aql_plan_table_lookup.md`, `c0_plan_table_lookup.md`, `efficiency_analysis_system.md`).
- `background_info/`: AOQ/ATI PDFs and related background materials.

### Quick Start (Local)
This is a static front‑end app—no dependencies to install.
1. Open `Advanced_Sampling_Plan_Analyzer_251008.html` directly in your browser, or
2. Serve locally to avoid file‑access restrictions:
   - Python: `python -m http.server 8000` → visit `http://localhost:8000/Advanced_Sampling_Plan_Analyzer_251008.html`
   - Node (http‑server): `npx http-server -p 8000` → visit `http://localhost:8000/Advanced_Sampling_Plan_Analyzer_251008.html`

### Usage Highlights
- **AQL–LTPD Balanced Plan**
  1) Enter AQL, LTPD, α/β, distribution, optional lot size N, and an optimization target.
  2) Click Calculate to get recommended `(n, c)`, Pa at AQL/LTPD, actual α/β, AOQL/ASN, the efficiency rating, suggestions, and the OC curve.
- **AQL Table Lookup**
  1) Enter Lot Size and Inspection Level, choose an AQL.
  2) Get `code letter`, `n`, `c`. If the table value is `up/down`, a note is shown and the OC curve is not drawn.
- **C=0 Plan**
  1) Enter Lot Size and AQL (or custom), choose a distribution.
  2) Lookup/compute `n` (with `c=0`), plot the OC curve; estimate LQ at Pa=10% if needed.

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

### License / Use
For educational and research use only. For commercial use or derivative works, please contact the author first.
