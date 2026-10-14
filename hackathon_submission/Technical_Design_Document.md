# Krishna Defence System / Garuda AI
## Technical Design & Architecture Document
**Hack for Social Cause 2026**  
**Theme:** *AI, GIS & Emerging Technologies (Predictive Models & Decision Support Systems for Critical Infrastructure & Societal Protection)*

---

### Executive Summary: World-Model Predictive Cyber Defence
**Learn the evolving network state, simulate multiple future states, explain the risk trajectory, and route evidence into controlled defensive action.**

Krishna Defence System is the complete platform. Garuda AI is its forecasting core. The separate Krishna Investigate lane handles unknown / unsupported evidence so the platform name and the triage function are not confused.

---

### 01 / End-to-End System Architecture

```
PCAP / Flow → Feature Extraction → Graph State → GNN → LSTM → Future Trajectory → MITRE / Explainability → Controlled Response
```

```
+-----------------------------------------------------------------------------------------------------------------------+
|                                              KRISHNA DEFENCE SYSTEM                                                   |
|                                       WORLD-MODEL PREDICTIVE CYBER DEFENCE                                            |
+-------------------+--------------------+--------------------+--------------------+------------------------------------+
| 01 / INGEST       | 02 / EXTRACT       | 03 / STATE         | 04 / SPATIAL       | 05 / TEMPORAL                      |
| PCAP + Flow       | Feature Pipeline   | Graph State        | GNN / GraphSAGE    | Garuda AI · LSTM                   |
| Classic PCAP,     | Flow + packet      | Host/service/proto | Relational message | Learns transition dynamics         |
| flow CSV, NetFlow | features, 10s      | graph, node/edge   | passing, spatial   | P(S_t+1 | S_t) from 8 observed     |
| telemetry         | graph windows      | ordered state S_t  | state embedding    | windows (~80s context)             |
+-------------------+--------------------+--------------------+--------------------+------------------------------------+
                                                         |
                                                         v
+----------------------------------------+---------------------------------------+--------------------------------------+
| 06 / FORWARD SIMULATION                | 07 / INTERPRET                        | 08 / DECISION                        |
| Autoregressive Rollout                 | Stage + Explainability                | Controlled Response Router           |
| Predicted state feeds next step:       | MITRE-aligned stage only when         | Separates known response, unknown    |
| +10s -> +20s -> +30s -> +40s           | supervised support exists; local      | triage and signed containment.       |
| future state + uncertainty + risk      | gradient x input feature attribution  | High risk alone never authorizes.    |
+----------------------------------------+---------------------------------------+--------------------------------------+
                                                                                                    |
                                                                                                    v
                                                 +--------------------+---------------------+--------------------+
                                                 | ARJUNA             | KRISHNA INVESTIGATE | SUDARSHANA         |
                                                 | Known / reviewed   | Unknown/unsupported | Authorized         |
                                                 | policy-gated block | triage + evidence   | containment        |
                                                 +--------------------+---------------------+--------------------+
```

#### Why This is a World Model
* **Models an ordered network state** instead of classifying individual flows independently.
* **Learns $P(S_{t+1} \mid S_t)$** and performs autoregressive K-step forward simulation (+10s, +20s, +30s, +40s).
* **Keeps forecasting, stage interpretation and actuation** as separate evidence lanes.
* **Abstains / stays shadow-only** when runtime support is unresolved (fail-closed safety).

#### Reference Tech Stack
* **Model:** Python 3.12 + NumPy reverse-mode GraphSAGE/GNN + LSTM; no heavy PyTorch/PyG dependency in the reference runtime.
* **Training / Eval:** scikit-learn, pandas, pytest; multi-seed experiments and frozen evidence manifests.
* **Serving / UI:** Python standard-library HTTP services + authenticated local bridge; HTML5/CSS3/vanilla JavaScript dashboard (`console.html`).
* **Audit / Control:** SQLite audit chain + HMAC-signed policy files; optional Node.js proxy for owned-lab enforcement.

---

### 02 / One Inference Cycle: 5 Explicit Stages

1. **Observe:** 8 ordered graph windows (~80 s observed context).
2. **Validate:** Schema • Chronology • Mode • Runtime Support.
3. **Encode:** GNN spatial representation + LSTM temporal dynamics state.
4. **Forecast:** +10 / +20 / +30 / +40 s forward state rollout + risk evidence.
5. **Explain & Route:** Feature drivers (Gradient x Input / SHAP) • MITRE stage status • Controlled response routing.

