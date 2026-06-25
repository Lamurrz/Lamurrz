# Lori Murray, Ph.D. â€” AI Security Architecture Portfolio

Senior cybersecurity professional with 20 years of experience spanning enterprise security architecture, AI/ML security research, detection engineering, and systems security engineering across defense, cloud, and commercial technology sectors.


---

## AI Security Engineering Portfolio

Six interconnected projects composing an end-to-end AI security engineering pipeline:

```
Raw vendor logs
      â”‚
      â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚   OCSF Transformer  â”‚  Normalize â†’ OCSF 1.3.0
â”‚   Entra Â· Okta      â”‚  Windows Â· Wiz Â· PAN
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
           â”‚  OCSF events
           â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚   CyberGraph-AD     â”‚  Detect â†’ behavioral anomalies
â”‚   Graph fusion Â· AE â”‚  (dissertation framework)
â”‚   + Isolation Forestâ”‚  NSL-KDD AUC=0.941
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
           â”‚  OCSF Detection Findings
           â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”     â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚   Meridian KG        â”‚     â”‚   Meridian          â”‚
â”‚   ATLAS/ATT&CK       â”‚â”€â”€â”€â”€â–ºâ”‚   Risk Scoring API  â”‚  Assess â†’ threat exposure
â”‚   4,641 nodes        â”‚     â”‚   9 REST endpoints  â”‚
â”‚   40,920 edges       â”‚     â”‚   + bridge          â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜     â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
           â–²                            â”‚
           â”‚   TTP enrichment           â”‚  Adjusted risk scores
           â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                        â”‚  Risk scores Â· attack paths
                                        â–¼
                             â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                             â”‚   AI CSF Profiler   â”‚  Comply â†’ NIST CSF 2.0
                             â”‚   6 functions Â· PDF â”‚  AI RMF Â· ISO 42001
                             â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
                                        â”‚  Coverage gaps
                                        â–¼
                             â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
                             â”‚  Meridian Emulation â”‚  Validate â†’ detection coverage
                             â”‚  Atomic Red Team    â”‚  697 ATT&CK techniques
                             â”‚  + CyberGraph-AD    â”‚  dry-run + live execution
                             â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

**The narrative: normalize â†’ detect â†’ assess â†’ comply â†’ validate.**

---

### [OCSF Transformer](https://github.com/Lamurrz/ocsf-transformer)

Normalizes raw vendor logs into OCSF 1.3.0 â€” the common schema for modern SIEMs and security data lakes.

- Five vendors: Microsoft Entra ID Â· Okta Â· Windows Security Event Log Â· Wiz Â· Palo Alto PAN-OS
- Four OCSF classes: Authentication (3002) Â· Process Activity (1007) Â· Account Change (5001) Â· Configuration Finding (5019)
- Zero dependencies â€” standard library only, runs anywhere Python 3.10+ is available
- Deterministic UUIDs enable upsert semantics in Iceberg/Delta without deduplication scans

---

### [CyberGraph-AD](https://github.com/Lamurrz/cybergraph-ad)

Multisensor behavioral anomaly detection built on a Neo4j property graph fusion architecture, directly implementing the framework from my 2019 Iowa State dissertation:

> *"A Framework Towards Fusing Multisensory Cyber Security Data Utilizing Graph Databases"*

- Six anomaly types: brute force, credential stuffing, lateral movement, data exfiltration, privilege escalation, off-hours access
- v2 ensemble detector: autoencoder + Isolation Forest, 16 behavioral features, trained on normal traffic only
- OCSF Detection Finding (class_uid 2004) output for SIEM ingestion
- **`pipeline.py`** â€” wires OCSF Transformer output directly into the fusion graph; processes all 5 vendor sources end-to-end with a single command; forwards Detection Findings to the Meridian Bridge automatically
- **Benchmark results** (two standard IDS datasets):

| Dataset | AUC-ROC | F1 | Notes |
|---------|---------|-----|-------|
| NSL-KDD | **0.941** | 0.878 | Classic benchmark â€” competitive with supervised baselines |
| UNSW-NB15 | **0.771** | â€” | Modern benchmark â€” 20% AUC improvement over v1 |

---

### [Meridian Knowledge Graph](https://github.com/Lamurrz/meridian-atlas-attack-kg)

Neo4j property graph encoding the full MITRE ATLAS + MITRE ATT&CK threat framework with an AI asset inventory and live vulnerability intelligence layer.

- 4,641 nodes Â· 40,920 edges from MITRE ATT&CK Enterprise + MITRE ATLAS STIX 2.1 bundles
- Three ATLAS tactics with no ATT&CK peer modeled as ATLAS-only nodes with `ENABLES` edges
- Validity-scoped `TARGETS` and `MITIGATES` edges prevent over-counting controls
- NVD CVE integration links ML framework vulnerabilities to asset nodes

---

### [Meridian Risk Scoring API](https://github.com/Lamurrz/meridian-api)

FastAPI REST query layer over the Meridian Knowledge Graph. Computes quantitative AI asset risk scores using a parallel failure mode model.

- 7 REST endpoints with auto-generated Swagger UI
- Risk score formula: R = (1 âˆ’ âˆ(1 âˆ’ Páµ¢)) Ã— criticality Ã— 10
- Attack path queries: which threat actors target which assets, through which techniques
- Control gap analysis: techniques targeting assets with no active mitigation

---

### [AI CSF Profiler](https://github.com/Lamurrz/ai-csf-profiler)

NIST CSF 2.0 profile builder and gap evaluator for AI systems, with live integration into the Meridian Risk API and CyberGraph-AD for evidence-backed subcategory assessment.

- **32 subcategories** across all 6 CSF 2.0 functions: Govern Â· Identify Â· Protect Â· Detect Â· Respond Â· Recover
- AI-specific informative references: NIST AI RMF Â· ISO/IEC 42001 Â· MITRE ATLAS Â· OWASP LLM Top 10
- Meridian integration auto-populates subcategory evidence from live asset and risk data
- Outputs: interactive CLI assessment Â· structured JSON Â· formatted PDF gap report

---

### [Meridian Emulation](https://github.com/Lamurrz/meridian-emulation)

ATT&CK technique emulation and detection validation pipeline with two integration paths: Atomic Red Team for host-based execution and MITRE Caldera for agent-based adversary emulation.

- Pulls control gap techniques from Meridian `/controls/gaps` â€” prioritizes unmitigated paths
- **Atomic Red Team path** â€” 697 ATT&CK techniques, 91 AI-relevant; dry-run and live execution via subprocess
- **Caldera Scope A** â€” generates adversary profile YAMLs from Meridian control gaps; verified ability IDs from live Caldera instance
- **Caldera Scope B** â€” pushes profiles programmatically via Caldera REST API; profiles appear in Caldera UI ready to run against agents
- Validates detection coverage by checking CyberGraph-AD findings for matching anomaly types
- Coverage matrix + gap report: detected / missed / undetected control gaps

---

## Additional Projects

### [ArchLens](https://github.com/Lamurrz/arch-lens)

DoDAF-inspired architecture modeling tool that transforms free-text personas and use cases into structured architectural views using the Claude API.

- **7 DoDAF views:** OV-2 Â· OV-5b Â· OV-6c Â· SV-6 Â· SV-1 Â· SV-4 Â· DIV-2
- Decomposition methodology: capabilities â†’ functions â†’ actor/input/process/output/destination sequences
- React + TypeScript frontend with Zustand state persistence Â· FastAPI backend stub ready

---

*More projects coming â€” spanning security data engineering, AI governance tooling, and applied ML security research.*

---

## Core domains

`NIST AI RMF` Â· `MITRE ATLAS/ATT&CK` Â· `OWASP LLM Top 10` Â· `Zero Trust Architecture` Â· `Adversarial ML` Â· `SABSA` Â· `ISO/IEC 42001` Â· `SIEM/SOAR` Â· `DevSecOps` Â· `DSPM` Â· `AISPM` Â· `DoDAF` Â· `Agentic AI Security` Â· `RMF/FedRAMP`

---

*Last updated: June 2026*

