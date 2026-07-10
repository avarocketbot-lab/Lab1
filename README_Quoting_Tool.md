# Rental Quoting Tools — Concepts A & B

Both built from the **`USD List`** tab of the *2026 Rental Pricing Workbook*. These
are the first step toward the web app: get the pricing logic right in a spreadsheet,
then port it. Both share the same **Price List** data and the same discount rules,
so you can compare the two ways of working and pick one to perfect.

## Files
- **`Rental_Quoting_Tool.xlsx`** — Concept A, the line-item builder (2 tabs).
- **`Rental_Kit_Configurator.xlsx`** — Concept B, the guided kit configurator (3 tabs).
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

---

# Concept B — Guided Kit Configurator (`Rental_Kit_Configurator.xlsx`)

The "select what you want, get a price" flow. You build **one kit** from category
menus, then say how many kits and how many days.

### `Build a Kit` tab
1. Set **Discount off list** (default 20%), **Number of kits**, and **Rental days**
   at the top.
2. Work down the menu sections, picking an item from each dropdown (or leave
   `- none -`):
   1. Base monitor  2. Sensors & cartridges  3. EXO modules & accessories
   4. Docking, charging & G7 accessories  5. G6 accessories & docks
   6. Monitoring & data plans  7. Gas cylinders & consumables
3. Set **Qty/kit** for each pick. Each row shows the discounted **rate/day**.
4. **Kit subtotal/day** sums the rows; **Kit total = subtotal × kits × days**.
5. **One-time services** (training, move fee, tech hours, call-out) are picked
   separately and charged **once** (not × days).
6. **Grand Total = Kit total + one-time subtotal.**

### `Menu` tab
Drives the dropdowns: a master `Label → SKU` table plus one option list per
section. It reads prices from the `Price List` tab, so prices stay single-sourced.

### A vs B — when to use which
- **A (line-item):** free-form — any mix of items and quantities. Best for
  arbitrary quotes.
- **B (configurator):** guided — build a standardized kit and scale it by
  #kits × days. Best for repeatable rental packages.

Both produce the same prices for the same items; they're just two entry styles.
Tell me which one to perfect and carry into the web app.
