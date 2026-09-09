# Tracking & Monitoring — Free EDM & SMS Reporting Dashboard for SFMC

A free, native-SFMC reporting dashboard for email and SMS performance —
built entirely on Salesforce Marketing Cloud's own tooling. No external
hosting, no third-party service, no license.

Marketing Cloud's built-in reporting splits Triggered Sends,
User-Initiated Sends, and Journey Builder Sends into separate contexts,
so comparing them side by side over time usually means stitching
together multiple reports by hand. This package gives you one dashboard
that shows all three — plus SMS — on the same page, with consistent
30/60/90-day and all-time trend views.

## What's included

`Tracking_and_Monitoring.json` — a Package Manager export containing:

- **Data Extensions** for pre-aggregated daily email and SMS reporting
- **SQL Query Activities** that populate those Data Extensions from
  standard SFMC tracking/sendlog data
- **An Automation** that runs the queries on a daily schedule, so the
  dashboard is always showing current data
- **CloudPages** that render the dashboard itself

## Features

**Email Reports**
- Summary cards — Total Sends, Total CTR, Unsub Rate, Bounce Rate — each
  with a trend sparkline and a 30D / 60D / 90D / All toggle
- All-send-types overview (Total Sent, Opens, Clicks, Bounces,
  Unsubscribes, CTR)
- Matched send-volume and CTR charts for Triggered, User-Initiated, and
  Journey Builder sends, side by side on the same time axis
- Full daily breakdown table with search and CSV export

**SMS Reports**
- Tracks both **contact-level** sends (unique subscribers reached) and
  **message-level** sends (total SMS dispatched) — reported
  independently, since collapsing the two into one number gives
  misleading delivery/undelivered rates
- Same summary-card, trend-toggle, and overview pattern as the email tab
- Contact Sends, SMS Sends, and Avg Messages per Contact charts

## Prerequisites

- Marketing Cloud user access to the target Business Unit, with
  permissions to create/edit: Data Extensions, Automations, Query
  Activities, and CloudPages.
- A Package Manager tool capable of importing this export into your
  Business Unit. (Consult that tool's own documentation for its specific
  import flow — the steps below cover what to do before and after the
  import, which is consistent regardless of which tool you use.)

## Installation

### 1. Import the package

1. Open your Package Manager tool in the target Business Unit.
2. Import `Tracking_and_Monitoring.json`.
3. When prompted to map source folders/categories to destination
   folders, create or select matching Content Builder / Data Extension
   folders — don't let the tool default to root unless that's where you
   actually want these assets.
4. Confirm the tool reports success for every asset type in the package
   (Data Extensions, CloudPages, Automation, Query Activities).

### 2. Verify the Data Extensions

Package Manager typically recreates DE **structure**, not the data rows
in it.

1. Open each imported reporting Data Extension and confirm the field
   list, types, lengths, and primary keys match what's expected.
2. Confirm the DEs are empty as expected — a fresh environment shouldn't
   carry over any rows from the source BU.

### 3. Re-point environment-specific values

Everything referencing a domain or CloudPage ID was written against the
*source* Business Unit and won't be correct after import.

1. Open each imported CloudPage's SSJS.
2. Update any hardcoded domain references to this Business Unit's actual
   `pub.sfmc-content.com` (or custom vanity) domain.
3. Update any hardcoded `CloudPagesUrl(...)` IDs or slugs — CloudPage IDs
   are BU-specific and will not carry over from the source environment.

### 4. Publish the CloudPages

1. Publish each imported CloudPage in the target BU.
2. Record each page's actual CloudPage ID and published URL for use in
   step 3 above, if not already done.

### 5. Activate the Automation

1. Open the imported Automation and confirm each Query Activity points
   at Data Extensions that exist in this BU.
2. Re-set the schedule — scheduled Automations are commonly deactivated
   on import as a safety default.
3. Run the Automation manually once and confirm each Query Activity
   completes and the target DEs populate correctly, before relying on
   the schedule.

### 6. Test

1. Open the published dashboard CloudPage and confirm it's reading data
   from the newly populated reporting DEs.
2. Let the Automation run on its schedule for a day and confirm the
   dashboard reflects fresh data the next time you check it.

## Known gaps to double-check

- Any hardcoded domain or page ID left unupdated will fail silently
  rather than throwing a clear error — this is the most common thing to
  miss after import.
- If your org enforces IP allowlisting or SSO on CloudPages, confirm the
  dashboard's access model still matches your requirements post-import.

## Roadmap

- Per-send drill-down from the daily table
- Threshold-based alerting on bounce/unsub rate, off the same
  Automation
- Combined email + SMS view for cross-channel campaigns
- Role-based dashboard access
- Rolling-average benchmarking to flag unusual days automatically
- Scheduled digest (email/CSV summary) for stakeholders who won't check
  the dashboard directly

## Contributing / issues

This is shared as-is, free to use and adapt. If you run into an issue
importing or running it, feel free to open a GitHub Issue on this repo.

## License

MIT License

Copyright (c) 2026 Max Chu

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
