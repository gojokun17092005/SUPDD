# 🏙️ Smart Urban Planning Decision Support System (DSS)

A human-centred, multi-domain **Municipal Decision Support System** that helps urban planners, municipal authorities, and civil engineers understand city conditions, evaluate infrastructure health, check planning benchmark compliance, and decide what to do next.

The system uses a **transparent, rule-based assessment engine**. There is no machine-learning model. Every result — risk classification, domain score, and benchmark check — is derived from explicit thresholds and formulas that you can read and verify.

---

## 🌟 Key Features

* **👋 Human-Centred Dashboard**: Conversational language, friendly insights, and plain-English explanations.
* **🔢 20-Parameter Input Form**: Enter precise values for each planning parameter across 8 domains using standard numeric input fields.
* **🚨 Rule-Based Risk Classifier**: Urban Stress Index (0–100) and urgency classification — **Critical**, **High**, **Moderate**, or **Low** — derived from documented, configurable thresholds.
* **💡 Key Insights Generator**: Automatically surfaces the most important findings in plain language.
* **📊 8-Domain Equilibrium Radar**: Chart.js radar chart with a dynamic narrative interpreting the sector shape.
* **📜 Planning Benchmark Checks**: Comparison against published reference guidelines (MoHUA, URDPFI 2014, CPCB, WHO, NDMA). *Not a formal statutory compliance determination.*
* **🏗️ Actionable Recommendations**: Prioritised capital projects with plain-English reasons.
* **🗺️ Spatial Zoning Sketch**: HTML5 Canvas visualisation of residential, commercial, green buffer, and transit zones.
* **⚖️ What-if Comparison**: Compare your current assessment against any preset scenario side-by-side.
* **✅ Input Validation**: Each numeric field shows its allowed range, and inline error messages appear on out-of-range entries.

---

## 🛠️ Technology Stack

| Layer           | Technology                   | Purpose                                          |
|:----------------|:-----------------------------|:-------------------------------------------------|
| **Frontend**    | HTML5, CSS3, Vanilla JS      | Dashboard, parameter forms, visualisations       |
| **Visualisation** | Chart.js (CDN) & Canvas    | Radar chart & spatial zoning sketch              |
| **Backend**     | Node.js 18+, Express 4       | REST API & rule-based assessment engine          |
| **Data format** | JSON                         | API communication                                |

> **No Python, no scikit-learn, no ML service.** The entire system runs with a single `npm start`.

---

## 📂 Project Structure

```text
SmartUrbanPlanningDecisionSystem/
├── package.json          # Node.js dependencies & start scripts
├── server.js             # Express server & rule-based assessment engine
├── README.md             # This file
└── public/
    ├── index.html        # Dashboard UI
    ├── style.css         # Light-theme stylesheet
    └── script.js         # Event handling, API calls, charts, canvas, fallback engine
```

---

## 🚀 Quick Start

### Prerequisites
* **Node.js** v18 or later
* **npm** (bundled with Node.js)

### Step 1 — Install dependencies
```bash
npm install
```

### Step 2 — Start the server
```bash
npm start
```

### Step 3 — Open the dashboard
```
http://localhost:5000
```

That's it. No Python environment, no model training, no second terminal.

---

## 📑 20 Urban Planning Parameters & 8 Core Domains

### 1. People & Population (Demographic)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Population Density | cap/km² | 1,000 – 35,000 | 12,500 |
| Population Growth Rate | %/yr | −2.0 – 8.0 | 2.4 |

### 2. Land & Zoning (Land Use)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Built-up Area | % | 10 – 90 | 62 |
| Vacant Land | % | 0 – 60 | 12 |
| Residential Land | % | 10 – 80 | 45 |
| Commercial Land | % | 2 – 50 | 18 |

### 3. Roads & Transit (Transport)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Road Density | km/km² | 2 – 25 | 11.5 |
| Traffic Congestion | % delay | 10 – 95 | 48 |
| Public Transit Accessibility | /100 | 10 – 100 | 65 |

### 4. Basic Utilities (Infrastructure)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Water Supply Coverage | % | 20 – 100 | 82 |
| Sewerage Coverage | % | 10 – 100 | 72 |
| Waste Management Coverage | % | 20 – 100 | 76 |

### 5. Nature & Air (Environment)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Air Quality Index (AQI) | AQI | 20 – 500 | 135 |
| Green Space | % | 2 – 45 | 14.5 |

### 6. Safety & Risks (Disaster Resilience)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Flood Risk Index | /100 | 0 – 100 | 32 |

### 7. Schools & Hospitals (Public Services)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Distance to Hospital | km | 0.3 – 15 | 2.4 |
| Distance to School | km | 0.2 – 6 | 0.9 |

### 8. Money & Costs (Economic Viability)
| Parameter | Unit | Range | Default |
|---|---|---|---|
| Average Monthly Household Income | ₹k/mo | 15 – 250 | 68 |
| Land Market Valuation | ₹k/m² | 10 – 300 | 85 |
| Infrastructure Capital Cost Index | ₹Cr/km² | 10 – 200 | 48 |

