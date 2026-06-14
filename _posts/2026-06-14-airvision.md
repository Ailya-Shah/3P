---
layout: post
title: "AirVision 2026: Turning Satellite Data into a Cleaner Story"
date: 2026-06-14
---

AirVision started as a simple question: can satellite and reanalysis data make air quality easier to understand across multiple cities, instead of leaving it buried in raw tables and charts?

## What I built

I built a Streamlit app that compares air-quality signals across 15 Pakistani cities and turns them into a visual study. The goal was not only to display data, but to make the differences easier to inspect and explain.

The project combines data collection, cleaning, feature engineering, and visualization. I used satellite and reanalysis sources such as Google Earth Engine, Sentinel-5P, ERA5, and CAMS, then organized the results into a small interface for exploration.

## Why this project mattered

The main challenge in a project like this is that environmental data is rarely clean or immediately comparable. Different sources have different scales, sampling patterns, and noise characteristics. That means the real work is not just plotting, but deciding how to align the data so the comparison is meaningful.

That is the part I found most useful to work through. It forced me to think about data consistency, feature selection, and the difference between a dataset that exists and a dataset that is actually readable.

## What I learned

- Raw environmental data needs more interpretation than it first appears to need.
- A good interface can make a technical project feel much more approachable.
- The strongest part of a project is often the reasoning behind the pipeline, not just the final model or chart.

## What I would improve next

If I extend the project, I would add clearer comparisons across time, stronger annotation of anomalies, and a short write-up explaining the assumptions behind each data source.

That would move the work even closer to a publication-style project rather than just a dashboard.