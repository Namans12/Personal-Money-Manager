# Personal Money Manager

Web app for tracking income, expenses, payments, and EMIs, with user accounts and email notifications. Node.js, Express, EJS, and MongoDB.

> ### 🔴 Committed database credentials — rotate these
>
> [`server.js`](server.js) contains a **live MongoDB Atlas connection string with username and password in plain text**, and this repository is public:
>
> ```js
> const url = 'mongodb+srv://<user>:<password>@cluster0.<id>.mongodb.net/MoneyManager'
> ```
>
> Anyone who has seen this repository can read and write that database. Rotating the password is the urgent part — removing the line from the file does not help while the old credential still works. See [Configuration](#configuration).

## Overview

A personal finance tracker: record what comes in, what goes out, what is scheduled, and what you owe on instalments. Users sign up with an email and password, and the app can send notifications by email.

Server-rendered with EJS — no client-side framework, no build step.

## Features

- **User accounts** — sign-up and login with bcrypt-hashed passwords
- **Session management** via `express-session` and cookies
- **Balance tracking** — running position across income and expenses
- **Add money** — record income
- **Payments** — record and review outgoings
- **EMI tracking** — instalment schedules
- **Profile** management
- **Email notifications** via Nodemailer

## Tech Stack

Node.js · Express · EJS · MongoDB with Mongoose · `bcrypt` · `express-session` · `cookie-parser` · `body-parser` · Nodemailer

## Prerequisites

- Node.js
- A MongoDB database — Atlas or local
- SMTP credentials for email notifications

## Installation

```bash
git clone https://github.com/Namans12/Personal-Money-Manager.git
cd Personal-Money-Manager
npm install
```

> `node_modules/` is committed to this repository (~5,000 files). It should be removed and ignored:
>
> ```bash
> git rm -r --cached node_modules
> echo "node_modules/" >> .gitignore
> ```

## Configuration

The database URL is currently hardcoded near the top of `server.js`. Move it to an environment variable:

```js
require('dotenv').config()
mongoose.connect(process.env.MONGODB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
```

Then create `.env` (and add it to `.gitignore`):

```
MONGODB_URI=mongodb+srv://user:password@cluster.mongodb.net/MoneyManager
SESSION_SECRET=some_long_random_string
SMTP_HOST=
SMTP_USER=
SMTP_PASS=
```

**Rotate the existing database password first.** Editing `server.js` leaves the old credential in git history and still valid on the cluster.

## Usage

```bash
node server.js
```

The app serves on Express's configured port. Sign up, then use the dashboard to record income and expenses.

## Pages

| View | Purpose |
|---|---|
| `home.ejs` | Dashboard and balance overview |
| `payments.ejs` | Payments list and entry |
| `emi.ejs` | EMI / instalment tracking |
| `profile.ejs` | User profile |
| `auth/` | Sign-up and login |
| `components/` | Shared partials |

## Project Structure

```
server.js            Express app, routes, Mongoose connection
bcrypt.js            password hashing helper
views/
  home.ejs  payments.ejs  emi.ejs  profile.ejs
  auth/              sign-up and login
  components/        shared partials
public/              static assets
Node/                supporting server code
plan.txt             original planning notes
*.png                UI screenshots
```

## Screenshots

Committed at the repository root: `login.png`, `SignUp.png`, `Balance.png`, `Add money.png`, and `11.png` / `22.png` / `33.png`.

## Limitations

- No automated tests
- Credentials and secrets are in source rather than configuration
- `node_modules/` is version-controlled
- No input validation layer beyond what Mongoose schemas enforce
- Single-currency; no multi-user sharing or export

## Related Repositories

| Repo | Relationship |
|---|---|
| [`stocks`](https://github.com/Namans12/stocks) | Personal equity portfolio analysis — adjacent domain, private |
