---
layout: post
title: "AirVision 2026: Turning Satellite Data into a Cleaner Story"
date: 2026-06-14
disc: data
excerpt: "Fifteen cities, a year of air-quality data from space, and one goal: turn a wall of numbers into something you can actually read."
---

Ask someone what their city's air was like last year and they'll shrug. Ask them
to read a spreadsheet of pollutant concentrations and they'll shrug harder.
AirVision started from that gap — the distance between data that *exists* and data
you can actually feel.

[**Try the live app →**](https://airvision-2026-esbig4eq22q7shndytr2si.streamlit.app/)

{% include figure.html src="/assets/images/airvision-overview.png" alt="AirVision dashboard overview" caption="AirVision — the overview you land on." %}

## What it is, in one line

AirVision is a study of air quality across **fifteen Pakistani cities**, built
from a year of satellite and weather data, designed so you can open any city and
understand how it breathes — without needing a science degree to read it.

## Why air quality, in plain words

Air pollution isn't one thing; it's a few invisible ones. Tiny particles small
enough to slip into your lungs. Gases that come off traffic and industry. On a
good day you don't notice them. Over a winter, they decide whether a city's air
is something you'd let a child run around in.

The trouble is that this is usually reported as raw numbers with names like NO₂
and PM2.5 — accurate, but meaningless to most people. I wanted the meaning back.

## Where the data comes from

Two sources do the heavy lifting, and both are easier than they sound:

- **Satellites** that read the sky from orbit and estimate what's in the air
  below — the same family of tech behind weather forecasts.
- **Reanalysis data**, which is basically the best reconstruction we have of past
  conditions, stitched together from many measurements into one consistent record.

Pulling fifteen cities together forced some discipline early: one shared format
so every city is comparable, honest handling of gaps instead of pretending
missing days are zero, and time-scales that match how people actually think —
daily, monthly, and seasonal rather than raw timestamps.

{% include figure.html src="/assets/images/airvision-trend.png" alt="Per-city trend chart" caption="One city, one year — the shape of the air over time." %}

## The hard part wasn't the data

It was making the data *readable*. A pollutant number means nothing on its own;
it only matters next to a baseline, a guideline, and the rest of the year. So the
whole app leans on three ideas:

1. **Comparison over absolutes.** Every reading is shown against the city's own
   normal range, not just a global threshold. "Worse than usual" lands harder
   than a bare figure.
2. **Visual first.** Trends and maps before tables, because the eye reads a shape
   faster than it reads a row of digits.
3. **One question per view.** Each screen answers a single thing well instead of
   showing everything at once.

{% include figure.html src="/assets/images/airvision-comparison.png" alt="City comparison view" caption="Comparing cities at a glance." %}

## What you can actually do

You can pick a city and watch its year unfold. You can see when the air turned —
and roughly why. You can put cities side by side and see which ones carry a
heavier load. The point throughout is the same: less "here is a number," more
"here is what happened, and here's how to read it."

> The goal was never a prettier dashboard. It was a study you could read like a
> story and still trust like a dataset.

## What I learned

<!--
  AILYA — add 2–3 real sentences here: something that surprised you in the data,
  a city that didn't behave how you expected, or a cleaning decision you'd defend.
  This is what makes it yours instead of generic. Keep it concrete.
-->

The biggest lesson was where the value actually lives: in the layer between the
data and the reader. The modelling is rarely the bottleneck. The translation is.

## What's next

A few things are on the list: uncertainty bands so confidence is visible rather
than implied, a light forecasting baseline to contrast "what happened" with "what
we'd expect," and a proper methodology write-up, because the cleaning choices
deserve more than a footnote.

If you build something similar, the advice that kept paying off is this: spend
your time on the part that turns measurement into meaning. That's the project.

[**Open AirVision →**](https://airvision-2026-esbig4eq22q7shndytr2si.streamlit.app/)
