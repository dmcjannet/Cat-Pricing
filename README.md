# Cat XL Tower Pricer

A lightweight, single-file browser app for repricing catastrophe excess-of-loss (Cat XL) reinsurance towers at renewal, using the risk-adjusted flat ("RA Flat") methodology: hold last year's margin on standard deviation (MSD) constant and reprice to this year's expected loss and standard deviation.

No install, no backend, no build step. Open `index.html` in any modern browser — everything runs client-side in JavaScript and nothing leaves your machine.

## Quick start

1. Open `index.html` in a browser (double-click it, or `open index.html` / drag into a browser tab).
2. Click **Load example tower** to see a populated 5-layer program, or start entering your own layers.
3. For each layer enter:
   - **Attachment** and **Limit** ($M) — defines the layer
   - **Expiring ROL (%)** — last year's rate on line, for reference and the rate-change comparison
   - **Expiring MSD** — last year's margin on standard deviation
   - **New expected loss ($M)** and **New standard deviation ($M)** — this year's modeled figures
4. The **New RA Flat premium**, **New RA Flat ROL**, and **rate change** columns calculate automatically.
5. Use **Export JSON** / **Import JSON** to save a tower to a file or share it with a colleague. The app also autosaves to your browser's local storage.

## Methodology

```
Expiring Premium    = Expiring ROL × Limit
New RA Flat Premium = New Expected Loss + (Expiring MSD × New Standard Deviation)
New RA Flat ROL     = New RA Flat Premium / Limit
Rate Change         = New RA Flat ROL / Expiring ROL − 1
```

**Margin on Standard Deviation (MSD)** is the risk margin per unit of loss standard deviation implied by, or agreed for, the expiring placement — conceptually `(Expiring Premium − Expiring Expected Loss) / Expiring Standard Deviation`. You enter it directly per layer, sourced from the prior year's pricing file, a market benchmark, or your own calculation.

Holding MSD flat year-over-year is what makes this a **risk-adjusted flat** reprice: any resulting change in rate on line is driven purely by the change in modeled expected loss and standard deviation between the expiring and current model views — not by a change in risk appetite. A rate increase is shown in red, a decrease in green, in both the results table and the ROL comparison chart.

## Inputs

All monetary layer inputs (attachment, limit, new expected loss, new standard deviation) are in **$ millions**, matching how towers are typically quoted in the market (e.g. "$10M xs $10M"). New expected loss and standard deviation should come from this year's catastrophe model output for each layer. Expiring ROL and expiring MSD come from the expiring placement.

## What this is not

This is a lightweight renewal repricing calculator, not a full actuarial or catastrophe modeling platform. It does not derive MSD for you, does not simulate loss distributions, and does not replace judgment on market cycle, capacity, or counterparty considerations. Use it to get to a fast, defensible risk-adjusted flat technical price at renewal — not as a substitute for full model output analysis.

## Files

- `index.html` — the entire app (HTML, CSS, and JavaScript in one file)
- `README.md` — this file
