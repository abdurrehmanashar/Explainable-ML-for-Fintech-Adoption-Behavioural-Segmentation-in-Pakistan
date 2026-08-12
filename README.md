# Beyond Access: Explainable Machine Learning for Fintech Adoption and Behavioural Segmentation in Pakistan

<p align="center">
  <strong>Understanding the gap between digital access and meaningful financial participation.</strong>
</p>

<p align="center">
  <a href="https://umema2004.github.io/Fintech-Adoption-Website">Research Website</a>
  ·
  <a href="#research">Research</a>
  ·
  <a href="#key-finding">Key Finding</a>
  ·
  <a href="#methodology">Methodology</a>
  ·
  <a href="#reproducibility">Reproducibility</a>
</p>

---

## Overview

Digital financial inclusion is often measured by whether people have access to financial accounts, mobile phones, or the internet.

But **access does not necessarily mean adoption**.

This project investigates that gap in Pakistan using the **Global Findex Database 2025 Pakistan microdata** and a combination of supervised machine learning, explainable AI, unsupervised behavioural segmentation, statistical testing, and robustness analysis.

The central question is:

> **If people have the digital infrastructure needed to participate in modern financial systems, why do many remain financially unengaged?**

Rather than treating financial inclusion as a binary outcome, this research examines the transition from **access → usage → engagement** and identifies distinct behavioural and access profiles within the analytical sample.

---

# Key Finding

## 44.3% of the analytical sample is "Connected but Unengaged"

The analysis identifies a large segment characterized by substantial digital access but relatively low formal financial adoption.

| Characteristic | Connected but Unengaged |
|---|---:|
| Share of analytical sample | **44.3%** |
| Mobile phone ownership | **98.0%** |
| Internet use | **62.3%** |
| Formally excluded | **66.8%** |
| Mean Adoption Tier | **0.80** |

This finding highlights an important distinction:

> **Digital connectivity is not equivalent to financial participation.**

The result suggests that expanding infrastructure alone may not be sufficient to close the financial-inclusion gap. The mechanisms behind this gap—such as trust, product design, financial literacy, documentation, perceived risk, or institutional barriers—require further investigation.

**Important:** 44.3% describes the analytical Findex sample. The analysis uses unweighted observations and therefore should not be interpreted as a population prevalence estimate for Pakistan.

---

# Research

## Research Questions

The study investigates four related questions:

### RQ1 — Access and adoption

To what extent does digital access translate into formal financial adoption among respondents in Pakistan?

### RQ2 — Behavioural segmentation

Can unsupervised learning identify distinct behavioural and digital-access profiles?

### RQ3 — Predictive factors

Which demographic, access, behavioural, and resilience variables are most influential in modelling financial adoption?

### RQ4 — Robustness and responsible use

Are the identified patterns robust across different modelling and clustering approaches, and what fairness, calibration, and methodological limitations affect their interpretation?

---

# Analytical Framework

The research combines two complementary perspectives:

```text
                         FINANCIAL INCLUSION
                                │
                ┌───────────────┴───────────────┐
                │                               │
             ACCESS                           USAGE
                │                               │
       ┌────────┴────────┐             ┌────────┴────────┐
       │                 │             │                 │
    Mobile            Internet      Payments           Saving
    Access             Access       Behaviour         Behaviour
       │                 │             │                 │
       └─────────────────┴─────────────┴─────────────────┘
                                │
                         ADOPTION TIER
                                │
                ┌───────────────┴───────────────┐
                │                               │
         SUPERVISED ML                   UNSUPERVISED ML
                │                               │
       Predictive modelling             Behavioural clusters
                │                               │
       Explainable features             Population profiles
                │                               │
                └───────────────┬───────────────┘
                                │
                                ▼
                    CONNECTED BUT UNENGAGED
                                │
                                ▼
                     FUTURE INTERVENTION
