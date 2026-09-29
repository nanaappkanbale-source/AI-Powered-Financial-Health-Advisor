# Credit Assistant — AI-Powered Credit Health & Financial Wellness Platform

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110%2B-green.svg)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-19-61dafb.svg)](https://react.dev/)
[![Tailored For](https://img.shields.io/badge/Ecosystem-India%20(CIBIL%20%7C%20INR%20%E2%82%B9)-orange.svg)]()
[![AI Engine](https://img.shields.io/badge/AI-Google%20Gemini-purple.svg)]()

> A full-stack, industry-ready financial wellness platform engineered specifically for the **Indian financial and banking ecosystem**. Credit Assistant enables users to monitor their CIBIL score, calculate Debt-to-Income (DTI) and credit utilization ratios, track loans and EMIs, and receive personalized 5-step improvement roadmaps powered by **Google Gemini AI**.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Key Features](#key-features)
3. [Technology Stack](#technology-stack)
4. [System Architecture](#system-architecture)
5. [Project Directory Structure](#project-directory-structure)
6. [Prerequisites & Requirements](#prerequisites--requirements)
7. [Installation & Setup (Windows)](#installation--setup-windows)
   - [Backend Setup](#1-backend-setup)
   - [Frontend Setup](#2-frontend-setup)
8. [Environment Variables & Configuration](#environment-variables--configuration)
9. [Running the Application](#running-the-application)
10. [One-Click Demo Mode (For Evaluators & Viva)](#one-click-demo-mode-for-evaluators--viva)
11. [API Documentation (Swagger UI)](#api-documentation-swagger-ui)
12. [Testing](#testing)
13. [Future Enhancements](#future-enhancements)

---

## Project Overview

In the Indian financial landscape, securing competitive interest rates on home loans, auto loans, and premium credit cards requires a strong **CIBIL score (750+)**, low **credit card utilization (<30%)**, and a safe **Debt-to-Income (DTI) ratio (<35%)**. 

**Credit Assistant** provides an all-in-one platform where Indian borrowers can:
- Safely log and monitor their credit scores over time.
- Instantly observe how loan payments reduce their DTI ratio.
- Identify credit bottlenecks (e.g. late payments, revolving card balances).
- Receive personalized, structured credit improvement roadmaps from **Google Gemini AI**.

---

## Key Features

- **Secure Authentication & Session Management**:
  - Direct Bcrypt password hashing (salted, 12 rounds).
  - Stateless JSON Web Token (JWT) Bearer authentication.
  - Protected API routes and protected React Router views.
- **Automated Metric Engines**:
  - **Debt-to-Income (DTI) Ratio**: $\text{DTI} = \left(\frac{\text{Total Monthly Debt Payments}}{\text{Gross Monthly Income}}\right) \times 100$
  - **Credit Card Utilization**: $\text{Utilization} = \left(\frac{\text{Outstanding Credit}}{\text{Total Credit Limit}}\right) \times 100$
  - Recalculates dynamically in real time as inputs change.
- **Credit Score Tracking & History**:
  - Records incremental point increases/decreases.
  - Tracks previous score, highest score, lowest score, and overall improvement.
  - Interactive **Recharts line chart** showing historical trajectory.
- **Loan & EMI Portfolio Manager**:
  - Add, track, and manage active and closed loans.
  - One-click **"Mark Paid"** feature that zeroes out loan balance, recalculates overall debt, drops DTI ratio, and refreshes dashboard charts.
- **Transparent Credit Health Assessment**:
  - Classifies health into **Excellent (750+)**, **Good (700-749)**, **Fair (650-699)**, or **Needs Attention (<650)**.
  - Transparently explains *why* the profile received its rating (utilization stress, high DTI, missed payments).
- **Google Gemini AI Financial Advisor**:
  - Dedicated AI service querying Gemini models with structured prompt engineering.
  - Structured JSON response containing:
    1. Credit Health Analysis
    2. Main Bottlenecks / Problems
    3. Practical **5-Step Action Plan**
    4. Personalized Recommendations
    5. Goal Strategy
    6. Educational disclaimer
  - **Resilient Fallback**: If no Gemini API key is configured or when offline, an expert Indian financial reasoning engine seamlessly generates the full structured advice with zero downtime.
- **Evaluator-Friendly Demo Mode**:
  - One-click **"Load Demo Data"** pre-populates a realistic Indian scenario (Rajesh Sharma: ₹50,000 income, ₹2,00,000 debt across HDFC & SBI loans, historical scores 680 → 705 → 735, and AI advice).

---

## Technology Stack

### Frontend
- **Framework**: React.js 19 with Vite 8
- **Language**: JavaScript (ES6+)
- **Routing**: React Router DOM (v7)
- **Data Visualization**: Recharts
- **HTTP Client**: Axios (configured with JWT interceptors)
- **Icons**: Lucide React
- **Design**: Custom FinTech CSS design system with responsive flex/grid and CSS variables

### Backend
- **Framework**: FastAPI (Python 3.8 - 3.14+)
- **Data Validation & Schemas**: Pydantic v2
- **ORM & Database**: SQLAlchemy 2.0 (SQLite for local development, PostgreSQL-ready)
- **Authentication**: Direct Bcrypt & PyJWT (HS256)
- **ASGI Server**: Uvicorn
- **HTTP Client**: HTTPX (asynchronous Gemini REST caller)
- **Testing**: Pytest & FastAPI TestClient

---

## System Architecture

```
User (Browser)
      │
      ▼
React 19 Frontend (Vite)
      │
      ▼  [HTTP REST + JWT Bearer via Axios / Proxy]
FastAPI REST API Server (Port 8000)
      ├── Authentication & Security (Bcrypt + PyJWT)
      ├── Business Engines (DTI %, Credit Utilization %, Tier Classifiers)
      ├── SQLAlchemy ORM (Dialect-agnostic models)
      │     └── SQLite Database (credit_assistant.db) [PostgreSQL-Ready]
      └── AI Advisor Service
            └── Google Gemini REST API (or Resilient Indian Financial Engine)
```

---

## Project Directory Structure

```
credit-assistant/
│
├── backend/
│   ├── main.py                     # FastAPI application entrypoint & middleware
│   ├── database.py                 # SQLAlchemy engine, sessionmaker & Base
│   ├── models/                     # SQLAlchemy ORM database models
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── financial_profile.py
│   │   ├── credit_score_history.py
│   │   ├── loan.py
│   │   └── ai_recommendation.py
│   ├── schemas/                    # Pydantic v2 request & response schemas
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── user.py
│   │   ├── financial_profile.py
│   │   ├── credit_score.py
│   │   ├── loan.py
│   │   ├── ai.py
│   │   └── dashboard.py
│   ├── routers/                    # REST API endpoints
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── users.py
│   │   ├── financial_profile.py
│   │   ├── credit_score.py
│   │   ├── loans.py
│   │   ├── dashboard.py
│   │   ├── ai.py
│   │   └── demo.py
│   ├── services/                   # Business logic calculations & demo seeds
│   │   ├── calculations.py
│   │   ├── health_assessment.py
│   │   └── demo_data.py
│   ├── utils/                      # Config settings & security utilities
│   │   ├── config.py
│   │   └── security.py
│   ├── ai/                         # Gemini AI service integration
│   │   └── gemini_service.py
│   ├── tests/                      # Automated test suites
│   │   ├── test_api.py             # Pytest unit & integration tests
│   │   └── verify_e2e.py           # End-to-end verification script
│   ├── requirements.txt
│   ├── .env.example
│   └── .env
│
├── frontend/
│   ├── src/
│   │   ├── components/             # Reusable UI components
│   │   │   ├── Navbar.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   ├── MetricCard.jsx
│   │   │   ├── ScoreGauge.jsx
│   │   │   ├── AddLoanModal.jsx
│   │   │   └── ProtectedRoute.jsx
│   │   ├── pages/                  # Application views
│   │   │   ├── LandingPage.jsx
│   │   │   ├── Register.jsx
│   │   │   ├── Login.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── FinancialProfile.jsx
│   │   │   ├── CreditScoreHistory.jsx
│   │   │   ├── Loans.jsx
│   │   │   ├── AIAdvisor.jsx
│   │   │   ├── RecommendationsHistory.jsx
│   │   │   └── ProfileSettings.jsx
│   │   ├── services/               # Axios API service callers
│   │   │   ├── api.js
│   │   │   ├── authService.js
│   │   │   ├── profileService.js
│   │   │   ├── creditScoreService.js
│   │   │   ├── loanService.js
│   │   │   ├── aiService.js
│   │   │   └── dashboardService.js
│   │   ├── context/
│   │   │   └── AuthContext.jsx     # Global authentication provider
│   │   ├── utils/
│   │   │   └── formatters.js       # Indian Rupee (₹) & CIBIL formatters
│   │   ├── App.jsx                 # Route definitions & DashboardLayout
│   │   ├── main.jsx
│   │   └── index.css               # FinTech design system styles
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   └── .env.example
│
├── README.md
└── .gitignore
```

---

## Prerequisites & Requirements

- **Operating System**: Windows 10/11 (or macOS / Linux)
- **Python**: Version 3.8 or higher (Python 3.10 - 3.14 supported)
- **Node.js**: Version 18.x or higher
- **npm**: Version 9.x or higher

---

## Installation & Setup (Windows)

### 1. Backend Setup

Open PowerShell or Command Prompt:

```powershell
# Navigate to the backend directory
cd C:\Users\suvid\.gemini\antigravity\scratch\credit-assistant

# Create Python virtual environment (if not already created)
python -m venv venv

# Activate virtual environment
.\venv\Scripts\activate

# Install required Python dependencies
pip install -r backend\requirements.txt
```

### 2. Frontend Setup

Open a second terminal window:

```powershell
# Navigate to the frontend directory
cd C:\Users\suvid\.gemini\antigravity\scratch\credit-assistant\frontend

# Install Node modules (use npm.cmd in Windows PowerShell)
npm.cmd install
```

---

## Environment Variables & Configuration

### Backend (`backend/.env`)

Copy `backend/.env.example` to `backend/.env`:

```ini
DATABASE_URL=sqlite:///./credit_assistant.db
SECRET_KEY=credit_assistant_jwt_secure_key_2026_dev_env_super_safe_32_bytes
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=1440
GEMINI_API_KEY=your_gemini_api_key_here
PORT=8000
```

> **Note on Gemini API Key**: 
> - If you have a Google Gemini API Key from [Google AI Studio](https://aistudio.google.com/), paste it into `GEMINI_API_KEY`.
> - If left blank or if an invalid key is provided, the platform automatically engages its expert Indian financial reasoning engine with zero downtime.

### Frontend (`frontend/.env`)

```ini
VITE_API_BASE_URL=/api
```

---

## Running the Application

### Start the Backend Server

```powershell
cd C:\Users\suvid\.gemini\antigravity\scratch\credit-assistant
.\venv\Scripts\activate
python -m uvicorn backend.main:app --host 127.0.0.1 --port 8000 --reload
```
The FastAPI backend will start at: `http://127.0.0.1:8000`

### Start the Frontend Dev Server

In another terminal:

```powershell
cd C:\Users\suvid\.gemini\antigravity\scratch\credit-assistant\frontend
npm.cmd run dev
```
The React application will start at: `http://localhost:5173` (or `http://localhost:5174`)

---

## One-Click Demo Mode (For Evaluators & Viva)

To test and demonstrate the entire platform in seconds without manually keying in numbers:

1. Open `http://localhost:5174` (or your Vite dev port).
2. Click **"Sign In"**.
3. Click the green button: **"Instant Demo Login (Rajesh Sharma)"**.
4. You will immediately be logged in and redirected to the **Dashboard**, pre-populated with:
   - **Current CIBIL Score**: 735 (Good Tier)
   - **Monthly Income**: ₹50,000
   - **Outstanding Debt**: ₹2,00,000
   - **DTI Ratio**: 20.0% (Healthy)
   - **Credit Card Utilization**: 35.0%
   - **Recharts Line Chart**: Showing trajectory (680 → 705 → 735)
   - **Active Loans**: HDFC Personal Loan & SBI Auto Loan
   - **AI Consultation**: Structured 5-step action plan and goal strategy
5. Try marking the SBI Auto Loan as **"Mark Paid"** in the **Loans** page — observe how the total debt decreases, DTI drops to 13%, and the dashboard instantly refreshes!

---

## API Documentation (Swagger UI)

FastAPI provides interactive Swagger documentation. When the backend is running, open:

- **Swagger UI**: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- **ReDoc**: [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

---

## Testing

### Automated Pytest Suite

Run the unit and API test suite:

```powershell
cd C:\Users\suvid\.gemini\antigravity\scratch\credit-assistant
$env:PYTHONPATH="."
.\venv\Scripts\pytest.exe backend\tests\test_api.py -v
```

### End-to-End Functional Verification

Run the full 14-step integration test:

```powershell
cd C:\Users\suvid\.gemini\antigravity\scratch\credit-assistant
.\venv\Scripts\python.exe backend\tests\verify_e2e.py
```

All tests will verify:
- Registration & Bcrypt hashing
- JWT login & protected route security
- Dynamic DTI & Credit Utilization calculations
- Credit score trajectory history
- Loan creation, deletion, and auto-sync
- AI advisor response structure
- Demo data seeding and clean reset

---

## Future Enhancements

- **Direct Bureau Connectors**: Integration with official TransUnion CIBIL / Experian APIs upon regulatory authorization.
- **Account Aggregator (AA) Framework**: Integration with RBI-licensed Account Aggregators (e.g., Setu, Anumati) for automated bank statement analysis.
- **Credit Card Recommendation Engine**: Automated matching with lifetime-free cards based on CIBIL tier and monthly spend.
- **Predictive ML Score Modeling**: Machine learning simulations projecting how specific prepayments will boost credit scores over 3, 6, and 12 months.
