# JAIN AGM Pharmacy Drug Stock Register Tool

**Version:** 7.0  
**Institution:** Jain AGM Ayurvedic Medical College & Hospital, Varur, Hubli, Karnataka  
**Developed by:** Astanga Wellness Pvt. Ltd., Hubli  
**Platform:** Standalone HTML — deployable on GitHub Pages or any web server  
**Dependencies:** [SheetJS (xlsx.full.min.js)](https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js) via CDN  

---

## Overview

A fully self-contained, browser-based pharmacy management tool designed for Ayurvedic medical college hospitals. It covers the complete dispensing and procurement workflow required for institutional inspection compliance — from daily stock tracking to purchase orders, invoices, and department indent registers (OPD and IPD).

No server, no database, no login required. All data is stored in the browser's `localStorage` and can be exported to Excel at any point.

---

## Features at a Glance

| Module | Description |
|---|---|
| 📋 Stock Register | Month-wise dispensed, opening, and closing stock for all medicines |
| ⚙️ Settings | Threshold configuration and data reset |
| 🛒 Purchase Orders | Auto PO (low stock trigger) and Manual PO builder |
| 🧾 Invoices | Tax invoice generation with GST for each PO |
| 📝 Dept. Indent | Separate OPD and IPD indent registers with HOD signatures |
| 💊 Medicine List | Add/remove/import custom medicines; Excel data import |
| 📊 Analytics | Month-wise dispensing bar chart per medicine |

---

## Modules — Detailed

### 📋 Stock Register

- Displays opening stock, dispensed quantity, unit type, number, and closing stock for every medicine across all 12 months
- Year selector: switch between **2025** (full year) and **2026** (Jan–Apr loaded; remaining months enterable)
- Filter by month, category, or search by name
- **Low Stock Only** toggle highlights medicines below the configured threshold
- **Edit Mode** (toggle button): turns cells yellow (stock/dispensed) or green (unit type/number) — click any cell to edit inline; changes save instantly to localStorage and cascade forward through subsequent months
- Closing stock = Opening − Dispensed, calculated on the fly
- Status badges: 🔴 LOW / 🟡 WATCH / 🟢 OK based on days-of-stock remaining
- Export full register to Excel

#### Unit Type & Number columns

Every medicine has two unit columns replacing the old single "Pack Unit" field:

| Category | Unit Type | Number |
|---|---|---|
| Churnas / Kwatha Choornas | gm | 25 |
| Ghritas / Lehas | gm | 150 |
| Vati / Rasas | Tablets | Actual monthly dispensed count |
| Guggulu | Tablets | Actual monthly dispensed count |
| Tailas (general) | ml | 100 |
| Tailas (Anu / Bilwa / Kumkumadi) | ml | 10 |
| Asava-Arishta / Kashayas / Syrups | ml | 200 |
| Eye Medicines | ml | 5 |
| Liniments | gm | 30 |
| External Use | gm | 25 |
| Ointments | gm | 25 |
| Miscellaneous | unit | 1 |

Unit type and number for all medicines can be overridden in Edit Mode. Vati/Rasas and Guggulu Number columns show actual dispensed count per month (dynamic, read-only).

---

### ⚙️ Settings

- **Minimum stock threshold (days):** triggers Low Stock flag and Auto PO (default: 60 days)
- **PO order period (months):** quantity to order in each PO (default: 2 months)
- **Reset All Data:** clears all manual edits, opening stock overrides, and unit overrides from localStorage, restoring original hospital records

---

### 🛒 Purchase Orders

Three sub-tabs:

#### ⚡ Auto PO
- Automatically identifies all medicines whose closing stock falls below the configured threshold
- Groups items by supplier and generates numbered POs: `PO-JAGM-YYYY-001`
- Each PO shows: medicine, category, unit type, number, current stock, days left, and order quantity
- Direct **Invoice** button on each PO card
- Filter by supplier; Export to Excel; Print

