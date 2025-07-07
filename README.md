# Dynamic Pricing for Urban Parking Lots
The Consulting & Analytics Club × Pathway is hosting the **Capstone Project | Summer Analytics 2025**.
## Project Summary
This project uses intelligent ML-inspired models and real-time data streams to simulate a **dynamic pricing engine** for 14 urban parking lots. Price optimization based on demand, occupancy, congestion, competition, and other factors is the aim in order to improve user experience and utilization. 
## Tech Stack
- **Python**
- **NumPy**, **Pandas** – Data processing
- **Bokeh** – Real-time visualizations
- **Pathway** – Real-time data streaming and event simulation
- **Google Colab** – Execution environment
- ## Project Architecture
```mermaid
graph TD
  A[CSV Dataset] --> B[Data Cleaning & Feature Engineering]
  B --> C1[Model 1: Baseline Pricing]
  B --> C2[Model 2: Demand-Based Pricing]
  B --> C3[Model 3: Competitive Pricing]
  C1 & C2 & C3 --> D[Real-Time Price Computation via Pathway]
  D --> E[Bokeh Dashboard - Real-Time Visualization]
## Workflow & Elements

1. **Preprocessing of Data**
   The dataset (`Occupancy`, `Capacity`, `QueueLength`, etc.) was loaded and cleaned.
   - Handled missing values and calculated `occupancy_rate`
   One-hot encoded categorical features (such as `TrafficLevel` and `VehicleType`)

2. Baseline Linear Pricing (Model 1)
   The formula is `price[t+1] = price[t] + α * occupancy_rate[t]`. Pricing rises linearly with occupancy.
   Serves as a point of comparison

3. **Model 2: Dynamic Pricing Based on Demand**
   A weighted formula that makes use of the:
     - occupancy rate
     - Traffic volume, queue length, and special day indicator
     - Type of vehicle
   Formula for demand:
     Demand = 0.4 * occupancy_rate + 0.3 * queue_length + 0.1 * is_special_day + vehicle_weight - traffic_penalty ```
   Pricing: `price = base_price * (1 + λ * demand)` - Clipped within `[0.5x, 2x]` of base price

4. Competitive Pricing (Model 3)
   - Determined the closest competitor using geographic distance (lat-long + KDTree)
   - Modified price based on: - If competitor is less expensive
    - slightly undercut or reroute
    - Charge a small premium if demand for your lot is higher.
   Reroute logic: If a competitor offers a lower price and occupancy is greater than 90%

5. **Simulation in Real Time**
- Used `Pathway` to convert data to a CSV stream
- Simulated a live stream using `pw.demo.replay_csv()`
- Instantaneously applied pricing logic

**Bokeh Visualizations** 
- Occupancy rate histogram
- Hourly average occupancy time series plot
- Real-time price visualization using `ColumnDataSource.stream()`
## Visual Outputs

- 📈 Histogram of occupancy rate (Bokeh)
- 🕒 Time-series of hourly average occupancy
- 💵 Baseline pricing curve for selected lots
- 🔁 Real-time price stream from Pathway shown using Bokeh line charts
- 🔍 Competitor comparison and rerouting flag output

## Setup Instructions

```bash
# Install required packages
!pip install pandas numpy bokeh pathway
A supplementary report is available with explanations, assumptions, and visuals:  
[📝 Google Doc Report](https://docs.google.com/document/d/1m6D25AUbQgIFj70JzcsiFlV47b-tKVOBpY57K0UcZis/edit?tab=t.0)

