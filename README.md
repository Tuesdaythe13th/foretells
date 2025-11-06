# FORETELLS

An advanced multi-agent hybrid forecasting architecture that prioritizes governance, memory, and epistemic diversity over single-model prediction. FORETELLS introduces a Live Cognitive Organization (LCO): a governed multi-agent system that fuses live world perception, agentic specialization, interpretable critique, and long-horizon memory with learning. The stack uses Pathway-style data connectors, hybrid semantic grounding, expert cadres including a quantum-inspired anomaly detector, Baby Dragon Hatchling interpretability circuits, and Mem0 persistent graph memory. Governance is integrated into inference, with auditable calibrated outputs bound to explicit policy triggers.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Tuesdaythe13th/foretells/blob/main/APART_ARTIFEX_FORETELLS_QVR_Minimal_Operable_Demo.ipynb)

---

## Project status

This repository contains the V10.1 Constitutional mode demo prepared for Apart Research’s AI Forecasting Hackathon, Nov 2025. It packages the core ideas from the V7 agenda into a governed multi-agent architecture with auditability and reflexive learning.

> This README accompanies the submission for Apart Research’s [AI Forecasting Hackathon](https://apartresearch.com/sprints/the-ai-forecasting-hackathon-2025-10-31-to-2025-11-02).

Related materials:
- R&D Document: <https://docs.google.com/document/d/1Q_4dvAc8CDkY8HHglbRuUUSO_gvnBuSVOL01HXK1tds/edit>
- Slide deck: <https://www.canva.com/design/DAG3l1Cws08/fdLDY9VwqbdBFmtwn5fyVw/edit>

---

## Why this exists

Classic forecasting fails on three fronts:
1. Reflexivity blindness: systems shape the future they predict
2. Regime change ignorance: stationary assumptions collapse during phase shifts
3. Epistemological monism: one model is not a worldview

FORETELLS addresses these by enforcing pluralism, explicit governance, and audit trails.

---

## High-level architecture

- **Expert Cadre**: specialized agents produce forecasts and evidence
- **CritiqueBot**: non-voting constitutional conscience that audits every forecast pre-synthesis
- **QVR Detector**: quantum-inspired regime shift signal used as an early warning indicator
- **Mem0Graph**: immutable graph memory that stores signed manifests of forecasts, evidence, critiques, and decisions
- **Reflexive Learner (L9 MWU)**: updates agent credibility using scoring rules once outcomes are known

---

## Core pillars and features

### Pillar 1. Hybrid forecasting and early warning (QVR)
- Combines a robust classical baseline (Prophet) with quantum-inspired signals
- Computes a decoherence rate \( D_\tau \) as a progress indicator for instability or acceleration in the time series
- Uses \( D_\tau \) to gate and annotate downstream synthesis

### Pillar 2. Constitutional governance with veto authority
- CritiqueBot audits every forecast for interpretability, evidence quality, and policy compliance
- Hard policy veto if both conditions hold:
  - \( D_\tau \ge \texttt{DECOH\_VETO} \)
  - ensemble overconfidence \( \ge \texttt{OVERCONFIDENCE\_THRESHOLD} \)
- Monitors epistemic diversity using Shannon entropy and flags low diversity states

### Pillar 3. Institutional memory and reflexive learning
- Mem0Graph writes a cryptographically signed manifest per episode
- L9 computes agent Brier scores after outcomes are known and feeds Multiplicative Weights Update to reweight agents

---

## Judging criteria mapping

| Judging Criterion | V10.1 Feature | Why it matters |
| --- | --- | --- |
| Forecasting relevance | QVR decoherence \( D_\tau \) | Provides a measurable progress indicator for regime shifts that aligns with capability acceleration risk |
| Strategic value | Constitutional veto in synthesis | Fails safely when uncertainty is extreme and confidence is high, which is where automated systems cause the worst damage |
| Execution quality | Mem0Graph with signed manifests | End-to-end auditability with verifiable lineage and replayable decisions |
| Track coverage | Tracks 1, 3, and 4 | Capability modeling, early warning, and governance are integrated as first-class citizens |

---

## Configuration overview

All parameters live in `config` and are surfaced in the notebook’s Explainr.

### Core parameters
- `SEED`: reproducibility
- `INTERVAL_WIDTH`: confidence interval width for time series baselines
- `DEEP_RESEARCH_FLAG`: threshold to trigger autonomous DeepResearchBot

### Governance thresholds
- `DECOHERENCE_THRESHOLD`: sensitivity baseline for regime shift detection
- `DECOH_ESCALATE`: prudence boost and governance alert trigger
- `DECOH_VETO`: synthesis veto threshold
- `CONSENSUS_LOW_H`: low entropy warning for epistemic diversity
- `CONSTITUTIONAL_REVIEW_H`: threshold for mandatory human oversight
- `OVERCONFIDENCE_THRESHOLD`: veto condition when uncertainty is high and the ensemble is overconfident

### Directories and I O
- `LOG_DIRECTORY`: signed manifests and constitutional alerts
- `DOC_ROOT`: incoming documents and evidence inbox
- `MEM0_GRAPH_PATH`: persistent graph memory store

### Historical credibility
- `HISTORICAL_CREDIBILITY`: priors for agent reliability used by MWU

---

## Provenance and logging

Each forecast episode produces a signed JSON manifest:
- full agent outputs
- supporting evidence
- governance review results
- final decision with veto or approval
- `manifest_hash` as the sha256 of a deterministic JSON serialization

Example helper contract:

```python
def sha256_hash(text: str) -> str:
    import hashlib
    return hashlib.sha256(text.encode("utf-8")).hexdigest()

def add_forecast_episode(self, manifest: dict) -> str:
    manifest_string = json.dumps(manifest, sort_keys=True, default=str)
    manifest_hash = manifest.get("manifest_hash") or sha256_hash(manifest_string)
    with_hash = dict(manifest, manifest_hash=manifest_hash)
    path = os.path.join(config["LOG_DIRECTORY"], f"{manifest_hash}.json")
    save_json_util(with_hash, path)
    self.graph.add_node(manifest_hash, **with_hash)
    self.graph.add_edge("root", manifest_hash, relationship="forecast_episode")
    return manifest_hash
