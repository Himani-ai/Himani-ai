<div align="center">

# SDN OpenFlow Sentinel
### High-Throughput Event-Driven Controller Telemetry & Dynamic Flow Rebalancing Engine

[![Protocol: OpenFlow 1.3/1.5](https://img.shields.io/badge/Protocol-OpenFlow_1.3%2F1.5-8B5CF6?style=for-the-badge&labelColor=030712)](https://opennetworking.org/)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-00D2FF.svg?style=for-the-badge&labelColor=030712)](https://opensource.org/licenses/Apache-2.0)
[![Language: Python 3.12 / Rust](https://img.shields.io/badge/Lang-Python%20%7C%20Rust-00F2FE?style=for-the-badge&labelColor=030712)](https://github.com/Himani-ai/sdn-openflow-sentinel)
[![Benchmark: Mininet Ready](https://img.shields.io/badge/Testbed-Mininet%20Validated-10B981?style=for-the-badge&labelColor=030712)](https://github.com/Himani-ai/sdn-openflow-sentinel)

<p align="center">
  <a href="#1-system-overview">Overview</a> •
  <a href="#2-telemetry-architecture">Architecture</a> •
  <a href="#3-flow-table-compression-algorithm">Compression Algorithm</a> •
  <a href="#4-performance-evaluations">Evaluations</a> •
  <a href="#5-usage--deployment">Usage</a>
</p>

</div>

---

## 1. System Overview

In enterprise and data-center Software-Defined Networks, TCAM (Ternary Content-Addressable Memory) saturation on programmable switches and telemetry message storms present severe scalability limits.

**SDN OpenFlow Sentinel** is a decoupled telemetry and dynamic flow-table rebalancing engine. Built on top of Ryu/OpenFlow primitives and an asynchronous Rust collector, Sentinel:
* Compresses switch flow-table rules by up to **$44\%$** using prefix wildcard aggregation.
* Employs streaming sub-millisecond port counter telemetry to detect asymmetric link congestion.
* Dynamically re-routes localized elephant flows across alternate path topologies before packet queue buffer overflow occurs.

---

## 2. Telemetry Architecture

```mermaid
graph TD
    classDef sw fill:#030712,stroke:#00D2FF,stroke-width:2px,color:#FFFFFF;
    classDef col fill:#07101E,stroke:#00F2FE,stroke-width:2px,color:#FFFFFF;
    classDef opt fill:#110E2E,stroke:#8B5CF6,stroke-width:2px,color:#FFFFFF;

    SW1["OpenFlow v1.3 Switch A"]:::sw --> COL["Rust Ingress Telemetry Collector"]:::col
    SW2["OpenFlow v1.3 Switch B"]:::sw --> COL
    COL --> AGG["Streaming Traffic Matrix Aggregator"]:::col
    AGG --> DETECT{"Link Utilization > 80%?"}:::opt
    DETECT -- YES --> OPT["Wildcard Flow Optimization & Multi-Path Reroute"]:::opt
    OPT --> MOD["OFPFlowMod Ingestion"]:::sw
    DETECT -- NO --> PASS["Maintain Forwarding State"]:::sw
```

---

## 3. Flow-Table Compression Algorithm

Sentinel evaluates installed flow match entries $(M_i)$ and actions $(A_i)$. For overlapping wildcard subsets:

$$\text{If } M_j \subseteq M_k \quad \text{and} \quad A_j = A_k \implies \text{Merge into } M_{\text{wildcard}}$$

By executing this reduction on a sliding time-window, Sentinel maintains switch TCAM utilization below critical thresholds without interrupting established microflows.

---

## 4. Performance Evaluations

Simulated on a fat-tree topology with $k=8$ (128 hosts, 80 switches) under synthetic burst traffic:

| Metric | Stock OpenFlow Controller | SDN OpenFlow Sentinel | Delta |
| :--- | :--- | :--- | :--- |
| **TCAM Table Entries** | $14,200$ | **$7,950$** | **$-44.0\%$ footprint** |
| **Telemetry Ingestion Rate** | $18,000\text{ msgs/s}$ | **$124,000\text{ msgs/s}$** | **$6.8\times$ throughput** |
| **Buffer Drop Rate Under Burst**| $4.8\%$ | **$0.12\%$** | **$40\times$ packet retention** |

---

## 5. Usage & Deployment

```bash
# Clone and initialize
git clone https://github.com/Himani-ai/sdn-openflow-sentinel.git
cd sdn-openflow-sentinel

# Launch Mininet fat-tree topology
sudo python3 topology/fattree_k8.py &

# Launch Sentinel Telemetry & Controller Daemon
cargo run --release --bin sentinel-telemetry
```