---

## 🧠 How the Assessment Engine Works

The engine runs entirely in Node.js (with a mirrored client-side fallback for offline use). It does not involve any machine-learning model.

### Step 1 — Input validation & clamping
Each of the 20 parameters is validated against its allowed range. Out-of-range values are clamped so a result is always produced; validation warnings are included in the API response.

### Step 2 — Domain scores (0–100)
Eight weighted domain scores are calculated from the input parameters using explicit formulas:

| Domain | Key parameters used |
|---|---|
| Demographic | Population density, growth rate |
| Land Use | Built-up area, vacant land |
| Transport | Road density, congestion, transit access |
| Infrastructure | Water, sewerage, waste management coverage |
| Environment | AQI, green space |
| Disaster | Flood risk index |
| Public Services | Distance to hospital, distance to school |
| Economic | Income/land-price ratio, infrastructure cost |

### Step 3 — Urban Stress Index (0–100)
Six indicators (AQI, green space, water, sewerage, congestion, flood risk) each contribute penalty points when they fall outside acceptable thresholds. The sum (capped at 100) is the Stress Index.

### Step 4 — Risk classification
| Stress Index | Risk Level |
|---|---|
| 0 – 19 | Low |
| 20 – 37 | Moderate |
| 38 – 59 | High |
| 60 – 100 | Critical |

### Step 5 — Benchmark checks
Six key indicators are compared against published planning reference targets and labelled Pass / Needs Improvement / Significant Gap.

### Step 6 — Recommendations
Five capital project types are prioritised (Critical / High / Medium / Low) based on the individual parameter values.

---

## 📡 REST API

### `POST /api/predict`
**Headers:** `Content-Type: application/json`

**Request body:** any subset of the 20 parameter keys. Missing keys fall back to defaults.

**Sample request:**
```json
{
  "populationDensity": 22000,
  "trafficCongestion": 72,
  "waterSupplyCoverage": 92,
  "aqi": 185,
  "greenSpacePct": 6.5,
  "floodRisk": 38
}
```

**Sample response:**
```json
{
  "params": { "...all 20 parameters with defaults applied..." },
  "urgency": {
    "riskLevel": "High",
    "stressIndex": 43,
    "explanation": "Elevated pressure: traffic congestion (72% delay)..."
  },
  "compositeInfrastructureScore": 74,
  "equilibriumDimensions": {
    "demographic": 78, "landUse": 85, "transport": 55,
    "infrastructure": 88, "environment": 48, "disaster": 62,
    "publicServices": 95, "economic": 72
  },
  "benchmarks": [
    { "name": "Water Supply Coverage", "target": "100%", "current": "92%", "status": "Moderate Deficit" }
  ],
  "projects": [
    { "name": "Universal Potable Water Pipeline", "priority": "High", "impact": "Eliminates 8% water deficit." }
  ],
  "validationWarnings": []
}
```

### `GET /api/health`
```json
{
  "status": "ONLINE",
  "service": "Smart Urban Planning DSS",
  "engine": "rule-based",
  "timestamp": "2026-10-11T00:00:00.000Z"
}
```

---

## ⚖️ Planning Benchmark References

| Benchmark | Reference |
|---|---|
| Water Supply Coverage (100%) | MoHUA Service Level Benchmarks — 135 lpcd universal coverage |
| Sewerage Coverage (100%) | MoHUA SLB — 100% underground network |
| Air Quality Index (≤ 100) | CPCB NAAQS & WHO Air Quality Guidelines |
| Green Space (≥ 15%) | URDPFI Guidelines 2014 & WHO urban green space norm |
| Traffic Congestion (≤ 35%) | Project-defined threshold |
| Flood Risk Index (< 25) | NDMA Urban Flood Management Guidelines |

> **Important:** Benchmark checks shown in the dashboard are based on published planning reference guidelines. They are **not** verified legal compliance determinations. Always consult a qualified planner or statutory authority for formal decisions.

---

## ⚠️ Known Limitations

* All thresholds in the rule-based engine are configurable development defaults — they are informed by published guidelines but are not automatically updated when regulations change.
* The zoning canvas is a schematic illustration based on percentage inputs, not a real spatial model.
* The stress index and domain scores are aggregate indicators intended to guide attention, not to substitute for detailed engineering surveys or legal planning assessments.

---

## 🔮 Possible Future Improvements

* Export assessment results as a PDF report.
* Multi-city comparison with persistent storage.
* Configurable thresholds via a settings panel.
* Integration with real open-data sources (Census of India, smart city APIs).

---

## 📜 License & Acknowledgments

* Developed for Smart Urban Planning & Municipal Decision Support education and exploration.
* Built using open web standards: HTML5, CSS3, ES6+, Express.js, Chart.js.
