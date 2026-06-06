# 🛢️ NMDPRA AI Data Dashboard

<p align="center">
  <img src="https://www.nmdpra.gov.ng/nmdpraimages/nmdpraLogo.png" alt="NMDPRA Logo" width="100"/>
</p>

<p align="center">
  <strong>AI-powered natural language data exploration for petroleum truckout records</strong><br/>
  Built for the Nigerian Midstream and Downstream Petroleum Regulatory Authority
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-2D8C4E?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Dash-2.18-2D8C4E?style=for-the-badge&logo=plotly&logoColor=white"/>
  <img src="https://img.shields.io/badge/Groq-LLaMA_3.3_70B-F5C518?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Status-Prototype-F5C518?style=for-the-badge"/>
</p>

---

## 📌 What Is This?

The **NMDPRA AI Data Dashboard** is a prototype intelligence tool that allows users to explore petroleum truckout data using plain English questions — no SQL, no Excel, no technical knowledge required.

Ask a question. Get a chart, map, or table. Instantly.

> *"Show me total PMS volume by state on a map"*
> *"Which transporters moved the highest volume in January?"*
> *"Compare AGO and PMS delivery trends over time"*

---

### NOTE: The Code base is currently private due to being realted to the government but is available upon request.

---
## ✨ Features

| Feature | Description |
|---|---|
| 🤖 **Natural Language Queries** | Ask questions in plain English and get instant visualisations |
| 📊 **Auto Chart Generation** | Bar, line, pie, and scatter charts generated automatically |
| 🗺️ **Nigeria Choropleth Map** | State-level distribution maps with bubble overlays using real GeoJSON |
| 📋 **Smart Tables** | Sortable, filterable data tables with aggregation |
| 💡 **AI Explanations** | Every result comes with a plain English insight powered by LLaMA 3.3 |
| 💬 **Sample Questions** | Pre-built questions to guide users |
| 🕓 **Query History** | Sidebar tracks recent queries with type labels |
| ⚠️ **Graceful Error Handling** | Unrelated questions are handled with helpful suggestions |

---

## 🏗️ Project Structure

```
smart_dashboard/
│
├── app.py                        # Entry point — starts the Dash server
├── .env                          # API keys and config (never commit this)
├── .env.example                  # Template for environment variables
├── .gitignore                    # Excludes .env, cache, data files
├── requirements.txt              # All dependencies with pinned versions
├── README.md                     # This file
│
├── config/
│   └── settings.py               # Loads and validates all env variables on startup
│
├── data/
│   ├── data.parquet              # Company petroleum truckout dataset
│   ├── nigeria.geojson           # Nigeria state boundary shapes
│   └── state_coordinates.json   # Lat/lon centres for all 37 states
│
├── services/
│   ├── data_service.py           # Loads Parquet, serves DataFrames, generates schema
│   ├── llm_service.py            # All Groq API calls — interprets questions + explanations
│   └── chart_service.py          # Builds Plotly charts, maps, and tables from instructions
│
├── layout/
│   └── main_layout.py            # Full Dash UI layout and component definitions
│
├── callbacks/
│   └── dashboard_callbacks.py    # All Dash interactivity and reactive logic
│
├── utils/
│   └── logger.py                 # Structured logging setup
│
└── assets/
    └── style.css                 # Custom CSS — NMDPRA brand colours and design
```

---

## ⚙️ How It Works

```
You type a question
        ↓
Dash detects the input and triggers a callback
        ↓
Your question + full dataset schema → sent to Groq API
        ↓
LLaMA 3.3 70B interprets the question
and returns structured JSON instructions
        ↓
Pandas queries your local Parquet file
using those instructions
        ↓
Plotly builds the chart / map / table
        ↓
Groq generates a plain English explanation
using the actual data results
        ↓
Dashboard renders everything on screen
```

