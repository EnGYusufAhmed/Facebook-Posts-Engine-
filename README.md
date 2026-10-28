# Digital Duo — Automated Social Media Content & Visual Generation

A Make.com automation that uses a Google Sheets content plan to generate a Facebook post and a matching visual for Digital Duo, then saves the generated assets to Google Drive and records the outputs back in Google Sheets.

## What it does

1. Reads the content plan from the `Plan` tab in Google Sheets.
2. Checks the current weekday and retrieves the matching row/plan details.
3. Routes the run to a weekday-specific branch (Saturday through Friday).
4. Uses Gemini 2.5 Flash to write a Facebook post in Digital Duo's professional Egyptian Arabic voice, using the planned segment, content history, and the stored tone/brand guidance.
5. Sends the post plus a detailed design prompt to Pollinations' image-generation endpoint to create a matching social visual.
6. Uploads the generated image to Google Drive.
7. Writes the generated post text, image link, and Drive file ID into the `Check` tab in Google Sheets.

## Stack

- **Make.com** — workflow orchestration and weekday routing
- **Google Sheets** — content plan, tone/history input, and output tracking
- **Google Gemini 2.5 Flash** — Facebook post generation
- **Pollinations image API** — social visual generation
- **Google Drive** — generated image storage

## Workflow overview

`Google Sheets (Plan) → Current-day lookup → Weekday Router → Gemini post → Pollinations image generation → Google Drive upload → Google Sheets output update`

The scenario has seven weekday branches. Each branch follows the same broad pattern, with a different weekday filter and corresponding row mappings.

## Required setup

Before importing/running the blueprint, configure your own resources and connections:

1. **Google Sheets**
   - Create a `Plan` tab for the content calendar. The blueprint expects the first columns to include:
     - `Day`
     - `Segment`
     - `History`
   - The original workflow also reads a cell from the sheet for the current brand tone/style guidance.
   - Create a `Check` tab to store generated content and image information. The original mapping writes the post text, image link, and Drive file ID into columns A, B, and F.
2. **Google Drive**
   - Choose a destination folder for generated images.
   - Provide a publicly accessible or otherwise usable logo file if the image endpoint needs to read it.
3. **Make connections**
   - Reconnect Google Sheets and Google Drive modules to your own accounts after importing.
4. **Gemini**
   - Reconnect/configure the Gemini integration in Make.
5. **Pollinations**
   - Add your own API key to the HTTP Authorization header as `Bearer YOUR_POLLINATIONS_API_KEY`.
   - Do not commit real API keys or tokens to GitHub.
6. **Brand design catalog**
   - The embedded design prompt contains a placeholder asking for the complete Digital Duo Design Style Catalog. Insert your approved catalog there if you want the generated images to follow it consistently.
   - Replace the logo file placeholder with your own logo file link/ID and ensure the endpoint can access it.

## Important notes

- The supplied blueprint uses a Google Sheets lookup and fixed row mappings. Review the row/cell references (including the `F20` lookup and the `Check` tab row mapping) against your own spreadsheet before running it.
- All weekday branches are similar but separately configured. If you modify the workflow, keep their weekday filters and output mappings consistent.
- Image generation quality and exact behavior depend on the external image API, its current parameters, and the prompt.
- This repository contains a sanitized blueprint template, not a plug-and-play deployment. Google connections, sheet IDs, Drive folders, logo access, and the API key must be configured by the importer.

## Security

The original uploaded blueprint contained a live-looking Pollinations API credential. It has been replaced in this public-safe copy, but **you should revoke/rotate the original key immediately** if it was active. Removing it from this copy does not invalidate the original credential.

Never publish private spreadsheets, Drive links, access tokens, or personal data in a public repository.

## Repository contents

- `digital-duo-social-content-blueprint-sanitized.json` — sanitized Make.com blueprint
- `README.md` — project overview and setup instructions

## License

For portfolio/demo use. Confirm you have permission to redistribute any third-party assets, brand logos, and prompt materials before publishing the repository.
