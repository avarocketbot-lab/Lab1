# Blackline Rental Quote Builder — best of breed

**`Blackline_Rental_Quote_Builder.xlsx`** is the current tool. It merges the two
earlier concepts (A's guided configurator + B's single-sourced price data) and
adds EXO area monitors, the EXO 8 gas expansion modules, and real quote-style
SKU generation. The two originals (`Rental_Quoting_Tool.xlsx`,
`Rental_Kit_Configurator.xlsx`) are kept for reference; `Rental_Price_List.csv`
remains the portable copy of the 114-item price list.

## Tabs

| Tab | What it does |
|---|---|
| **Start Quote** | Quote #, dates, customer, tax/freight/misc, totals roll-up. |
| **Guided Configurator** | The main interface — up to 10 configured units. |
| **Accessories & Services** | Free-form line items: docks, chargers, gas, training, monitoring, one-time fees. |
| **Quote Summary** | Internal consolidated view with totals. |
| **Printable Quote** | Customer-facing page (SKU + description + rates), print-ready. |
| **Instructions** | Usage notes and pricing assumptions. |
| Pricing Database / Sensor Rules / Lists | Hidden data sheets — all rates single-sourced from the 2026 USD List. |

## Guided Configurator

Per line: **Device → LEL → O2 → Toxic 1 → Toxic 2 → (EXO only) 8-Gas Expansion
Module → Qty / Days / Disc%**. The sheet computes base daily, sensor-upgrade
daily, module daily, unit daily, extended total — and builds the **SKU** and a
quote-ready description.

### Device choices
- **G7/G8c Diffusion / G7/G8c Pumped** — cellular personal monitor. Quoted and
  SKU'd as `RENT-G7C`; fulfilled from fleet inventory as either G7c or G8c
  (same rate). Label says G7/G8c so customers know either may ship.
- **G7X Diffusion / G7X Pump** — unchanged (no G8 X exists yet).
- **EXO Diffusion / EXO Pumped** — NEW. Pumped is priced as EXO base + pump
  module (`RENT-G7EXO` + `RENT-G7EXO-PUMP`), single lookup key
  `RENT-G7EXO-PUMPED` in the pricing database.

### EXO 8 gas expansion modules
A dropdown on each line (EXO devices only) with the eleven GEM combos from the
price list (e.g. `HF, NH3, SO2, PID (2ASV)` = `RENT-EX8-GEM-02`, $44/day). The
module's daily rate is added to the unit daily and its 4-letter gas code slots
into the SKU. Selecting a module on a non-EXO device flags
**"Module needs EXO device"** in the Status column. Modules can also be quoted
standalone on the Accessories & Services tab by SKU.

### SKU builder (matches the traditional quote format)
```
RENT-G7C-Q-DIOV-1D      G7/G8c, diffusion cartridge (Q), sensors DIOV
RENT-G7C-P-DIOV-1D      G7/G8c, pumped cartridge (P)
RENT-G7X-Q-HIOX-1D      G7X diffusion
RENT-EXO-X-X-DIOV-1D    EXO, no expansion module, diffusion
RENT-EXO-2ASV-P-HIOV-1D EXO, GEM 2ASV module, pumped
```
Sensor letters use the price-list selector codes (H=H2S, I=LEL-IR, O=O2,
V=PID, D=COSH, 2=HF, …), sorted ascending with `X` padding for empty slots —
the same convention as the GEM codes (2ASV, 6ALV, EHLS…) and existing quotes.
`-1D` = daily rate.

## Pricing rules (unchanged)
- Every rate comes from the **USD List column C (DAILY RATE)** via the hidden
  Pricing Database — nothing is hardcoded.
- Sensor upgrade tiers: Premium +$3.20/day, Advanced (CO2) +$8.00/day, VOC/HF
  (PID, HF) +$12.00/day; standard sensors (LEL, O2, H2S, CO, SO2) add $0.
- Extended = Qty × Days × Unit daily × (1 − Disc%). One-time services are flat.
- `RENTAL-ONE-TIME` still needs a manual price via Rate Override.

## Verification
Logic was verified against the 5.31.26 Sentry Safety FLNG quote: a G7/G8c
diffusion line with COSH + PID reproduces `RENT-G7C-Q-DIOV-1D` exactly, and the
EXO pumped rate equals base + pump module as quoted.
