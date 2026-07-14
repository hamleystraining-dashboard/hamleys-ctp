# ⭐ Hamleys — Certified to Play Dashboard

Live compliance tracking for the 6-week **Certified to Play** certification programme for new Fun Consultants across Hamleys stores.

## Files

| File | Purpose |
|---|---|
| `index.html` | Main dashboard — FC Progress, Store Compliance and ROM Compliance |
| `ctp_admin.html` | L&D Admin tool — upload, normalize, merge, deduplicate and generate the dashboard |
| `README.md` | Operating guide and architecture |

## Weekly workflow

1. Download the latest **FC/Joinee Report** (`CertifiedToPlay_Reports.xlsx`).
2. Download the latest **new branching-form response sheet**.
3. Open `ctp_admin.html`.
4. Upload the FC/Joinee Report.
5. Upload the new-form response sheet.
6. The Admin tool automatically:
   - retains the permanent **271 historical responses**;
   - retains previously processed new responses in browser storage;
   - detects the response-sheet schema;
   - normalizes the new branching-form structure;
   - derives **Assessor Name** from the selected SD;
   - derives **Store Code** from the embedded Base Store Data;
   - merges historical + previous + newly uploaded responses;
   - deduplicates by unique **Id**.
7. Click **Generate New index.html**.
8. Upload the generated `index.html` to GitHub.

## Permanent historical baseline

The final 271 responses from the old Microsoft Form are permanently embedded inside `ctp_admin.html`.

This means:

- the old 271 responses are never dependent on the new form export;
- clearing browser storage does **not** remove the historical baseline;
- every Master Dataset is rebuilt on top of those 271 protected records.

## New branching-form structure

The new form follows:

**ROM → mapped SDs → mapped Stores**

The raw response sheet contains:

- `ROM Name`
- six ROM-specific `X's SDs` columns
- eighteen SD-specific `X's Stores` columns
- the unchanged FC and assessment fields

For each submitted row, only one SD branch and one Store branch should contain a value.

The Admin tool converts the new structure back into the same internal structure used by the dashboard:

- selected SD → `Assessor Name`
- selected Store → `Store Name`
- embedded Base Store Data lookup → `Store Code`
- all FC, assessment, zone, outcome and certification fields remain unchanged

The dashboard UI, columns and compliance calculations remain unchanged.

## Master Dataset logic

The Admin tool uses:

**Permanent 271 historical responses  
+ retained previous new-form responses  
+ latest uploaded new-form responses  
= Master Response Dataset**

The merged dataset is deduplicated by `Id`.

If the same response is uploaded again, it is not double-counted.

## Embedded Base Store Data

The current 116-store mapping is embedded inside `ctp_admin.html`:

**Store Name → Store Code → ROM Name → SD Name**

This mapping is used to restore `Store Code` for the new branching-form responses.

If the store network or mappings change materially, regenerate the Admin tool with the latest Base Store Data.

## Compliance logic — unchanged

Ideal certifications are based on days elapsed since Day 0:

| Days since Day 0 | Zone | Ideal certifications |
|---|---|---:|
| 7+ | Demo | 1 |
| 15+ | CSD | 2 |
| 22+ | Product | 3 |
| 29+ | Selling | 4 |
| 36+ | VM | 5 |
| 43+ | Billing | 6 |

For each FC × Zone:

- any `Pass` means the zone is certified;
- multiple passes for the same FC × Zone still count as 1;
- `Reassessment` without a later pass counts as not certified.

**Compliance % = Actual Certifications ÷ Ideal Certifications × 100**

## Deployment

For the public GitHub Pages dashboard, upload the generated file as:

`index.html`

Keep `ctp_admin.html` restricted to the L&D team because it contains the permanent historical baseline and the embedded store mapping.
