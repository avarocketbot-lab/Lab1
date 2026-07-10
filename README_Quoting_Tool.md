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
   1. Base monitor
   2. **Cartridge & sensors** — build the gas cartridge one sensor at a time
      (see below)
   3. EXO modules & accessories  4. Docking, charging & G7 accessories
   5. G6 accessories & docks  6. Monitoring & data plans
   7. Gas cylinders & consumables
3. Set **Qty/kit** for each pick. Each row shows the discounted **rate/day**.

### Cartridge & sensor builder (section 2)
Modeled on the sensor selector in the *USD Purchase/Lease* price list, but priced
on the **rental sensor tiers**. Five picks build a multi-gas cartridge:

- **Cartridge type** — Diffusion (`RENT-CART-Q`, $6/day) or Pumped
  (`RENT-CART-P`, $10/day). This is the housing + standard sensors.
- **Combustible (LEL)** — LEL-IR or LEL-MPS (both standard, no upcharge).
- **Oxygen** — O2 (standard, no upcharge).
- **Toxic sensor #1** and **#2** — pick any gas; the price appears based on its
  rental tier.

**Sensor pricing tiers (rental, per day, before discount):**
| Tier | Sensors | Add/day |
|---|---|---|
| Standard (included in cartridge) | LEL-IR, LEL-MPS, O2, H2S, CO, SO2 | $0 |
| Premium | NH3, hi-range NH3, NO2, O3, hi-range H2S, hi-range CO, COSH, Cl2, ClO2, CO-H, HCN, H2 | +$4 |
| Advanced | CO2 | +$10 |
| VOC / HF | PID, HF | +$15 |

So a Diffusion + LEL-IR + O2 + **CO2** + **PID** cartridge = $6 + $0 + $0 + $10 +
$15 = **$31/day list → $24.80/day at 20% off**. Change a toxic pick and the price
updates instantly.
4. **Kit subtotal/day** sums the rows; **Kit total = subtotal × kits × days**.
5. **One-time services** (training, move fee, tech hours, call-out) are picked
   separately and charged **once** (not × days).
6. **Grand Total = Kit total + one-time subtotal.**

### `Menu` tab
Drives the dropdowns: a master `Label → SKU` table, one option list per section,
plus a **SensorMap** (`sensor → rental tier SKU`) for the cartridge builder. All
prices are read from the `Price List` tab, so they stay single-sourced — edit a
price once and both the line-item and configurator tools follow.

### A vs B — when to use which
- **A (line-item):** free-form — any mix of items and quantities. Best for
  arbitrary quotes.
- **B (configurator):** guided — build a standardized kit and scale it by
  #kits × days. Best for repeatable rental packages.

Both produce the same prices for the same items; they're just two entry styles.
Tell me which one to perfect and carry into the web app.
