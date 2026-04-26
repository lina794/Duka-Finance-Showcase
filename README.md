# Duka (ዱካ)

> A bilingual personal finance tracker for Ethiopian university students — built as a Telegram Mini App.

Duka helps you stop guessing where your money went. Paste a banking SMS from Telebirr, CBE Birr, or M-PESA — Duka reads it, extracts the details, and logs the transaction for you. All in English or አማርኛ.

---

## Core Features

### 💬 Smart SMS Parsing
- Paste receipts from **Telebirr, CBE Birr, and M-PESA** — Duka auto-extracts amount, type, provider, recipient, fees, VAT, and account balance
- Full **Amharic script (Ethiopic)** SMS support — the parser detects the script automatically and routes accordingly
- **Bulk paste**: paste multiple SMS messages at once; Duka detects and saves them all in one action
- **Duplicate detection** — checks by transaction reference, raw SMS content, and amount+type+date to prevent double-counting
- **Partial fallback** — if a message can't be fully parsed, Duka pre-fills what it could detect and opens the manual form

### ✏️ Manual Entry
- Add any transaction by hand: amount, type (sent / received), provider, category, and an optional note
- Transport category expands to sub-types: **Bus**, **Taxi**, **Ride**
- **Recurring quick-add**: tap a saved recurring template to pre-fill the form instantly

### 📊 Dashboard
- Monthly income, expense, and net balance at a glance
- **Provider filter** — view stats for Telebirr, CBE Birr, M-PESA, Cash, or all accounts combined
- Expandable account balance card showing the latest known balance per provider
- **Spending breakdown doughnut chart** by category
- **Budget progress alerts** — see how close you are to your monthly limits
- Recent transactions list (5 most recent, with edit and delete)
- **Privacy Mode toggle** — one tap blurs every amount on screen

### 📅 Transaction History
- Navigate month-by-month through your full transaction history
- Filter by payment provider
- **Full-text search** — by note, category, provider, amount, sender/recipient name, or transaction reference number
- Service fee and VAT summary (automatically extracted from Telebirr receipts)
- Expandable per-provider balance card

### 💰 Budget Manager
- Set a monthly spending limit for any category
- Visual progress bars with real-time status: **On track** → **Approaching limit** → **Almost there** → **Over budget**

### 🔁 Recurring Transactions
- Save templates for fixed monthly expenses (rent, subscriptions, transport, etc.)
- **Auto-detect patterns** — Duka scans your history and surfaces transactions that repeat
- Quick-add suggestions appear on the Add Transaction screen
- Manually add or remove recurring items from Settings

### 📄 Financial Reports & Export
- **PDF Statement** — a branded, multi-section document with: logo header, summary boxes (income/expense/net), by-provider breakdown, spending-by-category bar chart, and a full transaction table with color-coded amounts
- **CSV Export** — raw spreadsheet format for analysis in Excel or Google Sheets
- **Text Receipt** — a formatted plain-text summary; copy to clipboard or share as a file
- **Send directly to Telegram** — PDF and CSV are delivered to your Telegram chat via a secure serverless relay; no file manager needed
- **Monthly or All-Time** export mode
- **Preview** before exporting

### 🌐 Bilingual Interface — English / አማርኛ
- Every UI label, button, error message, and category is available in **English** and **Amharic**
- Language is **auto-detected** from your Telegram account's `language_code` on first launch
- Switchable at any time in Settings — preference saved to `localStorage`
- Amharic typography font is applied automatically when Amharic is active
- The SMS parser also handles **Amharic-language bank receipts**

### 🔒 Privacy & Offline
- **Privacy Mode** — blur all financial figures with one tap; toggleable from the Dashboard header or Settings
- **Local-first** — all transactions are saved to IndexedDB on your device immediately, before any network call
- **Offline capable** — the app works fully without internet; transactions are queued and synced automatically when reconnected
- **Dark / Light theme** toggle, persisted across sessions

### 🐛 In-App Bug Reporting
- Report issues directly from Settings
- Report is pre-filled with non-sensitive context (language, theme, connection status, transaction count, failed SMS draft)
- Sent to **@DukaSupport\_bot** via Telegram — you control what gets included

---

## Authentication

| Method | Environment |
|--------|-------------|
| Email / Password | Browser or Telegram Mini App |
| Google OAuth | Browser (redirect flow) |
| Telegram auto-login | Inside Telegram Mini App (HMAC-verified server-side) |
| Guest / Skip | No account required — data stored locally only |

---

## Supported Payment Providers

| Provider | SMS Parsing | English SMS | Amharic SMS |
|----------|:-----------:|:-----------:|:-----------:|
| Telebirr (Ethiotelecom) | ✅ | ✅ | ✅ |
| CBE Birr (Commercial Bank of Ethiopia) | ✅ | ✅ | — |
| M-PESA (Safaricom Ethiopia) | ✅ | ✅ | — |
| Cash (manual entry only) | Manual | ✅ | ✅ |

---

## Transaction Categories

Food & Drink · Transport (Bus / Taxi / Ride) · Shopping · Bills & Utilities · Health · Education · Entertainment · Transfer · Salary · Gift · Other

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | React 19 + Vite |
| State Management | Zustand |
| Animations | Framer Motion |
| Cloud Database & Auth | Supabase (PostgreSQL + Auth + Edge Functions) |
| Local Storage | IndexedDB via `idb` |
| PDF Generation | jsPDF + jspdf-autotable |
| Charts | Chart.js + react-chartjs-2 |
| Icons | Lucide React |
| Internationalization | i18next + react-i18next |
| Platform SDK | Telegram Web App SDK (`@twa-dev/sdk`) |
| Deployment | Vercel (serverless + static) |

---

## Running Locally

```bash
npm install
npm run dev
```

Create a `.env` file in the project root:

```env
VITE_SUPABASE_URL=your_supabase_project_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
TELEGRAM_BOT_TOKEN=your_bot_token   # required for /api/send-file
```

---

## Project Structure

```
src/
├── pages/          # Dashboard, AddTransaction, History, Settings, Login
├── components/     # BottomNav, TransactionItem, BudgetProgress, BudgetSetupModal,
│                   # RecurringSuggestions, OnboardingGuide, CategoryIcon, ProviderIcons
├── parsers/        # telebirr.js, cbe.js, mpesa.js, amharic.js, fallback.js, index.js
├── services/       # auth.js, sync.js, supabase.js
├── utils/          # exportPDF.js, exportCSV.js, fileDownload.js, sendToTelegram.js,
│                   # recurringDetector.js, index.js
├── i18n/           # en.json, am.json (Amharic), index.js
└── store/          # Zustand global state
api/
├── send-file.js    # Vercel serverless: delivers PDF/CSV to Telegram chat
└── warmup.js       # Keeps the serverless function warm
```

---

## Privacy

Duka is designed with privacy in mind:
- Your financial data is **yours** — never sold, never shared with advertisers
- Telegram authentication is validated **server-side only** — the bot token never touches the frontend
- File delivery to Telegram goes through a serverless relay — your chat ID is sent as a query parameter and used only for that single delivery
- See [PRIVACY_POLICY.md](./PRIVACY_POLICY.md) for the full policy

---

*Duka (ዱካ) — Your financial footprint, simplified.*
