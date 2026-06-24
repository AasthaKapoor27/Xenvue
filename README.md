# Xenvue — Contextual AI Report Generator

Xenvue is a full-stack web application that ingests structured datasets (CSV/XLSX) and automatically generates AI-powered, **stakeholder-tailored reports**. The same data is analyzed and narrated differently depending on the audience — Policy Managers get action-oriented policy briefs, Researchers get statistical deep-dives, Finance teams get ROI breakdowns, and Community Members get plain-language summaries.

---

## ✨ Features

- **File Upload & Auto-Analysis** — Upload a `.csv` or `.xlsx` file and the backend immediately cleans, analyses, and profiles it.
- **Stakeholder-Tailored Narratives** — Reports are generated separately for four audience types:
  - 🏛️ **Policy Manager** — actionable recommendations, compliance framing
  - 👥 **Community Member** — plain language, local context
  - 💰 **Finance Management** — ROI, cost-benefit analysis, budget variance
  - 🔬 **Researcher** — statistical rigour, p-values, confidence intervals, methodology
- **AI Narrative Generation** — Section narratives (Executive Summary, Data Limitations, Bias Risks, Community Concerns, Statistical Methods) are generated via the **Perplexity `sonar-pro` model** using the OpenAI-compatible API.
- **Data Processing Pipeline** — Automated cleaning (deduplication, median imputation), bias detection, trend analysis by age group, and demographic breakdowns.
- **Interactive Visualisations** — Plotly charts rendered per stakeholder preference (bar charts for policy, scatter plots for researchers, pie charts for community, financial dashboards for finance).
- **User Authentication** — JWT-free session-based auth with signup/login backed by a PostgreSQL/MySQL database via SQLAlchemy.
- **Department Reports Page** — Dedicated view for browsing reports by department/stakeholder type.

---

## 🏗️ Project Structure

```
Xenvue/
├── backend/
│   ├── app.py                  # Flask app, all REST API routes
│   ├── ml_main.py              # StakeholderReportGenerator class + WebIntegrationAPI wrapper
│   ├── data_processing.py      # Data cleaning, bias detection, trend & demographics extraction
│   ├── narrative_generation.py # Perplexity API calls for AI-generated narrative sections
│   ├── cloud.py                # DB_URI config (reads from environment)
│   ├── requirements.txt        # Python dependencies
│   └── uploads/                # Temporarily stores uploaded files
│
├── frontend/
│   ├── src/
│   │   ├── App.tsx             # Root component, client-side routing
│   │   ├── LoginPage.tsx       # Login form with auth flow
│   │   ├── SignupPage.tsx      # Registration form
│   │   ├── UploadPage.tsx      # File upload + stakeholder selector
│   │   ├── ReportPage.tsx      # Full report viewer with all sections & charts
│   │   └── DepartmentReportsPage.tsx  # Browse reports by department
│   ├── package.json            # React + Vite + Tailwind CSS dependencies
│   └── vite.config.ts          # Vite dev server config
│
└── README.md
```

---

## 🔌 API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/signup` | Register a new user |
| `POST` | `/api/login` | Authenticate a user |
| `POST` | `/api/generate-report` | Upload file + stakeholder type → returns `reportId` |
| `GET` | `/api/report/<report_id>` | Fetch a generated report by ID |
| `GET` | `/api/report/policy` | Live tailored report for Policy Manager |
| `GET` | `/api/report/community` | Live tailored report for Community Member |
| `GET` | `/api/report/finance` | Live tailored report for Finance Management |
| `GET` | `/api/report/researcher` | Live tailored report for Researcher |

All `/api/*` routes accept and return JSON. File upload uses `multipart/form-data`.

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| Backend | Python, Flask, Flask-SQLAlchemy, Flask-Migrate, Flask-CORS |
| AI / LLM | Perplexity API (`sonar-pro` model) via OpenAI-compatible client |
| Data | Pandas, NumPy, scikit-learn |
| Visualisation | Plotly, Matplotlib, Seaborn |
| Database | MySQL / PostgreSQL (via `DB_URI` env variable) |
| Auth | Werkzeug password hashing |
| NLP extras | HuggingFace Transformers, TextBlob, WordCloud |

---

## ⚙️ Local Setup

### Prerequisites

- Python 3.9+
- Node.js 18+
- A running MySQL or PostgreSQL instance
- A Perplexity API key (get one at [perplexity.ai/settings/api](https://perplexity.ai/settings/api))

### 1. Backend

```bash
cd backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file in `backend/`:

```env
DB_URI=mysql+mysqlconnector://user:password@localhost/xenvue_db
PERPLEXITY_API_KEY=your_pplx_key_here
```

Update `cloud.py` to read from environment:

```python
import os
DB_URI = os.environ.get("DB_URI")
```

Update `narrative_generation.py` to read the key from environment:

```python
import os
client = OpenAI(api_key=os.environ.get("PERPLEXITY_API_KEY"), base_url="https://api.perplexity.ai")
```

Run the Flask server:

```bash
python app.py
# Server starts at http://localhost:5000
```

### 2. Frontend

```bash
cd frontend
npm install
npm run dev
# App starts at http://localhost:5173
```

---

## 📊 How It Works

1. **User signs up / logs in** via the React frontend.
2. **File is uploaded** (CSV or XLSX) along with a stakeholder type selection on the Upload page.
3. The backend **cleans the data** — removes duplicates, imputes missing numeric values with medians, fills remaining nulls with `"Unknown"`.
4. **Bias detection** extracts gender distribution, age averages, and target variable skew.
5. **Trend & demographics extraction** bins data by age group and aggregates gender distribution.
6. Each report **section narrative is generated** by calling the Perplexity `sonar-pro` model with section-specific prompts tailored to the stakeholder type.
7. The assembled **report object** is stored in-memory with a UUID and returned to the frontend.
8. The **Report page** renders each section with the AI narrative and interactive Plotly charts appropriate to the stakeholder.

---

## 🔒 Security Notes

> **Important:** Never commit API keys or database credentials to version control.

- Store all secrets in a `.env` file (already in `.gitignore`)
- Use `os.environ.get()` to read keys at runtime
- The `PERPLEXITY_API_KEY` and `DB_URI` must be set as environment variables before running

---

## 📦 Requirements

```
Flask, Flask-Cors, Flask-SQLAlchemy, Flask-Migrate
mysql-connector-python
pandas, numpy, scikit-learn
transformers, torch
plotly, matplotlib, seaborn
wordcloud, textblob, requests
```

---

## 🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

---

## 📄 License

[MIT](LICENSE)
