# Rental Quoting Tool — v1 (Concept A: Line-Item Builder)

Built from the **`USD List`** tab of the *2026 Rental Pricing Workbook*. This is the
first step toward the web app: get the pricing logic right in a spreadsheet, then
port it.

## Files
- **`Rental_Quoting_Tool.xlsx`** — the working tool (2 tabs).
- **`Rental_Price_List.csv`** — the same 114-item price list as portable CSV
  (future data source for the web app / for auditing prices).

## How the tool works
Two tabs:

### `Quote` tab
1. Fill in the header: customer, date, prepared-by, rates-valid year.
2. **Discount off list** (cell C6) — a dropdown: `0 / 0.10 / 0.15 / 0.20 / 0.25`.
   Defaults to **0.20** (20% off = the current IES rate). Applies to every
   discountable line at once.
3. For each line, pick a **SKU** from the dropdown. The **Description**,
   **List/day**, and discounted **Unit rate/day** fill in automatically.
4. Enter **Qty** and **Days**. `Line Total = Unit rate × Qty × Days`.
5. **Subtotal → Tax % → Grand Total** compute at the bottom.

Yellow cells = your inputs. Everything else is a formula.

### `Price List` tab
The reference catalog: `SKU · Description · Category · List Price (daily) ·
Discountable`. Edit prices here and the Quote tab follows.

## Pricing rules
- **Daily-rate model:** every SKU is priced per day; a quote is
  `rate × quantity × days`.
- **Discountable items** (devices, sensors, accessories, gas, training) get the
  chosen discount off List Price. `20% off` reproduces the sheet's IES
  `DAILY RATE` column exactly (e.g. `RENT-G7C`: $10 list → **$8.00/day**).
- **Flat fees** (call-out surcharge, technician hours, one-time move fees) are
  **never discounted** — the tool applies the discount only where the source
  data shows a real discount.
- **One-time fees** (training, move fee, tech hours) are entered as lines with
  **Days = 1**.

## Next up — Concept B (Guided Configurator)
Per your pick (*A, then B*): a wizard-style "pick a device family → tick options →
get a bundled kit price" flow, layered on the same price list, as presets.
