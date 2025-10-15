# Shielding Federated Learning: Defense Against Data Poisoning attack.

## Overview

**Shielding Federated Learning** is a research project focused on securing *Federated Learning (FL)* systems from **data poisoning attacks**.  
Federated Learning enables multiple clients to collaboratively train a global model without sharing their private data — but this decentralized structure also opens doors to **malicious participants** who can poison the model by submitting corrupted updates.

This work proposes a **server-side, privacy-preserving anomaly detection mechanism** that identifies and isolates malicious clients **without accessing their private data**.  
The defense uses **L2-distance–based statistical anomaly detection** combined with an **adaptive threshold (μ + Kσ)** to flag poisoned updates.

---

## 🎯 Objectives

1. **Detect poisoned client updates** in a federated learning environment with minimal computational overhead.  
2. **Preserve data privacy** — no raw data from clients is ever accessed.  
3. **Achieve high detection accuracy** across various poisoning ratios and datasets.  
4. **Visualize detection decisions** to aid understanding and interpretability for analysts.  

---

## System Design

###  Architecture

1. **Federated Setup:**  
   - 10 simulated clients, each with a local dataset.  
   - Clients train local models and send updates (gradients or weights) to the central server.

2. **Server-Side Detection:**  
   - For every round, the server computes the **L2 distance** between each client’s update and the global mean update.  
   - The detector calculates a **dynamic threshold**:
     ```
     threshold = μ + Kσ
     ```
     where:
     - μ = mean of all client distances  
     - σ = standard deviation  
     - K = sensitivity constant (controls tolerance level)

3. **Flagging:**  
   - If a client’s distance > threshold → flagged as potential attacker.  
   - Results are visualized per round for interpretability.

---

## 💻 Implementation Details

| Component | Description |
|------------|-------------|
| **Framework** | Python (PyTorch) |
| **Learning Algorithm** | Federated Averaging (FedAvg) |
| **Model Type** | Multilayer Perceptron (MLP) |
| **Attack Type** | Label-flip data poisoning |
| **Detection Metric** | L2 distance in update space |
| **Thresholding Rule** | Adaptive (μ + Kσ) |
| **Visualization Tools** | Matplotlib, Seaborn |
| **Data Handling** | NumPy, Pandas |



---

## 📊 Experimental Evaluation

| Dataset | Poison Ratio | Precision | Recall | F1-score | Attacker Detection (Rounds) |
|----------|---------------|------------|----------|-----------|------------------------------|
| **MNIST** | 50% | **1.0** | **1.0** | **1.0** | 200/200 detected |
| **Breast Cancer Diagnostic** | 50% | 0.92 | 0.94 | 0.93 | 27/50 detected |
| **Breast Cancer Diagnostic** | 40% | 0.90 | 0.92 | 0.91 | 24/50 detected |
| **Breast Cancer Diagnostic** | 30% | 0.88 | 0.90 | 0.89 | 22/50 detected |

📈 **Observation:**  
- The method achieves **perfect detection on MNIST** at high poisoning ratios.  
- On smaller datasets like Breast Cancer, detection slightly decreases due to overlapping gradient patterns — showing dataset-size effects.  
- No false positives were observed on clean clients in the MNIST setup.

