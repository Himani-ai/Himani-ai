<div align="center">

# GNN Spatial Routing
### Spatio-Temporal Graph Neural Networks for Dynamic Packet Routing in Arbitrary Topologies

[![Framework: PyTorch Geometric](https://img.shields.io/badge/Framework-PyTorch_Geometric-EE4C2C?style=for-the-badge&labelColor=030712)](https://pyg.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-00D2FF.svg?style=for-the-badge&labelColor=030712)](https://opensource.org/licenses/MIT)
[![Domain: Graph AI](https://img.shields.io/badge/Domain-Graph_Neural_Networks-8B5CF6?style=for-the-badge&labelColor=030712)](https://github.com/Himani-ai/gnn-spatial-routing)
[![Status: Research Prototype](https://img.shields.io/badge/Status-Research_Prototype-10B981?style=for-the-badge&labelColor=030712)](https://github.com/Himani-ai/gnn-spatial-routing)

<p align="center">
  <a href="#1-model-architecture">Architecture</a> •
  <a href="#2-mathematical-message-passing-formulation">Mathematical Formulation</a> •
  <a href="#3-benchmark-results">Benchmarks</a> •
  <a href="#4-reproduction">Reproduction</a>
</p>

</div>

---

## 1. Model Architecture

Traditional routing protocols compute paths purely on static edge weights, failing to anticipate non-linear queue saturation and bursty traffic propagation across complex topologies.

**GNN Spatial Routing** frames packet route generation as node- and edge-level regression over dynamic graphs. Using Spatio-Temporal Graph Convolutional Networks (ST-GCN) with multi-head attention, the model predicts path delay matrices $\hat{Y}_{t+\Delta t}$ given historical traffic observations $X_{t-k:t}$.

```mermaid
graph LR
    classDef input fill:#030712,stroke:#00D2FF,stroke-width:2px,color:#FFFFFF;
    classDef gnn fill:#07101E,stroke:#00F2FE,stroke-width:2px,color:#FFFFFF;
    classDef out fill:#110E2E,stroke:#8B5CF6,stroke-width:2px,color:#FFFFFF;

    IN["Topological Ingress Matrix X_t"]:::input --> GAT["Graph Attention Convolution (PyG)"]:::gnn
    GAT --> GRU["Temporal Gated Recurrent Unit"]:::gnn
    GRU --> ATTN["Multi-Head Path Attention"]:::gnn
    ATTN --> PRED["Predicted Egress Delay Matrix"]:::out
```

---

## 2. Mathematical Message-Passing Formulation

At spatial convolution layer $l$, the node representation $h_i^{(l)}$ is updated via anisotropic edge attention:

$$h_i^{(l)} = \sigma \left( W^{(l)} h_i^{(l-1)} + \sum_{j \in \mathcal{N}(i)} \alpha_{ij}^{(l)} \Theta^{(l)} h_j^{(l-1)} \right)$$

where attention coefficients $\alpha_{ij}$ model dynamic link congestion:

$$\alpha_{ij} = \frac{\exp\left(\text{LeakyReLU}\left(a^T [W h_i \parallel W h_j \parallel e_{ij}]\right)\right)}{\sum_{k \in \mathcal{N}(i)} \exp\left(\text{LeakyReLU}\left(a^T [W h_i \parallel W h_k \parallel e_{ik}]\right)\right)}$$

---

## 3. Benchmark Results

Evaluated on the GEANT and NSFNET research network topologies:

| Metric | Shortest Path (Dijkstra) | Deep Q-Learning Routing | GNN Spatial Routing (Ours) |
| :--- | :--- | :--- | :--- |
| **Mean End-to-End Latency** | $48.2\text{ ms}$ | $34.1\text{ ms}$ | **$21.6\text{ ms}$** |
| **Max Network Throughput** | $1.2\text{ Gbps}$ | $1.8\text{ Gbps}$ | **$2.7\text{ Gbps}$** |
| **Model Inference Latency** | N/A | $18.4\text{ ms}$ | **$1.85\text{ ms}$ (TorchScript)** |

---

## 4. Reproduction

```bash
# Clone repository
git clone https://github.com/Himani-ai/gnn-spatial-routing.git
cd gnn-spatial-routing

# Install requirements
pip install -r requirements.txt

# Run training on NSFNET topology
python train.py --topology nsfnet --epochs 100 --device cuda
```
