# Advanced Sampling Plan Analyzer

**An interactive, web-based tool for comprehensive analysis and comparison of statistical sampling plans.**

This powerful tool is designed for quality management professionals, engineers, and students of statistics to visualize, analyze, and compare various acceptance sampling plans. It transforms complex statistical concepts into intuitive, interactive charts and tables, moving beyond simple table lookups to a deeper understanding of the risks and discriminating power of each plan.

**Access the tool here: [https://3kids68.github.io/Advanced-Sampling-Plan-Analyzer/]**
<img width="1107" height="911" alt="image" src="https://github.com/user-attachments/assets/263d2134-7431-4488-94f2-c6f5861ab896" />

---

## Features

This tool is a comprehensive suite for acceptance sampling, offering a range of functionalities from basic probability distribution comparisons to advanced reverse queries and standard plan lookups.

### 1. Probability Distribution Analysis

This is the foundational feature for understanding the behavior of sampling plans. It allows for a direct comparison of three key statistical distributions used in quality control:

*   **Hypergeometric Distribution:** The most precise model, ideal for finite lot sizes where sampling is done without replacement.
*   **Binomial Distribution:** A common approximation for large lots or when sampling with replacement, assuming a constant probability of defects.
*   **Poisson Distribution:** A useful approximation for large sample sizes and low defect rates.

By inputting lot size (N), sample size (n), and acceptance number (c), you can instantly visualize and compare the OC curves for these distributions, providing a clear understanding of their similarities and differences.

### 2. Multiple Plan Comparison

This feature allows for the simultaneous comparison of multiple sampling plans on a single chart. This is invaluable when you need to choose the most suitable plan from several options. You can:

*   Add custom sampling plans by defining the sample size (n), acceptance number (c), and an optional AQL (Acceptable Quality Level).
*   Visually compare the OC curves of different plans to assess their respective risks and discriminating power.
*   Import plans from other sections of the tool (like the AQL and C=0 lookups) to analyze them alongside your custom plans.

### 3. Reverse Sampling Query

One of the most powerful features of this tool, the Reverse Sampling Query allows you to work backward from your desired quality objectives. Instead of just analyzing a given plan, you can design a plan that meets your specific needs. You can fix any three of the following parameters to calculate the fourth:

*   Lot Size (N)
*   Sample Size (n)
*   Acceptance Number (c)
*   AQL (%)

This is incredibly useful for custom-designing sampling plans when standard tables don't fit your specific requirements.

### 4. AQL Plan Table Lookup

This feature provides a quick and easy way to look up standard sampling plans based on the ANSI/ASQ Z1.4-2003 standard. Simply input your lot size, desired AQL, and inspection level, and the tool will provide the corresponding sample size (n) and acceptance number (c).

### 5. C=0 Plan Table Lookup

For situations requiring zero-acceptance sampling plans, this feature allows you to look up plans based on the work of Nicholas L. Squeglia. By providing the lot size and AQL, you can instantly find the required sample size for a c=0 plan.

---

## How to Use

1.  **Navigate between features:** Use the tabs at the top of the page to switch between the different analysis modes.
2.  **Input your parameters:** In each section, use the control panel on the left to input the relevant parameters for your analysis.
3.  **Visualize the results:** The OC curves and other results will be instantly displayed on the chart on the right.
4.  **Compare and analyze:** Use the interactive charts to compare different plans and understand their performance.
5.  **Export your work:** You can export the charts as PNG images and the data as CSV files for your reports and further analysis.

Youtube：https://www.youtube.com/watch?v=iOWHm5hUIGE
---

## About the Developer

This tool was developed by Chun-Chieh Chang. You can reach him at 3kids68@gmail.com. All Rights Reserved.

---

## Feedback and Contributions

Your feedback is valuable! If you have any suggestions or encounter any issues, please feel free to open an issue in this repository or contact the developer.
