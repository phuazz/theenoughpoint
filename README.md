# Middlegame · Singapore — concept mockup

**Status: MOCKUP. Not a live site, not a product, not a claim.**

A single-page visual mockup of a Singapore personal-finance media concept — reading plus decision tools across property, investments, insurance, planning and lifestyle. It exists to look at and argue about. Nothing more.

## What is real and what is not

| | |
|---|---|
| **Real** | The layout, the type scale, the colour tokens, the component patterns. |
| **Not real** | Every figure, rating, article, sponsor and broker on the page. |

All numbers, star ratings, article headlines and dates are **illustrative placeholder content**. None has been sourced or verified. Brokers appear as "Broker A / B / C" rather than real firms, deliberately — no real product is described, ranked or rated here. The sponsored and affiliate slots are layout placeholders showing where such content *would* sit; no commercial relationship exists.

The page carries `noindex, nofollow, noarchive` and is not listed on the landing hub. It is public only because GitHub Pages has no private-serving tier below Enterprise.

**Do not add a `robots.txt` to `docs/`.** It would do nothing today — crawlers honour robots.txt only at the domain root, and `phuazz.github.io/robots.txt` does not exist. Worse, if a custom domain is ever mapped to this repo, `docs/robots.txt` *becomes* the root file and would silently apply to the whole site. The meta `noindex` is the correct control, and it works precisely because nothing blocks crawlers from fetching the page and reading it. Blocking the fetch would prevent de-indexing, not cause it.

## Brand name

`Middlegame` is a **placeholder**, chosen because `fireat40.com` is registered and a numeric name dates the brand and contradicts the intended audience. Not decided, not registered, no trade mark search done. To swap it, change:

- `<h1 id="brand">` in `template.html`
- `<title>` in `template.html`
- this file, and the repo/folder name

## Structure

```
middlegame-sg/
├── template.html     # source — the only file you edit
├── docs/index.html   # GitHub Pages output (currently a straight copy of template.html)
└── README.md
```

There is no `scripts/pipeline.py` and no `data/`. The page is static with no data feeds, so the usual inject-data-into-template build step has nothing to do. If this ever grows real data, add the pipeline then and stop hand-copying.

To update the published page:

```
cp template.html docs/index.html && git commit -am "..." && git push origin main
```

Local dev: `npx serve .` from the project root, then open `template.html`.

## Design

Styled per `C:\dev\design.md` — tokens and font link copied verbatim. The shell, masthead, tab, card and pill patterns are lifted from `money-snowball`, `sg-property-decision` and `etf-starter-sg` so those three tools drop in as sub-pages without restyling.

Two deliberate calls worth knowing before editing:

- **Document type scale, not dashboard scale.** Body copy is 16px at 1.65 line-height per the split `design.md` draws between the two. Do not inherit the 13px dashboard sizes onto prose.
- **No brand accent colour.** Green and red stay semantic across the vault, so the brand is carried by the serif masthead and the 2px rule. Blue is interactive, amber marks sponsorship, purple marks affiliate.

## Open

- Brand name undecided; domain not secured; no IPOS search done.
- Placeholder figures need sourcing before any of this is shown to a third party as fact.
- The regulatory question around the underlying concept is unresolved and out of scope for this repo.

*Last updated: 2026-07-17*
