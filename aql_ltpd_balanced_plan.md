## AQL-LTPD Balanced Sampling Plan — Page Requirements (v1)

Purpose: Design optimal sampling plans that simultaneously satisfy both AQL (Acceptable Quality Level) and LTPD (Lot Tolerance Percent Defective) requirements using mathematical optimization models. Users input AQL and LTPD values to obtain the most suitable sampling plan parameters (n, c) with corresponding OC curve visualization.

### 1) Core Concepts & Parameters
- **AQL (Acceptable Quality Level)**: The maximum percent defective that can be considered satisfactory as a process average
- **LTPD (Lot Tolerance Percent Defective)**: The percent defective that will be accepted with a low probability (typically 10%)
- **Producer's Risk (α)**: Probability of rejecting a lot at AQL quality level (typically 5%)
- **Consumer's Risk (β)**: Probability of accepting a lot at LTPD quality level (typically 10%)
- **n**: Sample size
- **c**: Acceptance number (maximum number of defectives allowed)

### 2) Input Parameters (Left panel)
- **AQL Input** (`#aql_ltpd_aql_input`): number, 0.01-10.0, default 1.0
- **LTPD Input** (`#aql_ltpd_ltpd_input`): number, 0.1-50.0, default 5.0
- **Lot Size** (`#aql_ltpd_lot_size`): number, >0, optional for Hypergeometric calculations
- **Distribution Type** (`#aql_ltpd_dist_select`): select options
  - Binomial (default)
  - Poisson
  - Hypergeometric (requires Lot Size)
- **Optimization Target** (`#aql_ltpd_optimization`): select options
  - "Minimize Sample Size" (default)
  - "Balance AQL-LTPD"
  - "Maximize Producer Protection"
  - "Maximize Consumer Protection"
- **Risk Constraints**:
  - Producer's Risk α (`#aql_ltpd_alpha`): 0.01-0.20, default 0.05
  - Consumer's Risk β (`#aql_ltpd_beta`): 0.01-0.20, default 0.10
- **Actions**: Calculate Plan (`#aql_ltpd_calculate_btn`), Clear (`#aql_ltpd_clear_btn`)

### 3) Calculation Results (Left panel)
- **Recommended Plan**:
  - Sample Size n (`#aql_ltpd_result_n`)
  - Acceptance Number c (`#aql_ltpd_result_c`)
  - Plan Efficiency Score (`#aql_ltpd_efficiency`)
- **Performance Metrics**:
  - AQL Point: Pa at AQL (`#aql_ltpd_aql_pa`)
  - LTPD Point: Pa at LTPD (`#aql_ltpd_ltpd_pa`)
  - Actual Producer's Risk (`#aql_ltpd_actual_alpha`)
  - Actual Consumer's Risk (`#aql_ltpd_actual_beta`)
- **Plan Analysis**:
  - AOQL (Average Outgoing Quality Limit) (`#aql_ltpd_aoql`)
  - ASN (Average Sample Number) (`#aql_ltpd_asn`)
- **Efficiency Rating & Recommendations** (NEW):
  - Efficiency Rating (`#aql_ltpd_efficiency_rating`): Visual rating with emoji indicators
    - 🌟 Excellent (95%+)
    - ✅ Very Good (85-95%)
    - 👍 Good (75-85%)
    - ⚠️ Fair (65-75%)
    - ❌ Poor (50-65%)
    - 🚫 Very Poor (<50%)
  - Improvement Suggestions (`#aql_ltpd_improvements`): AI-generated recommendations
    - Sample size optimization suggestions
    - Risk constraint analysis
    - Distribution model recommendations
    - AQL-LTPD ratio analysis
- **Notes** (`#aql_ltpd_notes`)

### 4) Mathematical Model & Optimization Algorithm