---

### 03 / Subsystem Responsibilities & Safety Boundaries

| Subsystem | Responsibility | Output / Boundary |
|---|---|---|
| **Telemetry + Parser** | Ingests PCAP / flow telemetry; normalizes timestamps and packet/flow evidence into versioned schema. | Unsupported formats or missing required evidence fail explicitly. |
| **State / Graph Builder** | Creates 10-second graph snapshots and ordered histories from service / host / protocol relationships. | Sequence gaps break histories; future windows never enter observed context. |
| **Garuda Core** | GraphSAGE/GNN encodes relational state; LSTM learns temporal transition dynamics from observed history. | State forecasting remains separate from risk, stage and response claims. |
| **Forward Simulation** | Autoregressively rolls predicted state to +10 / +20 / +30 / +40 seconds forward. | Future state + uncertainty + risk evidence; not a guaranteed compromise path. |
| **Interpretability / Stage** | Ranks local feature drivers; MITRE-aligned stage is shown only when supervised support exists. | Unresolved / insufficient evidence is preferred to an invented stage. |
| **Defence + Audit** | Arjuna = reviewed known-response; Krishna Investigate = unknown triage; Sudarshana = authorized containment. | Human/policy gates, expiry, revoke/kill-switch and audit prevent blind auto-blocking. |

---

### 04 / Deployment, Feasibility & Scale

#### Deployment Topology
* **Pipeline:** Sensor / TAP / SPAN → Local Collector → Graph Builder → Garuda Inference → Analyst Console → Optional Enforcement Proxy.
* **Offline-First:** Inference runs fully offline; no cloud API or external internet dependency.
* **Authenticated APIs:** Local authenticated APIs keep runtime credentials strictly server-side.
* **Reproducibility:** Model/input hashes and versioned artifacts support verifiable reproducibility.
* **Enterprise / CII Applicability:** Critical Information Infrastructure deployment remains human-gated until domain validation is complete.

#### Feasibility & Scale Path
* **Capacity Controls:** Reference controls support up to 32 graph nodes, ~250k CSV rows and ~10k occupied windows; larger networks use subnet partitioning rather than silent truncation.
* **CPU Efficiency:** Measured compatible CPU runtime: **~2.9 ms mean forecast latency**, 332–349 sequential calls/s in-process, ~45 MiB max RSS.
* **Scale Path:** Parallel sensor workers + subnet/service graph partitions + central evidence dashboard; benchmark on target hardware before production deployment.

---

### 05 / Hack for Social Cause Requirement Mapping

| Track Requirement | Implemented Architecture | Visible Verification |
|---|---|---|
| **Structured Network State** | 10-second service/host/protocol graph snapshots with flow + packet evidence. | State / Graph Builder |
| **Learn State Dynamics $P(S_{t+1} \mid S_t)$** | GraphSAGE spatial encoder + LSTM temporal transition model. | Garuda Core |
| **K-step Future Simulation** | Autoregressive +10s / +20s / +30s / +40s future-state rollout. | Forecast Trajectory UI |
| **Explainable AI & Stage Mapping** | Gradient x input drivers; MITRE stage output only with supervised support. | Stage / Attribution UI |
| **Decision Support & Containment** | Tiered response routing: Arjuna (Known Block), Krishna (Triage), Sudarshana (Quarantine). | Controlled Response Layer |
| **Offline Demo + Baselines** | Local PCAP/CSV inference; Logistic Regression, LSTM, and Persistence comparisons. | Dashboard (`console.html`) |

---

### 06 / Measured Experimental Results

* **CICAPT-IIoT2024:** **~27.5% lower state MSE** vs persistence baseline on Phase-2 forecasting across 3 fixed seeds.
* **UNSW-NB15:** **~98.7% recall · ~0.29% FPR · ~94.8% F1 score** under frozen same-input independent-source protocol.
* **CPU Runtime:** **~2.9 ms mean latency** forecast-only compatible in-process benchmark.

#### Evaluator Takeaway
> **Krishna Defence is not a static IDS dashboard.** It is an AI-powered network-state simulation pipeline that learns how traffic evolves over time, forecasts multiple future states, explains the underlying drivers, and connects that evidence to layered, human-gated cyber-defence controls to protect societal and critical public infrastructure.
