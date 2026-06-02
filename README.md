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
| `index.html` | Main dashboard — FC Progress, Store Compliance, ROM Compliance |
| `ctp_admin.html` | Admin page — Upload Excel files, Set Day 0 per FC |
| `data/responses.json` | Placeholder for future Power Automate live feed |
| `README.md` | This file |

---

## 🚀 How to Use

### Every time responses are updated:
1. Download **CertifiedToPlay_Reports.xlsx** from SharePoint (joinees + Day 0)
2. Download **Certified_To_Play.xlsx** from SharePoint (MS Forms responses)
3. Open the **Admin page** → Upload Files tab
4. Upload both Excel files directly (no CSV conversion needed)
5. Click **Open Dashboard →** — compliance recalculates instantly

### One-time setup:
- Also upload **Base_Store_Data.xlsx** for complete Store → ROM → SD mapping

---

## 📊 How Compliance Works

### The three input files:

| File | What it provides |
|------|-----------------|
| `CertifiedToPlay_Reports.xlsx` | FC master list — Name, Emp Code, Store, SD, ROM, DOJ, **Day 0** |
| `Certified_To_Play.xlsx` | MS Forms responses — FC Name, Emp Code, Zone, Outcome, Certification Date |
| `Base_Store_Data.xlsx` | Store Code → ROM Name + SD Name mapping |

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

## 🔮 Future: Power Automate Live Data

When ready, configure Power Automate to:
1. Trigger on every MS Forms submission
2. Read all rows from the SharePoint responses Excel
3. Write JSON to `data/responses.json` in this repo via GitHub API
4. Dashboard auto-refreshes — zero manual uploads needed

---

## 👥 Owned By
Hamleys Training & People Development Team
