# Lori Murray, Ph.D. — AI Security Architecture Portfolio

Senior cybersecurity professional with 20 years of experience spanning enterprise security architecture, AI/ML security research, detection engineering, and systems security engineering across defense, cloud, and commercial technology sectors.

**Ph.D. Secure and Reliable Computing** · Iowa State University, 2019  
**CISSP** (active since 2010) · **SABSA** · dual M.S. in Cybersecurity Engineering and Data Analytics  
**2026 MasterClass Instructor** · COSAC/SABSA Security World Congress

---

## AI Security Engineering Portfolio

Four interconnected projects that compose into an end-to-end AI security engineering pipeline:

```
Raw vendor logs
      │
      ▼
┌─────────────────────┐
│   OCSF Transformer  │  Normalize → OCSF 1.3.0
│   Entra · Wiz · PAN │
└──────────┬──────────┘
           │  OCSF events
           ▼
┌─────────────────────┐
│   CyberGraph-AD     │  Detect → behavioral anomalies
│   Graph fusion · AE │  (dissertation framework)
└──────────┬──────────┘
           │  OCSF Detection Findings
           ▼
┌─────────────────────┐
│   Meridian +        │  Assess → threat exposure
│   Risk Scoring API  │  MITRE ATLAS/ATT&CK
└──────────┬──────────┘
           │  Risk scores · attack paths
           ▼
┌─────────────────────┐
│   AI CSF Profiler   │  Comply → NIST CSF 2.0
│   6 functions · PDF │  AI RMF · ISO 42001
└─────────────────────┘
```

**The narrative: normalize → detect → assess threat exposure → evaluate compliance.**

---

### [OCSF Transformer](https://github.com/Lamurrz/ocsf-transformer)

Normalizes raw vendor logs from Microsoft Entra ID, Wiz, and Palo Alto PAN-OS into OCSF 1.3.0 — the common schema for modern SIEMs and security data lakes.

- Zero dependencies — standard library only, runs anywhere Python 3.10+ is available
- Deterministic UUIDs enable upsert semantics in Iceberg/Delta without deduplication scans
- Auto-detection of vendor from payload shape
- Downstream query example: `WHERE class_uid = 3002 AND status_id = 2` finds all auth failures across all vendors

---

### [CyberGraph-AD](https://github.com/Lamurrz/cybergraph-ad)

Multisensor behavioral anomaly detection built on a Neo4j property graph fusion architecture, directly implementing the framework from my 2019 Iowa State dissertation:

> *"A Framework Towards Fusing Multisensory Cyber Security Data Utilizing Graph Databases"*

- Six anomaly types: brute force, credential stuffing, lateral movement, data exfiltration, privilege escalation, off-hours access
- Autoencoder trained on normal behavioral feature vectors extracted from the fusion graph
- OCSF Detection Finding (class_uid 2004) output for SIEM ingestion
- **Benchmark results on UNSW-NB15** (50K samples, 9 attack categories):

| Metric | Value |
|--------|-------|
| F1 | 0.436 |
| Precision | 0.372 |
| Recall | 0.527 |
| AUC-ROC | 0.653 |

---

### [Meridian + Risk Scoring API](https://github.com/Lamurrz/meridian-api)

REST query layer over a Neo4j MITRE ATLAS/ATT&CK knowledge graph. Cross-references threat actor TTPs, AI asset inventory, and vulnerability intelligence to compute quantitative risk scores using a parallel failure mode model.

- 4,641 nodes · 40,920 edges from MITRE ATT&CK Enterprise + MITRE ATLAS
- 7 REST endpoints with auto-generated Swagger UI
- Risk score formula: R = (1 − ∏(1 − Pᵢ)) × criticality × 10
- Asset types: AIModel · InferenceAPI · TrainingData · MLPipeline · ModelRegistry

---

### [AI CSF Profiler](https://github.com/Lamurrz/ai-csf-profiler)

NIST CSF 2.0 profile builder and gap evaluator for AI systems, with live integration into the Meridian Risk API and CyberGraph-AD for evidence-backed subcategory assessment.

- **32 subcategories** across all 6 CSF 2.0 functions: Govern · Identify · Protect · Detect · Respond · Recover
- AI-specific informative references per subcategory: NIST AI RMF · ISO/IEC 42001 · MITRE ATLAS · OWASP LLM Top 10
- Meridian integration auto-populates ID.AM-02, ID.RA-01, ID.RA-05, GV.RM-06 from live asset and risk data
- CyberGraph-AD integration auto-populates DE.CM-01, DE.CM-03, DE.AE-02, DE.AE-06 from anomaly detection telemetry
- Outputs: interactive CLI assessment · structured JSON · formatted PDF gap report

---

## Research & Publications

- Murray, L. (2019). *A Framework Towards Fusing Multisensory Cyber Security Data Utilizing Graph Databases.* Iowa State University. *(Dissertation — the foundation for CyberGraph-AD)*
- 8 peer-reviewed publications in cybersecurity and systems security

## Speaking & Teaching

- **2026 MasterClass Instructor** — COSAC/SABSA Security World Congress: *"Making Digital Empathy Real: Turning People-Centric Security a Reality"* (with Siân John MBE, EY and Dr. Char Sample)
- Active mentor — Women in Science and Engineering · Iowa State Industry Design Review Board

## Core domains

`NIST AI RMF` · `MITRE ATLAS/ATT&CK` · `OWASP LLM Top 10` · `Zero Trust Architecture` · `Adversarial ML` · `SABSA` · `ISO/IEC 42001` · `SIEM/SOAR` · `DevSecOps` · `DSPM` · `AISPM` · `DoDAF` · `Agentic AI Security` · `RMF/FedRAMP`

---

## Additional Projects

*More projects coming — spanning security data engineering, AI governance tooling, and applied ML security research.*

---

*Last updated: June 2026*
