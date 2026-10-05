# SecureShope

SecureShop — Vulnerable Web App Security Lab

A deliberately vulnerable full-stack e-commerce app, built as a hands-on playground for learning web application security testing. I'm using it to practice OWASP-style methodology: recon, authentication testing, authorization testing, input testing, and more — then documenting each vulnerability I find like a real pentest finding.

This is a learning project, not a production app, and it is designed to be run locally only.

Why I built this

I wanted to go beyond reading about vulnerabilities (IDOR, broken access control, XSS, SQL injection, etc.) and actually find them myself in a realistic-looking application, the way a security analyst would — using browser DevTools, the API docs, and a repeatable testing methodology, rather than just being told where the bugs are.

Stack
Backend: Python, FastAPI, SQLAlchemy, SQLite
Frontend: React, Vite, Tailwind CSS
Auth: JWT
Running it locally
bash
# Backend
cd backend
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python seed_data.py
uvicorn main:app --reload --port 8000

# Frontend (separate terminal)
cd frontend
npm install
npm run dev

App: http://localhost:5173 · API docs: http://localhost:8000/docs

Sample accounts: see README.md in this repo.

Methodology

I'm working through this in phases, loosely following standard web app pentest methodology:

Reconnaissance — mapping endpoints, inspecting requests/responses, understanding auth tokens
Authentication testing — JWT handling, login behavior, rate limiting
Authorization testing — IDOR, horizontal/vertical privilege escalation
Input testing — search, forms, comments, URL/body parameters
Advanced testing — file upload handling, business logic, misconfiguration, information disclosure

Findings are logged in FINDINGS.md as I confirm them, written in the style of a real vulnerability report (endpoint, impact, severity, remediation).
