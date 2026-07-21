# Cat XL Tower Pricer

A lightweight, single-file browser app for pricing catastrophe excess-of-loss (Cat XL) reinsurance towers using a risk-adjusted (expected loss + volatility loading) pricing methodology.

No install, no backend, no build step. Open `index.html` in any modern browser — everything runs client-side in JavaScript and nothing leaves your machine.

## Quick start

1. Open `index.html` in a browser (double-click it, or `open index.html` / drag into a browser tab).
2. Click **Load example tower** to see a populated 5-layer program, or start entering your own layers.
3. Enter each layer's attachment, limit, expected loss (AAL), and standard deviation from your cat model output.
4. Tune the global risk load (λ), expense ratio, and ROL floor assumptions to your market view.
5. Read off technical premium, gross premium, rate on line (ROL), and multiple of expected loss per layer, plus tower-level totals and charts.
6. Use **Export JSON** / **Import JSON** to save a tower to a file or share it with a colleague. The app also autosaves to your browser's local storage.

## Pricing methodology

Each layer is priced using the standard-deviation risk loading approach — a widely used actuarial method for risk-adjusted premium:

```
CoV               = Std Dev / Expected Loss
Risk Load         = λ × Std Dev
Technical Premium = Expected Loss + Risk Load
Gross Premium      = Technical Premium / (1 − Expense Ratio)
                      floored at (ROL Floor × Limit) if higher
ROL               = Gross Premium / Limit
Multiple of EL    = Gross Premium / Expected Loss
Payback (years)   = Limit / Gross Premium
```

- **λ (lambda)** is your market price of risk — the risk aversion parameter multiplying the layer's loss standard deviation. Higher λ produces more conservative (higher) technical pricing. It can be set globally and overridden per layer (e.g. to reflect different views on a specific peril or territory).
- **Risk load override** lets you type a risk load ($M) directly for a layer, bypassing the `λ × σ` formula entirely — useful when you have a risk load from another model, a broker indication, or a negotiated number. Layers using this show a "custom" badge in the results table.
- **Expense ratio** grosses up the technical premium for brokerage, ceding commission, and internal expense load.
- **ROL floor** applies a minimum market rate-on-line regardless of technical price — common for remote/high layers where technical pricing alone would fall below what the market will actually clear at. A "floor" badge appears on any layer where the floor is binding.

### Reinstatements

Expected reinstatement premium is approximated as the expected fraction of the layer limit eroded by loss (`Expected Loss / Limit`, capped at the number of reinstatements purchased), charged at the reinstatement rate:

```
Expected Reinstatement Premium = Gross Premium × Reinstatement Rate × min(Reinstatements, EL / Limit)
```

This is a simplified expected-value approximation for quick pricing, not a full simulation of reinstatement burn.

### Tower aggregation

Layer standard deviations are combined **assuming independence between layers**:

```
Tower Std Dev = sqrt(Σ Std Dev_i²)
```

If your cat model provides a correlated/aggregate standard deviation for the whole tower, use that instead — layers within a single cat program are not actually independent (they respond to the same events), so this is a simplifying approximation for the blended tower CoV shown in the totals row.

## Inputs

All monetary layer inputs (attachment, limit, expected loss, standard deviation) are in **$ millions**, matching how towers are typically quoted in the market (e.g. "$10M xs $10M"). Expected Loss (AAL) and standard deviation should come from your catastrophe model output for each layer. Probability of attachment / exhaustion fields are optional and for reference/sanity-check only — they don't feed the pricing calculation.

## What this is not

This is a lightweight technical-pricing calculator, not a full actuarial or catastrophe modeling platform. It does not simulate loss distributions, does not account for inter-layer correlation beyond the simplifying independence assumption above, and does not replace judgment on market cycle, capacity, or counterparty considerations. Use it to get to a fast, defensible technical price and sanity-check quotes — not as a substitute for full model output analysis.

## Files

- `index.html` — the entire app (HTML, CSS, and JavaScript in one file)
- `README.md` — this file