#### 4.1 Core Calculation Logic
```javascript
function calculateOptimalPlan(aql, ltpd, lotSize, distribution, optimizationTarget, alpha, beta) {
    // 1. Define search space for (n, c) combinations
    // 2. For each combination, calculate:
    //    - Pa at AQL point
    //    - Pa at LTPD point
    //    - Actual producer's risk (1 - Pa at AQL)
    //    - Actual consumer's risk (Pa at LTPD)
    // 3. Apply optimization criteria
    // 4. Return best (n, c) combination with metrics
}
```

#### 4.2 Optimization Strategies
- **Minimize Sample Size**: Find smallest n that satisfies risk constraints
- **Balance AQL-LTPD**: Minimize deviation from ideal Pa values (1-α at AQL, β at LTPD)
- **Maximize Producer Protection**: Minimize actual producer's risk
- **Maximize Consumer Protection**: Minimize actual consumer's risk

#### 4.3 Search Algorithm
- **Search Range**: n from 1 to 1000, c from 0 to n
- **Convergence Criteria**: Stop when risk constraints are satisfied within tolerance
- **Fallback Strategy**: If no plan satisfies constraints, return closest feasible plan with warnings

#### 4.4 Efficiency Calculation Algorithm (NEW)
```javascript
function calculatePlanEfficiency(plan, idealPaAql, idealPaLtpd, alpha, beta) {
    // Calculate deviations from ideal performance
    const aqlDeviation = Math.abs(plan.paAql - idealPaAql);
    const ltpdDeviation = Math.abs(plan.paLtpd - idealPaLtpd);
    
    // Penalty for constraint violations
    let constraintPenalty = 0;
    if (plan.actualAlpha > alpha) {
        constraintPenalty += (plan.actualAlpha - alpha) * 2;
    }
    if (plan.actualBeta > beta) {
        constraintPenalty += (plan.actualBeta - beta) * 2;
    }
    
    // Efficiency score (0-1, higher is better)
    const totalDeviation = aqlDeviation + ltpdDeviation + constraintPenalty;
    const efficiency = Math.max(0, 1 - totalDeviation);
    
    return efficiency;
}
```

#### 4.5 Improvement Suggestions Algorithm (NEW)
```javascript
function generateImprovementSuggestions(plan, efficiency, alpha, beta, aql, ltpd, lotSize) {
    const suggestions = [];
    
    // Sample size analysis
    if (plan.n > 200) {
        suggestions.push("• Consider reducing sample size: Current n=" + plan.n + " is quite large.");
    } else if (plan.n < 20) {
        suggestions.push("• Sample size is very small (n=" + plan.n + "). Consider increasing for better statistical power.");
    }
    
    // Risk constraint analysis
    if (plan.actualAlpha > alpha * 1.2) {
        suggestions.push("• Producer's risk is high. Consider increasing sample size or adjusting AQL.");
    }
    
    if (plan.actualBeta > beta * 1.2) {
        suggestions.push("• Consumer's risk is high. Consider increasing sample size or adjusting LTPD.");
    }
    
    // Distribution-specific suggestions
    if (lotSize < 1000) {
        suggestions.push("• For small lots (N=" + lotSize + "), consider using Hypergeometric distribution.");
    } else if (lotSize > 10000) {
        suggestions.push("• For large lots (N=" + lotSize + "), Binomial or Poisson distributions are appropriate.");
    }
    
    // AQL-LTPD ratio analysis
    const ratio = ltpd / aql;
    if (ratio < 3) {
        suggestions.push("• AQL-LTPD ratio is small (" + ratio.toFixed(1) + "). Consider wider separation.");
    } else if (ratio > 10) {
        suggestions.push("• AQL-LTPD ratio is very large (" + ratio.toFixed(1) + "). This may require very large sample sizes.");
    }
    
    return suggestions.join('\n');
}

### 5) Chart Behavior (Right panel)
- **Canvas**: `#ocChartAQL_LTPD`
- **X-axis**: Defect Rate (0 → `#aql_ltpd_x_max`, default 10%)
- **Y-axis**: Acceptance Probability (Pa)
- **Main OC Curve**: Recommended plan's operating characteristic curve
- **Key Points Markers**:
  - AQL point: Green marker with label "AQL (Pa=1-α)"
  - LTPD point: Red marker with label "LTPD (Pa=β)"
