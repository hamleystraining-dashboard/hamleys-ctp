```markdown
# ⭐ Hamleys — Certified to Play Dashboard

Live compliance tracking for the 6-week **Certified to Play** certification programme for all new Fun Consultants across Hamleys stores.

---

## 🔗 Live Links

| Page | URL |
|------|-----|
| 📊 **Dashboard** | `https://hamleystraining-dashboard.github.io/hamleys-ctp/` |
| ⚙️ **Admin / Upload** | `https://hamleystraining-dashboard.github.io/hamleys-ctp/ctp_admin.html` |

---

## 📁 Files in This Repo

| File | Purpose |
|------|---------|
| `index.html` | Main dashboard — FC Progress, Store Compliance, ROM Compliance (unchanged) |
| `ctp_admin.html` | Admin page — Upload Excel files, auto-normalize new form schema, maintain Master Dataset, Set Day 0 per FC |
| `data/responses.json` | Placeholder for future Power Automate live feed |
| `README.md` | This file |

---

## 🆕 New MS Forms Structure (Branching Form)

To reduce SD scroll-fatigue on a 120-store dropdown, the form now branches:
**ROM → SD (mapped to that ROM) → Store (mapped to that SD)**.

This means the raw responses sheet no longer has a single `Store Name` / `Store Code` / `Assessors Name` column — instead it has:
- 6 "X's SDs" columns (one per ROM: Divya, Shirish, Ashlin, Ajay, Chetan, Lalit)
- 18 "X's Stores" columns (one per SD)

**Only one SD column and one Store column will be non-empty per row**, since each respondent only ever sees their own branch.

The Admin tool automatically detects this new schema and **normalizes** it back to the original shape before any calculation happens — see "How the Admin Tool Handles This" below. No manual conversion is required, and `index.html`'s logic, columns, and calculations are **completely unchanged**.

---

## 🚀 How to Use (Weekly Workflow — Unchanged for You)

### Every time responses are updated:
1. Download **CertifiedToPlay_Reports.xlsx** from SharePoint (joinees + Day 0)
2. Download **Certified_To_Play.xlsx** from SharePoint (MS Forms responses — new branched schema)
3. Open the **Admin page** → Upload Files tab
4. Upload both Excel files directly (no CSV conversion, no manual editing needed)
5. Click **Open Dashboard →** — compliance recalculates instantly, using historical + new data combined

### One-time setup (already done once):
- Historical `Certified_To_Play.xlsx` (271 legacy rows) seeded once into the Admin tool's persistent Master Dataset
- **Base_Store_Data.xlsx** uploaded for complete Store → ROM → SD mapping (also used to resolve Store Code for new-schema rows)

---

## 🧠 How the Admin Tool Handles This (Behind the Scenes)

1. **Schema detection** — on each upload, the tool checks for old-schema columns (`Store Name`, `Store Code`, `Assessors Name`) vs new-schema columns (`Divya's SDs`, `Ashlin's Stores`, etc.).
2. **Normalization** (new schema only) — for each row:
   - `Assessor Name` = the one non-empty value among the 6 "X's SDs" columns
   - `Store Name` = the one non-empty value among the 18 "X's Stores" columns
   - `Store Code` = looked up from `Base_Store_Data.xlsx` using the resolved Store Name
   - All other fields (FC Name, Emp Code, DOJ, Assessment Type, Zone, Outcome, Cert Date, Areas of Improvement, Photo) pass through unchanged
3. **Merge into Master Dataset** — normalized rows are merged into a persistent Master Dataset (stored in the browser), **deduped by `Id`**, so re-uploads never double-count and the original 271 historical rows are never lost or re-entered.
4. **Everything downstream** (Emp Code/fuzzy-name matching, ideal/actual cert calculation, Generate → `index.html`) runs on the full merged Master Dataset, exactly as before.

---

## 🏁 How Compliance Works (Unchanged)

### The three input files:

| File | What it provides |
|------|-----------------|
| `CertifiedToPlay_Reports.xlsx` | FC master list — Name, Emp Code, Store, SD, ROM, DOJ, **Day 0** |
| `Certified_To_Play.xlsx` | MS Forms responses — FC Name, Emp Code, Zone, Outcome, Certification Date (normalized internally if new schema) |
| `Base_Store_Data.xlsx` | Store Code ↔ ROM Name + SD Name mapping |

### Calculation logic:

**Ideal Certifications (denominator)** — based on days elapsed since Day 0:

| Days Since Day 0 | Zone | Ideal Certs |
|---|---|---|
| 7+ | Demo | 1 |
| 15+ | CSD | 2 |
| 22+ | Product | 3 |
| 29+ | Selling | 4 |
| 36+ | VM | 5 |
| 43+ | Billing | 6 |

**Actual Certifications (numerator)** — from responses sheet:
- For each FC × Zone combination: if **any** response row has Outcome = "Pass" → that zone is certified (= 1)
- Multiple "Pass" entries for the same FC + Zone = still counts as 1
- "Reassessment" only entries for a zone = 0 (not certified)
- Zones: Demo · CSD · Product · Selling · VM · Billing

**Compliance % = Actual ÷ Ideal × 100**

- FC with 0 ideal → "Not yet due"
- FC with 6/6 → "🏆 Fully Certified"

---

## ⚙️ Day 0 Settings

- **Day 0** = official start of the FC's 6-week journey (col L in Reports sheet)
- Always set to a **Friday** so all assessment dates align to Fridays
- Assessment dates: W1 = Day 0 + 7, W2 = +14, W3 = +21, W4 = +28, W5 = +35, W6 = +42
- Day 0 can be overridden per FC on the Admin page without editing Excel

---

## 🗄️ Master Dataset & Backup

- The Admin tool maintains a persistent **Master Dataset** in the browser (`localStorage`), seeded once with the 271 historical rows, and growing weekly with normalized new-schema rows.
- Use **"Export Master Backup"** on the Admin page after each Generate — downloads the full merged dataset as `.xlsx`. Keep this safe; clearing browser data on the admin laptop would otherwise wipe the in-browser Master Dataset.
- **"Reset Master Data"** is available for a full re-seed, if ever required.

---

## 🔮 Future: Power Automate Live Data

When ready, configure Power Automate to:
1. Trigger on every MS Forms submission
2. Read