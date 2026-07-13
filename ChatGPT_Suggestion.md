---
description: ChatGPT Suggestion 2026-07-13
---

# Master's Thesis Direction Review & Recommendations

**Author:** Walter Telsnig  
**Program:** Applied Computer Science (Angewandte Informatik)  
**Date:** July 2026

---

# Executive Summary

The original thesis idea is strong and significantly above the complexity of a typical Master's thesis. The main challenge is not originality but scope management.

The strongest version of the thesis is not:

> "Supply Function Equilibrium versus Machine Learning"

but rather:

> "Economics-Informed Machine Learning for Electricity Market Forecasting implemented within a reproducible Event-Driven MLOps Architecture."

This framing preserves:

- Energy market theory
- Machine learning
- Software engineering
- MLOps
- Reproducibility

while avoiding the risk of becoming a purely economic modeling thesis.

---

# Recommended Thesis Positioning

## Core Idea

The thesis investigates whether economic knowledge derived from structural electricity market models can improve modern machine learning forecasting systems.

The focus is not on developing a new equilibrium theory.

The focus is on integrating domain knowledge into machine learning and evaluating the resulting forecasting performance within a production-inspired MLOps architecture.

---

# Recommended Working Titles

## Preferred Title

### Economics-Informed Machine Learning for Hourly Price Forward Curve Forecasting: An Event-Driven MLOps Architecture

---

## Alternative 1

### Hybrid Economic–Machine Learning Models for Hourly Price Forward Curve Forecasting

---

## Alternative 2

### Structural Electricity Market Modeling and Machine Learning for HPFC Forecasting within an Event-Driven MLOps Framework

---

## Alternative 3

### An Event-Driven MLOps Platform for Economics-Informed Electricity Price Forecasting

---

# Scope Recommendations

## Keep

### Software Engineering

- Microservices
- Containerization
- Docker Compose
- REST APIs
- CI/CD
- Reproducibility
- Event-driven communication

---

### MLOps

- Automated ingestion
- Feature engineering
- Experiment tracking
- Model versioning
- Model deployment
- Reproducible training pipelines

---

### Machine Learning

- LightGBM
- Temporal Fusion Transformer (TFT)

---

### Economic Modeling

- Supply Function Equilibrium theory
- Structural market modeling
- Market fundamentals
- Renewable generation impacts
- Storage impacts

---

## Reduce

### Mathematical Complexity

Avoid attempting to:

- derive new equilibrium existence proofs
- create entirely new SFE theory
- solve complete market bidding games

This would shift the thesis toward mathematical economics rather than applied computer science.

---

## Avoid

### Scope Explosion

Avoid simultaneously introducing:

- custom equilibrium theory
- multiple deep-learning architectures
- Chronos
- reinforcement learning
- agent-based simulation

These topics could each become a separate thesis.

---

# Supply Function Equilibrium Recommendation

## Current Concern

A full SFE implementation requires information that is often unavailable:

- generator cost curves
- bidding strategies
- startup costs
- strategic behavior
- plant constraints

---

## Recommended Approach

Instead of claiming:

> "I implement a full Supply Function Equilibrium"

use:

> "I implement an SFE-inspired structural electricity market model."

The model should generate economically meaningful signals based on:

- Load
- Renewable generation
- Hydro generation
- Imports
- Exports
- Fuel prices
- Storage indicators

These signals become additional inputs for machine learning models.

---

## New Narrative

The objective becomes:

### Can economic structure improve machine learning?

instead of

### Can SFE beat machine learning?

This is both more modern and more realistic.

---

# Forecasting Models

## Recommended Experimental Setup

### Model 1

Structural Market Model

Outputs:

- Structural Price Estimate

---

### Model 2

LightGBM

Inputs:

- Market Features

Outputs:

- Forecast

---

### Model 3

Temporal Fusion Transformer (TFT)

Inputs:

- Market Features

Outputs:

- Forecast

---

### Model 4

Hybrid LightGBM

Inputs:

- Market Features
- Structural Price Estimate

Outputs:

