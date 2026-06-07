# Bosch Vision-Language Model Assessment Report

## 1. Executive Summary

This project investigates the application of Vision-Language Models (VLMs) to autonomous driving question-answering tasks using the DriveLM dataset.

The objective was to establish a baseline benchmark, analyze dataset characteristics, design a parameter-efficient fine-tuning strategy, and propose a deployment architecture suitable for production environments.

Qwen2-VL-2B-Instruct was selected as the baseline model due to its balance between multimodal reasoning capability and computational efficiency.

---

## 2. Dataset Understanding

DriveLM is a driving-focused multimodal dataset built on top of NuScenes.

The dataset contains reasoning tasks across four categories:

* Perception
* Prediction
* Planning
* Behavior

A flattened dataset was created by extracting:

* Scene identifiers
* Frame identifiers
* Questions
* Answers
* Image references

This transformation enabled efficient benchmarking and evaluation.

### Dataset Distribution

| Category   | Count |
| ---------- | ----- |
| Perception | 337   |
| Prediction | 271   |
| Planning   | 173   |
| Behavior   | 9     |

### Observations

The dataset is dominated by perception and prediction tasks. Behavior-oriented reasoning is comparatively rare, introducing potential class imbalance during model evaluation.

---

## 3. Data Preparation

The following preprocessing steps were performed:

1. Exploration of DriveLM annotation structure.
2. Extraction of QA pairs from hierarchical JSON annotations.
3. Linking image references to corresponding questions and answers.
4. Creation of a flattened tabular dataset.
5. Generation of exploratory statistics.

Output files:

* processed_dataset.csv
* train_subset.csv

---

## 4. Baseline Benchmarking

### Model Selection

Selected Model:

Qwen2-VL-2B-Instruct

Selection Criteria:

* Strong multimodal capabilities
* Compatibility with Tesla T4 GPU
* Lower inference cost
* Practical deployment footprint

### Evaluation Metrics

The following metrics were used:

1. Semantic Similarity
2. Inference Latency

### Baseline Results

| Metric              | Value    |
| ------------------- | -------- |
| Semantic Similarity | 0.266    |
| Average Latency     | 5.56 sec |

### Qualitative Findings

The model successfully identified general traffic scenes and safety-related concepts.

However, several outputs failed to align with the detailed autonomous-driving annotations provided by DriveLM.

This indicates a domain gap between general-purpose multimodal training and driving-specific reasoning tasks.

---

## 5. Fine-Tuning Strategy

### Selected Method

QLoRA (Quantized Low-Rank Adaptation)

### Justification

Full fine-tuning of modern VLMs requires significant computational resources.

QLoRA was selected because it:

* Reduces memory requirements
* Supports rapid experimentation
* Preserves most pretrained capabilities
* Enables efficient domain adaptation

### Training Subset

A representative subset of 500 samples was selected for experimentation.

This subset size was chosen to balance:

* Compute efficiency
* Experiment turnaround time
* Data diversity

---

## 6. Comparative Evaluation Framework

The proposed evaluation framework compares:

| Metric              | Baseline | Fine-Tuned |
| ------------------- | -------- | ---------- |
| Semantic Similarity | 0.266    | TBD        |
| Latency             | 5.56 sec | TBD        |
| Memory Usage        | TBD      | TBD        |

This framework enables measurement of both quality improvements and deployment tradeoffs.

---

## 7. Deployment Architecture

### Proposed System Design

Client
↓
FastAPI
↓
Request Queue
↓
Image Processing
↓
Qwen2-VL Inference
↓
Response Generation

### Optimization Techniques

1. Quantized inference
2. Embedding caching
3. Dynamic batching
4. Asynchronous request processing

These techniques reduce inference latency while maintaining response quality.

---

## 8. Limitations

Several limitations were identified:

* Evaluation performed on a representative subset rather than the full dataset.
* Limited compute resources constrained experimentation.
* Fine-tuning results were not fully benchmarked at large scale.
* Behavior reasoning samples were underrepresented.

---

## 9. Future Improvements

Potential future enhancements include:

* Full dataset fine-tuning
* Larger-scale benchmarking
* Multi-camera fusion
* Retrieval-augmented reasoning
* Real-time deployment testing

---

## 10. Conclusion

The project established a complete workflow for evaluating Vision-Language Models on autonomous driving reasoning tasks.

The baseline evaluation highlighted the gap between general-purpose multimodal understanding and domain-specific driving reasoning. QLoRA was identified as a practical adaptation strategy for improving performance under realistic compute constraints.

The proposed deployment architecture provides a scalable foundation for serving autonomous-driving VLM workloads in production environments.
