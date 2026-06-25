---
layout: post
title: "AirVision 2026: Turning Satellite Data into a Cleaner Story"
date: 2026-06-14
disc: data
excerpt: "AirVision started as a simple question: can satellite and reanalysis data make air quality easier to understand across multiple cities, instead of leaving it locked in raw numbers?"
---

AirVision started as a simple question: can satellite and reanalysis data make
air quality easier to understand across multiple cities, instead of leaving it
locked in raw numbers?

The honest answer most dashboards give you is a wall of pollutant readings with
no story attached. I wanted the opposite — something where you could open a city,
see how it breathes over a year, and actually feel the difference between a good
day and a bad one.

[**Try the live app →**](https://airvision-2026-esbig4eq22q7shndytr2si.streamlit.app/)

{% include figure.html caption="AirVision — overview dashboard (screenshot to be added)." %}

## The data

The project pulls together satellite observations and reanalysis data across
**15 Pakistani cities**. Working at that scale forces a few good habits early:

- A consistent schema, so every city is comparable.
- Sensible handling of gaps — missing days are common and pretending they're
  zero is worse than admitting them.
- Aggregations that match how people actually think about air: daily, monthly,
  and seasonal views rather than raw timestamps.

## Making it interpretable

The interpretation layer is where most of the effort went. A number like a
pollutant concentration means nothing on its own; it only matters relative to a
baseline, a guideline, and the rest of the year.

So the app leans on a few ideas:

1. **Comparison over absolutes** — every reading is shown against the city's own
   typical range, not just a global threshold.
2. **Visual first** — trends and maps before tables, because shape is easier to
   read than rows.
3. **One question per view** — each screen answers a single thing well instead
   of showing everything at once.

{% include figure.html caption="Per-city trend view (screenshot to be added)." %}

## What I'd change next

A few things are on the list:

- Add uncertainty bands so the confidence in each reading is visible, not implied.
- Bring in a light forecasting baseline to contrast "what happened" with "what we
  might expect."
- Write up the methodology in its own post, because the cleaning choices deserve
  more than a footnote.

> The goal was never a prettier dashboard. It was a study you could read like a
> story and still trust like a dataset.

If you build something similar, the lesson that kept paying off was this: spend
your time on the layer between the data and the reader. The model is rarely the
hard part. The translation is.
