# Supply CRM

Driver pipeline and retention console for the Zeds Hub supply team.

Built on Google Apps Script with a Google Sheet as the database. This repository
holds the branded wrapper page; the application itself runs as an Apps Script
web app.

## What it does

Tracks every driver through their life on the platform:

```
New Lead -> Onboarding -> New Active -> Active -> Churned
                                                   Rejected
```

The first and last moves are made by an agent. The middle three are calculated
every morning from ride data, so a driver's stage always reflects what they are
actually doing rather than what someone last typed.

Each agent sees only the drivers assigned to them. That filtering happens on the
server, so another agent's records never reach the browser.

## Repository layout

```
index.html    branded wrapper, embeds the web app
logo.png      full Zeds mark, shown in the header and used as the app icon
favicon.png   the cyan d from the wordmark, transparent, used as the favicon
```

`logo.png` is also what the CRM itself loads for its sidebar. Once Pages is
live, put its URL into `LOGO_URL` at the top of the script in the Apps Script
`Index.html`:

```
https://<org>.github.io/<repo>/logo.png
```

The Apps Script source (`Code.gs` and `Index.html`) lives in the Apps Script
project attached to the spreadsheet.

## Setup

1. Deploy the Apps Script project as a web app
   (Execute as: **Me**, Access: **Anyone with a Google account**).
2. Copy the deployment URL.
3. Open `index.html` here and paste it into `APP_URL`.
4. In this repository: Settings -> Pages -> deploy from the `main` branch.

The site is then served at `https://<org>.github.io/<repo>/`.

## Signing in

Agents on a company email address are recognised automatically.
Agents on a personal Gmail address open their own link instead:

```
https://<org>.github.io/<repo>/?t=THEIR_TOKEN
```

Tokens live in the `users` tab of the spreadsheet. Treat them as passwords.

## Daily routine

Paste the platform ride export into the `import_rides` tab and run `syncRides()`.
That refreshes ride counts, recalculates every stage, and records each move in
`stage_history`.
