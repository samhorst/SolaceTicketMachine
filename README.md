# CX Ticket Decision Tree

Internal tool for Solace CX agents to triage communications issues (phone, video, fax, messaging, transcripts) and produce well-formatted Linear tickets.

## What it does

- Walks the agent through a category-specific decision tree
- Surfaces troubleshooting steps tied to the [CX Troubleshooting Guide](https://www.notion.so/findsolace/CX-Troubleshooting-Guide-Communications-Issues-User-Facing-3529601d59a98193940ad1b67d1bfb21)
- Warns when an answer suggests the issue is likely user-side, configuration, or device-related
- Collects only the fields engineering actually needs for each specific endpoint
- Generates a structured ticket body with Summary, Decision Path, Troubleshooting Info, and Ticket Details sections
- Supports screenshot attachments (rendered inline on the final screen, ready to drag into Linear)

## Running locally

This is a single HTML file with no build step required.

```bash
# Just open index.html in a browser
open index.html
```

Or serve it with any static server, e.g.:

```bash
npx serve .
```

## Tech notes

- React 18 loaded via CDN
- Babel Standalone for in-browser JSX compilation (fine for an internal tool; can be precompiled later if needed)
- Pure client-side — no backend, no data persistence, nothing leaves the browser
- All state is in-memory; refreshing the page resets everything

## Privacy & compliance

- The app never transmits or stores any data. Everything stays in the user's browser.
- Form fields explicitly warn against entering PHI. Patient ID only.
- A persistent banner at the bottom of every screen reinforces this.

## Adding a new endpoint

1. Add a new entry to the `ENDPOINTS` object with `troubleshoot`, `escalate`, `title`, and `summary` properties
2. Add the routing logic to `resolveEndpoint()`
3. If it's reached via a new branch question, add the question to the appropriate category's `TREES` entry

## Adding a new Twilio failure guidance type

1. Add a new entry to the `GUIDANCE` object
2. Add the option label → guidance key mapping to `TWILIO_GUIDANCE_MAP`
3. Add the option to the `twilioFailed` branch question in `TREES["Phone Call"]`

## Adding a new fax error code

Add an entry to `FAX_CODE_INFO` with `label` and `action`. The live advisory under the error code field updates automatically.
