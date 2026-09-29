# ⚡ FitBuddy – AI Fitness Plan Generator using Gemini Models

FitBuddy is a full-stack, AI-powered fitness and nutrition assistant built with **FastAPI**, **Jinja2**, **SQLAlchemy**, and **Google Gemini Models (Gemini 1.5 Pro & Gemini 1.5 Flash)**. It dynamically designs personalized 7-day workout routines, provides tailored nutrition and recovery tips, updates plans interactively based on natural language user feedback, and provides an administrative tracking dashboard.

---

## 🏗️ Project Architecture & Workflow

```
                                  +-----------------------+
                                  |     User / Browser    |
                                  +-----------+-----------+
                                              |
                                              v
                              +-------------------------------+
                              |    FastAPI Backend (app)      |
                              |  [main.py | routes.py]        |
                              +---------------+---------------+
                                              |
                +-----------------------------+-----------------------------+
                |                             |                             |
                v                             v                             v
+-------------------------------+ +-----------------------+ +-------------------------------+
|     Frontend (Jinja2/CSS)     | |   Google Gemini AI    | |  SQLite + SQLAlchemy ORM       |
| • index.html (Input Form)     | | • Gemini 1.5 Pro:     | |  fitbuddy.db                  |
| • result.html (Plan & Advice) | |   - 7-day workout     | | • User Table (users)          |
| • all_users.html (Admin Panel)| |   - Feedback update   | | • WorkoutPlan Table (plans)   |
| • style.css & gym-bg.jpg      | | • Gemini Flash:       | |                               |
+-------------------------------+ |   - Nutrition & tips  | +-------------------------------+
                                  +-----------------------+
```

---

## 📁 Project Structure

```
Fitbuddy/
├── .env                          # Local environment variables
├── .env.example                  # Environment configuration template
├── .gitignore                    # Git exclusions
├── requirements.txt              # Project dependencies
├── README.md                     # Documentation & setup guide
├── fitbuddy.db                   # SQLite database (auto-generated)
├── app/
│   ├── __init__.py               # Python package initialization
│   ├── main.py                   # FastAPI application entry point & lifespan
│   ├── routes.py                 # Core routing logic (HTML & REST endpoints)
│   ├── database.py               # SQLAlchemy ORM models & CRUD operations
│   ├── schemas.py                # Pydantic validation schemas
│   ├── gemini_generator.py       # Gemini 1.5 Pro workout generator
│   ├── gemini_flash_generator.py # Gemini Flash nutrition tip generator
│   ├── updated_plan.py           # Feedback-based plan reviser
│   └── nutrition.py              # Macro calculations & recovery advice
├── templates/
│   ├── index.html                # Form page for user registration & plan generation
│   ├── result.html               # 7-day routine, nutrition tip & feedback form
│   └── all_users.html            # Admin dashboard to view & delete users
├── static/
│   ├── css/
│   │   └── style.css             # Fitness UI styling (dark theme, responsive)
│   └── images/
│       └── gym-bg.jpg            # Gym background visual asset
├── tests/
│   └── test_fitbuddy.py          # Automated test suite (9 test cases covering all scenarios)
└── .vscode/
    ├── launch.json               # One-click debugging & server launch configuration
    └── settings.json             # VS Code Python environment & test runner settings
```

---

## 🚀 Quick Setup & Installation in VS Code

### Step 1: Open the Project in VS Code
1. Launch **Visual Studio Code**.
2. Click **File > Open Folder...** and select the `Fitbuddy` directory:
   ```
   C:\Users\DELL\Downloads\Fitbuddy
   ```

### Step 2: Open Terminal in VS Code
Open the integrated terminal with `` Ctrl + ` `` (or **Terminal > New Terminal**).

### Step 3: Virtual Environment Setup
If the virtual environment is not already active, activate it:

**On Windows (PowerShell):**
```powershell
# Create virtual environment if not created
python -m venv fitbuddy-env

