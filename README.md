# Decision Cards

A single-file, local-first tool for making hard choices easier. List your options as cards, add weighted pros and cons to each, and Decision Cards ranks them for you — no build step, no backend, no account. Open `index.html` and start deciding.

![No dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)
![Offline first](https://img.shields.io/badge/offline-first-blue)
![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey)

## Why

Most "pros and cons" lists treat every point as equally important, which quietly biases the decision toward whichever option you happened to list more reasons for. Decision Cards fixes that by letting you **weight** each pro and con, then computing a single comparable score per option.

## Features

- **Weighted pros/cons per option** — every point carries a numeric weight, not just a checkmark
- **Automatic ranking** across all your options, live as you type
- **Search & sort** by rank, net score, pros score, cons score, or title
- **Local-first storage** — everything is saved to `localStorage` in your browser; nothing leaves your machine
- **Corruption-safe** — if stored data can't be read, it's quarantined (never silently erased) and you're offered recovery via import
- **Backup on import** — importing data automatically snapshots what you had before, with one-click restore
- **Export** to JSON (full backup), CSV (spreadsheet-ready), or a standalone HTML report (shareable, no app needed)
- **Zero dependencies** — one HTML file, vanilla JS, no build tooling, no CDN calls, no tracking

## Getting started

Download `index.html` and open it in any modern browser. That's it. To host it, drop it on GitHub Pages, Netlify, or any static file server — no server-side logic required.

## The mathematical model

Decision Cards implements a simplified **weighted additive scoring model** (the same family as Multi-Attribute Utility Theory / weighted decision matrices, reduced to two attribute classes: *supporting* and *opposing* factors).

### Definitions

For an option (card) $c$ with:
- a set of pros $P_c = \{p_1, p_2, \dots, p_n\}$, each with weight $w(p_i)$
- a set of cons $C_c = \{q_1, q_2, \dots, q_m\}$, each with weight $w(q_j)$

the tool computes three scores:

```
Pros Score(c) = Σ w(p_i)      for i = 1..n
Cons Score(c) = Σ w(q_j)      for j = 1..m
Net Score(c)  = Pros Score(c) − Cons Score(c)
```

By default every new pro/con is created with weight `1`, so an unweighted card behaves exactly like a classic tally (count of pros minus count of cons). Adjusting a weight up or down lets a single point outweigh several trivial ones — e.g. a con weighted `5` overrides three pros weighted `1` each.

Weights accept any real number, including decimals and negative values. A non-numeric or empty weight is treated as `0` (it's counted but contributes nothing), so incomplete entries never break a comparison.

### Ranking

Options are ranked by **Net Score, descending**, using **standard competition ranking** ("1224" ranking): tied scores receive the same rank, and the next distinct score skips ahead by the number of tied entries. For example, three options tied for the best net score are all ranked `#1`, and the next option is ranked `#4`, not `#2`.

```
rank(c) = 1 + |{ c' ∈ cards : NetScore(c') > NetScore(c) }|
```

This avoids the misleading impression of a false tie-break that plain "next integer" ranking would otherwise introduce.

### Why this model, and its limits

A weighted additive model is transparent, easy to audit, and fast to update as new information arrives — you can see exactly why one option outranks another by comparing individual weighted items. Its known limitation, shared by all linear weighted-sum methods, is that it assumes pros and cons are **independent and commensurable** on a single scale: it can't natively express that two factors interact (e.g. "cost only matters if timeline also slips") or that one factor is a hard constraint rather than a tradeable weight. For most everyday decisions — job offers, apartments, vendors, purchases — that simplicity is a feature, not a bug.

## Data format

Exported/imported JSON follows this schema:

```json
{
  "version": 1,
  "cards": [
    {
      "id": "card_...",
      "title": "Option A",
      "description": "Optional notes",
      "pros": [{ "id": "item_...", "text": "Cheaper", "weight": 2 }],
      "cons": [{ "id": "item_...", "text": "Slower", "weight": 1 }]
    }
  ]
}
```

Import is validated against this schema before anything is written to storage, so a malformed file is rejected with an explanation rather than corrupting your data.

## Privacy

Decision Cards makes no network requests. All data lives in your browser's `localStorage` under the `decisionCardsData` key, scoped to whatever origin you open the file from. Clearing your browser data for that origin removes it. Exports you trigger are the only way data leaves the browser.

## Browser support

Any evergreen browser (Chrome, Firefox, Safari, Edge). Requires `localStorage` and standard ES5+ JavaScript — no build step or polyfills needed.

## Contributing

Issues and pull requests are welcome. Since this is a single static file, most changes are a straightforward edit to `index.html` — please keep it dependency-free and self-contained.

## License

MIT — see [LICENSE](LICENSE).