- Forecast

---

### Model 5

Hybrid TFT

Inputs:

- Market Features
- Structural Price Estimate

Outputs:

- Forecast

---

# Why LightGBM?

Advantages:

- Strong baseline
- Industry standard
- Fast training
- Explainable
- Works well with SHAP

---

# Why TFT?

Advantages:

- Modern architecture
- Designed for multivariate forecasting
- Handles future covariates naturally
- More relevant than LSTM

---

# Why Not Chronos?

Chronos is impressive but risks changing the thesis into:

> "Evaluating Foundation Models"

rather than:

> "Building and evaluating a forecasting platform."

Chronos may be discussed in related work but is not recommended as a primary model.

---

# Recommended Architecture

## High-Level Overview

```text
                    External Data Sources
     --------------------------------------------------
     ENTSO-E | EXAA | Weather | Fuel | PV | Wind | Load
     --------------------------------------------------
                             |
                             v
                    Ingestion Service
                             |
                             v
                          RabbitMQ
                             |
          +------------------+------------------+
          |                                     |
          v                                     v
 Feature Engineering Service      Data Validation Service
          |                                     |
          +------------------+------------------+
                             |
                             v
                   PostgreSQL Feature Store
                             |
          +------------------+------------------+
          |                                     |
          v                                     v
     Training Service              Inference Service
          |                                     |
          |                            Prediction Database
          |
          v
                MLflow Model Registry
          |
          v
                     Selected Model
                             |
                             v
                          REST API
                             |
                             v
                    Streamlit Dashboard
```

---

# RabbitMQ Recommendation

## Keep RabbitMQ If

The system is genuinely event-driven.

Example:

```text
New Data Arrives

↓

Publish Event

↓

Feature Generation

↓

Store Features

↓

Retrain Model

↓

Generate Prediction

↓

Update Dashboard
```

In this scenario RabbitMQ becomes a central architectural component.

---

## Remove RabbitMQ If

The workflow is simply:

```text
Cron Job

↓

Download Data

↓

Run Forecast

↓

Store Result
```

In this case RabbitMQ adds complexity without meaningful benefit.

---

# Additional Components Recommended

## MLflow

Purpose:

- Model Registry
- Experiment Tracking
- Reproducibility

Tracks:

- Hyperparameters
- Metrics
- Artifacts
- Model Versions
- Dataset Versions

---

## Feature Store

Purpose:

Prevent training-serving skew.

Ensures:

- Training
- Validation
- Inference

all use identical feature transformations.

---

# Suggested Research Questions

## RQ1

Can a structural electricity market model derived from Supply Function Equilibrium concepts generate competitive Hourly Price Forward Curve forecasts compared with modern machine learning methods?

---

## RQ2

Does incorporating structural market information improve forecasting performance compared with purely data-driven machine learning approaches?

---

## RQ3

How can an event-driven MLOps architecture support reproducible, extensible, and experimentally rigorous electricity market forecasting?

---

# Expected Contributions

## Scientific Contributions

- Structural market-informed forecasting
- Hybrid economic-machine learning modeling
- Comparative evaluation framework
- HPFC forecasting methodology

---

## Software Engineering Contributions

- Event-driven architecture
- Microservice decomposition
- Reproducible MLOps workflows
- Model lifecycle management
- Containerized deployment

---

## Practical Contributions

- End-to-end forecasting platform
- Reproducible experiments
- Extensible architecture for future research

---

# Final Recommendation

The thesis should be framed as:

> An investigation into how economic knowledge can be integrated into modern machine learning forecasting systems and operationalized through a reproducible event-driven MLOps architecture.

The architectural platform should be treated as a first-class research artifact rather than merely implementation infrastructure.

The strongest balance for the final thesis is approximately:

- 30% Software Architecture & MLOps
- 20% Structural Market Modeling
- 30% Machine Learning & Forecasting
- 20% Experimental Evaluation & Discussion

This balance aligns well with the expectations of an Applied Computer Science Master's thesis while preserving the interdisciplinary strengths of the original idea.