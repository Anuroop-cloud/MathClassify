# Design Choices

## Problem Overview

I approached this problem as a **text classification task**, where the goal was to automatically classify math questions into subtopics such as Algebra, Geometry, Number Theory, etc.

Instead of jumping straight into modeling, I first focused on understanding how the data was structured and what kind of signals the questions naturally contained. This helped me decide whether a simple, interpretable approach would be sufficient before considering more complex models.

---

## Data Understanding

The dataset was organized in a **folder-based format**, where each folder represented a topic and contained multiple JSON files. Each JSON file stored a single question along with metadata such as difficulty level and solution.

Before doing any modeling, I spent time **inspecting the dataset manually** to confirm:
- where the actual question text lived (the `problem` field),
- whether the JSON schema was consistent across folders,
- and how many samples were present per class.

This step helped me avoid label leakage and data-loading bugs later, especially since the folder name itself already provided a clean ground-truth label.

---

## Model Choice

I chose a **classical machine learning approach**.

Math questions are usually **short, structured, and keyword-driven** (for example: *integral*, *probability*, *factor*, *angle*). Because of this, **TF-IDF** is a natural way to represent the text — it highlights topic-specific terms while down-weighting common words.

For classification, I used **Logistic Regression** because:
- it naturally supports **multi-class classification**,
- it is fast to train and easy to debug,
- and most importantly, it is **interpretable**, which matters for an educational use case.

I prioritized a model that I could **clearly explain and justify**, rather than using a more complex architecture without a strong reason.

---

## Debugging & Problem Solving

While building the pipeline, I encountered several real-world issues, including:
- mismatched feature and label lengths,
- incorrect assumptions about JSON keys,
- and noisy tokens caused by mathematical formatting.

Instead of applying quick fixes, I addressed these issues by:
- enforcing a **single source of truth** for text–label pairs,
- validating dataset consistency before training,
- and inspecting TF-IDF features directly to understand what the model was actually learning.

This approach helped stabilize the pipeline and ensured that the model behavior remained predictable and debuggable.
