# Proton Calendar

Sync your Proton Calendar with Cal.com using a secure ICS feed.

## Why a dedicated app?

While the generic ICS Feed app works for most providers, Proton Calendar has
specific quirks that cause issues:

1. **Ghost events** — Proton includes `STATUS:CANCELLED` events in its ICS
   feeds, which the generic app treats as busy slots. This blocks availability
   for meetings that were already cancelled.
2. **Cancelled recurring occurrences** — When a single occurrence of a weekly
   meeting is cancelled, Proton creates a separate `VEVENT` with both
   `STATUS:CANCELLED` and a `RECURRENCE-ID`. Without special handling, these
   phantom slots block the entire time window.
3. **Domain validation** — This app only accepts `proton.me` and
   `protonmail.com` URLs, preventing misuse and SSRF attacks.

## Features

- **Read-Only Sync**: Cal.com reads your Proton Calendar for busy slots
- **Privacy First**: Uses Proton's secure ICS feed — no password or API key
- **Encrypted Storage**: ICS URL is encrypted at rest with `CALENDSO_ENCRYPTION_KEY`
- **SSRF Protected**: Strict hostname validation, HTTPS-only
- **Recurring Events**: Properly expands RRULEs within the query window
- **Ghost Event Fix**: Filters `STATUS:CANCELLED` events that Proton includes
- **Cancelled Occurrence Fix**: Tracks `RECURRENCE-ID` to skip phantom slots

## Setup

1. Open [Proton Calendar](https://calendar.proton.me) → **Settings** → **Calendars**
2. Select your calendar → **Share** → **Create link**
3. Copy the ICS feed URL
4. In Cal.com → **Apps** → **Proton Calendar** → **Install**
5. Paste the ICS URL and save

## How it works

Cal.com fetches the ICS feed on each availability check. Events (including
recurring) are parsed and returned as busy times. Since Proton uses a
zero-knowledge architecture, the ICS feed is the only way to integrate
without compromising Proton's security model.

This follows the approach suggested by the Cal.com team — a Proton-specific
app based on the ICS feed pattern where Proton quirks can be handled without
breaking the generic ICS app for other providers.
