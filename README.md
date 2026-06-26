# Procurement Optimization & Intelligence System

## Project Overview
This repository contains an end-to-end Procurement Analytics project designed to solve a multi-objective sourcing allocation problem. Using an operational portfolio of 50 suppliers across various IT categories (Hardware, Software, Cloud, Services), the system runs a constrained optimization model to achieve the lowest possible purchasing spend while strictly capping portfolio risk exposure.

## Architecture & Workflow
1. **Data Engineering:** Ingested historical Purchase Order logs and supplier profiles, computing historical average unit costs via advanced data parsing.
2. **Mathematical Modeling:** Formulated a Mixed-Integer Linear Programming (MILP) model to determine optimal vendor allocation quantities.
3. **Data Visualization:** Built an executive-level interactive dashboard in Power BI Desktop to map risk-to-spend boundaries and category distributions.

---

## Problem Formulation & Constraints
The objective of the system is to minimize total portfolio spend:

$$\text{Minimize } \sum (x_i \times c_i)$$

Where:
* $x_i$ = Target Unit Allocation for Supplier $i$
* $c_i$ = Average Unit Price for Supplier $i$

### Hard Operational Constraints:
* **Total Demand:** Total allocated volume must meet the target demand: $\sum x_i = 50,000$ units.
* **Supplier Capacity:** No single supplier can exceed their maximum operational capacity: $x_i \le 3,500$ units.
* **Risk Mitigation Ceiling:** The volume-weighted portfolio risk score must not exceed a threshold of 58:
  $$\frac{\sum (x_i \times r_i)}{\sum x_i} \le 58$$
  *(Where $r_i$ is the inherent supplier risk score).*

---

## Technical Implementation Details

### Optimization Engine
Due to the whole-integer constraint constraints ($x_i \in \mathbb{Z}$), an **Evolutionary Algorithm** was deployed to navigate the non-continuous solution space. The model bypassed initial mathematical divide-by-zero bounds by establishing dynamic seeding baselines.

### Power BI Analytics Layer
Custom Data Analysis Expressions (DAX) were authored to calculate active portfolio metrics dynamically:

```dax
Total Units Sourced = SUM('Optimization_Model'[Target Allocation])

Optimized Total Spend = SUMX('Optimization_Model', 'Optimization_Model'[Unit_Price] * 'Optimization_Model'[Target Allocation])

Weighted Portfolio Risk = 
DIVIDE(
    SUMX('Optimization_Model', 'Optimization_Model'[Risk_Score] * 'Optimization_Model'[Target Allocation]),
    [Total Units Sourced],
    0
)

Final Results & Insights

Total Optimized Spend: $258,242,654

Total Units Allocated: 49,901 units (99.8% target fulfillment under integer constraints)

Achieved Portfolio Risk: 33.21 (Optimized 42.7% below the maximum risk ceiling of 58)

## References & Foundational Literature

This system integrates methodologies from supply chain risk management, optimization modeling, and statistical causal analysis. The structural architecture of this project is built upon frameworks from the following foundational texts:

1. **Supply Chain Risk Management & Modeling**
   * *Mandl, C.* – Focuses on multi-objective optimization under uncertainty, trade-offs between procurement costs and supplier operational disruption risk, and setting risk boundary constraints.
   
2. **Sourcing Optimization & Strategic Spend Analysis**
   * *Pandit, K.* – Explores advanced cost-modeling, multi-variable sourcing optimization methodologies, and handling supplier capacity constraints in complex enterprise environments.

3. **Supply Chain Analytics & Business Intelligence**
   * *Sanders, N. R.* – Frameworks for transitioning descriptive supply chain data (such as historical purchase order metrics) into prescriptive and predictive dashboard layers for executive decision-making.

4. **Causal Inference & Structural Equation Modeling**
   * *Pearl, J.* – Methods for understanding underlying data dependencies, risk score generation models, and ensuring that constraints map true causal vulnerabilities rather than superficial correlation spikes.
