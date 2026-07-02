# BEPBO Sales Dashboard

Live, static dashboard for BEPBO (Quán Bò Xèo) monthly sales. Hosted on GitHub
Pages at **report.somtamzaap.vn**.

- `index.html` — fixed dashboard shell (layout, tabs, charts, CSS/JS). Built
  once; do not edit per month.
- `data/manifest.json` — list of available months, powers the month dropdown.
- `data/YYYY-MM.json` — one file per month with all KPIs, chart data, and
  pre-written Vietnamese insight text.

The site does zero AI/analysis work. All numbers and insight text are
computed locally via the `bb-sales-report` Claude skill and simply pasted
into a new `data/YYYY-MM.json` file here.

## Monthly update procedure

1. Upload the new month's POS export to Claude and run the `bb-sales-report`
   skill as usual to get the analysis.
2. Ask Claude (chat or Claude Code) to convert that month's output into the
   `data/YYYY-MM.json` schema — use any existing file in `data/` as the
   reference shape (all 4 seed months share the same keys).
3. Add the new file and update the dropdown list:
   ```bash
   cp new-month.json data/2026-07.json
   ```
   Edit `data/manifest.json`:
   - Add `{ "key": "2026-07", "label": "Tháng 7 · 2026" }` to `months`.
   - Update `"default"` to `"2026-07"`.
4. Commit and push:
   ```bash
   git add data/2026-07.json data/manifest.json
   git commit -m "Add Tháng 7 2026 data"
   git push
   ```
5. GitHub Pages redeploys automatically (usually under a minute). Refresh
   `report.somtamzaap.vn` and confirm the new month appears in the dropdown.

No other files need to change for a routine monthly update — `index.html`
only changes if the dashboard's layout/charts/design system changes.

## DNS setup (one-time)

At your domain registrar for `somtamzaap.vn`, add:

| Type  | Name     | Value                     |
|-------|----------|---------------------------|
| CNAME | `report` | `tiendat281094.github.io` |

GitHub Pages reads the `CNAME` file committed in this repo
(`report.somtamzaap.vn`) to know which custom domain to serve. Once the DNS
record propagates, GitHub auto-provisions HTTPS for the domain (can take up
to ~24h, usually much faster).