> **Important:** The LLM never touches your data. It only translates English into instructions. All data processing happens locally on your machine using Pandas.

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/nmdpra-dashboard.git
cd nmdpra-dashboard
```

### 2. Create a virtual environment

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# Mac / Linux
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Get your free Groq API key

Go to [console.groq.com](https://console.groq.com) → Sign up → API Keys → Create Key.
No credit card required.

### 5. Set up your environment file

```bash
cp .env.example .env
```

Open `.env` and fill in your values:

```env
GROQ_API_KEY=your_groq_api_key_here
DATASET_PATH=data/data.parquet
STATE_COORDS_PATH=data/state_coordinates.json
GEOJSON_PATH=data/nigeria.geojson
APP_ENV=development
APP_DEBUG=True
APP_PORT=8050
```

### 6. Run the app

```bash
python app.py
```

Open your browser and go to:

```
http://localhost:8050
```

---

## 🌍 Environment Variables

| Variable | Required | Description |
|---|---|---|
| `GROQ_API_KEY` | ✅ Yes | Your Groq API key from console.groq.com |
| `DATASET_PATH` | No | Path to your Parquet file (default: `data/data.parquet`) |
| `STATE_COORDS_PATH` | No | Path to state coordinates JSON (default: `data/state_coordinates.json`) |
| `GEOJSON_PATH` | No | Path to Nigeria GeoJSON (default: `data/nigeria.geojson`) |
| `APP_ENV` | No | `development` or `production` (default: `development`) |
| `APP_DEBUG` | No | `True` or `False` (default: `True`) |
| `APP_PORT` | No | Port to run on (default: `8050`) |

---

## 📦 Dependencies

| Package | Version | Purpose |
|---|---|---|
| `dash` | 2.18.1 | Web framework and UI |
| `plotly` | 5.24.1 | Charts and map visualisations |
| `pandas` | 2.2.3 | Data processing and aggregation |
| `pyarrow` | 17.0.0 | Reading Parquet files |
| `groq` | 0.13.0 | LLaMA 3.3 70B API client |
| `python-dotenv` | 1.0.1 | Environment variable management |

---

## 📊 Dataset Columns

| Column | Type | Description |
|---|---|---|
| `Truckout date` | Date | Date the product left the depot |
| `Product` | String | Petroleum product (PMS, AGO, DPK, LPG, ATK) |
| `Source depot` | String | Origin depot or terminal |
| `Refinery` | String | Where the product was refined |
| `Quantity loaded` | Float | Volume in litres |
| `Price` | Float | Price per litre in Naira |
| `Transporter` | String | Logistics company |
| `Destination state` | String | Nigerian state receiving the product |
| `Customer` | String | End customer or company |
| `Location` | String | Specific delivery location |
| `Product supply` | String | Supply source (Import, Refinery, etc.) |
| `Truckout distribution` | String | Local or Bridging |
| `Off-taker` | String | End-use category (Retail, Marketer, etc.) |

---

## 💡 Example Questions to Ask

- *Show me total PMS volume by destination state on a map*
- *Which transporters moved the highest quantity loaded?*
- *Compare PMS and AGO volumes by month as a line chart*
- *What is the breakdown of off-takers as a pie chart?*
- *Show me the top 10 customers by quantity received*
- *What is the total value of deliveries per product?*
- *Show bridging vs local distribution breakdown*
- *Which source depots supplied the most volume?*

---

## 🔒 Security Notes

- Your API key is stored in `.env` which is excluded from git via `.gitignore`
- The LLM returns **instructions only** — it never executes code or accesses your data directly
- All data processing happens locally on your machine
- Never commit your `.env` file to version control

---

## 🗺️ Map Coverage

The dashboard includes full choropleth map support for all **37 Nigerian states and FCT** using an official GeoJSON boundary file. Each state is filled by value intensity with yellow bubble overlays showing relative magnitude.

---

## 📈 Groq Free Tier Limits

| Limit | Value |
|---|---|
| Requests per minute | 30 |
| Tokens per minute | 6,000 |
| Tokens per day | 500,000 |
| Approximate questions per day | ~700 |

When the limit is reached the dashboard shows a friendly error message and resets automatically — no charges, no action needed.

---

## 🛣️ Roadmap (Future Versions)

- [ ] Date range filter panel
- [ ] Export chart as PNG / table as CSV
- [ ] Multi-chart KPI dashboard view
- [ ] User authentication
- [ ] Production deployment (Railway / Render)
- [ ] Support for multiple dataset uploads

---

## 👨‍💻 Built With

- [Dash by Plotly](https://dash.plotly.com/) — Python web framework for data apps
- [Groq](https://groq.com/) — Ultra-fast LLM inference
- [LLaMA 3.3 70B](https://groq.com/) — Meta's open source language model
- [Pandas](https://pandas.pydata.org/) — Data analysis library
- [Plotly](https://plotly.com/) — Interactive visualisation library

---

## 📄 License

This project is proprietary and built exclusively for the **Nigerian Midstream and Downstream Petroleum Regulatory Authority (NMDPRA)**.

---

<p align="center">
  Built by Eze with Love &nbsp;·&nbsp; Powered by Groq &nbsp;·&nbsp; Made for NMDPRA
</p>
