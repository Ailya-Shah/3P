---
layout: post
title: "When the Target Is a Person: Notes on the Therapy Predictor"
date: 2026-06-10
disc: psyche
excerpt: "Building a classifier for therapy-related outcomes taught me less about modelling and more about what a label quietly assumes."
---

Building a classifier for therapy-related outcomes taught me less about
modelling and more about what a label quietly assumes.

The setup is the usual one: features in, a category out, a score that tells you
how often the category is right. The trouble is that the category here is a
**person's outcome**, and that changes how much you're allowed to trust the
number.

## The comfortable part

The mechanics were familiar and, honestly, the easy bit:

- Clean the data, encode the categories, split it honestly.
- Try a few models, compare them on more than just accuracy.
- Look at precision and recall separately, because the cost of a false negative
  and a false positive are nothing alike here.

{% include figure.html src="/assets/images/confusion-matrix.png" alt="Confusion matrix across model variants" caption="Confusion matrix across model variants" %}

## The uncomfortable part

A model that's right 85% of the time sounds great until you ask *which* 15% it's
wrong about, and whether that error lands on the people who can least afford it.
That's not a tuning problem. It's a framing problem, and no amount of
cross-validation fixes a question that was set up wrong.

So I started writing down assumptions next to the metrics:

1. Who is in the training data, and who isn't?
2. What does the label actually measure versus what we *want* it to measure?
3. If this were used for real, what's the worst plausible misuse?

> A score answers "how often." It never answers "for whom" or "at what cost."
> Those stay your job.

## Where physics quietly helps

The physicist's reflex — sanity-check the magnitude before trusting the
decimals — turns out to be just as useful here. Before celebrating a metric, ask
whether the result is even plausible given how small and messy the data is. Most
of the time, the honest answer is "be careful," and that's a finding too.

This is exactly the kind of project 3P exists for: a Python model, a psychology
question, and a habit borrowed from physics, all arguing with each other until
something honest comes out.
