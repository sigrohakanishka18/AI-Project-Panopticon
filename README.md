#  Project Panopticon: Intelligent Proctoring AI

## EduGuard AI — Time-Series Classification Pipeline

Project Panopticon is a time-series machine learning pipeline designed to detect suspicious exam behavior while prioritizing precision and minimizing false accusations.

##  Objective

The project addresses the following challenges:

1. **Asynchronous merge** — Align video telemetry and system-event logs using `pandas.merge_asof`.
2. **Missing telemetry** — Preserve the examination timeline and handle missing telemetry values.
3. **Micro-movement noise** — Use 10-second rolling features to capture sustained behavior.
4. **False accusations** — Use `predict_proba()` with a strict **0.90 decision threshold**.

> **Important:** The supplied `system_events.csv` contains the `is_cheating` label. After a backward `merge_asof`, the latest event label is carried forward until the next event. This follows the project specification, but the resulting label semantics should be validated before any real-world deployment.

---

##  Project Workflow

```text
Video Telemetry + System Events
              │
              ▼
      Timestamp Conversion
              │
              ▼
   Asynchronous Temporal Merge
       (pandas.merge_asof)
              │
              ▼
      Missing Data Handling
              │
              ▼
     10-Second Rolling Features
              │
              ▼
       Train/Test Split
              │
              ▼
   Balanced Random Forest Model
              │
              ▼
       Probability Prediction
              │
        ┌─────┴─────┐
        ▼           ▼
     0.50          0.90
   Threshold      Threshold
        │           │
        ▼           ▼
   Standard      Strict,
 Classification  Precision-First
                 Classification
              │
              ▼
       Model Evaluation
              │
              ▼
      Feature Importance