# Activate virtual environment
.\fitbuddy-env\Scripts\activate
```

**On macOS / Linux:**
```bash
python3 -m venv fitbuddy-env
source fitbuddy-env/bin/activate
```

### Step 4: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 5: Configure Environment Variables
Copy `.env.example` to `.env` (already created for you):
```bash
# Add your Google Gemini API Key in .env:
GOOGLE_API_KEY=your_actual_gemini_api_key_here
```
> **Tip:** You can obtain a free Gemini API key from [Google AI Studio](https://aistudio.google.com/).
> *Note: If you run FitBuddy without an API key, the app automatically runs in curated demo mode so you can test all features and UI without interruption!*

---

## 🏃 Running the Application

### Option A: Using the VS Code Run & Debug Menu (One-Click)
1. Press `F5` or click on the **Run & Debug** icon on the VS Code Activity Bar.
2. Select **"FitBuddy: Run Server (FastAPI Uvicorn)"**.
3. The server starts automatically with live hot-reloading!

### Option B: Using the Command Line
```powershell
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

Once started:
- 🌐 **Interactive Web UI:** [http://127.0.0.1:8000](http://127.0.0.1:8000)
- 👥 **Admin Dashboard:** [http://127.0.0.1:8000/view-all-users](http://127.0.0.1:8000/view-all-users)
- 📑 **Swagger API Docs:** [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- 📄 **ReDoc Documentation:** [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

---

## 🧪 Testing the 4 Project Scenarios

### Scenario 1: Generate a Personalized 7-Day Workout Plan
1. Navigate to [http://127.0.0.1:8000](http://127.0.0.1:8000).
2. Fill in:
   - **Full Name:** e.g., `Alex Rivers`
   - **User ID:** e.g., `101`
   - **Age:** e.g., `28`
   - **Weight:** e.g., `74`
   - **Fitness Goal:** e.g., `Muscle Gain`
   - **Intensity:** e.g., `High`
3. Click **"Generate Fitness Plan"**.
4. The system invokes **Gemini 1.5 Pro** to construct a day-by-day plan with Warm-up, Main Workout, and Cooldown, plus a **Gemini Flash** nutrition recommendation.

### Scenario 2: Dynamic Plan Feedback & Revision
1. On the plan result page, scroll down to the **"Refine Your Workout with AI"** section.
2. Enter feedback (e.g., *"Focus more on core cardio and add 1 extra rest day for recovery"*).
3. Click **"Submit Feedback & Regenerate Plan"**.
4. **Gemini 1.5 Pro** updates the routine. Both the revised plan and the original plan are preserved in the SQLite database and rendered for comparison.

### Scenario 3: Request Nutrition or Recovery Advice
- The nutrition tip is automatically presented on the plan page.
- You can also query it directly via API:
  ```bash
  curl -X GET "http://127.0.0.1:8000/nutrition-tip?goal=Weight%20Loss"
  ```

### Scenario 4: Admin Oversight Dashboard
1. Click **"Admin Panel"** in the top navigation bar or go to [http://127.0.0.1:8000/view-all-users](http://127.0.0.1:8000/view-all-users).
2. View all registered athletes, their goals, metrics, original plans, and updated plans in a responsive table.
3. Use the instant search bar to filter by athlete name or goal.
4. Admins can delete users and their associated routines using the **Delete** button.

---

## 🔬 Running Automated Tests

Run the complete test suite verifying all 4 scenarios, database persistence, and API endpoints:
```powershell
pytest -v
```
All 9 test cases will execute and validate:
- Homepage rendering (`test_home_page`)
- Scenario 1 plan generation and DB persistence (`test_scenario_1_workout_generation_web`)
- Scenario 2 feedback revision (`test_scenario_2_submit_feedback_web`)
- Scenario 3 nutrition tips (`test_scenario_3_nutrition_tip_api`)
- Scenario 4 admin table and search (`test_scenario_4_admin_dashboard_view`)
- API endpoints (`test_api_generate_workout_gemini`, `test_api_generate_plan`, `test_api_update_plan`)
- User record deletion (`test_delete_user_admin`)
