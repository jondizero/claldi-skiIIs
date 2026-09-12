# Template Library — Master Funnel Pages

Reference templates for the outreach-asset / audit-prospect pipeline. Each
prospect's assets start as a copy of one of these, then get the identity +
copy swapped in.

## Files

| File | Type | When to use |
|---|---|---|
| `webinar-master.html` | Short registration form (3 fields) | Top-of-funnel capture for a live/replay webinar |
| `webinar-master-2.html` | Dark/aqua registration page with live countdown timer | Alt. style for webinar capture — more urgency-driven |
| `vsl-master.html` | VSL + application form | Mid-funnel — qualifies fit before booking a call |
| `lowticket-master.html` | Long-form sales page, tokenized | Self-serve low-ticket offer, no call needed |

## Swap list — what to change per prospect

**webinar-master.html / vsl-master.html**
- Brand name, logo placeholder, color variables in `:root` (currently teal/cream
  for webinar, blue/red for VSL)
- Headline, subheadline, credibility line
- Form destination / success message copy

**lowticket-master.html**
- Every `{{TOKEN}}` placeholder — these mark exactly what needs prospect-specific
  copy (promise, proof stats, module titles, bonuses, guarantee, FAQ, etc.)
- `:root` CSS variables — primary/accent color, ink, paper, gold
- Repeat the commented `.module` block once per curriculum module, and the
  `.bonus` block once per bonus offered (structure supports any count)

## Design pattern

Both webinar and VSL use inline CSS custom properties (`:root{--ink:...}`) as
the identity layer — swapping those + the Google Fonts link is most of the
brand reskin. `lowticket-master.html` follows the same pattern plus explicit
`{{TOKEN}}` placeholders for copy, since that page has far more prospect-specific
claims (curriculum, bonuses, testimonials) than the shorter funnel pages.

## Note on the low-ticket template

This structure was generalized from studying a real long-form sales page
(hero+VSL → price → curriculum → bonus stack → comparison table → guarantee
→ authority → FAQ → final CTA countdown). The layout pattern is reusable;
the copy is intentionally all placeholder tokens rather than that page's
specific claims, since those numbers/testimonials belong to that business.
