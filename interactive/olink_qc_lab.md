---
title: "Interactive Lab: Plasma Proteomics QC & Paired Trajectory Explorer"
subtitle: "Explore high-throughput Olink PEA Quality Control, LOD filtering, and matched clinical trajectories in real time"
format:
  html:
    page-layout: full
---

# Interactive Virtual Lab

Welcome to the **Interactive Plasma Proteomics Lab**! This web application allows you to explore the quality control and statistical modeling steps of a paired clinical cohort ($N = 30$ patients / 60 plasma draws: Baseline vs. Day 14 post-treatment) run across 2 microtiter plates with bridge controls.

::: {.callout-tip}
### How to Use This Interactive Tool
* Use the **top navigation tabs** to progress through the experimental design, internal control deviations, paired volcano plot, and individual patient trajectory spaghetti plots.
* Test the **interactive LOD slider** on Tab 2 to see in real time how tightening the detectability threshold (% samples $>$ LOD) filters out low-abundance background noise.
* Toggle the **Bridge Normalization switch** to observe how median centering eliminates inter-plate batch shifts.
* Click on any marker in the **Paired Volcano Plot** on Tab 3 to inspect its exact effect size ($\Delta\text{NPX}$), $p$-value, and biological annotation.
:::

---

<div style="width: 100%; border: 1px solid #cbd5e1; border-radius: 12px; overflow: hidden; box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);">
  <iframe src="olink_explore_interactive_qc_analysis.html" width="100%" height="950px" style="border: none;"></iframe>
</div>

---

## Guided Laboratory Exercises

Try answering the following questions using the interactive controls in the dashboard:

### Exercise 1: Internal Control Evaluation
1. Navigate to **Tab 2 (Quality Control & LOD)**.
2. Inspect the **Extension Control vs. Incubation Control** scatter plot.
3. Find the sample in the top-right corner that falls outside the green $\pm 0.3\text{ NPX}$ acceptance boundary.
   * *Question:* Which sample is it, and what failure mode does a simultaneous elevation of both Incubation and Extension controls indicate?
   * *Answer:* Sample `PT_08_Day14` failed both controls, indicating a reaction well failure (pipetting volume error, bubble blockage, or severe enzymatic inhibition).

### Exercise 2: Limit of Detection (LOD) Thresholding
1. On **Tab 2**, move the **Minimum Detection Rate (% samples > LOD)** slider from `0%` to `50%` and then to `75%`.
2. Observe the number of retained assays in the counter.
   * *Question:* Why is it forbidden in affinity proteomics to impute values below LOD with $0$ or $\text{LOD}/2$?
   * *Answer:* Imputing an arbitrary constant causes an artificial zero-variance spike that distorts Gaussian normality assumptions, artificially deflates standard errors, and inflates false-positive discovery rates.

### Exercise 3: Patient Response Trajectories
1. Switch to **Tab 4 (Patient Trajectories)**.
2. Select **IL-6** and **CXCL10**, and compare them to **FGF21**.
   * *Question:* How do individual patient slopes confirm that the decrease in IL-6 and CXCL10 is a true class effect rather than driven by a single extreme outlier?
