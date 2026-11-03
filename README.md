# Nexachain AI — Task 1: Database Schema Design

This folder implements the four Mongoose models requested in the technical assessment:

- `User.js` — account details, referral relationship, wallet totals, and password hashing.
- `Investment.js` — investment plan, amount, dates, daily ROI percentage, and status.
- `ReferralIncome.js` — level-income records and optional idempotency fields for source investment/date/level.
- `ROIHistory.js` — daily ROI history with a unique investment/date key to prevent duplicate daily records.

## Setup

```bash
npm install
```

In your app, connect Mongoose to MongoDB before using these models:

```js
const mongoose = require('mongoose');
await mongoose.connect(process.env.MONGODB_URI);
```

Set `MONGODB_URI` in your environment; do not commit credentials.

## Notes / assumptions

- Passwords are hashed with `bcryptjs` before saving; they are not stored as plaintext. Query the password with `.select('+password')` when checking login credentials.
- `dailyRoiPercentage` is a percentage number (for example, `1` means 1% per day).
- `ROIHistory.dateKey` should be a UTC date string in `YYYY-MM-DD` format. Its unique compound index allows only one ROI history record per investment per day.
- `ReferralIncome` has optional `sourceInvestment` and `sourceDateKey` fields so the application can enforce duplicate prevention for recurring level-income calculations. Populate these consistently when creating records.
- Mongoose's `unique` options create database indexes; they are not ordinary validation. Ensure indexes are built in the database and handle duplicate-key errors in API code.
- This deliverable covers Task 1 only. It does not implement the assessment's APIs, ROI calculation engine, React dashboard, or cron scheduler.
