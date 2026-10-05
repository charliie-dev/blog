# blog

Source of [blog.charliie.dev](https://blog.charliie.dev): notes on Linux, Logseq, Vim, Python,
machine learning and food, built with [Hugo](https://gohugo.io/) and the
[Hextra](https://github.com/imfing/hextra) theme in Catppuccin colours.

## Local development

[mise](https://mise.jdx.dev/) installs the pinned Hugo from `mise.toml`, and Go from the
`toolchain` line in `go.mod`. Go is only needed to fetch Hextra as a Hugo module.

```sh
mise run serve   # drafts and live reload on http://localhost:1313
mise run build   # production build into public/
```

The dev server does not always pick up new templates, data files or taxonomy terms. Restart it
if a new tag or partial does not show up.

## Layout

| Path                    | Holds                                                                    |
| ----------------------- | ------------------------------------------------------------------------ |
| `content/_index.md`     | The home page: intro, Explore cards and contact links                    |
| `content/page/`         | Blog posts; `content/page/Logseq/` holds the Logseq series               |
| `content/foodie/`       | Food reviews                                                             |
| `content/dj/`           | The Disc Jockey section                                                  |
| `static/images/`        | Images for posts, one folder per topic, plus the logo                    |
| `assets/css/custom.css` | The Catppuccin palette (Latte light, Mocha dark) and theme overrides     |
| `layouts/_partials/`    | Overrides of Hextra partials: backlinks, tags, dates and the page footer |
| `layouts/shortcodes/`   | Shortcodes left over from the Logseq export                              |
| `data/icons.yaml`       | Custom icons used by the home page cards (disc, ramen, Ko-fi)            |
| `i18n/en.yaml`          | The footer copyright line                                                |
| `hugo.yaml`             | Site configuration: the Hextra module, menus, taxonomies and math        |

## Writing a post

Posts follow a few conventions:

- **Title** reads `Topic | Subject`, such as `Logseq | Queries`.
- **`description`** is a sentence or two that appears in the post list.
- **`tags`** is the only taxonomy; there are no categories.
- **`date`** is when the underlying note was written; **`lastMod`** is the last edit. Give a time
  zone when the date is today, such as `2026-10-06T00:00:00+08:00`, or Hugo treats the post as
  scheduled in the future and skips it.
- **Sources** go into footnotes at the end, with link text in the form `Title - section`.
- **Math** renders with KaTeX: `\( … \)` inline and `$$ … $$` or `\[ … \]` for blocks. A plain `$`
  stays a dollar sign.
- **Long code** can be folded with Hextra's `details` shortcode.

## Deployment

Every push to `main` runs `.github/workflows/publish.yml`: mise sets up the same Hugo and Go, Hugo
builds the site, and the result is pushed to the `gh-pages` branch, which GitHub Pages serves at
the domain in `static/CNAME`. Dependabot keeps the GitHub Actions up to date every week.