#### ✏️ Manual PO Builder
- Full medicine list with checkboxes — tick any medicines to order
- Enter order quantity per medicine
- Select **month and year** of receipt — can be backdated to any past month (e.g. April 2025)
- On generation: opening stock of the selected month is updated and cascades forward through all subsequent months
- PO is numbered: `PO-JAGM-YYYY-M001`
- Stock register updates immediately

#### 📜 PO History
- All manual POs are saved to localStorage and persist across sessions
- Each entry shows: PO number, date, month/year, items ordered
- Direct Invoice button per PO
- Delete individual POs or clear all history
- Export history to Excel

---

### 🧾 Invoices

- Generated automatically from any Auto PO or Manual PO
- Supplier: **Astanga Wellness Distributors, Vidyanagar, Hubli**  
  GSTIN: `29AAPCA9132R1ZP`
- Billed to: Principal, JAIN AGM Ayurvedic Medical College & Hospital, Varur, Hubli
- Features:
  - Editable price per pack per line item
  - Per-item GST selector: 5% or 12% (Apply to all button available)
  - Auto-calculates Base Amount, GST Amount, Grand Total
  - Amount in words (Indian numbering: Lakhs, Crores)
  - Due date = Invoice date + 30 days (Net 30 terms)
  - PO reference number on every invoice
  - Invoice numbered: `INV-JAGM-YYYY-001`
- Print and Excel export per invoice

---

### 📝 Department Indent Register

Two fully separate registers — **OPD** and **IPD** — each with its own sub-tab.

#### Workflow
1. Click **Create / Edit OPD Indent** or **Create / Edit IPD Indent**
2. All medicines from current Auto POs and saved Manual POs are loaded as line items
3. For each medicine, select the requesting **department** from the dropdown
4. Adjust **indent quantity** if needed
5. Use **+ Dept** to split a medicine across multiple departments (adds a new row)
6. Filter by PO reference if needed
7. Click **Save & Preview** — the formatted indent document appears in the tab

#### Indent document includes
- Indent number: `IND-JAGM-OPD-YYYY-001` / `IND-JAGM-IPD-YYYY-001`
- Date, type badge (OPD/IPD), item count, PO references
- Full line-item table: medicine, category, unit type, number, PO ref, PO qty, department, indent qty, remarks
- **Department-wise summary table**
- **Signature blocks for all 7 HODs** (labelled with OPD or IPD)
- Signature lines for Medical Superintendent and Pharmacist In-charge

#### Departments
1. Kayachikitsa
2. Shalya Tantra
3. Shalakya Tantra
4. Panchakarma
5. Kaumara Bhritya
6. Prasuti Tantra & Stri Roga
7. Agada Tantra

Both OPD and IPD indents can be saved simultaneously and printed as separate documents.

---

### 💊 Medicine List

#### Add New Medicine
- Fields: Name, Category (dropdown with existing categories + New Category option), Unit Type (dropdown: gm / ml / Tablets / Capsules / unit / Other), Number per pack
- Custom medicines tagged with CUSTOM badge throughout the tool
- Saved to localStorage; persists across sessions

#### Import / Export Custom Medicines
- Export custom medicine list as JSON
- Import JSON to restore or share the custom list across devices

#### Import Dispensed Data (Excel)
- Upload an `.xlsx` file with dispensed data
- Sheet names must contain "2025" or "2026"
- Column 0 = Medicine name; Columns 1–12 = Jan–Dec dispensed quantities
- Imported data overrides the built-in hospital records for matched medicines

#### Full Medicine List table
- Shows all base + custom medicines with unit type, number, and source
- Filter by category or search by name
- Remove custom medicines individually

---

### 📊 Analytics

- Select any medicine and year to view a month-by-month dispensing bar chart
- Shows: Annual total, Monthly average, Daily dispensing rate, Closing stock
- Unit type displayed on the chart axis

---

## Data Loaded

### 2025 — Full Year (Jan–Dec)
Actual dispensed quantities from hospital records for all medicines across all 12 months.

### 2026 — Jan to Apr
Actual dispensed quantities for Jan, Feb, Mar, Apr 2026. May–Dec can be entered manually using Edit Mode in the Stock Register.

