# Render deployment checklist

## 1. Repository
- Push the contents of this package to the GitHub repository connected to Render.
- Confirm `server/index.js` is the complete ~224-line file from this package, not the truncated 2,210-line version from the earlier failed deployment.

## 2. Render service
- Runtime: Node
- Build command: `npm install --no-audit --no-fund`
- Start command: `npm start`
- Health check: `/api/health`

## 3. Required environment variables
Set these in Render (never commit real values):

- `DATABASE_URL` — Supabase/PostgreSQL connection string
- `JWT_SECRET` — long random secret, preferably 32+ characters
- `ADMIN_EMAIL` — admin email
- `ADMIN_PASSWORD` — unique password, 12+ characters
- `APP_URL` — public Render URL or custom domain
- `CORS_ORIGIN` — same public origin; comma-separate additional trusted origins if needed

Optional features:
- `RESEND_API_KEY`
- `EMAIL_FROM`
- `CHATWAY_WIDGET_ID`
- `PASSWORD_RESET_HOURS`
- payment beneficiary/bank variables

## 4. Database
The startup sequence runs:
1. `server/migrate.js`
2. `server/seed.js`
3. `server/index.js`

Use a dedicated production database. If reusing an older database, review schema compatibility before deployment.

## 5. Verify after deploy
Open:
- `/api/health` — should return JSON with `ok: true`
- `/` — marketplace UI
- `/api/config` — should return Chatway configuration without exposing secrets

Then test registration/login, product browsing, cart/checkout, seller flow, admin login, oil market, disputes, chat, shipment tracking, and password reset if email is configured.
