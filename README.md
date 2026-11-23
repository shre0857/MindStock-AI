# MindStock AI

Frontend-first college pharmacy medicine-inventory management prototype built with React, Tailwind, Recharts, localStorage and Google Gemini.

## Features
- Responsive sage-green and cream SaaS dashboard with inventory KPIs and interactive charts
- Medicine add/edit/delete/search, including expiry dates with browser localStorage persistence
- Rule-based low-stock detection and suggested reorder quantities
- Local draft purchase orders with CSV export
- Gemini inventory chatbot that receives current medicine data with every question

## Setup
1. Install Node.js 18+.
2. Run `npm install`.
3. Run `npm run dev` to preview the frontend at the displayed localhost address.
4. For **working Gemini chat**, deploy to Vercel and set `GEMINI_API_KEY` in Project Settings → Environment Variables. Optionally set `GEMINI_MODEL` (default `gemini-2.5-flash`). Redeploy after setting it. Vercel automatically deploys `api/chat.js` as a serverless function.
5. For local end-to-end testing, install the Vercel CLI (`npm i -g vercel`), run `vercel login`, `vercel link`, `vercel env pull .env.local`, and `vercel dev`. The plain Vite dev server does not serve `/api/chat`.

Get an API key from Google AI Studio. Never prefix the key with `VITE_` or place it in frontend source. `.env*` is gitignored.

## Notes
- Inventory and purchase drafts are stored **only in this browser**, not synced across devices.
- Reorder quantity uses `max(0, monthlyDemand + reorderLevel - quantity)`. This is a transparent heuristic, not a machine-learning forecast.
- Purchase orders are local drafts only; they are not sent to suppliers.
- The serverless endpoint is a demonstration endpoint with input validation and no login/rate limiting. For a public production deployment, add authentication, quotas and rate limiting to prevent API abuse.
- Chat messages are stored in localStorage and sent to Gemini when chatting. Do not enter sensitive business data in the demo.

## Deploy on Vercel (Gemini enabled)
1. Push the extracted project folder to a GitHub repository (keep `.env` files and API keys out of Git).
2. In Vercel, choose **Add New → Project**, import the repository, select **Vite** as the framework, and deploy.
3. Open **Project → Settings → Environment Variables**. Add `GEMINI_API_KEY` with your Google AI Studio API key (no `VITE_` prefix). Optionally add `GEMINI_MODEL=gemini-2.5-flash`.
4. Under **Deployments**, redeploy the project after adding the key.
5. Visit your Vercel URL, open **AI Assistant**, and ask “Which medicines are low in stock?”

**Deployment requires your own Vercel/GitHub access and Gemini key; the downloaded ZIP is deployment-ready but not hosted yet.**

Existing users of the generic-inventory prototype will receive fresh medicine demo data under a new localStorage key (`mindstock-medicines-v2`).


## Demo inventory
The prototype includes **25 sample medicines** across common pharmacy categories, with quantities, reorder thresholds, unit prices, estimated monthly demand, and expiry dates. These are illustrative demo values, not clinical or procurement advice. Existing localStorage inventory from v2 is retained where present; to see the new 25-item seed on an existing browser, clear the `mindstock-medicines-v2` and `mindstock-medicines-v3` localStorage keys and reload.