---

## Dispensing Logic

- **Opening stock** defaults to 0 for January unless manually overridden
- **Closing stock** = Opening − Dispensed (never below 0)
- **Cascade:** editing any month's opening stock automatically recalculates closing stock for that month and opening/closing for all subsequent months
- **PO receipt:** when a PO is generated for a specific month, the ordered quantity is added to that month's opening stock and cascades forward
- **Daily rate:** average of all non-zero monthly dispensed values ÷ 30
- **Days left:** closing stock ÷ daily rate
- **Low stock threshold:** configurable (default 60 days)

---

## Supplier Mapping

| Category | Default Supplier |
|---|---|
| Churnas, Kwatha Choornas, External Use | SDM Pharmaceuticals, Udupi |
| Ghritas, Lehas | AVN Arogya Pvt Ltd, Coimbatore |
| Vati/Rasas | Shankar Narayan Ayurvedic, Pune |
| Tailas | Nagarjuna Ayurvedic, Thrissur |
| Eye Medicines, Liniments, Syrup, Ointments | Arco Lab, Bengaluru |
| Guggulu | Impcops, Chennai |
| Asava-Arishta, Kashayas | Veda Pharma, Mysuru |
| Miscellaneous | Other Suppliers |

All POs and invoices are raised through **Astanga Wellness Distributors, Hubli** as the single distribution entity.

---

## Data Persistence (localStorage)

| Key | Contents |
|---|---|
| `jagm_custom` | Custom medicines added by user |
| `jagm_disp2025` | Edited dispensed data for 2025 |
| `jagm_disp2026` | Edited dispensed data for 2026 |
| `jagm_open2025` | Opening stock overrides for 2025 |
| `jagm_open2026` | Opening stock overrides for 2026 |
| `jagm_units` | Unit type/number overrides |
| `jagm_pohistory` | Saved manual PO history |

All data is stored locally in the browser. Clearing browser data or using Incognito mode will reset the tool. Use the Excel export regularly to back up your data.

---

## Deployment on GitHub Pages

1. Save the tool as `index.html`
2. Create a GitHub repository (e.g. `jain-agm-pharmacy`)
3. Upload `index.html` to the repository
4. Go to **Settings → Pages → Deploy from main branch**
5. The tool will be live at:  
   `https://yourusername.github.io/jain-agm-pharmacy`

To update the tool, simply replace `index.html` with the new version and push to GitHub.

---

## Technical Notes

- **No server required** — runs entirely in the browser
- **No login or authentication** — secure within your institution's network
- **Single file** — the entire tool is one `index.html` with no external dependencies except SheetJS (loaded from CDN; requires internet connection)
- **Print/PDF** — use the browser's Print function (Ctrl+P / Cmd+P); non-printable controls are hidden automatically via CSS `@media print`
- **Excel export** — uses SheetJS library; all exports include cover sheet with institution name and date
- **Edit Mode** — yellow cells = stock/dispensed editable; green cells = unit type/number editable; changes auto-save on blur (clicking away from the cell)
- **Browser compatibility** — tested on Chrome, Edge, Firefox (latest versions)

---

## Regulatory Compliance Notes

This tool supports the following records required under CCIM / NCISM inspection norms for Ayurvedic medical college hospitals:

- ✅ Monthly pharmacy stock register (opening, dispensed, closing)
- ✅ Purchase order register with supplier details
- ✅ Tax invoice register (GST-compliant)
- ✅ Department-wise OPD medicine indent register
- ✅ Department-wise IPD medicine indent register
- ✅ HOD and Medical Superintendent signature provisions
- ✅ Pharmacist In-charge countersignature provision

---

## Support & Customisation

For additions, edits, or new modules, contact:

**Astanga Wellness Pvt. Ltd.**  
Vidyanagar, Hubli, Karnataka — 580021  
GSTIN: 29AAPCA9132R1ZP

---

*Generated by JAIN AGM Pharmacy Management System*  
*Astanga Wellness Pvt. Ltd. © 2025–2026*
