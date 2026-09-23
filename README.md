# mroops.dev

A personal index, served from GitHub Pages at [mroops.dev](https://mroops.dev). Two pages, one
stylesheet, no build step and no framework.

## Design Principles

The page is an index rather than a landing page, and every decision below follows from that.

- **Index over Pitch**: visitors arrive from a repository README or a GitHub profile, so they
  already know one of the projects. There is no hero section, no greeting and no scroll animation,
  because none of those are needed on someone who is already here.
- **Typography over Decoration**: the page carries one diagram and nothing else visual. Everything
  else is type, rule and space.
- **Evidence over Claims**: project descriptions state behaviour a reader could go and verify. There
  is no skills list, because every engineer's list looks the same and none of them are falsifiable.
- **Nothing at Runtime**: no CSS framework, no JavaScript framework and no third-party request. The
  typeface is served from this repository.

## Typography

One family does the work, chosen as a contemporary text serif rather than a classical one, so the
page reads as considered without reading as retro.

- **Source Serif 4**: a variable face covering weight 300 to 700, self-hosted under `fonts/`
- **Two Subsets**: `latin` loads always, `latin-ext` only when a character in its range appears
- **System Sans for Interface**: section labels, dates and the language switch, so an interface
  control never reads as part of the text
- **CJK Fallbacks**: the Latin face still leads on the Chinese page, and the fallback chain prefers a
  modern Chinese serif before landing on a modern sans, never on MingLiU

## Colour

Warm paper and warm ink rather than pure white and pure black. The difference is small in any single
token and decisive in aggregate.

- **One Accent**: it appears only on link underlines, which is the one place on the page where
  colour does work that nothing else can do
- **Same Hue in Both Themes**: chroma rises and lightness inverts between light and dark, but the
  hue numbers do not move, because a dark mode that shifts hue reads as a different site
- **Contrast**: every text pair clears WCAG AA, checked by converting oklch to linear sRGB and
  computing the ratio rather than by eye

## Theme

Left alone the page follows the operating system. The button in the masthead pins it, and the choice
survives a reload.

- **Three States**: system default, pinned light and pinned dark
- **One Definition per Token**: `light-dark()` holds both values in a single declaration, so no
  colour is ever written twice
- **No Flash**: a small script in `<head>` applies the stored choice before first paint

## Layout

The page spans 80 percent of the viewport, and that width goes into structure rather than into
longer lines.

- **Two Columns**: section labels sit in a left gutter, content runs in the right column
- **One Column below 52rem**: the gutter has nowhere to go at that width, so labels move above their
  sections and the side padding returns
- **Diagram in Markup**: the relationship between the three projects is drawn in HTML and CSS, so it
  inherits the type, follows the theme and reflows on a phone

## Motion

There is exactly one animation on the site.

- **Alternating Portrait**: two images cross-fade on a nine second cycle
- **Reduced Motion**: a looping animation is what `prefers-reduced-motion` exists to stop, so the
  second portrait simply never appears rather than appearing without the fade

## Tokens

Seventeen custom properties hold every value the page reuses. Changing the look means changing one
of these, never hunting for a hardcoded value.

| Group | Tokens |
| --- | --- |
| Colour | `paper`, `paper-sunk`, `ink`, `ink-soft`, `rule`, `accent` |
| Layout | `page`, `page-max`, `gutter`, `step` |
| Type | `t-meta`, `t-small`, `t-body`, `t-title`, `t-name` |
| Assets | `avatar`, `avatar-alt` |

The two asset tokens are the reason a portrait can move to external hosting without touching markup.

## Files

Nothing here is generated, so the repository is the deployed site byte for byte.

```
index.html     English
zh.html        Traditional Chinese
style.css      tokens and every rule
favicon.svg    follows the system theme
fonts/         Source Serif 4, latin and latin-ext subsets
CNAME          mroops.dev
```

## Local Preview

Any static server works, since there is nothing to build.

```bash
python3 -m http.server 4173
```

Then open `http://127.0.0.1:4173`. That server sends no cache headers, so a stylesheet edit needs a
hard reload with `Cmd+Shift+R` before it shows.

## Deployment

GitHub Pages serves `master` at the repository root, and a push is the whole deployment.

- **Custom Domain**: `CNAME` holds `mroops.dev`, and `mroops0111.github.io` redirects to it
- **HTTPS Is Mandatory**: `.dev` sits in the HSTS preload list, so Enforce HTTPS has to stay on
- **Cloudflare Proxy Stays Off**: the orange cloud blocks GitHub's certificate validation
