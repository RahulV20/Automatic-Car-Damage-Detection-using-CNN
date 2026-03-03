# Auto Car Damage Detection using CNN (VGG16)

An end-to-end computer vision pipeline that helps automate **vehicle damage assessment** from a single image — designed for **insurance/claim support** style workflows.

This is a **solo project** built as a multi-stage decision pipeline that:
1) validates whether the input is a car,  
2) predicts if the car is damaged,  
3) identifies damage location, and  
4) estimates damage severity.

---

## Why this matters (non-technical)
In many insurance claim processes, damage inspection is manual and time-consuming. This project demonstrates how image-based AI can support faster triage by extracting:
- **Damage presence** (damaged vs not damaged)
- **Damage location** (front / rear / side)
- **Damage severity** (minor / moderate / severe)

> ⚠️ This project is intended as a **decision-support prototype**, not a replacement for human assessment in high-severity claims.

---

## Key Results
Evaluated across the pipeline stages:

| Stage | Task | Classes | Accuracy |
|------|------|---------|---------:|
| Check 2 | Damage Detection | Damaged / Not Damaged | **90.2%** |
| Check 3 | Damage Location | Front / Rear / Side | **73.9%** |
| Check 4 | Damage Severity | Minor / Moderate / Severe | **68.8%** |

Confusion matrices are included below (see **Evaluation Visuals** section).

---

## Pipeline Overview

```mermaid
flowchart TD
    A[Input: Vehicle Image] --> B{Check 1: Is it a car?}
    B -- No --> X[Stop: Ask user to upload a valid car image]
    B -- Yes --> C{Check 2: Damaged or Not?}
    C -- Not Damaged --> Y[Output: No damage detected]
    C -- Damaged --> D{Check 3: Damage Location}
    D --> D1[Front]
    D --> D2[Rear]
    D --> D3[Side]
    D --> E{Check 4: Damage Severity}
    E --> E1[Minor]
    E --> E2[Moderate]
    E --> E3[Severe]

## Approach (high-level)

This project uses **transfer learning** with **VGG16 (ImageNet weights)** in a lightweight but effective way:

- **VGG16** is used as a **feature extractor**
- Features are taken from the **fc1 layer** (4096-dimensional embeddings)
- A **Logistic Regression** classifier is trained for each stage:
    - Check 2 classifier: Damaged vs Not Damaged
    - Check 3 classifier: Front vs Rear vs Side
    - Check 4 classifier: Minor vs Moderate vs Severe

Why this approach:

- Works well when dataset size is limited
- Faster training compared to fine-tuning a full CNN
- Easier to interpret and iterate


