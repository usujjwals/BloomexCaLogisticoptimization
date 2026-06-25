# Bloomex.ca Logistics Optimization Using Linear Programming

## Project Overview

This project analyzes the logistics and distribution network for **Bloomex.ca**, an online flower delivery company, and develops an optimized weekly shipping plan using a **Linear Programming (LP) Network Flow / Transshipment Model**.

The goal of the project is to determine the **lowest-cost shipping plan** that satisfies customer demand across multiple Canadian cities while considering available supply, transshipment hubs, and transportation costs between nodes.

The optimization model was built and solved in **Python** using the **PuLP** library. The final result identifies the most cost-effective way to move flower boxes from global and local suppliers through distribution hubs to final demand locations.

---

## Business Problem

Bloomex needs to ship flowers from several supply regions to customers across Canada. The company has multiple potential supply sources and transshipment hubs, each with different shipping costs.

The key business question is:

> How can Bloomex meet all weekly demand at the lowest possible transportation cost?

This problem is modeled as an **unbalanced transshipment network** because total available supply exceeds total demand. Therefore, supply constraints are modeled as “less than or equal to” constraints, while demand constraints must be fully satisfied.

---

## Network Structure

The logistics network includes three main types of nodes:

### Supply Origins

| Origin            |                Type |                   Supply |
| ----------------- | ------------------: | -----------------------: |
| South America     |         Pure Supply |              1,050 boxes |
| Southeast Asia    |         Pure Supply |                 30 boxes |
| Europe            |         Pure Supply | Unlimited / large supply |
| Ontario / Toronto | Hybrid Supply + Hub |                100 boxes |

### Transshipment Hubs

| Hub              | Role                   |
| ---------------- | ---------------------- |
| Toronto, Ontario | Hub and destination    |
| Miami, Florida   | Pure transshipment hub |
| Vancouver        | Hub and destination    |

### Demand Locations

| Destination      |    Demand |
| ---------------- | --------: |
| Calgary          | 171 boxes |
| Winnipeg         |  64 boxes |
| Ottawa           | 127 boxes |
| Montreal         | 184 boxes |
| Halifax          |  64 boxes |
| Toronto, Ontario | 381 boxes |
| Vancouver        | 191 boxes |

Total weekly demand equals **1,182 boxes**.

---

## Methodology

This project uses a **Linear Programming transshipment model** to minimize total transportation cost.

The model includes:

* Decision variables for shipments from origins to hubs
* Decision variables for shipments from hubs to final destinations
* Supply constraints for each origin
* Demand constraints for each destination
* Flow-balance constraints for each hub
* Non-negativity constraints to prevent negative shipment quantities

---

## Decision Variables

Let:

* `x[i,j]` = number of boxes shipped from origin `i` to hub `j`
* `y[j,k]` = number of boxes shipped from hub `j` to destination `k`

Where:

* Origins: South America, Southeast Asia, Europe, Ontario/Toronto
* Hubs: Ontario/Toronto, Miami, Vancouver
* Destinations: Calgary, Winnipeg, Ottawa, Montreal, Halifax, Ontario/Toronto, Vancouver

---

## Objective Function

The objective is to minimize total weekly transportation cost:

```text
Minimize Total Cost =
Sum of all origin-to-hub shipping costs
+
Sum of all hub-to-destination shipping costs
```

The model selects the cheapest combination of routes while ensuring all city demand is satisfied.

---

## Constraints

The model includes the following constraints:

### 1. Supply Constraints

Each origin cannot ship more than its available supply.

```text
Total shipments from each origin <= available supply
```

### 2. Demand Constraints

Each destination must receive exactly the number of boxes required.

```text
Total shipments into each destination = demand
```

### 3. Hub Flow-Balance Constraints

For each hub, total inflow must equal total outflow.

```text
Total boxes entering hub = total boxes leaving hub
```

### 4. Non-Negativity Constraints

All shipment values must be greater than or equal to zero.

```text
x[i,j] >= 0
y[j,k] >= 0
```

---

## Tools and Technologies

* **Python**
* **PuLP** for linear programming optimization
* **Pandas** for data setup and tabular output
* **Jupyter Notebook**
* **Excel** for supporting documentation and model verification

