# 📋 Whatnot Receipt Generator

A Progressive Web App (PWA) that generates thermal label packing slips from Whatnot order CSV exports. Built for buyers who receive shipments without packing slips and need a clean receiving record with true cost basis tracking.

---

## What It Does

- Imports your Whatnot order export CSV
- Groups purchases by **seller + purchase date** — one slip per shopping session
- Generates a formatted packing slip for each session showing:
  - Item name, item number, order ID, and purchase date
  - Sale amount, shipping paid, tax paid
  - Item subtotal
  - Estimated Whatnot fee (13% of sale amount)
  - **True cost basis** per item
  - Session grand total with full breakdown
- Prints directly to a **4" × 6" thermal label printer** (Rollo, Dymo 4XL, Zebra)
- Installs as a home screen app on Android and iOS

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app — single file, no dependencies |
| `manifest.json` | PWA install metadata (name, icon, display mode) |
| `sw.js` | Service worker — enables offline use and home screen install |
| `icon-192.png` | App icon (Android home screen, small) |
| `icon-512.png` | App icon (Android home screen, large / splash) |

---

## How to Use

### 1. Get your Whatnot CSV
In the Whatnot app or website go to **Account → Orders → Export**. Download the CSV — it will be named something like `order-report-xxxxxxxx.csv`.

### 2. Load the app
Open the app in your browser or from your home screen. Tap **Browse Files** and select your CSV.

### 3. Review your orders
The app displays all imported orders grouped by seller and date. Each group header has a **master checkbox** to select or deselect the entire session. Use the search bar or dropdowns to filter by seller or date.

The stats bar at the top shows running totals for everything selected — items, slips, total sale, shipping, tax, estimated fees, and true cost basis.

### 4. Generate slips
Select the items you want slips for and tap **Generate Slips →**.

Each slip shows a checkbox control bar above it. Uncheck any slips you don't want to print — they dim out and are excluded from the print job.

### 5. Print
Tap **Print Checked**. Set your print dialog to:
- Paper size: **4 × 6"** (101.6 × 152.4mm)
- Margins: **None**
- Scale: **100%**
- Headers and footers: **Off**

The app auto-scales slips with many items to fit on a single label.

---

## Installing on Android

1. Open Chrome and navigate to the app URL
2. Tap the **three-dot menu → Add to Home screen**
3. Tap **Install**
4. The app appears on your home screen with the packing slip icon
5. Opens fullscreen with no browser chrome

To update after a new version is deployed: uninstall the app from your home screen and reinstall from the URL. This forces the new version to load rather than the cached one.

---

## Installing on iPhone / iPad

1. Open Safari and navigate to the app URL
2. Tap the **Share button** (box with arrow)
3. Tap **Add to Home Screen**
4. Tap **Add**

Note: File picker behavior on iOS Safari may differ. If the CSV import does not open the file browser, try using the app in Chrome for iOS instead.

---

## Printing from Android

The Dymo 4XL is a USB-only printer with no official Android driver. Options for printing from your tablet:

**Option A — Print via shared network printer**
If the Dymo is connected to a Mac or PC on the same WiFi network, share the printer over the network. Android can discover and print to shared printers via the built-in print service.

**Option B — Generate on tablet, print from Mac/PC**
Use the tablet to import the CSV and select your slips, then send the page to your Mac/PC (email, AirDrop, Google Drive) and print from there.

**Option C — Wireless label printer**
Rollo X and Munbyn label printers have native Android apps and WiFi/Bluetooth connectivity and work without a computer.

---

## Cost Basis Calculation

The 13% Whatnot fee estimate is applied to the **sale amount only** — not shipping or tax — which matches how Whatnot's seller/buyer fee structure actually works.

```
True Cost Basis = Sale + Shipping + Tax + (Sale × 13%)
```

This gives you a conservative estimate of what each item actually cost you all-in, useful for inventory tracking and resale pricing decisions.

---

## CSV Column Mapping

The app expects the standard Whatnot order export format with these columns:

| CSV Column | Used For |
|---|---|
| `order id` | Order reference |
| `order numeric id` | Item number |
| `buyer` | Your username (static) |
| `seller` | Seller name on slip |
| `product name` | Item title |
| `processed date` | Purchase date + grouping key |
| `order status` | Displayed as badge in table |
| `sold price` | Sale amount |
| `shipping price` | Shipping paid |
| `taxes` | Tax paid |

---

## Hosting

This app is hosted on **GitHub Pages** — free, HTTPS included, no server required. The service worker caches all assets on first load so the app works fully offline after that.

To deploy your own copy:
1. Fork this repository
2. Go to **Settings → Pages**
3. Set source to **main branch / root**
4. Your URL will be `https://yourusername.github.io/wn-receipts`

---

## Notes

- This is a **self-generated buyer record** — not an official Whatnot document
- No data leaves your device — the CSV is processed entirely in the browser
- No login, no account, no tracking
- Works on any modern browser — Chrome, Safari, Firefox, Edge
