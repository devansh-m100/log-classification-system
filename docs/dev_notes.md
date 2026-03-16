# Development Notes

This document records implementation details, observations, experiments, and challenges encountered during the development of the log classification system.

---

# 1. Dataset Exploration

1. The dataset contains logs generated from multiple system components.
2. Each log entry includes the fields: `timestamp`, `source`, `log_message`, `target_label`, and `complexity`.
3. Initial dataset exploration was performed using pandas functions such as `value_counts()` and `groupby()`.
4. Some logs follow structured patterns (e.g., backup events), while others contain free-form messages.
5. Structured logs can potentially be captured using regex-based classification.

---

# 2. Implementation

## Filtering Logs for Further Processing

1. Split dataset into training (70%) and testing (30%) sets.
2. Trained a Logistic Regression classifier on the training data.
3. Generated predictions on the test set.
4. Evaluated model performance using precision, recall, and F1-score via `classification_report`.

## Model Persistence

1. Imported the `joblib` library to enable model serialization.
2. Saved the trained Logistic Regression classifier (`clf`) using `joblib.dump`.
3. Stored the model in the file `../models/log_classifier.joblib`.
4. This allows the trained model to be reused later for inference without retraining.

- Also maintained a separate regex, bert, llm classification logic into dedicated respective files to improve code organization, reusability, and maintainability.

- Those non regex target labels, having 5 or less log entries can be processed using llm and others with BERT.

- A classify.py file is separately maintained which allows the classification of log entries to be further classfied using llm or bert.

---

# 7. Challenges Encountered

1. HuggingFace model download warnings appeared due to Windows symlink restrictions.
2. Virtual environment configuration required alignment with the notebook kernel.
3. Clustering parameter tuning required experimentation.
4. Some logs contained ambiguous semantics, resulting in mixed clusters.

---

# 8. Potential Improvements

1. Implement automatic log template extraction.
2. Add visualization of clusters using dimensionality reduction techniques.
3. Extend the pipeline to support real-time log streams.
4. Improve anomaly detection capabilities.

---
