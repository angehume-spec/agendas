# Agenda Reader

A single-page web tool that turns NSW council **business papers** (agenda PDFs) into a readable on-screen summary and a structured spreadsheet. Drop in a PDF, get back the meeting details and a list of every report — with reference numbers, titles, and summaries — plus a one-click Excel download.

Part of the [councilpapers.app](https://councilpapers.app) civic-transparency project.

**Live:** https://angehume-spec.github.io/agenda-reader/

---

## What it does

Council agendas are published as long InfoCouncil PDFs that are hard to skim. This tool parses one and gives you:

- the **meeting header** — committee/meeting name, date, time, location, and submission deadlines
- the **agenda detail** — one row per report, with its content type, reference number (e.g. `CCL028-25`), title, and the officer summary lifted from the paper
- a **downloadable `.xlsx`** matching the councilpapers pipeline schema, so it slots straight into the rest of the workflow

It reads agendas only — not minutes.

## How to use it

1. Open the [live page](https://angehume-spec.github.io/agenda-reader/).
2. Drop or select an InfoCouncil agenda PDF.
3. Read the parsed summary on screen.
4. Click **download** for the Excel workbook.

That's the whole thing — no sign-up, no account.

## Privacy

Everything runs **in your browser**. The PDF is parsed client-side using [pdf.js](https://mozilla.github.io/pdf.js/), and the spreadsheet is built client-side with [SheetJS](https://sheetjs.com/). **No file is ever uploaded to a server** — there's no server to upload to. The page is static.

## Supported councils

Built and tested against **Bayside Council (NSW)** business papers. Because Bayside and several other NSW councils (including Georges River) run on the same **InfoCouncil** (Civica) platform, the same document structure applies, so other InfoCouncil councils should parse with little or no change.

## Output format

The download is a two-sheet `.xlsx` workbook joined on `meeting_uid`:

- **`meeting_header`** — one row: `meeting_uid`, meeting name, date, time, location, submission deadlines, and the source link.
- **`agenda_detail`** — one row per report: `meeting_uid`, `content_type`, `ref_number`, `title`, `summary`.

## Running it locally

It's a single static file, but it **must be served over http(s)** — opening `index.html` directly off the disk (`file://`) stops pdf.js from loading its worker, and parsing silently fails. The simplest local server:

```bash
# from the folder containing index.html
python -m http.server 8000
# then open http://localhost:8000
```

## Deploying

Hosted on **GitHub Pages** from this repo (root of the `main` branch). Any static host works — Cloudflare Pages, Netlify, etc. To point a custom domain (e.g. `reader.councilpapers.app`), set it under **Settings → Pages → Custom domain** and add a matching `CNAME` record at your DNS provider pointing to `angehume-spec.github.io`.

## Tech

- [pdf.js](https://mozilla.github.io/pdf.js/) — client-side PDF text extraction
- [SheetJS](https://sheetjs.com/) — client-side `.xlsx` generation
- No build step, no framework, no dependencies to install — a single `index.html`.

## Licence

Add your preference — [MIT](https://choosealicense.com/licenses/mit/) is a sensible default for open civic tooling. Drop a `LICENSE` file in the repo root to make it official.
