# PayPilot AI

**Your money, your business, one AI assistant.**

PayPilot AI is a hackathon prototype for freelancers and small businesses. It combines an AI-assisted payment operations interface with a PayPal sandbox checkout workflow.

## What works in this prototype

- Finance dashboard with clearly labelled illustrative sample invoice records.
- Invoice list with status, due dates, and outstanding amounts.
- Assistant that summarizes the sample invoice data. If `OPENAI_API_KEY` is configured, it can use an LLM; otherwise a local rule-based demo assistant works.
- PayPal sandbox order creation and checkout approval when sandbox credentials are configured.
- Server-side PayPal order capture and status update after successful capture.
- Responsive layout for desktop and mobile.

## Important limitations

- The invoice records are sample data stored in memory. They are not connected to a real accounting system or a user's PayPal account.
- The prototype does not send emails or reminders.
- PayPal checkout requires your own developer sandbox REST API credentials. Do not use live credentials for this hackathon demo.
- This is a demo, not production-ready financial software. Before production use, add authentication, persistent storage, CSRF protections where applicable, webhook-based reconciliation, audit logging, robust authorization, and comprehensive tests.
- Never commit `.env` or API secrets.

## Run locally

Requires Python 3.10+.

### 1. Create and activate a virtual environment

```bash
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows PowerShell
.venv\Scripts\Activate.ps1
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure optional integrations

```bash
cp .env.example .env
```

On Windows, copy `.env.example` to `.env` manually if `cp` is unavailable.

- For AI responses, set `OPENAI_API_KEY`.
- For PayPal checkout, create a **sandbox** REST app in the PayPal Developer Dashboard and set `PAYPAL_CLIENT_ID` and `PAYPAL_CLIENT_SECRET`.
- Keep the base URL set to `https://api-m.sandbox.paypal.com`.

The app can run without credentials in demo mode; AI and PayPal integrations will be limited accordingly.

### 4. Start the app

From the repository root:

```bash
uvicorn app.main:app --reload
```

Open http://localhost:8000. API health status is available at http://localhost:8000/api/health.

## Demo flow

1. Open the dashboard and review the sample figures.
2. In the AI Assistant, choose **Show my unpaid invoices**.
3. Select **Prepare PayPal payment** for an unpaid invoice. If sandbox credentials are configured, the app creates a PayPal sandbox order and redirects to its approval page.
4. Approve with a PayPal sandbox buyer account.
5. On return, confirm capture. The app verifies the capture response and marks the corresponding in-memory sample invoice as paid.

Because the sample records are stored in memory, a server restart resets the status.

## API endpoints

- `GET /api/health` — reports demo/sandbox and AI configuration mode.
- `GET /api/invoices` — returns illustrative sample invoices.
- `POST /api/assistant` — body: `{"message":"Show my unpaid invoices"}`.
- `POST /api/paypal/create-order` — body: `{"invoice_id":"INV-104"}`.
- `POST /api/paypal/capture-order` — body: `{"order_id":"PAYPAL_ORDER_ID"}`.

## Tools used

- Python, FastAPI, HTTPX
- PayPal REST APIs (sandbox)
- Optional OpenAI API for LLM responses
- HTML, CSS, JavaScript

## License

MIT — see `LICENSE`.
