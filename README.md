# 🔋 EV Battery Multi-Echelon Supply Chain Optimisation

## Overview

This project develops an end-to-end Operations Research framework for optimizing an Electric Vehicle (EV) battery supply chain consisting of:

* 3 Manufacturing Plants
* 5 Distribution Centers (DCs)
* 40 Dealers

The objective is to jointly optimize inventory replenishment, plant-to-distribution-center allocation, and last-mile dealer deliveries under uncertain demand.

The framework integrates:

* (s,S) Inventory Control Policies
* Network Flow Mixed Integer Linear Programming (MILP)
* Vehicle Routing Problem (VRP) Optimization
* Supply Chain Cost and Service-Level Analytics

The optimization framework improves service levels while reducing inventory, transportation, and stockout costs across the entire supply chain network.

---

# 🎯 Business Problem

EV battery supply chains face significant operational challenges:

* Uncertain dealer demand
* High inventory holding costs
* Expensive stockouts
* Limited manufacturing capacity
* Long transportation distances
* Inefficient last-mile deliveries

Traditional planning methods optimize inventory, transportation, and replenishment separately.

This project develops a coordinated optimization framework that simultaneously improves:

* Fill Rate
* Inventory Efficiency
* Transportation Efficiency
* Service Reliability
* Total Supply Chain Cost

---

# 🏭 Supply Chain Network

```text
Manufacturing Plants

P1          P2          P3
 \           |          /
  \          |         /
   \         |        /

Distribution Centers

D1    D2    D3    D4    D5

 |     |     |     |     |

--------------------------------

40 Dealer Locations
```

---

# ⚙️ Optimization Framework

```text
Dealer Demand Forecasts
          |
          v
+---------------------------+
| (s,S) Inventory Policy    |
+---------------------------+
          |
          v
+---------------------------+
| Network Flow MILP         |
| Plant -> DC Allocation    |
+---------------------------+
          |
          v
+---------------------------+
| Last-Mile Vehicle Routing |
+---------------------------+
          |
          v
      KPI Evaluation
```

---

# 📐 Mathematical Formulation

## Sets

| Symbol | Description                 |
| ------ | --------------------------- |
| P      | Set of Plants               |
| D      | Set of Distribution Centers |
| I      | Set of Dealers              |
| T      | Planning Horizon            |
| K      | Set of Vehicle Routes       |

---

## Parameters

| Symbol | Description                                       |
| ------ | ------------------------------------------------- |
| c_pd   | Cost of shipping one battery from plant p to DC d |
| Cap_p  | Production capacity of plant p                    |
| R_d    | Replenishment requirement of DC d                 |
| h_d    | Inventory holding cost                            |
| Q      | Vehicle capacity                                  |
| Dem_i  | Dealer demand                                     |
| M      | Stockout penalty coefficient                      |

---

## Decision Variables

### Shipment Quantity

$$
x_{pd}
$$

Quantity shipped from plant p to distribution center d.

### Unmet Replenishment

$$
u_d
$$

Unfulfilled replenishment quantity at DC d.

### Inventory Level

$$
I_{dt}
$$

Inventory level at DC d during period t.

---

# Stage 1: Inventory Optimization

Each DC follows an (s,S) inventory policy.

Inventory is continuously monitored.

A replenishment order is triggered whenever:

$$
I_{dt} \le s_d
$$

where:

* $I_{dt}$ = inventory position
* $s_d$ = reorder point

The replenishment quantity is:

$$
Q_{dt}=S_d-I_{dt}
$$

where:

* $S_d$ = order-up-to level

---

## Safety Stock

Safety stock is calculated as:

$$
SS_d=z\sigma_d\sqrt{L}
$$

where:

* $z$ = service level factor
* $\sigma_d$ = demand standard deviation
* $L$ = replenishment lead time

---

## Reorder Point

$$
s_d=\mu_dL+z\sigma_d\sqrt{L}
$$

where:

* $\mu_d$ = average demand
* $\sigma_d$ = demand standard deviation

---

# Stage 2: Plant-to-DC Network Flow MILP

The replenishment allocation problem determines the optimal shipment quantities from plants to distribution centers.

---

## Objective Function

Minimize transportation and shortage costs:

$$
\min Z =
\sum_{p \in P}\sum_{d \in D} c_{pd}x_{pd}
+
\sum_{d \in D} M u_d
$$

---

## Plant Capacity Constraint

$$
\sum_{d \in D}
x_{pd}
\le
Cap_p
\qquad
\forall p \in P
$$

---

## Replenishment Satisfaction Constraint

$$
\sum_{p \in P}
x_{pd}
+
u_d
\ge
R_d
\qquad
\forall d \in D
$$

---

## Non-Negativity Constraints

$$
x_{pd}\ge0
$$

$$
u_d\ge0
$$

---

# Stage 3: Last-Mile Vehicle Routing

After batteries arrive at the DCs, deliveries are routed to dealers.

---

## Vehicle Capacity Constraint

$$
\sum_{i \in Route}
Dem_i
\le
Q
$$

where:

* $Q$ = vehicle capacity

---

## Route Representation

$$
R_k=[0,i_1,i_2,\ldots,i_n,0]
$$

where:

* 0 represents the distribution center

---

## Route Distance

$$
D(R_k)=\sum_{i=0}^{n} Dist_{i,i+1}
$$

---

## Routing Objective

$$
\min \sum_{k \in K} D(R_k)
$$

---

# 💰 Integrated Supply Chain Cost Function

The total supply chain cost is:

$$
TC = HC + TC_{Plant} + TC_{LastMile} + SC
$$

---

## Inventory Holding Cost

$$
HC=\sum_{d \in D} h_d I_d
$$

---

## Plant-to-DC Transportation Cost

$$
TC_{Plant}=\sum_{p \in P}\sum_{d \in D} c_{pd}x_{pd}
$$

---

## Last-Mile Transportation Cost

$$
TC_{LastMile}=\sum_{k \in K} D(R_k)C_{km}
$$

---

## Stockout Cost

$$
SC=Penalty \times StockoutUnits
$$

---

# 📊 Performance Metrics

## Fill Rate

$$
FillRate=
\frac{FulfilledDemand}{TotalDemand}
\times 100
$$

---

## Cost Savings

$$
Savings=
\frac{TC_{Baseline}-TC_{Optimized}}
{TC_{Baseline}}
\times 100
$$

---

## Stockout Reduction

$$
StockoutReduction=
\frac{Stockout_{Baseline}-Stockout_{Optimized}}
{Stockout_{Baseline}}
\times 100
$$

---

## Vehicle Utilization

$$
VehicleUtilization=
\frac{ActualLoad}
{VehicleCapacity}
\times 100
$$
---

# 📈 Results

| KPI                 | Baseline | Optimized |
| ------------------- | -------: | --------: |
| Fill Rate           |      81% |       95% |
| Stockout Units      |     High |   Reduced |
| Total Cost          |     High |   Reduced |
| Last-Mile Distance  |     High |   Reduced |
| Vehicle Utilization | Moderate |      High |

---

# 🏆 Business Impact

* Improved fill rate from 81% to 95%
* Reduced stockout events
* Reduced transportation costs
* Improved inventory utilization
* Projected 30% annual supply chain savings
* Increased service-level reliability

---

# 🛠 Technology Stack

* Python
* NumPy
* Pandas
* SciPy
* PuLP
* Matplotlib
* Network Flow Optimization
* Inventory Optimization
* Vehicle Routing Optimization

---

# 🚀 Installation

```bash
pip install numpy pandas scipy pulp matplotlib
```

---

# ▶️ Run

```bash
python optimizer.py
```

---

