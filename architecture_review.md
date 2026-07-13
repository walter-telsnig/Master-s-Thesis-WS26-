---
description: 
---

# Master's Thesis Architecture Review

## Context

The additional information that this thesis is being written within an **Applied Computer Science (Angewandte Informatik)** program changes the evaluation considerably.

Unlike a thesis in Energy Economics or Data Science, the software architecture is expected to be a significant part of the scientific contribution.

Therefore, the focus should not only be on forecasting performance but also on demonstrating sound software engineering principles, reproducibility, modularity, and maintainability.

---

# Overall Opinion

I actually like the microservice approach.

Many Master's theses still consist of something similar to

```text
main.py
train.py
predict.py
```

While functional, this demonstrates relatively little software architecture.

Your proposed system instead demonstrates knowledge of

- Distributed Systems
- Microservices
- MLOps
- Event-Driven Architectures
- Reproducible Machine Learning
- Containerization

These are all valuable skills for an Applied Computer Science thesis.

Therefore,

**I would keep the microservice architecture.**

---

# RabbitMQ

RabbitMQ is the component that deserves the most critical evaluation.

The key question is not

> "Can I use RabbitMQ?"

but rather

> "Does RabbitMQ solve a real architectural problem?"

---

## Case A — RabbitMQ is justified

Suppose the system behaves like this:

```text
ENTSO-E updates

↓

Publish Event

↓

Feature Engineering

↓

Store Features

↓

Retrain Models

↓

Run Predictions

↓

Store Results

↓

Notify Dashboard
```

Here RabbitMQ becomes the communication backbone.

Every service is independent.

If one service is temporarily unavailable, the remaining services continue operating.

This is a textbook example of an event-driven architecture.

In this case,

**RabbitMQ is absolutely justified.**

---

## Case B — RabbitMQ is unnecessary

If the workflow is simply

```text
Cron Job

↓

Download CSV

↓

Run Script

↓

Predict

↓

Exit
```

then RabbitMQ adds complexity without providing meaningful benefits.

---

## Recommendation

Keep RabbitMQ **only if the thesis genuinely becomes an event-driven forecasting platform**.

If every component communicates through events,

RabbitMQ strengthens the architectural argument.

Otherwise,

it risks becoming unnecessary complexity.

---

# Missing Components

I believe two important MLOps components are currently missing.

---

## 1. Model Registry

A Model Registry is almost mandatory in modern MLOps systems.

Instead of storing models as

```text
model.pkl
model_final.pkl
model_final_v2.pkl
```

the registry stores

- model versions
- training data versions
- hyperparameters
- metrics
- artifacts
- Git commit references

Possible implementations include

- MLflow
- Weights & Biases
- DVC

This significantly improves reproducibility.

---

## 2. Experiment Tracking

Closely related to the Model Registry is experiment tracking.

Each training run should automatically record

- hyperparameters
- evaluation metrics
- dataset versions
- execution timestamps
- generated artifacts

This allows every experiment to be reproduced later.

This is a hallmark of professional MLOps.

---

# Feature Store

I like the idea of introducing a Feature Store.

Many academic projects ignore this entirely.

The Feature Store guarantees that

- training
- validation
- inference

all use identical feature engineering pipelines.

This prevents one of the most common production problems:

**training-serving skew.**

---

# PostgreSQL

PostgreSQL is an excellent choice.

Advantages include

- mature ecosystem
- excellent Python support
- Docker integration
- reliability
- SQL flexibility

I would not replace it.

---

# Docker

Docker should definitely remain.

I would even emphasize

```text
docker compose
```

as the primary deployment mechanism.

The ability to start the complete system with

```bash
docker compose up
```

demonstrates reproducibility extremely well during the thesis defense.

---

# Should Feast be used?

This depends on your objectives.

Feast is a very capable Feature Store.

However,

every additional framework increases complexity.

The important question becomes

> "Does Feast solve a problem that PostgreSQL cannot solve for this thesis?"

If the answer is no,

