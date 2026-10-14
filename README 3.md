# ♠ Poker Stats

A small, fast dashboard for my poker results: cumulative PnL, ROI, streaks, drawdown and a full session log, filterable by year, month and venue.

**Live:** [heyitssupro.github.io/poker-ledger](https://heyitssupro.github.io/poker-ledger/)

<!-- Add a screenshot named screenshot.png to the repo, then remove this line's comment markers:
![Poker Stats dashboard](screenshot.png)
-->

## What it shows

- **Two charts:** cumulative PnL and cumulative ROI, both plotted against session number. Losses are shaded red, profits green, and the line turns red wherever the curve is falling.
- **Eight stat cards:** total sessions, total buy-in, net PnL, overall ROI, win rate, longest win streak, best session and max drawdown.
- **Year and month tabs:** Lifetime, then each year. Picking a year reveals a month row underneath. Every view has a shareable link, for example `#2026` or `#2026-04`.
- **Venue breakdown:** sessions, buy-in, PnL, ROI and win rate per venue. Clicking a venue filters the charts, cards and log together.
- **Session log:** every session, newest first, with the total buy-in and PnL for the current view at the bottom.

## How it works

It is a single static page with no framework, build step, backend or login.

1. `data.md` holds the raw results as a Markdown table.
2. When the page loads it fetches `data.md`, parses the rows in the browser and computes every statistic on the spot.
3. Charts are drawn as inline SVG.

Visitors can only read. Nothing on the page can edit the data, so the numbers change only when `data.md` is committed to the repo. Because the repo is public, `data.md` is readable by anyone.

```
poker-ledger/
├── index.html   # the whole app: layout, styles, parser, stats, charts
├── data.md      # the results (the only file that changes)
└── README.md
```

## Data format

Each session is one row of the table in `data.md`:

```
| Buyin | Count | PnL  | Notes          | Date    |
| ----- | ----- | ---- | -------------- | ------- |
| 100   | 2     | -200 | ISI Tournament | 2026-04 |
| 100   | 1     | -100 | hostel         |         |
```

| Column | Meaning |
| --- | --- |
| `Buyin` | Amount of one buy-in |
| `Count` | Number of buy-ins in that session (rebuys included) |
| `PnL` | Net result of the session. `+11`, `-100`, `1,200` and `₹500` all work |
| `Notes` | Venue name. Rows with the same name (ignoring capitalisation) are grouped together |
| `Date` | Optional month, such as `2026-04`, `2026-04-14` or `Apr 2026` |

New sessions go at the bottom. Rows are numbered in file order, and that order is also the order of the charts.

**Dates carry downward.** A row with a date dates itself and every row below it, until the next row that has a date. So if row 1 is `2026-04` and row 10 is `2026-05`, rows 1 to 9 are April and row 10 onwards is May. Rows above the first dated row appear under an **Undated** tab. A heading such as `## 2026-04` above a batch of rows works the same way.

## How the numbers are defined

| Stat | Definition |
| --- | --- |
| Total buy-in | Sum of `Buyin × Count` over the sessions shown |
| Net PnL | Sum of `PnL` |
| Overall ROI | Net PnL ÷ total buy-in. Larger buy-ins weigh more |
| Avg ROI / session | Plain mean of each session's own ROI. It differs from overall ROI whenever buy-in sizes differ |
| Win rate | Share of sessions with PnL above zero. Break-even sessions are counted separately |
| Longest win streak | Most consecutive sessions with PnL above zero, plus the current streak |
| Max drawdown | Largest peak-to-trough fall of the cumulative PnL curve in the current view, starting from zero |
| Longest losing streak | Most consecutive sessions with PnL below zero |

All stats follow the current filter, so a month view shows that month's drawdown and streaks, not the lifetime ones.

## Run locally

Opening `index.html` directly will not work, because browsers block reading local files with `fetch`. Serve the folder instead:

```
python -m http.server
```

Then open http://localhost:8000.

## Settings

The `CONFIG` block at the top of the script in `index.html` controls:

| Setting | Effect |
| --- | --- |
| `dataFile` | Name of the data file |
| `siteUrl` | Where the name in the header and footer links to |
| `currency`, `locale` | Currency symbol and number grouping (`₹`, lakh/crore style) |
| `pageSize` | Rows shown in the session log before "Show all" |
| `redDownLine` | Red and green line segments on the charts. Set to `false` for a single-colour line |

## Built with

Plain HTML, CSS and JavaScript. No dependencies.

---

Made by [Supratik Biswas](https://heyitssupro.github.io)
