# Stock Tracker (FastAPI + Marketstack + React)

A simple stock tracking app:
- Backend: FastAPI, SQLAlchemy, Marketstack API (EOD), Excel import/export
- DB: SQLite (default) or PostgreSQL
- Frontend: React (Vite) and a simple static HTML/JS page

## Features
- Sync EOD prices from Marketstack by symbol
- Store and query prices from SQLite or PostgreSQL
- Import prices from Excel files
- Export prices to Excel files
- React UI with chart, search, upload, and export
- Simple static HTML page for quick tests

## Prerequisites
- Python 3.10+
- Node.js 18+
- (Optional) Docker & docker-compose

## Configure Environment
Copy and edit env example:
```
cp backend/.env.example backend/.env
```
Edit variables:
- MARKETSTACK_API_KEY
- DATABASE_URL (default SQLite)
- CORS_ORIGINS (comma-separated origins, e.g. http://localhost:5173)

## Backend
Install and run:
```
cd backend
python -m venv .venv
. .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

Health check:
- GET http://localhost:8000/api/health

API examples:
- Sync prices: POST http://localhost:8000/api/stocks/sync body: {"symbol":"AAPL","start_date":"2024-01-01","end_date":"2024-12-31","limit":1000}
- List stocks: GET http://localhost:8000/api/stocks
- Get prices: GET http://localhost:8000/api/stocks/AAPL/prices?start=2024-01-01&end=2024-12-31
- Import Excel: POST http://localhost:8000/api/stocks/import/excel (form-data file=<xlsx>)
- Export Excel: GET http://localhost:8000/api/stocks/AAPL/export/excel

Excel columns required for import:
- symbol,date,open,high,low,close,volume
- date format ISO (YYYY-MM-DD or datetime)

## Frontend (React)
```
cd frontend
npm install
npm run dev
```
Open http://localhost:5173

## Simple Static Frontend
Open static/simple.html in a browser or serve via any static server.

## PostgreSQL via Docker Compose (optional)
```
docker-compose up -d
# Then set DATABASE_URL in backend/.env to:
# postgresql+psycopg://postgres:postgres@localhost:5432/stockdb
```

## Notes
- Marketstack free plan uses EOD data and typically HTTP base URL. Ensure your API key is valid.
- The app deduplicates prices by (stock_id, date).
