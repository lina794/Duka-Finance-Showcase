# Duka (ዱካ) — Privacy Policy

**Last Updated:** April 26, 2026
**Effective Date:** April 26, 2026

---

## 1. Introduction

Welcome to **Duka** (ዱካ), a bilingual personal finance tracking application designed for Ethiopian university students. Duka helps you log, categorize, and understand your spending across Telebirr, CBE Birr, M-PESA, and cash — in English or አማርኛ.

This Privacy Policy explains exactly what information Duka collects, why it is collected, how it is stored, and what rights you have over your data.

Duka is available on the following platforms:

- **Duka Telegram Mini App** — accessed inside the Telegram messaging app
- **Duka Web App** — accessed directly in a browser (supports email/password and Google sign-in)

By using any Duka platform, you agree to the practices described in this Privacy Policy. If you do not agree, please do not use our services.

---

## 2. Who This App Is For

Duka is intended for users who are **18 years of age or older**. We do not knowingly collect personal information from anyone under the age of 13. If we become aware that a user under 13 has provided us with personal data, we will take steps to delete it promptly.

---

## 3. Information We Collect

### 3.1 Account Information

| Data | Purpose | Platform |
|------|---------|----------|
| Email address | Account creation and authentication | Web App |
| Password (hashed by Supabase) | Authentication | Web App |
| Telegram user ID & display name | Auto-login via Telegram initData | Telegram Mini App |
| Google user ID & email | Secure authentication via Google OAuth | Web App (browser) |

**Guest Mode:** If you choose "Skip for now" on the login screen, no account is created and no data is synced to the cloud. Your transactions are stored locally on your device only.

### 3.2 Financial Transaction Data

When you **manually paste** an SMS receipt or enter a transaction by hand, we extract and store only the fields you provide:

| Field | Source |
|-------|--------|
| Transaction amount (ETB) | Parsed from SMS or manually entered |
| Transaction type (sent / received) | Parsed or selected |
| Payment provider (Telebirr, CBE Birr, M-PESA, Cash) | Parsed or selected |
| Spending category | Parsed (estimated) or selected by you |
| Counterparty name (recipient or sender) | Parsed from SMS |
| Transaction date and time | Parsed from SMS or defaulted to current time |
| Transaction reference number | Parsed from SMS |
| Service fee and VAT | Parsed from SMS (when present) |
| Account balance after transaction | Parsed from SMS (when present) |
| Optional note | Entered by you |

> **Important:** Duka does **not** automatically read, intercept, or monitor your SMS messages, notifications, or any other app. Every transaction is logged only because **you chose to paste** the SMS text into the app or enter it manually.

### 3.3 Preference Data (Stored Locally)

The following preferences are stored in your **browser's localStorage** only — they are never sent to any server:

| Key | What it stores |
|-----|----------------|
| `duka-lang` | Your chosen language: `en` (English) or `am` (Amharic) |
| `duka-theme` | Your chosen theme: `dark` or `light` |
| `duka_draft_sms` | A temporary draft of an SMS you are currently working with — cleared after saving or cancelling |
| `duka_bug_draft` | A temporary draft of a bug report you are writing — cleared after submission |

### 3.4 Budget & Recurring Transaction Data

- **Budget limits** you set per category are stored locally in **IndexedDB** on your device.
- **Recurring transaction templates** you save are stored locally in IndexedDB.
- Neither budget data nor recurring templates are currently synced to the cloud server.

### 3.5 Bug Report Data

When you submit a bug report from Settings, a pre-formatted report is opened in Telegram (directed to **@DukaSupport\_bot**). The report includes:

- Your written description (which you type)
- Non-sensitive context automatically appended: current language, theme, online/offline status, and total number of transactions stored on your device
- The failed SMS draft (if any is saved in `duka_draft_sms`) — limited to the first 500 characters

**You review this report before sending.** Nothing is sent automatically or without your explicit action.

### 3.6 Technical Data

- Online/offline connection status (used to determine when to sync)
- Sync timestamps (used to reconcile local and cloud data)

We do **not** collect:

- GPS or location data
- Contact lists or phone numbers
- Call logs
- Browsing history
- Notifications from any application
- Data from any app other than what you explicitly paste into Duka

---

## 4. How We Use Your Information

| Purpose | Description |
|---------|-------------|
| **Core service** | Display your financial summary, transaction history, and spending insights |
| **Cloud sync** | Securely replicate your transactions between your devices via Supabase |
| **Offline functionality** | Store data locally so the app works without internet access |
| **Language detection** | Detect your Telegram `language_code` on first launch to set the default language (Amharic or English); saved to localStorage and not synced |
| **File delivery** | Route generated PDF or CSV reports to your Telegram chat via a secure serverless function |

We do **not**:

- Sell your personal or financial data to any third party
- Use your data for advertising, profiling, or marketing
- Share your data with financial institutions, credit bureaus, or government agencies
- Make automated financial decisions based on your data (e.g., credit scoring)

