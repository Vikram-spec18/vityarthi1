# Personal Finance & Expense Tracker

A command-line personal finance management system built in pure Python. Users
can register an account, log income and expenses, set category budgets, and
get analytical reports — including a simple linear-regression forecast of
next month's spending.

## Overview

Managing day-to-day personal finances across scattered notes, bank apps and
spreadsheets makes it hard to see spending patterns or stay within a budget.
This project gives a single, structured place to record every transaction,
see where money goes by category, track budgets against actual spend, and
get an early warning about next month's likely expenses — all backed by a
local SQLite database, with no external services or accounts required.

## Features

- **User accounts** — registration and login with salted, hashed passwords (PBKDF2-HMAC-SHA256)
- **Transaction CRUD** — add, list (with filters/pagination), update, and delete income/expense entries
- **Category budgets** — set a monthly spending cap per category
- **Reporting & analytics**
  - Category-wise spending breakdown
  - Month-by-month income / expense / net summary for a given year
  - Budget-vs-actual comparison for the current month
  - Running net-worth (cumulative balance) trend
- **Spending forecast** — least-squares linear regression over recent months projects next month's likely spend per category and flags rising/falling trends
- **Input validation** on every field (usernames, emails, passwords, amounts, categories, dates)
- **Structured logging** of every operation to `data/app.log`
- **Automated tests** covering validators and core transaction/analytics workflows

## Technologies / Tools Used

- Python 3.10+ (standard library only for the app itself: `sqlite3`, `hashlib`, `logging`, `dataclasses`)
- SQLite for persistent storage
- `pytest` for automated testing (dev dependency)

## Project Structure

```
finance_tracker/
├── main.py                    # CLI entry point / menu-driven workflow
├── config.py                  # Central configuration (paths, limits, categories)
├── database.py                # SQLite connection + schema management
├── models.py                  # Dataclasses: User, Transaction, Budget
├── user_manager.py            # Module 1: registration, login, profile CRUD
├── transaction_manager.py     # Module 2: transaction & budget CRUD
├── analytics.py                # Module 3: reports & aggregations
├── budget_predictor.py         # Module 4: linear-regression spend forecast
├── validators.py               # Input validation + custom exceptions
├── logger.py                   # Centralized logging configuration
├── tests/
│   ├── test_validators.py
│   └── test_transaction_manager.py
├── data/                       # SQLite DB + log file created at runtime (gitignored)
├── requirements.txt
├── statement.md
└── README.md
```

## Steps to Install & Run

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd finance_tracker
   ```

2. **(Optional) create a virtual environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate   # Windows: venv\Scripts\activate
   ```

3. **Install dependencies** (only needed to run the test suite)
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the application**
   ```bash
   python main.py
   ```
   The database and log file are created automatically in `data/` on first run.

5. **Try it out**
   - Choose `2` to register a new account, then log in.
   - Add a few `INCOME` and `EXPENSE` transactions across different categories and dates.
   - Set a budget for a category, then check "Budget vs actual".
   - After you have a couple of months of expense history, try "Forecast next month's spending".

## Instructions for Testing

Run the automated test suite from the project root:

```bash
pip install -r requirements.txt
pytest tests/ -v
```

Tests cover:
- Field validation rules (`tests/test_validators.py`)
- Transaction CRUD, category breakdown, budget-vs-actual, and forecast generation against an isolated temporary database (`tests/test_transaction_manager.py`)

## Screenshots

_Not included — this is a terminal (CLI) application. Run `python main.py` to see it live._
