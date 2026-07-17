# The Enough Point · Singapore — concept mockup

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

**The Enough Point**, chosen 2026-07-17 over the `Middlegame` placeholder and over `FireAt40` (whose `.com` is registered, and whose numeric name dates the brand and contradicts the intended audience of early-30s to 50s).

The name and the product landed on each other by accident, which is a point in its favour: the hero calculator was already called "the enough point" before the name was picked. The site's hallmark tool *is* the brand. The hero tool is now labelled "Your enough point" to avoid reading as a tautology against the wordmark.

**Still open:**

- **TLD undecided.** As of 2026-07-17 both `theenoughpoint.com` and `theenoughpoint.sg` return no nameservers, which is a proxy for unregistered, **not proof** — verify at a registrar before relying on either. `.com` pairs naturally because the name is not geographic; `.sg` would signal the Singapore-specificity that is the actual moat. The wordmark deliberately carries no TLD so it does not commit.
- **Nothing registered.** No domain, no IPOS trade mark search in classes 36 and 41. "The enough point" is a coined phrase rather than a description of financial services, so it is a plausible mark — but plausible is not searched.

To swap the name again, change `<a id="brand">` and `<title>` in `template.html`, this file, and the repo/folder name.

## Structure

```
theenoughpoint/
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

Colour tokens and the font link are copied verbatim from `C:\dev\design.md`. Nothing in the palette is forked.

### The third scale

`design.md` defines two type scales: **dashboard** (11px labels, 13px body, dense on purpose) and **document** (16px body, 1.65 line-height, read line by line). A landing page is neither. Its job is persuasion, and applying the dashboard scale to it — which the first version of this page did — produces something that looks like an internal tool rather than a destination.

So this page introduces a third scale, declared in `:root` under a `LANDING` comment and **not yet codified in `design.md`**. It is not invented; it is measured from computed styles on Linear, Stripe, Ramp, Mercury, Wise, FT, NYT and the Economist in July 2026:

| Value | Here | Measured source |
|---|---|---|
| Section padding | 96px (72 / 56 at breakpoints) | Linear 128, Stripe 96, Ramp 64 — dashboards use ~20 |
| Display | `clamp(40px, 5.6vw, 66px)`, lh 1.02, weight 400 | Modal is 64px at lh 1.0, weight 300–510 |
| Body | 17px | Consumer fintech 17–18; dev tools 15 |
| Prose measure | 62ch | Linear measures ~66ch |
| Lead : secondary | 40px : 20px = 2.0× | FT's exact broadsheet ratio |

**If this concept proceeds, that block belongs in `design.md` as a landing scale before any second page is built.** Right now it is a justified one-off; a second page copying it would be a silent fork.

### Rules that carried over from the research

- **Editorial uses hairline rules, never cards.** FT, NYT and the Economist are unanimous, and the Economist explicitly ships `box-shadow: none`. Cards on an article list are the single strongest dashboard tell. Tools are a *product* surface, so cards are correct there — the split is deliberate.
- **Hierarchy comes from demoting secondaries, not promoting the lead.** The lead inherits the default size; `.sec-item h4` is the rule that shrinks. This is FT's own mechanism and it is drift-proof — a new story defaults to prominent, so you never hunt for a missing modifier.
- **Display type is never bold.** The measured range is weight 300–510. Bold display at 64px is the cheapest-looking thing you can do. Instrument Serif at 400 sits correctly in that band.
- **Sponsored content is marked by typography and whitespace, not a coloured box.** A hairline sandwich, a sans label where editorial is serif, true small-caps (`text-transform:lowercase` + `font-variant:small-caps`), and the advertiser named before the headline. The first version used a tinted amber box; that is the cheap tell.
- **No entrance motion on hero type.** Linear and Stripe both ship `animation-name: none` on the h1. `prefers-reduced-motion` is honoured anyway.
- **No brand accent colour.** Green and red stay semantic across the vault. The brand is carried by the serif and the whitespace. Blue is interactive, amber is caution, purple marks affiliate.

### The hero calculator

The hero leads with a working calculator rather than a screenshot. This is the **Wise pattern** — and it is worth knowing that it is *not* category consensus. Of eight world-class sites surveyed, only Wise puts a live tool near the fold, and only on its dedicated converter page. The pattern applies when the tool *is* the product, which is the whole premise here.

The mechanics that make it work, copied deliberately:

- **Pre-computed on first paint.** There is no empty state and no Calculate button. The answer exists before the user does anything; their only job is to change a number they can already see.
- **Money fields are `type="text"` + `inputmode="numeric"`**, regrouped on blur. Not a style choice — the HTML spec requires `type="number"` to hold a valid floating-point number, so a comma makes the value invalid and the browser silently discards it. Wise does exactly this, independently corroborating the `design.md` grouping rule.
- **Every read goes through `numOf()`.** `+"90,000"` is `NaN`.

The maths: months until the pot reaches 25× annual spending, at a 4% real return, solved by stepping rather than logs so the zero-payment and zero-rate cases stay sane. Verified against the closed-form future-value formula on both sides of the boundary (month 165 = S$2,242,288, below target; month 166 = S$2,256,129, at target). The 4% return and 4% withdrawal assumptions are stated on the page itself, because they are contestable and doing otherwise would be dishonest.

## Open

- Brand name undecided; domain not secured; no IPOS search done.
- Placeholder figures need sourcing before any of this is shown to a third party as fact.
- The regulatory question around the underlying concept is unresolved and out of scope for this repo.

*Last updated: 2026-07-17*