then a PostgreSQL-based Feature Store may actually be the cleaner solution.

---

# Forecasting Models

This is probably the most important design decision.

---

## LightGBM

### Advantages

- Extremely fast
- Excellent for structured tabular data
- Easy to interpret
- Works very well with SHAP
- Strong industrial baseline

### Disadvantages

- Limited sequential modelling
- Relies on manually engineered temporal features

---

## Temporal Fusion Transformer (TFT)

### Advantages

Designed specifically for multivariate time-series forecasting.

Handles

- weather
- holidays
- calendar effects
- fuel prices
- renewable forecasts
- known future covariates

Provides attention mechanisms and interpretability.

Scientifically much stronger than LSTM.

### Disadvantages

- More difficult to train
- Requires more computational resources
- More complex implementation

---

## Chronos

Chronos is currently one of the most interesting forecasting foundation models.

Advantages

- Very modern
- Strong zero-shot forecasting
- Excellent forecasting performance

Disadvantages

The thesis suddenly becomes

> "Evaluating Foundation Models"

instead of

> "Developing an Event-Driven Forecasting Platform."

The foundation model begins to dominate the research story.

---

# Recommendation

My preferred experimental setup would be

```
Model 1
Supply Function Equilibrium

↓

Model 2
LightGBM

↓

Model 3
Temporal Fusion Transformer

↓

Model 4
Hybrid
SFE + LightGBM

↓

Model 5
Hybrid
SFE + TFT
```

This tells a very coherent scientific story.

---

# Why not Chronos?

Not because Chronos is weak.

Quite the opposite.

Chronos is almost *too* powerful.

The risk is that reviewers begin discussing

- pretraining
- transfer learning
- foundation models

instead of

- your software architecture
- your hybrid methodology
- your scientific contribution

I would prefer keeping the focus on your own work.

---

# Suggested Architectural Improvement

I would add a dedicated

**Feature Engineering Service**

instead of performing feature generation directly inside the ingestion service.

---

## Proposed Architecture

```text
                    +----------------+
                    External APIs
                    ENTSO-E
                    EXAA
                    Weather
                    Fuel Prices
                    PV Forecasts
                    +-------+--------+
                            |
                    Ingestion Service
                            |
                        RabbitMQ
                            |
         +------------------+-------------------+
         |                                      |
 Feature Engineering                 Data Validation
         |                                      |
         +------------------+-------------------+
                            |
                     PostgreSQL Feature Store
                            |
         +------------------+------------------+
         |                                     |
     Training Service                  Inference Service
         |                                     |
         |                             Prediction Database
         |
     MLflow Model Registry
         |
      Selected Model
         |
        REST API
         |
   Streamlit Dashboard
```

This architecture has several desirable properties.

Every service has exactly one responsibility.

Responsibilities are clearly separated.

The system is scalable.

The system is reproducible.

Each service can evolve independently.

---

# Scientific Framing

One of the most important improvements would be changing how the architecture is presented.

Instead of writing

> "I implemented a microservice architecture."

consider formulating the contribution as

> **How can an event-driven MLOps architecture support reproducible, extensible, and experimentally rigorous electricity market forecasting?**

Now the architecture itself becomes part of the scientific investigation.

Architectural decisions can then be justified using established software engineering principles such as

- coupling
- cohesion
- scalability
- fault isolation
- reproducibility
- maintainability
- extensibility

This transforms the architecture from implementation detail into scientific contribution.

---

# Final Thoughts

Based on the previous projects and discussions, this proposal has the potential to become much more than a typical Master's thesis.

The combination of

- modern software architecture
- reproducible MLOps
- energy market modelling
- hybrid economic and machine learning approaches

is uncommon and highly relevant.

The key challenge will not be originality.

The key challenge will be **scope management**.

Every technology that is included should answer a clear research or engineering question.

If a component cannot be justified scientifically or architecturally,

it should be removed.

If every component has a clear purpose,

the resulting system will be both academically strong and professionally impressive.