- **Risk Regions**:
  - Producer's Risk Zone: Shaded area above AQL point
  - Consumer's Risk Zone: Shaded area below LTPD point
- **Series Label**: `Balanced Plan (n={n}, c={c}, AQL={aql}%, LTPD={ltpd}%)`

### 6) User Interactions
- **Calculate Plan**:
  - Validates all inputs (AQL < LTPD, positive values, valid risks)
  - Runs optimization algorithm
  - Displays results and draws OC curve
  - Shows performance metrics and analysis
- **Input Validation**:
  - AQL must be < LTPD
  - Risk values must be between 0.01 and 0.20
  - Lot size required for Hypergeometric distribution
- **Real-time Updates**:
  - X-axis max changes update chart immediately
  - Distribution type changes require recalculation
- **Clear**: Resets all inputs and clears results/chart

### 7) Advanced Features

#### 7.1 Plan Comparison
- **Standard Plan Comparison**: Show nearest ANSI/ASQ Z1.4 plan for reference
- **Export to Multiple Plan Comparison**: Button to send current plan to comparison page
- **Plan History**: Keep track of last 5 calculated plans for quick comparison

#### 7.2 Sensitivity Analysis
- **AQL Sensitivity**: Show how plan changes with ±10% AQL variation
- **LTPD Sensitivity**: Show how plan changes with ±10% LTPD variation
- **Risk Sensitivity**: Display risk curves for different α/β values

#### 7.3 Plan Recommendations
- **Efficiency Warnings**: Alert if plan is inefficient (high n for given risks)
- **Practical Constraints**: Suggest alternative plans if calculated n > 500
- **Industry Standards**: Compare with common industry practices

### 8) Export & Data Management
- **Chart Export**: PNG (high resolution) and CSV data export
- **CSV Headers**: `x_defect_rate_percent,y_acceptance_prob,n,c,aql,ltpd,alpha,beta,distribution,optimization_target`
- **Plan Export**: JSON format for plan parameters and results
- **Report Generation**: PDF summary with plan details and OC curve

### 9) Default Values & Validation
- **AQL**: 1.0%
- **LTPD**: 5.0%
- **Producer's Risk (α)**: 0.05 (5%)
- **Consumer's Risk (β)**: 0.10 (10%)
- **Distribution**: Binomial
- **Optimization Target**: Minimize Sample Size
- **X-axis Max**: 10%

### 10) Error Handling & Edge Cases
- **Invalid Inputs**: Clear error messages for out-of-range values
- **No Feasible Plan**: Warning when constraints cannot be satisfied
- **Extreme Values**: Handle cases where AQL/LTPD are very close or very far apart
- **Large Sample Sizes**: Warn when n > 500 and suggest alternatives
- **Distribution Limitations**: Handle Hypergeometric edge cases (n > N, etc.)

### 11) UI/UX Requirements
- **Responsive Design**: Works on desktop and tablet
- **Theme Support**: Full compatibility with all 7 application themes
- **Accessibility**: WCAG AA compliance for color contrast and keyboard navigation
- **Loading States**: Show progress during calculation
- **Tooltips**: Helpful explanations for technical terms
- **Input Formatting**: Auto-format percentages and validate in real-time

### 12) Integration with Existing System
- **Navigation**: Add new tab "AQL-LTPD Balanced Plan" to main navigation
- **Data Sharing**: Export functionality to Multiple Plan Comparison page
- **Consistent Styling**: Follow existing UI design standards and glass button components
- **Chart Library**: Use same Chart.js configuration as other pages
- **Export Standards**: Follow established PNG/CSV export patterns

### 13) Performance Requirements
- **Calculation Speed**: Optimize algorithm to complete within 2 seconds
- **Memory Usage**: Efficient handling of large search spaces
- **Chart Rendering**: Smooth 60fps chart updates
- **Responsive Updates**: Real-time input validation without lag

