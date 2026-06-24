# Lori Murray, Ph.D. — AI Security Architecture Portfolio

Senior cybersecurity professional with 20 years of experience spanning enterprise security architecture, AI/ML security research, detection engineering, and systems security engineering across defense, cloud, and commercial technology sectors.


---

## AI Security Engineering Portfolio

Six interconnected projects composing an end-to-end AI security engineering pipeline:

```
Raw vendor logs
      │
      ▼
┌─────────────────────┐
│   OCSF Transformer  │  Normalize → OCSF 1.3.0
│   Entra · Okta      │  Windows · Wiz · PAN
└──────────┬──────────┘
           │  OCSF events
           ▼
┌─────────────────────┐
│   CyberGraph-AD     │  Detect → behavioral anomalies
│   Graph fusion · AE │  (dissertation framework)
│   + Isolation Forest│  NSL-KDD AUC=0.941
└──────────┬──────────┘
           │  OCSF Detection Findings
           ▼
┌──────────────────────┐     ┌─────────────────────┐
│   Meridian KG        │     │   Meridian          │
│   ATLAS/ATT&CK       │────►│   Risk Scoring API  │  Assess → threat exposure
│   4,641 nodes        │     │   9 REST endpoints  │
│   40,920 edges       │     │   + bridge          │
└──────────────────────┘     └──────────┬──────────┘
           ▲                            │
           │   TTP enrichment           │  Adjusted risk scores
           └────────────────────────────┘
                                        │  Risk scores · attack paths
                                        ▼
                             ┌─────────────────────┐
                             │   AI CSF Profiler   │  Comply → NIST CSF 2.0
                             │   6 functions · PDF │  AI RMF · ISO 42001
                             └──────────┬──────────┘
                                        │  Coverage gaps
                                        ▼
                             ┌─────────────────────┐
                             │  Meridian Emulation │  Validate → detection coverage
                             │  Atomic Red Team    │  697 ATT&CK techniques
                             │  + CyberGraph-AD    │  dry-run + live execution
                             └─────────────────────┘
```

**The narrative: normalize → detect → assess → comply → validate.**

---

### [OCSF Transformer](https://github.com/Lamurrz/ocsf-transformer)

Normalizes raw vendor logs into OCSF 1.3.0 — the common schema for modern SIEMs and security data lakes.

- Five vendors: Microsoft Entra ID · Okta · Windows Security Event Log · Wiz · Palo Alto PAN-OS
- Four OCSF classes: Authentication (3002) · Process Activity (1007) · Account Change (5001) · Configuration Finding (5019)
- Zero dependencies — standard library only, runs anywhere Python 3.10+ is available
- Deterministic UUIDs enable upsert semantics in Iceberg/Delta without deduplication scans

---

### [CyberGraph-AD](https://github.com/Lamurrz/cybergraph-ad)

Multisensor behavioral anomaly detection built on a Neo4j property graph fusion architecture, directly implementing the framework from my 2019 Iowa State dissertation:

> *"A Framework Towards Fusing Multisensory Cyber Security Data Utilizing Graph Databases"*

- Six anomaly types: brute force, credential stuffing, lateral movement, data exfiltration, privilege escalation, off-hours access
- v2 ensemble detector: autoencoder + Isolation Forest, 16 behavioral features, trained on normal traffic only
- OCSF Detection Finding (class_uid 2004) output for SIEM ingestion
- **Benchmark results** (two standard IDS datasets):

| Dataset | AUC-ROC | F1 | Notes |
|---------|---------|-----|-------|
| NSL-KDD | **0.941** | 0.878 | Classic benchmark — competitive with supervised baselines |
| UNSW-NB15 | **0.771** | — | Modern benchmark — 20% AUC improvement over v1 |

---

### [Meridian Knowledge Graph](https://github.com/Lamurrz/meridian-atlas-attack-kg)

Neo4j property graph encoding the full MITRE ATLAS + MITRE ATT&CK threat framework with an AI asset inventory and live vulnerability intelligence layer.

- 4,641 nodes · 40,920 edges from MITRE ATT&CK Enterprise + MITRE ATLAS STIX 2.1 bundles
- Three ATLAS tactics with no ATT&CK peer modeled as ATLAS-only nodes with `ENABLES` edges
- Validity-scoped `TARGETS` and `MITIGATES` edges prevent over-counting controls
- NVD CVE integration links ML framework vulnerabilities to asset nodes

---

### [Meridian Risk Scoring API](https://github.com/Lamurrz/meridian-api)

FastAPI REST query layer over the Meridian Knowledge Graph. Computes quantitative AI asset risk scores using a parallel failure mode model.

- 7 REST endpoints with auto-generated Swagger UI
- Risk score formula: R = (1 − ∏(1 − Pᵢ)) × criticality × 10
- Attack path queries: which threat actors target which assets, through which techniques
- Control gap analysis: techniques targeting assets with no active mitigation

---

### [AI CSF Profiler](https://github.com/Lamurrz/ai-csf-profiler)

NIST CSF 2.0 profile builder and gap evaluator for AI systems, with live integration into the Meridian Risk API and CyberGraph-AD for evidence-backed subcategory assessment.

- **32 subcategories** across all 6 CSF 2.0 functions: Govern · Identify · Protect · Detect · Respond · Recover
- AI-specific informative references: NIST AI RMF · ISO/IEC 42001 · MITRE ATLAS · OWASP LLM Top 10
- Meridian integration auto-populates subcategory evidence from live asset and risk data
- Outputs: interactive CLI assessment · structured JSON · formatted PDF gap report

---

### [Meridian Emulation](https://github.com/Lamurrz/meridian-emulation)

ATT&CK technique emulation and detection validation pipeline with two integration paths: Atomic Red Team for host-based execution and MITRE Caldera for agent-based adversary emulation.

- Pulls control gap techniques from Meridian `/controls/gaps` — prioritizes unmitigated paths
- **Atomic Red Team path** — 697 ATT&CK techniques, 91 AI-relevant; dry-run and live execution via subprocess
- **Caldera Scope A** — generates adversary profile YAMLs from Meridian control gaps; verified ability IDs from live Caldera instance
- **Caldera Scope B** — pushes profiles programmatically via Caldera REST API; profiles appear in Caldera UI ready to run against agents
- Validates detection coverage by checking CyberGraph-AD findings for matching anomaly types
- Coverage matrix + gap report: detected / missed / undetected control gaps

---

## Additional Projects

### [ArchLens](https://github.com/Lamurrz/arch-lens)

DoDAF-inspired architecture modeling tool that transforms free-text personas and use cases into structured architectural views using the Claude API.

- **7 DoDAF views:** OV-2 · OV-5b · OV-6c · SV-6 · SV-1 · SV-4 · DIV-2
- Decomposition methodology: capabilities → functions → actor/input/process/output/destination sequences
- React + TypeScript frontend with Zustand state persistence · FastAPI backend stub ready

---

*More projects coming — spanning security data engineering, AI governance tooling, and applied ML security research.*

---

## Core domains

`NIST AI RMF` · `MITRE ATLAS/ATT&CK` · `OWASP LLM Top 10` · `Zero Trust Architecture` · `Adversarial ML` · `SABSA` · `ISO/IEC 42001` · `SIEM/SOAR` · `DevSecOps` · `DSPM` · `AISPM` · `DoDAF` · `Agentic AI Security` · `RMF/FedRAMP`

---

*Last updated: June 2026*
