<div align="center">

# 🧠 Xenvue — Contextual AI Report Generator

### Stakeholder-tailored AI reports from any structured dataset — powered by Perplexity Sonar Pro, Flask & React

[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Flask](https://img.shields.io/badge/Flask-Python-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![Perplexity](https://img.shields.io/badge/Perplexity-Sonar_Pro-20808D?style=for-the-badge)](https://perplexity.ai/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

[![Demo Video](https://img.shields.io/badge/Demo_Video-Google_Drive-4285F4?style=for-the-badge&logo=google-drive)](https://drive.google.com/file/d/1H0WCEDv7dOmABd7vYwn8uNWfJGReon61/view?usp=sharing)

</div>

---

## 🌐 Demo

| | Link |
|---|---|
| 🎬 **Demo Video** | [Watch on Google Drive](https://drive.google.com/file/d/1H0WCEDv7dOmABd7vYwn8uNWfJGReon61/view?usp=sharing) |

> [!WARNING]
> **This project is not deployed publicly.** It uses the Perplexity `sonar-pro` API for AI narrative generation, which incurs billing beyond the free tier. To try it, please **run it locally** following the [Getting Started](#️-local-setup) steps below.

---

## 🚀 What is This?

**Xenvue** is a full-stack web application that ingests structured datasets (CSV/XLSX) and automatically generates AI-powered, stakeholder-tailored reports. The same data is analyzed and narrated differently depending on the audience — Policy Managers get action-oriented policy briefs, Researchers get statistical deep-dives, Finance teams get ROI breakdowns, and Community Members get plain-language summaries.

---

## ✨ Key Features

| Feature | Description |
|--------|-------------|
| 📤 **File Upload & Auto-Analysis** | Upload a `.csv` or `.xlsx` file and the backend immediately cleans, analyses, and profiles it |
| 🏛️ **Policy Manager Report** | Actionable recommendations and compliance framing |
| 👥 **Community Member Report** | Plain language summaries with local context |
| 💰 **Finance Management Report** | ROI, cost-benefit analysis, and budget variance |
| 🔬 **Researcher Report** | Statistical rigour, p-values, confidence intervals, and methodology |
| 🤖 **AI Narrative Generation** | Section narratives generated via Perplexity `sonar-pro` with stakeholder-specific prompts |
| 📊 **Interactive Visualisations** | Plotly charts rendered per stakeholder preference (bar, scatter, pie, financial dashboard) |
| 🔒 **User Authentication** | Session-based auth with signup/login backed by PostgreSQL/MySQL via SQLAlchemy |
| 🗂️ **Department Reports Page** | Dedicated view for browsing reports by department/stakeholder type |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        BROWSER                              │
│   React 18 + TypeScript + Vite + Tailwind CSS               │
│   • Upload page — CSV/XLSX + stakeholder selector           │
│   • Report page — AI narrative + Plotly charts per section  │
│   • Department Reports — browse by stakeholder type         │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP (JSON / multipart)
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                    Flask Backend                             │
│                                                             │
│  POST /api/generate-report                                  │
│    └─ data_processing.py — clean, deduplicate, impute       │
│    └─ bias detection — gender dist, age avg, skew           │
│    └─ trend & demographics extraction by age group          │
│    └─ narrative_generation.py — Perplexity sonar-pro calls  │
│    └─ assemble report object with UUID → store in memory    │
│                                                             │
│  GET /api/report/<report_id> — fetch report by ID           │
│  GET /api/report/policy|community|finance|researcher        │
│  POST /api/signup + /api/login — session-based auth         │
│                                                             │
│  Database: MySQL / PostgreSQL via SQLAlchemy                │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧩 Tech Stack

### Frontend
- **[React 18](https://react.dev)** — Component-based UI framework
- **[TypeScript](https://www.typescriptlang.org)** — Full type safety across components
- **[Vite](https://vitejs.dev)** — Fast build tool and dev server
- **[Tailwind CSS](https://tailwindcss.com)** — Utility-first styling

### Backend
- **[Flask](https://flask.palletsprojects.com)** — Python web framework
- **[Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com)** + **Flask-Migrate** — ORM + migrations
- **[Flask-CORS](https://flask-cors.readthedocs.io)** — Cross-origin support
- **[Werkzeug](https://werkzeug.palletsprojects.com)** — Password hashing

### AI & Data
- **[Perplexity API](https://perplexity.ai)** — `sonar-pro` model via OpenAI-compatible client
- **[Pandas](https://pandas.pydata.org)** + **[NumPy](https://numpy.org)** — Data cleaning and analysis
- **[scikit-learn](https://scikit-learn.org)** — ML utilities
- **[Plotly](https://plotly.com)** + **Matplotlib** + **Seaborn** — Interactive and static visualisations
- **HuggingFace Transformers** + **TextBlob** + **WordCloud** — NLP extras

### Database
- **MySQL / PostgreSQL** — via `DB_URI` environment variable

---

## 📁 Project Structure

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
└── frontend/
    ├── src/
    │   ├── App.tsx                    # Root component, client-side routing
    │   ├── LoginPage.tsx              # Login form with auth flow
    │   ├── SignupPage.tsx             # Registration form
    │   ├── UploadPage.tsx             # File upload + stakeholder selector
    │   ├── ReportPage.tsx             # Full report viewer with all sections & charts
    │   └── DepartmentReportsPage.tsx  # Browse reports by department
    ├── package.json                   # React + Vite + Tailwind CSS dependencies
    └── vite.config.ts                 # Vite dev server config
```

---

## 🚦 Local Setup

### Prerequisites
- Python 3.9+
- Node.js 18+
- A running MySQL or PostgreSQL instance
- A Perplexity API key → [perplexity.ai/settings/api](https://perplexity.ai/settings/api)

### 1. Clone the Repository

```bash
git clone https://github.com/AasthaKapoor27/Xenvue.git
cd Xenvue
```

### 2. Start the Backend

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

```bash
python app.py
```

✅ Backend runs at `http://localhost:5000`

### 3. Start the Frontend

```bash
cd frontend
npm install
npm run dev
```

✅ Open [http://localhost:5173](http://localhost:5173) 🚀

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

## 🔒 Security Notes

> **Important:** Never commit API keys or database credentials to version control.

- Store all secrets in a `.env` file (already in `.gitignore`)
- Use `os.environ.get()` to read keys at runtime
- Both `PERPLEXITY_API_KEY` and `DB_URI` must be set as environment variables before running

---

## 🗺️ Roadmap

- [ ] Persistent report storage in database (currently in-memory with UUID)
- [ ] PDF export of generated reports
- [ ] Support for additional stakeholder types
- [ ] Hosted deployment with managed API billing tier
- [ ] Report comparison view across stakeholder types

---

## 👩‍💻 Team

| Name |
|------|
| Aastha Kapoor |
| Aarushi Shreevastava |
| Rakshita Singh |
| Spriha Podder |

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with ❤️ by [Aastha Kapoor](https://github.com/AasthaKapoor27)**

*If you found this useful, please ⭐ star the repository!*

</div>
