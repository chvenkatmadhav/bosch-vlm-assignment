# Bosch VLM Assignment

## Objective

This project evaluates Vision-Language Models (VLMs) on autonomous driving question-answering tasks using the DriveLM dataset. The work covers dataset preparation, baseline benchmarking, evaluation methodology, fine-tuning strategy design, and deployment architecture considerations.

---

## Dataset

Dataset Used:

* DriveLM (NuScenes-based driving QA dataset)

Dataset Characteristics:

* Perception reasoning
* Prediction reasoning
* Planning reasoning
* Behavior reasoning

The original hierarchical DriveLM annotations were transformed into a flat tabular format containing:

* Scene ID
* Frame ID
* Question Category
* Question
* Answer
* Camera Image Path

---

## Data Preparation

Steps performed:

1. Explored DriveLM dataset structure
2. Parsed scene-level and frame-level annotations
3. Extracted question-answer pairs
4. Linked image paths with corresponding QA records
5. Generated a flattened dataset for benchmarking

Output Artifacts:

* processed_dataset.csv
* train_subset.csv
* baseline_results.csv

---

## Exploratory Data Analysis

Question Category Distribution:

| Category   | Count |
| ---------- | ----- |
| Perception | 337   |
| Prediction | 271   |
| Planning   | 173   |
| Behavior   | 9     |

Observations:

* Perception and prediction dominate the dataset.
* Behavior-related reasoning is significantly underrepresented.
* The dataset emphasizes driving scene understanding and decision making.

---

## Baseline Model

Model:
Qwen2-VL-2B-Instruct

Rationale:

* Strong multimodal reasoning capability
* Compatible with limited compute environments
* Efficient inference on Tesla T4 GPU
* Suitable for rapid experimentation

---

## Evaluation Methodology

Metrics:

1. Semantic Similarity

   * SentenceTransformer embeddings
   * Cosine similarity between prediction and ground truth

2. Inference Latency

   * Average response time per sample

---

## Baseline Results

| Metric                  | Value                |
| ----------------------- | -------------------- |
| Model                   | Qwen2-VL-2B-Instruct |
| Evaluation Samples      | 20                   |
| Avg Semantic Similarity | 0.266                |
| Avg Latency             | 5.56 sec             |

Key Observation:

The pretrained model demonstrates general scene understanding capabilities but struggles with domain-specific autonomous driving reasoning and planning tasks.

---

## Fine-Tuning Strategy

Selected Approach:
QLoRA (Quantized Low-Rank Adaptation)

Reasons:

* Parameter-efficient adaptation
* Reduced memory requirements
* Suitable for limited GPU resources
* Faster experimentation cycle

Training Subset:

* 500 representative samples

Expected Outcome:

Improved alignment with DriveLM-specific reasoning patterns while maintaining deployment efficiency.

---

## Deployment Architecture

Client
↓
FastAPI API Layer
↓
Request Processing
↓
Image Preprocessing
↓
Qwen2-VL Inference
↓
Response Generation

Planned Optimizations:

* 4-bit quantization
* Dynamic batching
* Embedding caching
* Asynchronous request handling

---

## Repository Structure

bosch-vlm-assignment/

├── notebooks/

├── src/

├── results/

├── report/

├── Dockerfile

├── requirements.txt

└── README.md

---

## Future Work

* Full-scale QLoRA training
* Larger evaluation dataset
* Retrieval-augmented reasoning
* Real-time deployment benchmarking
