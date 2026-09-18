# COMRADE-REPO⚡
GridWise: Smart Campus Microgrid Management System
GridWise is an AI-assisted microgrid energy management and optimization engine designed for smart campus infrastructure. It bridges unstructured operator insights with mathematical optimization to schedule 24-hour energy storage, grid imports, and solar generation efficiently.
The system combines LLM-driven natural language interpretation, deterministic guardrails, and linear programming (PuLP) to minimize total grid power costs while strictly respecting battery limits and operational grid constraints.
🌟 Key Features
●	Natural Language Operator Directives: Input operational constraints in plain English (e.g., cloud cover forecasts, discharge block windows, or reserve overrides).
●	Structured LLM Parsing: Powered by OpenAI (gpt-4o-mini) using Pydantic structured outputs to safely extract numerical targets and time windows.
●	Deterministic Safety Guardrails: Sanitizes, checks bounds, and validates all LLM output before passing constraints to the mathematical solver.
●	Optimal 24-Hour Energy Scheduling: Utilizes PuLP (CBC Solver) to find the globally optimal energy balance, minimizing cost (BDT) while accounting for tariffs, battery state of charge (SoC), and load requirements.
●	Interactive Web Dashboard: Built with Tailwind CSS and Chart.js for real-time visualization of hourly power flows, cost metrics, and battery charge states.
🏗️ System Architecture
[ Operator Input (NL) ] ──> [ LLM Interpreter ] ──> [ Deterministic Guardrails ]
                                                             │
                                                             ▼
[ 24-Hour Load Profile ] ───────────────────────> [ Linear Optimizer (PuLP) ]
                                                             │
                                                             ▼
[ Visual Dashboard / API ] <─────────────────────── [ Hourly Schedule & KPIs ]

Supported Directive Types
1.	solar_reduction: Derates solar output by a specified fraction across target hours.
2.	minimum_battery_reserve: Sets a higher battery energy floor (kWh).
3.	no_charge_window: Restricts battery charging during high-demand or high-cost windows.
4.	no_discharge_window: Restricts battery discharge during key operational periods.
5.	max_grid_window: Imposes a power import cap on grid electricity for specific hours.
6.	no_op: Safely ignores irrelevant or unparseable notes.
📂 Project Structure
.
├── main.py              # FastAPI application & endpoint routing
├── models.py            # Pydantic schema validation for request/response & LLM output
├── llm_interpreter.py   # OpenAI client setup for structured interpretation
├── guardrails.py       # Deterministic input validation & fallback mechanics
├── optimizer.py         # PuLP linear programming optimization logic
└── index.html           # Interactive web dashboard (Tailwind CSS + Chart.js)

🚀 Getting Started
Prerequisites
●	Python 3.9+
●	OpenAI API Key (or set via environment variable)
Installation
1.	Clone the Repository
git clone https://github.com/your-username/gridwise.git
cd gridwise

2.	Create a Virtual Environment & Install Dependencies
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install fastapi uvicorn pulp openai pydantic

3.	Set Up API Key
Set your OpenAI API key in your environment variables:
export OPENAI_API_KEY="your-actual-api-key"

🏃 Running the Application
1.	Start the FastAPI Backend
uvicorn main:app --reload --port 8000

The server will be live at http://localhost:8000. You can inspect the API documentation at http://localhost:8000/docs.
2.	Launch the Dashboard
Simply open index.html in your web browser, or serve it using a lightweight server:
python -m http.server 8080

Open http://localhost:8080 to access the interactive dashboard.
🔌 API Reference
POST /optimize-energy
Executes the optimization pipeline.
Sample Request Payload:
{
  "scenario_id": "CAMPUS_OPT_001",
  "operator_notes": [
    "Expected heavy cloud cover reduces solar by 80% from 11 AM to 2 PM",
    "Do not discharge battery during peak hours 18 to 21"
  ],
  "battery": {
    "capacity_kwh": 100.0,
    "initial_energy_kwh": 20.0,
    "minimum_energy_kwh": 10.0,
    "max_charge_kwh_per_hour": 25.0,
    "max_discharge_kwh_per_hour": 25.0
  },
  "hours": [ /* 24 hourly entries containing demand_kwh, solar_kwh, tariff_bdt_per_kwh */ ]
}

🛡️ License
Distributed under the MIT License. See LICENSE for more information.
