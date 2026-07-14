# Hamleys — Certified to Play Dashboard V2

## V2 principle
The generated `index.html` is the single source of truth for the public dashboard. It does not read joinee or response data from browser `localStorage`. This prevents an older browser dataset from overriding a newly generated dashboard.

## Files
- `index.html` — public GitHub Pages dashboard
- `ctp_admin.html` — L&D-only generator
- `README.md` — operating guide

## Permanent historical baseline
The Admin permanently contains the 271 old-form historical responses. Every generation starts with these protected records.

## Supported response sheets
The Admin accepts the old flat form and the new branching Microsoft Form.

For the new form:
- `ROM Name` is read directly
- the populated `X's SDs` / `X’s SDs` branch becomes Assessor Name
- the populated `X's Stores` / `X’s Stores` branch becomes Store Name
- Store Code is derived from Base Store Data

The dashboard's normalized response structure and compliance logic remain unchanged.

## Optional Base Store Data upload
The Admin contains an embedded fallback mapping.

Upload Base Store Data only when Store / SD / ROM mappings change. If uploaded, the new mapping becomes active for that Admin session and is used to normalize the branching-form responses.

Expected columns:
- Store Name
- Store Code
- ROM Name
- SD Name

If no Base Store Data is uploaded, the embedded mapping is used.

## Weekly workflow
1. Open `ctp_admin.html`.
2. Upload the latest FC/Joinee Report.
3. Optional: upload latest Base Store Data if mappings changed.
4. Upload the latest **cumulative** Microsoft Forms response export.
5. Confirm the Admin summary: joinee count, uploaded response count and Master Dataset count.
6. Review/adjust Day 0 if required.
7. Generate and download `index.html`.
8. Replace GitHub's existing `index.html` with the newly generated file.
9. Wait for GitHub Pages deployment and hard-refresh the dashboard.

## Master Dataset
`271 permanent historical responses + latest cumulative response export = Master Dataset`

Records are deduplicated by unique `Id`.

Because the new response export is cumulative, no browser storage or separate Master file is required.

## Dashboard validation
The top-right badge shows:

`Embedded data - X joinees | Y responses`

`X` must equal the joinee count shown in Admin immediately before generation.

## Compliance logic — unchanged
| Days since Day 0 | Zone | Ideal certifications |
|---|---|---:|
| 7+ | Demo | 1 |
| 15+ | CSD | 2 |
| 22+ | Product | 3 |
| 29+ | Selling | 4 |
| 36+ | VM | 5 |
| 43+ | Billing | 6 |

For each FC × Zone:
- any Pass means certified
- repeated Pass records count as one certification
- Reassessment without a Pass means not certified

`Compliance % = Actual Certifications / Ideal Certifications × 100`

## Security
Keep `ctp_admin.html` restricted to L&D because it contains the permanent historical baseline and embedded store mapping.
