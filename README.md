# mchrestkha.github.io

The source for [mchrestkha.github.io](https://mchrestkha.github.io) — a single
page of what I have built, spoken about and written, built with Jekyll and
served by GitHub Pages.

## Adding something

Both lists are data, not markup. Nothing below needs HTML or CSS.

**A talk, post or session** goes at the top of [`_data/entries.yml`](_data/entries.yml):

```yaml
- date: 2026-01-15
  title: "The title, as it reads at the source"
  note: "with Some Company"        # optional context line; omit if there is none
  links:
    - label: Video
      url: https://example.com/watch
    - label: Slides
      url: https://example.com/slides.pdf
```

`date` drives both the displayed date and the year heading, so the page stays
sorted and grouped by construction — there is no year heading to add by hand.

**An app** goes in [`_data/apps.yml`](_data/apps.yml), same idea: `name`, `url`,
an optional one-line `tagline`, and a `description`.

**Session screenshots and other images** go in `images/` and are linked as
`/images/name.png`, served from this site rather than from
`raw.githubusercontent.com` — a raw link is pinned to a branch and file name and
breaks quietly when either changes.

## How it is put together

```
_config.yml           title, tagline, the social links, SEO and plugin config
_data/entries.yml     talks, writing and sessions
_data/apps.yml        things I have built
_layouts/default.html the page shell: <head>, masthead, footer, and all the CSS
_layouts/home.html    renders the two lists on top of the shell
index.md              front matter and the intro paragraphs
404.html
assets/favicon.svg    with a favicon.png fallback for older browsers
images/
```

There is no theme gem. The whole design is about 200 lines of CSS inlined in
`_layouts/default.html`, which is what keeps the page to a single request: at
this size a separate stylesheet costs a round trip and saves nothing.

Colour, type and space are tokens at the top of that `<style>` block, and
nothing below it invents a value. Both palettes are contrast-checked rather than
eyeballed — body text clears WCAG AAA (7:1) and muted text and links clear AA
(4.5:1) on every surface they sit on, in light and dark.

**Images are the whole performance budget**, so they are sized for where they
are displayed: the avatar is served at 144px for a 72px slot (WebP, ~4 KB) rather
than at its original 837px (699 KB). The page as a whole is around 10 KB over the
wire. If you add a screenshot, resize it first.

## Local preview

```bash
bundle install
bundle exec jekyll serve   # http://127.0.0.1:4000
```

GitHub Pages builds this on push to `master`; the `Gemfile` is only so the same
thing can be rendered locally first.

## Analytics

None. The old Universal Analytics property stopped collecting when UA shut down
in July 2023, so the dead snippet was removed rather than left in looking like it
worked. To add GA4: put the `G-` id in `_config.yml` as `google_analytics` and add
the `gtag.js` snippet to `_layouts/default.html` — the old UA snippet does not
work with a `G-` id.

## Credit

The page this replaced was forked from
[evanca/quick-portfolio](https://github.com/evanca/quick-portfolio).
