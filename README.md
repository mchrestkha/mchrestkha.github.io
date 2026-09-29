# mchrestkha.github.io

The source for [mchrestkha.github.io](https://mchrestkha.github.io) — a single
page of what I have built, spoken about and written, built with Jekyll and
served by GitHub Pages.

## Adding something

Both lists are data, not markup. Nothing below needs HTML or CSS, and nothing
on the page is counted by hand: the stat line, the training log and the list
all move with the data.

**A talk, post or session** goes at the top of [`_data/entries.yml`](_data/entries.yml):

```yaml
- date: 2026-01-15
  kind: talk                        # talk, post or project
  title: "The title, as it reads at the source"
  with: ["Some Company", "Another"] # optional: who shared the stage
  note: "Cloud Next '26"            # optional: any other context line
  links:
    - label: Video
      url: https://example.com/watch
    - label: Slides
      url: https://example.com/slides.pdf
```

`date` drives the displayed date, the year heading and the entry's square in
the training log, so the page stays sorted and grouped by construction — there
is no year heading to add by hand. `kind` sets the entry's colour and shape and
which count it adds to; a `project` is listed but kept out of the log. Each
company in `with` is joined into "with A, B & C" under the title and counted
once in "Companies on stage with me".

**An app** goes in [`_data/apps.yml`](_data/apps.yml), same idea: `name`, `url`,
an optional one-line `tagline`, a `description`, `released` (the day the first
version went live, which places it in the training log) and `cover` (an SVG in
`_includes/covers/`, drawn as the record sleeve — use the app's own tab icon).

**The intro, the "In rotation" line and the other one-liners** are in
[`index.md`](index.md): the intro is its body, the rest is front matter.

**Session screenshots and other images** go in `images/` and are linked as
`/images/name.png`, served from this site rather than from
`raw.githubusercontent.com` — a raw link is pinned to a branch and file name and
breaks quietly when either changes.

## How it is put together

```
_config.yml             title, tagline, the social links, SEO and plugin config
_data/entries.yml       talks, writing and sessions
_data/apps.yml          things I have built
_includes/covers/       each app's mark, inlined as its record sleeve
_includes/and-list.html joins a list as "A, B & C"
_layouts/default.html   the page shell: <head>, footer, and all the CSS
_layouts/home.html      the stage, side projects and the list, from the data
index.md                front matter and the intro
404.html
assets/fonts/           Archivo, subset, with its licence
assets/favicon.svg      with a favicon.png fallback for older browsers
images/og.png           the link-preview card
```

There is no theme gem. The whole design is inlined in `_layouts/default.html`,
which keeps the page to one request for HTML and CSS: at this size a separate
stylesheet costs a round trip and saves nothing.

## The design

The page opens on **the stage**, one dark band whose ground is the page's own
ink: the name, the intro, the five things in rotation, the training log and
the stat line. Everything below it is the light page. It is a fixed part of
the design, not a dark mode — everyone sees the same page.

**The training log** is one square per month, the way a running log or a
contribution graph is, with a mark in every month that had a talk, a post or
an app. It is a real `<table>` (years are row headers, months column headers),
every mark links to its entry in the list, and every mark's tooltip is also its
accessible name, so hover, keyboard focus and a screen reader all get the same
words. The list underneath is the log's table view: nothing is only in the
chart. On load the marks light up in the order they happened, unless the
reader has asked for reduced motion.

**Colour is measured, not eyeballed.** Colour, type and space are tokens at
the top of the `<style>` block, and nothing below it invents a value. Text
clears WCAG AA (4.5:1) everywhere and body copy clears AAA (7:1), on every
surface it sits on. The three mark colours — talk, post, app — take their hues
from the two apps (Sound Travels' red and gold, Hip Hop Lineage's A-train
blue), stepped once for the light page and once for the stage, and each set is
validated as a categorical palette over all pairs: lightness band, chroma,
colour-blind separation (worst protan/deutan ΔE 12.7 on the page, 9.9 on the
stage, against a target of 8), normal-vision separation, and 3:1 contrast
against its surface. Colour is never the only cue either: talks are circles,
posts are squares, apps are ▶.

**Type.** Body text is the system font. Headings and figures are Archivo — the
face Sound Travels and Hip Hop Lineage set their titles in, so the three read
as one body of work — self-hosted, subset to Basic Latin and pinned to a single
width with only weight left variable: 11.8 KB, down from 90 KB for the family.

**No JavaScript** beyond analytics. The tooltips and the Talks/Posts filter are
CSS: the filter is three radio buttons and `:has()`, and a browser without
`:has()` simply shows everything.

**Images are most of the performance budget**, so they are sized for where they
are displayed: the avatar is served at 144px for a 72px slot (WebP, ~4 KB)
rather than at its original 837px (699 KB). A first visit downloads about
28 KB in all — 12 KB of compressed HTML and CSS, the avatar and the font. If
you add a screenshot, resize it first.

**The link-preview card** (`images/og.png`, 1200 × 630) is a screenshot of the
stage, rendered from the built page, reduced to 256 colours (39 KB). Its
numbers are a picture, so retake it when the stat line changes.

## Local preview

```bash
bundle install
bundle exec jekyll serve   # http://127.0.0.1:4000
```

GitHub Pages builds this on push to `master` with its own pinned toolchain —
Jekyll 3.10 and Liquid 4.0 at the time of writing — and does not read the
`Gemfile`, which is only so the same thing can be rendered locally first. The
templates are written for both: they avoid anything newer than Liquid 4.0,
such as the `find` filter, and both versions render the same page.

## Analytics

Google Analytics 4, measurement ID `G-FEVFDVT16F`, set as `google_analytics` in
`_config.yml`. `_layouts/default.html` adds the `gtag.js` snippet only in a
production build, which is what GitHub Pages runs, so `jekyll serve` previews
send nothing. The same property also collects from the
[Hip Hop Lineage](https://mchrestkha.github.io/hiphoplineage/) and
[Sound Travels](https://mchrestkha.github.io/soundtravels/) projects under this
domain; filter reports by page path to separate them.

## Credit

The page this replaced was forked from
[evanca/quick-portfolio](https://github.com/evanca/quick-portfolio). Archivo is
by Omnibus-Type, under the SIL Open Font License.
