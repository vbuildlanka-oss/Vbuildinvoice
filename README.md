# VBUILD Invoices

A small invoice tool for VBUILD. Two things it does:

- **Proforma invoice** — quotes the full project value and asks the client for the advance. The document shows the total, the advance payable now, and the balance left for later.
- **Final invoice** — asks whether you already took an advance. Say yes and it deducts it and bills the balance; say no and it bills the whole project value.

Everything lives in `index.html`. One file, no build step, no dependencies, no network calls.

## Deploying to Vercel

There's nothing to build, so a deploy costs no build minutes.

```bash
npx vercel        # preview
npx vercel --prod # production
```

Or point Vercel at the GitHub repo and accept the defaults — framework preset **Other**, no build command, output directory the repo root. `vercel.json` already says as much.

To run it locally, open `index.html` in a browser, or:

```bash
python3 -m http.server 8000
```

## Before your first invoice

Open **Company details** in the top right and fill in your address, bank details, logo and defaults (currency, tax rate, usual advance %, invoice number prefixes). You only do this once — every invoice after that inherits it.

## Printing a clean PDF

Hit **Download PDF**, which opens your browser's print dialog. Two settings matter:

| Setting | Set it to | Why |
| --- | --- | --- |
| **Headers and footers** | off | Otherwise the browser stamps the page URL and date across your client's invoice |
| **Background graphics** | on | Keeps the accent colour and the highlighted amount-due panel |

Leave margins at **Default** — the invoice sets its own A4 margins. The saved file is named after the invoice number.

Up to eight line items fit on a single A4 page. Beyond that it flows onto a second page and repeats the column headers.

## Where your data lives

In your browser's `localStorage`, on the machine you typed it on. Nothing is uploaded and there are no third-party requests, which also means:

- Invoices saved on your laptop won't appear on your phone.
- Clearing site data clears your saved invoices and company details.

If you need invoices on more than one device, deploy it once and use the same browser profile, or save the PDFs somewhere shared.

## Invoice numbering

Numbers are issued per type, per year, and increment when you save: `VB-PF-2026-001`, `VB-INV-2026-001`. You can always type over a number by hand. Change the prefixes in Company details.

## Handy bits

- **Copy** on a recent invoice duplicates it with a fresh number and today's date.
- On a final invoice, once you answer "yes" to the advance you can **pull straight from a saved proforma** — it copies the client, the line items and the advance amount across, and references the proforma number on the invoice.
- `Ctrl`/`Cmd` + `S` saves, `Ctrl`/`Cmd` + `P` prints.
- Amounts are written out in words under the amount due. Turn that off in Company details if you'd rather not.