---

## How to Run the Project

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/bloomex-logistics-optimization.git
cd bloomex-logistics-optimization
```

### 2. Install Required Libraries

```bash
pip install pulp pandas
```

### 3. Open the Jupyter Notebook

```bash
jupyter notebook
```

Then open the notebook file:

```text
MSBA_204_part_2.1.ipynb
```

### 4. Run All Cells

The notebook will:

1. Define the supply, demand, hub, and cost data
2. Build the LP transshipment model
3. Solve the optimization problem using PuLP
4. Print the optimal shipment routes
5. Generate sensitivity analysis results

---

## Optimal Solution Summary

The model found an optimal weekly transportation cost of:

```text
$18,503.50 per week
```

The optimal solution shows that most shipments should be routed through the **Miami hub**, while Toronto is used to satisfy local demand and Halifax demand.

---

## Optimal Shipping Plan

### Origin to Hub Shipments

| From                   | To          | Quantity |
| ---------------------- | ----------- | -------: |
| South America          | Miami       |      705 |
| South America          | Toronto Hub |      345 |
| Europe                 | Miami       |       32 |
| Ontario/Toronto Origin | Toronto Hub |      100 |
| Southeast Asia         | Any Hub     |        0 |

### Hub to Destination Shipments

| From        | To                  | Quantity |
| ----------- | ------------------- | -------: |
| Miami       | Calgary             |      171 |
| Miami       | Winnipeg            |       64 |
| Miami       | Ottawa              |      127 |
| Miami       | Montreal            |      184 |
| Miami       | Vancouver           |      191 |
| Miami       | Toronto Destination |      345 |
| Toronto Hub | Halifax             |       64 |
| Toronto Hub | Toronto Destination |      381 |

---

## Sensitivity Analysis

The sensitivity analysis helps determine how valuable additional supply would be from each grower.

### Shadow Price Interpretation

| Source          | Shadow Price | Interpretation                                                       |
| --------------- | -----------: | -------------------------------------------------------------------- |
| South America   |           -6 | Each additional box could reduce total cost by $6                    |
| Southeast Asia  |            0 | Additional supply does not reduce cost                               |
| Ontario/Toronto |          -20 | Each additional box could reduce total cost by $20                   |
| Europe          |            0 | Additional supply does not reduce cost because supply is not binding |

The most valuable additional supply would come from the **Ontario/Toronto grower**, followed by **South America**.

---

## Business Insights

The optimization model provides several important insights:

1. **Miami is the most cost-effective hub** for most shipments, even though it is geographically distant from some Canadian cities.
2. **Toronto supply is highly valuable** because local supply reduces the need for more expensive shipping.
3. **South America is also valuable**, and additional supply from this region could reduce total weekly cost.
4. **Southeast Asia is not used** in the optimal solution because its transportation costs are not competitive.
5. The model supports better supplier decisions by identifying which growers provide the greatest cost-saving potential.

---

## Key Results

* Built a linear programming network-flow model for a real-world logistics problem
* Minimized total weekly shipping cost to **$18,503.50**
* Identified optimal origin-to-hub and hub-to-destination routes
* Conducted sensitivity analysis to evaluate the value of additional supply
* Provided actionable recommendations for cost reduction and supplier planning

---

## Repository Structure

```text
Bloomex-Logistics-Optimization/
│
├── MSBA_204_part_2.ipynb
├── MSBA_204_part_2.1.ipynb
├── Supporting Document.xlsx
├── MSBA204 Team Case Study 1_Bloomex_GROUP1_Complete.docx
└── README.md
```

---

## Skills Demonstrated

* Linear Programming
* Network Flow Optimization
* Transshipment Modeling
* Supply Chain Analytics
* Logistics Cost Optimization
* Sensitivity Analysis
* Python Programming
* PuLP Optimization Modeling
* Data Analysis with Pandas
* Business Decision Support

---

## Conclusion

This project demonstrates how optimization modeling can help businesses make better logistics and supply chain decisions. By using a linear programming transshipment model, Bloomex can satisfy all weekly demand while minimizing transportation costs. The final solution not only provides an optimal shipping plan but also identifies which suppliers are most valuable for future cost savings.
