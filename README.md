# NZ NMV Database — Deployment & Update Guide

## File structure

```
nz-nmv-database/
├── index.html              ← Website (never edit to update data)
├── README.md               ← This guide
└── data/
    ├── nmv_studies.json    ← NMV study records (EDIT THIS to update)
    └── es_values.json      ← ES value records  (EDIT THIS to update)
```

---

## How to update data (most important)

**The website reads all data from the two JSON files. You never need to touch index.html to update content.**

### Adding a new NMV study

Open `data/nmv_studies.json` and add a new object to the `"studies"` array:

```json
{
  "id": "NMV011",
  "author": "Smith, J. and Jones, A.",
  "year": 2022,
  "region": "Waikato",
  "topic": "Freshwater quality in Lake Taupo catchment",
  "method": "CE",
  "ecosystem_type": "Freshwater",
  "es_category": "Regulating",
  "es_subtype": "Water purification",
  "wtp_value": "$55/household/year",
  "wtp_unit": "NZ$/household/year",
  "std_value_nzd_ha_yr": 450,
  "sample_n": 580,
  "model_spec": "Mixed logit — panel data",
  "full_citation": "Smith, J. and Jones, A. (2022) ...",
  "agency": "Waikato Regional Council",
  "benefit_transfer_notes": "...",
  "free_access": true
}
```

Then update `"last_updated"` in `_metadata` at the top of the file.

### Valid field values

| Field | Valid values |
|-------|-------------|
| `method` | `"CVM"` `"TCM"` `"CE"` `"HP"` |
| `ecosystem_type` | `"Freshwater"` `"Wetland"` `"Grassland"` `"Forest"` `"Coastal"` `"Marine"` |
| `es_category` | `"Regulating"` `"Cultural"` `"Provisioning"` `"Supporting"` |
| `region` | Any NZ region name |
| `std_value_nzd_ha_yr` | Number, or `null` if not applicable |

---

## How to deploy (step by step)

### Option A — Netlify (recommended, free tier)

1. Go to [netlify.com](https://netlify.com) and create a free account
2. Drag the entire `nz-nmv-database` folder onto the Netlify dashboard
3. Your site is live at a URL like `https://amazing-name-123.netlify.app`
4. To update: drag the folder again, or connect to GitHub for automatic updates

### Option B — GitHub + Netlify (best for regular updates)

1. Create a free [GitHub](https://github.com) account
2. Create a new repository called `nz-nmv-database`
3. Upload all files to the repository
4. Connect Netlify to the GitHub repository
5. Every time you push changes to GitHub, the site updates automatically within 30 seconds

To update data with GitHub:
- Edit `data/nmv_studies.json` or `data/es_values.json` directly in GitHub's web editor
- Click "Commit changes" — site updates automatically

### Option C — Custom domain

After deploying to Netlify:
1. Buy a domain (e.g. `nznmvdatabase.nz`) from Domainz or Snap
2. In Netlify settings → Domain management → Add custom domain
3. Follow DNS instructions — takes 24 hours to propagate

---

## Testing locally

Because the site loads JSON files via `fetch()`, you need a local server to test it:

```bash
# If you have Node.js:
npx serve .

# If you have Python:
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

---

## Adding subscription/payment (Stripe)

When ready to take payments:

1. Create a [Stripe](https://stripe.com/nz) account
2. Use [Memberstack](https://memberstack.com) or [Outseta](https://outseta.com) — both integrate with plain HTML sites and Stripe
3. Wrap paid content in their gating system
4. No code changes to index.html needed — they handle login and access control

Monthly suggested pricing: NZ$49/month or NZ$399/year

---

## Suggested update schedule

| When | What to do |
|------|-----------|
| New study published | Add to `nmv_studies.json`, update `last_updated` |
| Annual | Review ES standardised values, update NZ$/ha/yr if needed |
| NZ ETS carbon price changes | Update ES008 and ES011 carbon sequestration values |
| New regional council report | Check for new benefit transfer opportunities |