### 14) Testing & Quality Assurance
- **Unit Tests**: Test optimization algorithms with known test cases
- **Integration Tests**: Verify chart rendering and data export
- **User Acceptance Tests**: Validate with real-world AQL/LTPD scenarios
- **Cross-browser Testing**: Ensure compatibility with Chrome, Firefox, Safari, Edge
- **Theme Testing**: Verify functionality across all 7 application themes

### 15) Documentation & Help
- **In-app Help**: Contextual tooltips explaining AQL, LTPD, and optimization concepts
- **User Guide**: Step-by-step instructions for common use cases
- **Mathematical Background**: Brief explanation of the optimization model
- **Best Practices**: Guidelines for selecting appropriate AQL/LTPD values

### 16) 最新功能更新（2025-01）
#### 16.1 效率公式優化
- **動態公式顯示**：標籤更新為 `Plan Efficiency (E = 1 - |Pa_AQL - (1-α)| - |Pa_LTPD - β| - penalty)`
- **動態計算**：效率計算使用用戶設定的實際α和β值，而非固定值
- **公式解釋**：清楚說明效率計算的數學基礎和參數含義

#### 16.2 教學整合
- **教學步驟**：新增"AQL-LTPD Balanced Plans"教學章節
- **概念解釋**：詳細說明AQL、LTPD、生產者風險、消費者風險
- **優化策略**：解釋四種優化策略的適用場景
- **實務應用**：提供具體的使用案例和最佳實踐

#### 16.3 測驗題庫
- **新增題目**：5題專門針對AQL-LTPD平衡計畫功能
- **涵蓋範圍**：優化策略、效率計算、風險平衡概念
- **難度分級**：從基礎概念到進階應用

### 17) 技術改進（2025-01）
#### 17.1 計算精度提升
- **數值穩定性**：改進極端值情況下的計算穩定性
- **收斂優化**：優化搜尋演算法，提高計算效率
- **邊界處理**：更好的邊界條件處理和錯誤恢復

#### 17.2 用戶體驗改進
- **即時驗證**：輸入參數時即時顯示有效性
- **智能建議**：根據輸入參數提供優化建議
- **視覺回饋**：改進圖表標記和標籤的可讀性

### 18) 整合功能（2025-01）
#### 18.1 跨頁面整合
- **匯出功能**：支援匯出到多計畫比較頁面
- **數據一致性**：確保匯出的計畫包含完整的效率計算數據
- **格式標準化**：統一的計畫數據格式

#### 18.2 教學系統整合
- **互動式教學**：整合到整體教學流程中
- **實作練習**：提供具體的練習場景
- **知識評估**：透過測驗驗證學習效果

### 19) 效能優化（2025-01）
#### 19.1 計算效能
- **演算法優化**：改進搜尋演算法的效率
- **記憶體管理**：優化大型搜尋空間的記憶體使用
- **快取機制**：快取常用計算結果

#### 19.2 響應性改進
- **非阻塞計算**：避免長時間計算阻塞UI
- **進度指示**：提供計算進度回饋
- **取消機制**：允許用戶取消長時間計算

### 20) 未來增強功能（v2+）
- **雙重抽樣計畫**：支援雙重抽樣方案
- **序貫抽樣**：實作序貫抽樣計畫
- **成本優化**：在優化模型中包含檢驗成本
- **批次處理**：同時計算多個計畫
- **自訂約束**：允許用戶定義標準風險之外的約束
- **產業模板**：常見產業的預配置計畫
- **AI輔助**：使用機器學習優化計畫選擇
- **雲端整合**：支援雲端計算和協作功能

---

*此規格提供了實施AQL-LTPD平衡抽樣計畫設計工具的全面框架，該工具與現有的Advanced Sampling Plan Analyzer應用程式無縫整合，並包含最新的功能改進和教學整合。*
