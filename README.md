# ⭐ Hamleys — Certified to Play Dashboard

A live, shareable web dashboard to track the 6-week **Certified to Play** certification journey for all new Fun Consultants across Hamleys stores.

---

## 🔗 Live Dashboard

👉 **[Open Dashboard](https://hamleystraining-dashboard.github.io/hamleys-ctp/)**

👉 **[Admin / Set Day 0](https://hamleystraining-dashboard.github.io/hamleys-ctp/ctp_admin.html)**

---

## 📁 Files in This Repo

| File | Purpose |
|------|---------|
| `index.html` | Main dashboard — FC Progress, Store Compliance, ROM Compliance |
| `ctp_admin.html` | Admin page — Upload CSV data, Set Day 0 per FC |
| `data/responses.json` | Placeholder for future Power Automate live data feed |
| `README.md` | This file |

---

## 🚀 How to Use (Manual CSV Upload)

1. Open the MS Forms responses Excel file from SharePoint
2. Go to **File → Save As → CSV UTF-8**
3. Open the **[Admin page](https://hamleystraining-dashboard.github.io/hamleys-ctp/ctp_admin.html)**
4. Click **"Upload Responses CSV"** and select your file
5. The dashboard updates instantly — compliance, next certs due, all views refresh automatically
6. Your data is remembered in the browser — no need to re-upload unless data has changed

---

## 📊 How Compliance Is Calculated

| Days since Day 0 | Certifications Due |
|---|---|
| 7+ days | 1 (Demo) |
| 15+ days | 2 (Demo + CSD) |
| 22+ days | 3 |
| 29+ days | 4 |
| 36+ days | 5 |
| 43+ days | 6 (Fully Certified) |

**Compliance % = Actual Certifications Done ÷ Ideal Certifications Due × 100**

- FC with 0 ideal certs → shown as "Not yet due"
- Week result: `1` = Pass · `0` = Reassessment · blank = Not yet assessed

---

## 📋 CSV Column Reference

Your CSV export must have these column headers (exact names, case-insensitive):

```
Name, Emp Code, Desgn, Store, Region, Store Code, SD, ROM, DOJ, Day 0,
Demo, CSD, Product, Selling, VM, Billing, Remarks
```

---

## ⚙️ Setting Day 0

- Day 0 = the official start date of the FC's 6-week journey
- Always set to a **Friday** so all weekly assessments fall on Fridays
- W1 Assessment = Day 0 + 7 days
- W2 Assessment = Day 0 + 14 days … and so on
- Manage Day 0 per FC on the [Admin page](https://hamleystraining-dashboard.github.io/hamleys-ctp/ctp_admin.html)

---

## 🔮 Future: Automated Live Data (Power Automate)

When ready, Power Automate can be configured to:
1. Trigger on every MS Forms submission
2. Read all rows from the SharePoint Excel responses file
3. Write to `data/responses.json` in this repo via GitHub API
4. The dashboard auto-fetches and refreshes every 5 minutes

Setup instructions are available in the **Live Data Setup** section of the dashboard.

---

## 👥 Owned By

Hamleys Training & People Development Team  
For queries contact the L&D team.
