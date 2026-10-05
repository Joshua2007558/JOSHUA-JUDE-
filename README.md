# FitBuddy – AI Fitness Plan Generator using Gemini Models

### St. Thomas College of Arts and Science
**Affiliated to the University of Madras | 2(f) Status Under UGC Act**  
**Accredited by NAAC with 'A+' Grade | An ISO 21001: 2018 Certified Institution**  
Koyambedu, Chennai - 600 107.

**Department of Computer Science (Shift - II)**  
**Tn Skills Project – Generative AI**

---

### 👥 Team Members
| Sl No | Name | NM ID |
|---|---|---|
| 1 | Joshuajude | `5B8BCE2D308872A95A04B1063A0B25BF` |
| 2 | Prasanth S | `9737E423A4B4FD9902C4BCA7D9BACB0A` |
| 3 | Abilesh V | `80FF6E0E3DCAFB597904F8C308A0445F` |
| 4 | Abinash G | `B5CCA5D755A76AA9CC946F3619DCAE0C` |
| 5 | Santhosh Kumar B | `E3D7334FB228380BE87424541050B785` |

---

## 📌 Project Description
**FitBuddy** is a modern, responsive web application that leverages Google's Gemini Generative AI models to generate personalized 7-day workout plans and tailored nutrition/recovery tips based on a user's fitness profile (name, age, weight, goal, and workout intensity).

FitBuddy features an intelligent feedback revision loop: users can submit direct feedback on their routine (e.g. *"add more core exercises"* or *"focus on flexibility"*), and the AI dynamically refines and saves the updated plan while preserving historical records in SQLite. An administrative panel allows trainers and coaches to review both original and updated plans for all registered users.

---

## 🏗️ Technical Architecture
```
              User (Browser)
                     │
                     ▼
          FastAPI Backend (app/main.py)
         ┌───────────┼───────────┐
         ▼           ▼           ▼
   HTML Templating  Workout Logic Admin Panel
    (Jinja2 + CSS)  (routes.py)  (/view-all-users)
         │           │           │
         ▼           ▼           ▼
     index.html  result.html   all_users.html
                     │
                     ▼
       Gemini 1.5 Pro & Gemini Flash API
            (Google Generative AI)
                     │
                     ▼
           AI Plan / Tip Generator
            (updated_plan.py)
                     │
                     ▼
        SQLite + SQLAlchemy ORM (fitbuddy.db)
          ├── User Table (users)
          └── WorkoutPlan Table (plans)
```

---

## 📂 Project Directory Structure
```
fitbuddy/
│
├── requirements.txt           # Project dependencies
├── .env                       # Environment variables (Gemini API Key)
├── .env.example               # Example template for environment configuration
├── fitbuddy.db                # SQLite database (auto-generated)
├── README.md                  # Project documentation
│
├── app/
│   ├── __init__.py            # Python package indicator
│   ├── main.py                # FastAPI entry point
│   ├── routes.py              # Web and API route handlers
│   ├── database.py            # SQLAlchemy models and database logic
│   ├── schemas.py             # Pydantic models for validation
│   ├── gemini_generator.py    # Gemini Pro - 7-day workout generator
│   ├── gemini_flash_generator.py # Gemini Flash - concise nutrition tips
│   ├── updated_plan.py        # Feedback-based workout updater
│   └── nutrition.py           # Nutrition & macro calculation helpers
│
├── templates/
│   ├── index.html             # User input form
│   ├── result.html            # Workout plan, nutrition tip, and feedback
│   └── all_users.html         # Admin dashboard (all users & plans)
│
└── static/
    └── images/
        └── gym-bg.jpg         # Gym-themed background image
```

---

## 🚀 Setup & Installation Instructions

### 1. Prerequisites
- Python 3.10+ (Python 3.12 recommended)
- Pip package installer
- Internet access (for Google Gemini Generative AI API)

### 2. Configure Environment Variables
Create or edit `.env` in the project root:
```ini
GOOGLE_API_KEY=your_actual_gemini_api_key_here
GEMINI_API_KEY=your_actual_gemini_api_key_here
```
*(Get a free API key from [Google AI Studio](https://aistudio.google.com/))*

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Run the Application
```bash
uvicorn app.main:app --reload
```
or run with python:
```bash
python -m app.main
```

The web application will be accessible at:
- **Web Interface:** `http://127.0.0.1:8000`
- **Admin Dashboard:** `http://127.0.0.1:8000/view-all-users`
- **Interactive Swagger API Docs:** `http://127.0.0.1:8000/docs`
- **ReDoc API Documentation:** `http://127.0.0.1:8000/redoc`

---

## 🔗 Available Endpoints

### Web Templates:
- `GET /` — Renders the homepage form (`index.html`)
- `POST /generate-workout` — Processes user input, generates 7-day plan & nutrition tip, and renders `result.html`
- `POST /submit-feedback` — Updates an existing plan using AI feedback and re-renders `result.html` with confirmation badge
- `GET /view-all-users` — Admin view displaying all registered users and their original & updated workout plans (`all_users.html`)
- `POST /delete-user/{user_id}` — Deletes a user profile and workout plan record

### REST APIs:
- `POST /generate-workout/gemini` — JSON API to generate a workout plan using Gemini Pro
- `GET /nutrition-tip?goal={goal}` — JSON API to fetch a quick nutrition tip using Gemini Flash
- `POST /generate-plan` — JSON API to save user info and generate a plan
- `POST /update-plan/{user_id}` — JSON API to submit feedback and update an existing plan