---

## 5. Data Storage & Security

### 5.1 Local Storage (IndexedDB)

- All transaction data is first saved to **IndexedDB** on your device immediately — before any network call
- The app works fully offline; transactions made offline are queued and synced automatically when your connection returns
- Local data persists even if you lose internet access
- You can clear IndexedDB at any time through your browser settings

### 5.2 Cloud Storage (Supabase)

- When online and signed in, your transactions are synced to **Supabase** — a secure, encrypted cloud database
- Data is transmitted over **HTTPS / TLS**
- Authentication tokens are stored securely by the Supabase client library and refreshed automatically
- Each user's data is strictly isolated — you can only access records tied to your own account

### 5.3 Authentication

**Telegram Mini App:**
Telegram authentication uses Telegram's `initData` mechanism. The raw `initData` is sent to a **Supabase Edge Function** running on the server. The Edge Function validates the HMAC signature using the bot token. The bot token is stored **server-side only** and is never included in the app's frontend code or bundle.

**Google OAuth:**
Sign-in is handled through Supabase's Google OAuth integration. We receive only your primary Google email address and a Google user ID. We do not access your Google Drive, Contacts, Calendar, or any other Google service.

**Email / Password:**
Passwords are never stored in plain text. Hashing is handled entirely by Supabase Auth.

### 5.4 Telegram File Delivery

When you choose to send a PDF or CSV report to your Telegram chat:

1. The file is generated entirely on your device (in the browser)
2. The file is POSTed as raw binary to a **Vercel serverless function** (`/api/send-file`) running on Duka's backend
3. The serverless function delivers the file to your Telegram chat using the Telegram Bot API (`sendDocument`)
4. Your Telegram `chat_id` is used only for that single delivery and is not stored by the serverless function

The bot token used for delivery is stored as a **server-side environment variable only** — it is never exposed in the app.

---

## 6. Data Retention

- Your transaction data is retained as long as your account is active
- You may delete individual transactions at any time from within the app (Dashboard or History)
- You may wipe **all** transactions (local and cloud) from **Settings → Wipe All Data** — this action is permanent and cannot be undone
- To request full account deletion, contact us (see Section 10) and we will permanently remove your account and all associated data within **30 days**
- Local data (IndexedDB, localStorage) can be cleared at any time through your browser or device settings

---

## 7. Data Sharing

We do **not** share your personal or financial data with any third parties, except as described below:

| Recipient | Purpose | Data Shared |
|-----------|---------|-------------|
| **Supabase** (infrastructure provider) | Database hosting, authentication, and sync | Encrypted transaction records, authentication credentials |
| **Google** (authentication provider) | Google OAuth sign-in | Email address and Google user ID only — no financial data |
| **Vercel** (deployment platform) | Hosting and serverless function execution | Raw file bytes during Telegram file delivery (not stored) |
| **Telegram** (messaging platform) | Delivering PDF/CSV reports to your chat | File bytes and your Telegram `chat_id` |

Supabase acts as a **data processor** on our behalf and operates under its own security and privacy policies. In the normal course of operation, no human at Supabase has access to your personal data.

We may disclose data if required by the laws of the Federal Democratic Republic of Ethiopia or a valid court order. We will attempt to notify you of such a requirement unless legally prohibited from doing so.

---

## 8. Your Rights

You have the right to:

| Right | How to Exercise It |
|-------|--------------------|
| **Access** | All your data is visible inside the app at any time |
| **Correction** | Edit any transaction directly from the Dashboard or History screen |
| **Deletion** | Delete individual transactions in-app, or use "Wipe All Data" in Settings |
| **Export** | Generate a PDF or CSV report of all your transactions from Settings |
| **Withdraw consent** | Stop using the app at any time and clear local data via browser settings |
| **Object** | Contact us with concerns about how your data is used (see Section 10) |

---

## 9. Guest Mode

If you use Duka without creating an account ("Skip for now"):

- No personal information is collected or stored on any server
- Your transactions are saved only in your device's IndexedDB
- If you clear your browser storage or switch devices, your data will be lost — it cannot be recovered
- You may create an account at any time; guest data is not automatically migrated to a new account

---

## 10. Contact Us

If you have questions, concerns, or data requests related to this Privacy Policy, please contact us:

- **Telegram:** [@DukaSupport\_bot](https://t.me/DukaSupport_bot)
- **Email:** [swiftdelux100@gmail.com]

---

## 11. Changes to This Policy

We may update this Privacy Policy from time to time. When we do:

- The "Last Updated" date at the top of this document will be revised
- Significant changes will be communicated within the app
- Continued use of Duka after changes are posted constitutes your acceptance of the updated policy

---

## 12. Governing Law

This Privacy Policy is governed by the laws of the **Federal Democratic Republic of Ethiopia**, including applicable provisions on personal data protection.

---

*Duka (ዱካ) — Your financial footprint, simplified.*
