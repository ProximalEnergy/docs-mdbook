# Combiner Fuse Health

## Description
The Combiner Fuse Health KPI is a measure of the DC health of a project calculated using only combiner current data. Each combiner is given a daily score from 0 to 1, with 1 indicating the healthiest combiners on a project.

## Methodology
1. Pull DC combiner current for each combiner box on a 5-minute basis.
2. Normalize the current for each combiner against its own DC capacity.
3. Identify the "ideal" combiner as the 99th percentile of all combiner normalized current values. 99% chosen as the threshold to avoid outliers.
4. Normalize all ratios against the ideal combiner. This gives the ideal combiner a score of 1.0, and all other combiners a score up to 1.0.
5. Calculate the daily mean of all combiner scores. This gives the normalized fuse health score per combiner box per day.
