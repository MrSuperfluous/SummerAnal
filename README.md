# 🅿️ Dynamic Pricing for Urban Parking Lots

This repository implements a **real-time, competitive pricing engine** for urban parking lots using **Pathway** and **Bokeh**, as part of the Summer Analytics 2025 capstone project hosted by the Consulting & Analytics Club and Pathway.

---

## 🚀 Overview

The goal is to **dynamically price 13+ parking lots** in Birmingham, UK based on:
- Real-time occupancy
- Queue length
- Traffic congestion
- Vehicle type
- Special days (events/holidays)
- Nearby competitor pricing

The system streams live parking data and outputs updated prices in real-time using **Pathway’s streaming engine**, and visualizes them with **Bokeh** dashboards.

---

## 📦 Models Implemented

### ✅ Model 1: Static Linear Pricing

- A simple regression-based model.
- Predicts price using a linear combination of demand-related features.
- **Limitations**: Doesn’t adapt to current demand or competition.

---

### ✅ Model 2: Demand-Based Pricing

- Adjusts price based on demand indicators like:
  - Occupancy rate
  - Queue length
  - Traffic level
  - Special day flag
  - Vehicle type (proxy for willingness to pay)

**Formula (simplified):**

```math
raw_demand = α * occupancy_rate + β * queue_length - γ * traffic + δ * is_special_day + ε * vehicle_type_weight
```

- Price increases as demand grows.
- Fixed min/max bounds on price.

---

### ✅ Model 3: Competitive Pricing (Final)

> **This is the main deployed model in real-time**

- **Extends Model 2** with real-world logic:
  - Uses geographic proximity (Haversine distance)
  - Detects nearby competitors
  - Reacts to competitor prices:
    - **If others are cheaper & lot is full** → price drops or reroute suggested
    - **If others are expensive** → price surges up

- Outputs:
  - Final dynamic price
  - Reroute flag (`True/False`)

**Price can now reach ₹25**, maximizing revenue during high demand.

---

## ⚡ Real-Time Architecture (Using Pathway)

- Data streamed from the `./parking/` directory using **Pathway's CSV streaming ingestion**
- Each row is timestamped and parsed into a Pathway schema
- Real-time pricing logic is applied as rows stream in
- **Pathway `.plot()`** method renders live dashboards using **Bokeh** and **Panel**

---

## 📊 Visualizations (Bokeh Dashboard)

For each parking lot, we provide:

### ✅ 1. **Real-Time Price Plot**
- Y-axis: Final price
- X-axis: Timestamp
- Updated automatically

### ✅ 2. **Final Price vs Avg Competitor Price** (Optional overlay)
- Compares own price with nearby lots

### ✅ 3. **Occupancy vs Capacity Plot**
- Helps visualize how usage affects pricing

### ✅ 4. **Reroute Flag Timeline**
- Flags if rerouting would’ve been suggested due to high occupancy

> **All dashboards are interactive and auto-updating** via `.plot()` and Panel.

---

## 📂 File Structure

```
📁 parking/                ← Streaming input data (CSV chunks)
📄 model1_baseline.ipynb   ← Linear regression model
📄 model2_demand.ipynb     ← Demand-based pricing
📄 model3_competitive.ipynb← Real-time competitive pricing (final model)
📄 dashboard.ipynb         ← Full streaming dashboard (Model 3)
📄 Sample_Notebook.ipynb   ← Reference architecture (from organizers)
📄 README.md               ← You are here
```

---

## 🧪 How to Run (in Google Colab)

1. Upload CSV chunks to `./parking/`
2. Open `dashboard.ipynb`
3. Run all cells — Panel + Pathway will begin plotting
4. Dashboard will update as new data arrives

> No need for `panel serve` — `.plot()` handles everything in-notebook.

---

## ✅ Project Deliverables

- ✅ Three pricing models (static, demand-based, competitive)
- ✅ Real-time ingestion and processing with Pathway
- ✅ Visual Bokeh dashboard per lot
- ✅ Pricing explanation with reroute behavior
- ✅ Full codebase and explanation in Colab notebooks

---

## 🧠 Assumptions

- Vehicle type weight reflects customer willingness to pay
- Traffic level is a negative indicator of parking convenience
- Special days may increase parking demand
- Competitors are defined as lots within a **1 km radius**
- Price caps at **₹25** and bottoms at **₹5**

---

## 👤 Contributors

- **Anshu** – Real-time logic, dashboard integration, pricing strategies  
- **C&C Club + Pathway** – Dataset & Sample notebook

---

## 📜 License

MIT License.

---

## 📎 References

- [Pathway Streaming Docs](https://pathway.com/developers)
- [Panel Bokeh Integration](https://panel.holoviz.org)
- [Summer Analytics 2025 Homepage](https://www.linkedin.com/company/consultingclubiitg)
