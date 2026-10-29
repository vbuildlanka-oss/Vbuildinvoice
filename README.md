# VBUILD Invoices

A small invoice tool for VBUILD. It makes three kinds of invoice:

- **Proforma invoice** — quotes the full project value and asks the client for the advance. The document shows the total, the advance payable now, and the balance left for later.
- **Final invoice** — asks whether you already took an advance. Say yes and it deducts it and bills the balance; say no and it bills the whole project value.
- **Monthly invoice**: for retainers, maintenance and hosting. Save a client once, then each month pick the month, check the amount and generate.

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

## Monthly invoices

Open **Monthly clients** in the top bar and add a client. You set these once:

- who they are (name, company, email, address)
- what you bill them for, e.g. *Website maintenance & hosting*, plus an optional line of small print about what's included
- the usual monthly amount and currency
- their tax rate, how many days they get to pay, and any terms that differ from your defaults

The right side shows a sample invoice while you type, so you can see exactly what they'll get.

After that, each client has a card. Pick the month and year, change the amount if this month is different, and hit **Generate invoice** (or press Enter in the amount box). The invoice is saved and opens ready to download. Its line reads *Website maintenance & hosting — October 2026*, and the invoice shows October 2026 as the billing period.

The card remembers where you are. After you invoice September, it suggests October next time. If you pick a month you've already invoiced, the card tells you and links to the existing invoice. You can still make a second one if you need to.

Need to add something one-off, like extra hours? Generate as normal, then add a line in the editor before downloading. Changing a client's setup only affects invoices you make afterwards. Removing a client keeps the invoices you already made for them.

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
- Clearing site data clears your saved invoices, monthly clients and company details.

If you need invoices on more than one device, deploy it once and use the same browser profile, or save the PDFs somewhere shared.

## Invoice numbering

Numbers are issued per year and increment when you save. Proformas have their own series (`VB-PF-2026-001`). Final and monthly invoices are both tax invoices, so they share one unbroken series (`VB-INV-2026-001`, `-002`, ...).

You can type over a number by hand. If a new invoice ends up with a number that's already taken, it gets the next free one when you save and you'll be told. Change the prefixes in Company details.

## Handy bits

- **Copy** on a recent invoice duplicates it with a fresh number and today's date.
- On a final invoice, once you answer "yes" to the advance you can **pull straight from a saved proforma** — it copies the client, the line items and the advance amount across, and references the proforma number on the invoice.
- `Ctrl`/`Cmd` + `S` saves an invoice or a monthly client, `Ctrl`/`Cmd` + `P` prints.
- The home screen lists your 8 most recent invoices. **Show all** lists everything (up to 1,000 are kept).
- Amounts are written out in words under the amount due. Turn that off in Company details if you'd rather not.
