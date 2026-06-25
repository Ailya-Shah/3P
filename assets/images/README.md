# Images go here

Drop your screenshots and figures in this folder, then reference them from a
post or page.

## In a post (recommended)

Use the figure include — it handles the styling, lazy-loading, and caption:

```liquid
{% include figure.html
   src="/assets/images/airvision-overview.png"
   alt="AirVision dashboard overview"
   caption="Fig 1 — AirVision overview" %}
```

Leaving out `src` renders a labelled placeholder, which is what the posts use
right now. Add `src` when your screenshot is ready and the placeholder is
replaced automatically.

## Plain Markdown

```markdown
![AirVision dashboard]({{ "/assets/images/airvision-overview.png" | relative_url }})
```

## Naming

Keep names lowercase with hyphens, e.g. `airvision-overview.png`,
`therapy-confusion-matrix.png`. PNG or JPG for screenshots, SVG for diagrams.

`favicon.svg` in this folder is the site icon — replace it if you make your own.
