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

- Implemented the `classify.py` module to orchestrate the log classification process.

- The module uses a hybrid classification pipeline combining Regex, BERT, and LLM-based processors.

- Classification method selection is determined by the log source and whether a regex pattern match is found.

- Logs from `LegacyCRM` are routed to the LLM-based classifier, while other logs are first processed using regex rules with a BERT fallback if no regex match is found.

- The module also supports processing logs from an input CSV file.

- The classified results are written to a new CSV file containing an additional column `target_label` that stores the predicted log category.

- Installed `python-dotenv` and `groq` packages and added a `.env` file to store the Groq API key securely.

- The Groq API is used as a free service with usage limits; API consumption and remaining quota can be monitored through the Groq dashboard on the official website.

- Developed a FastAPI server to expose the log classification logic as an API.
- Used direct script execution (`classify.py`) for local testing and debugging.
- Used FastAPI endpoints for simulating real-world API-based interaction.

- Successfully integrated FastAPI server to expose the log classification pipeline as an API endpoint.
- Implemented CSV file upload functionality using FastAPI (`UploadFile`) for batch log processing.
- Installed and used Postman as an external API client to simulate real-world API interactions.
- Successfully sent CSV file requests via Postman using `form-data` and received processed output.
- Verified end-to-end pipeline: CSV input → API request → hybrid classification (Regex + BERT + LLM) → labeled output.
- Confirmed that the API correctly processes and returns classified results for multiple log entries.
- Observed and validated API response behavior (CSV output format) in Postman.

---

# 7. Challenges Encountered

- Updated Groq LLM model in processor_llm.py file after `deepseek-r1-distill-llama-70b` was deprecated and replaced it with a supported model.

---

# 8. Potential Improvements

1. Implement automatic log template extraction.
2. Add visualization of clusters using dimensionality reduction techniques.
3. Extend the pipeline to support real-time log streams.
4. Improve anomaly detection capabilities.

---
