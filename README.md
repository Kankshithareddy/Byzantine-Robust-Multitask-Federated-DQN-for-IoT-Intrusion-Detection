
***

# Byzantine-Robust Multitask Federated DQN with Dual-Stream Attention-SAE for IoT Intrusion Detection and Response

## 📌 Project Overview
**FIN_MODEL** is a next-generation Intrusion Prevention System (IPS) designed for decentralized IoT environments. It utilizes **Federated Learning (FL)** to train a global model across multiple clients without compromising data privacy. The system is unique in its integration of **Deep Reinforcement Learning (DRL)** for autonomous threat mitigation and a **Byzantine-Robust** aggregation mechanism to defend against malicious or hijacked network nodes.

### Key Performance Highlights:
*   **Binary IPS Accuracy:** Achieved **99.29%** in autonomous threat response.
*   **Response Latency:** Optimized architecture allows for rapid inference (~0.19 ms per traffic window) [User Session].
*   **Task 1 Classification:** Attained **72.23%** accuracy in broad threat family identification.

## 🏗 System Architecture
The model features a "special" **dual-stream architecture** designed to extract deep temporal patterns while maintaining a compact state representation for the RL agent.

### 1. Dual-Stream Feature Encoder
*   **Temporal Stream (CNN + Attention):** Utilizes **1D-Convolutional** layers followed by **Multi-Head Attention** to capture long-range dependencies in network packet sequences.
*   **Latent Stream (Stacked Autoencoder - SAE):** Implements a bottleneck architecture with an **8-neuron latent layer** to create a compressed, noise-resistant state representation.

### 2. Multi-Task Learning (MTL) Heads
The model simultaneously processes four distinct security objectives:
*   **Task 1:** Threat Family Classification (6-class).
*   **Task 2:** Threat Subtype Classification (19-class).
*   **Task 3:** Attack Intensity Regression (MSE-based magnitude scoring).
*   **Task 4:** RL-based IPS Decision-making (DQN).

## 🤖 Reinforcement Learning & Active Learning
The system employs a **Deep Q-Network (DQN)** to determine the optimal response to detected traffic.
*   **Action Space:** 0 (Allow), 1 (Rate-Limit), 2 (Drop), 3 (Request Oracle Label).
*   **Active Learning Oracle:** When the agent faces high uncertainty, it can request an "Oracle" label, ensuring high reliability in critical IoT scenarios.
*   **Cost-Sensitive Reward Function:** Heavily penalizes false negatives (-150.0) while rewarding successful drops (+50.0).

## 🛡 Byzantine Robustness
To operate in untrusted federated environments, the server utilizes **Robust Multi-Krum Aggregation**.
*   **Byzantine Defense:** Automatically filters out updates from "hijacked" or malicious clients by calculating Euclidean distances between local weight updates and selecting the most consistent "honest" participants.
*   **Simulation:** The framework includes a simulation mode that injects Gaussian noise into malicious client weights to test the aggregator's resilience.

## 🚀 Getting Started

### Prerequisites
*   TensorFlow 2.x
*   Scikit-learn
*   Pandas / PyArrow (for `.parquet` data handling)

### Installation & Setup
1.  **Data Placement:** Ensure training and testing parquet files are in the specified `DATA_PATH`.
2.  **Directory Setup:** The script automatically generates folders for `Models`, `Checkpoints`, and `Logs`.
3.  **Hyperparameters:** Default settings include 6 clients, 50 federated rounds, and a window size of 10.

### Execution Flow
1.  **Phase 1: Supervised Warm-up:** The model undergoes 15 epochs of supervised training to prime the classification heads before the RL loop begins.
2.  **Phase 2: Federated RL Loop:** Clients perform local DQN training, followed by Multi-Krum weight aggregation at the server.

## 📊 Evaluation Metrics
The system provides comprehensive logs and visualizations, including:
*   **RL Decision Matrix:** Visualizes the agent's effectiveness in dropping attacks vs. allowing benign traffic.
*   **SAE Reconstruction Error:** Distributions used to identify anomalies.
*   **Feature Importance:** Permutation-based ranking of top driver features (e.g., IAT, Weight, Header_Length).

## 📜 License
This project is intended for research and educational purposes in the field of IoT security and Federated Learning.

*** 
