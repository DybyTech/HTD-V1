# Host TalkDrove (HTD-V1)

Host TalkDrove is a Node.js application that provides a complete web interface for hosting and managing WhatsApp bots.
It offers authentication, dashboard views, admin and moderator tools, deployment helpers and wallet
features. The project uses **Express** for the HTTP server, **EJS** for templating and **MySQL** for persistence.

## About

The system is designed around a coin based economy. Users can deploy premium bot templates to Heroku
with a few clicks, claim free coins daily and purchase additional coins to unlock more deployments or
buy full Heroku accounts.  Administrators can manage the available bots, Heroku API keys and user
accounts while moderators review bot reports or deposit requests.  A built in support ticket system
provides customer help straight from the dashboard.

## Features

- User authentication with signup, login and password reset routes
- Admin interface for managing bots, users, API keys and support tickets
- Moderator routes for handling bot reports and deposit requests
- Bot deployment workflows with Heroku integration
- Purchase and manage full Heroku accounts from the dashboard
- Daily coin claims, wallet management and deposit requests
- In‑browser bot logs and terminal access for deployed apps
- Built in support ticket system with admin chat and activity log
- Cron jobs for email sender rotation and statistics tracking
- REST style APIs under `api/routes/apis`

## Project Structure

```
Hamza.js               # Application entry point
api/                   # API routes, middlewares and database helpers
public/                # Static files served by Express
views/                 # EJS views used for server-rendered pages
```

## Scripts

- `npm start` – start the application
- `npm run dev` – start with nodemon for development

## Environment Variables

Create a `.env` file in the project root and provide the following variables.
The included `.env` example lists all available keys:

```
DB_HOST=
DB_USER=
DB_PASSWORD=
DB_NAME=
SESSION_SECRET=
NODE_ENV=production
PORT=3000
SITE_URL=
MAINTENANCE_MODE=false
HTD_API_KEY=
CREEM_API_URL=
CREEM_API_KEY=
```

`SESSION_SECRET` should be a random string used to sign cookies. The database settings should point to a
MySQL server. Update `PORT` if you want the server to listen on a different port.

## Running the Application

Install dependencies and run the server:

```bash
npm install
npm start
```

The service will listen on the port configured in the `.env` file (default `3000`).

For additional details about preparing the environment and database, see
[`docs/setup.md`](docs/setup.md).

### Web Based Installer

On a fresh clone with no database configured you can visit `/install` after
starting the server. This page lets you provide MySQL credentials and create the
first admin account. The installer imports `api/talkdrove.sql`, saves the
database details into `.env` and then removes itself for security.

## Deploying Bots to Heroku

TalkDrove integrates with the Heroku platform for bot hosting. When a user starts
a deployment the server picks a valid Heroku API key with available app slots,
creates an app, sets environment variables and triggers a build from the bot's
GitHub repository. Deployment cost is deducted from the user's coin balance and a
portion is sent to the bot developer. Deployment progress and errors are stored
in the `deployed_apps` table and can be viewed from the dashboard. Logs and a
web terminal are also available directly in the browser.

Administrators can add multiple Heroku API keys under `/api/admin/heroku-accounts`
and the system automatically tracks usage limits and invalid keys.

## Purchasing Heroku Accounts

Users may exchange coins for a full Heroku account through the `/buy-heroku-account`
endpoint. Purchased credentials are stored securely and can be reviewed from the
`/my-heroku` page of the dashboard.

## Support System

Every user can open support tickets directly from the site. Tickets can include
chat style discussions and attachments. Admins have a dedicated interface to
manage incoming tickets, reply to users and view activity logs. Daily cron jobs
reset email sender quotas and ensure notification emails are delivered
reliably.
