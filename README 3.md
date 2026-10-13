# ♠ Poker Ledger

A static, read-only poker PnL tracker. The page (`index.html`) reads your games from `data.md`. There is no backend, login or sync. Only people who can commit to the repo can change the numbers.

## Files

```
poker-ledger/
├── index.html   ← the whole app (UI, charts, parser)
├── data.md      ← your games, the only file you edit
└── README.md
```

## Deploy on GitHub Pages

1. Create a public repo (e.g. `poker-ledger`) and upload `index.html` and `data.md`.
2. Optional safeguard: also create an empty file named `.nojekyll` so GitHub never processes the markdown.
3. Repo → **Settings → Pages** → Source: *Deploy from a branch* → `main` / `(root)` → Save.
4. Your site appears at `https://<username>.github.io/poker-ledger/` after about a minute.

## Adding a game

Open `data.md` on GitHub (pencil icon, works on a phone too), paste the row at the bottom of the table, commit. The site updates in about a minute.

```
| 100 | 1 | -100 | hostel |
```

Columns: `Buyin | Count | PnL | Notes`. An optional fifth column `Date` takes `2026-10` or `2026-10-14`.

## Dates

Games are numbered in file order, which is also the order of the charts. Dates are only needed for the year/month dropdowns.

- Put a heading such as `## 2025-08` above a batch of rows to date all of them until the next heading. `## Undated` switches back to no date.
- A value in the `Date` column overrides the heading for that row.
- Rows with no date show up in Lifetime and under **Undated** in the year dropdown.

Pitfall: a new row pasted under an old `## YYYY-MM` heading inherits that month. When the month changes, add a new heading or fill in the `Date` column.

## Preview locally

Opening `index.html` by double-click will not work (browsers block `fetch` on `file://`). Instead run this in the folder and open http://localhost:8000:

```
python -m http.server
```

## Customising

Everything you might want to change is in the `CONFIG` block at the top of the script in `index.html` (currency, number format, rows per page, footnote). The title is in the `<h1>` in the header.

A view can be linked directly: `#2026`, `#2026-10`, `#undated`.
