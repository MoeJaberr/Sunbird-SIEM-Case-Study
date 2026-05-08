# Sunbird SIEM Case Study

> A public-facing case study documenting my attempt to design a SIEM from scratch while keeping the private Sunbird source code confidential.

## Repository Purpose

This repository documents my attempt to design a SIEM from scratch to deepen and demonstrate my understanding of security engineering, detection engineering, telemetry pipelines, threat intelligence workflows, and SOC operations.

Sunbird is a self-directed SIEM/security platform design project aimed at small businesses that handle sensitive data but have limited IT and security staffing. The goal is to reason through the architecture, security tradeoffs, telemetry model, detection logic, alert workflow, and deployment constraints behind a practical security monitoring platform.

This repository is documentation-only. It does not contain proprietary source code, private implementation details, secrets, credentials, internal configuration, or production data.

**Private source code:** Available upon request.

## Project Status

Sunbird is currently an active design and prototype effort, not a production-deployed commercial platform.

The current focus is documenting and refining the architecture, security model, detection engineering approach, telemetry pipeline, and operational workflows. The case study is intended to show security engineering judgment, design reasoning, and implementation planning rather than claim a finished production system.

## Target Users

Sunbird is designed around the needs of:

- Small businesses with sensitive data
- Small IT teams without dedicated SOC capacity
- Organizations that need practical security visibility without enterprise SIEM complexity
- Teams that need endpoint, network, alert, and case workflows in one self-hostable system

## Architecture Overview

Sunbird is designed as a modular SIEM/security platform with separate components for endpoint telemetry, network telemetry, ingestion, detection, storage, API access, and analyst workflows.

| Area | Planned / Prototype Technology |
|---|---|
| Endpoint agent | Go |
| Ingest service | Go |
| Detection engine | Python 3.11 |
| Detection rules | Sigma-style and custom YAML rules |
| API server | Go REST/WebSocket API with JWT authentication |
| NIDS sensor | Go with libpcap |
| Frontend | React 18, TypeScript, Tailwind CSS, shadcn/ui |
| Event store | ClickHouse |
| Config and cases store | PostgreSQL |
| Queue | Kafka in KRaft mode |
| Community deployment | Docker Compose |
| Enterprise deployment concept | Helm chart |
| CI/CD | GitHub Actions |
| Container registry | GHCR |

## Core Design Goals

Sunbird is being designed around the following engineering goals:

- Security-first architecture
- Clear trust boundaries between components
- Input validation at ingestion, API, and rule-processing layers
- Secret-handling discipline
- Auditability for analyst and system actions
- Safe defaults for deployment and configuration
- Detection engineering workflows based on structured rules
- MITRE ATT&CK mapping for detection coverage
- Endpoint and network telemetry normalization
- Threat intelligence ingestion and enrichment
- Alert triage and case management workflows
- SOAR-style playbook concepts
- CI/CD security validation before release

## What This Case Study Demonstrates

This repository is intended to demonstrate practical security engineering ability across multiple domains.

### Security Engineering

- Designing trust boundaries between agents, ingest services, APIs, databases, and analyst-facing interfaces
- Identifying abuse cases and misuse paths
- Reasoning about authentication, authorization, validation, and audit controls
- Separating sensitive implementation details from public documentation

### Detection Engineering

- Designing Sigma-style and custom YAML detection rules
- Mapping detection logic to MITRE ATT&CK techniques
- Thinking through false positives, severity, enrichment, and triage context
- Building detection workflows around endpoint and network telemetry

### SOC Engineering

- Modeling alert lifecycle stages from ingestion to investigation
- Designing analyst-facing alert and case management workflows
- Capturing triage decisions, evidence, status changes, and remediation notes
- Planning SOAR-style response playbooks for repeatable investigations

### Cloud and Platform Security

- Designing containerized deployment workflows
- Considering CI/CD security checks
- Planning image publishing through GHCR
- Separating community self-hosted deployment from enterprise deployment concepts

### Product Security

- Considering secure defaults for small teams
- Designing around usability constraints without ignoring security controls
- Thinking through auditability, access control, and operator safety

## Repository Contents

```text
docs/
  architecture.md              System architecture, services, trust boundaries, and data flow
  threat-model.md              Abuse cases, assets, trust boundaries, and mitigations
  detection-engineering.md     Detection rule design, ATT&CK mapping, severity, and triage logic
  telemetry-pipeline.md        Endpoint, network, ingest, queue, storage, and normalization flow
  ci-cd-security.md            Planned CI/CD security validation and release safety controls
  sample-alert-triage.md       Example alert investigation workflow
  resume-bullets.md            Resume-ready project bullets

diagrams/
  Placeholder location for architecture and data-flow diagrams

screenshots/
  Placeholder location for sanitized UI mockups or prototype screenshots

samples/
  Sanitized sample alerts, telemetry records, detection rules, and case records

roadmap/
  Planned implementation steps, known limitations, and resource constraints

references/
  MITRE ATT&CK mapping notes, security principles, and reading list
```

## High-Level Data Flow

```text
Endpoint Agent / NIDS Sensor
        |
        v
Ingest Service
        |
        v
Kafka Queue
        |
        v
Detection Engine
        |
        +------------------+
        |                  |
        v                  v
ClickHouse Event Store   PostgreSQL Config / Cases Store
        |                  |
        +--------+---------+
                 |
                 v
          Go API Server
                 |
                 v
        React Analyst Interface
```

## Security Themes

Sunbird is being designed with the assumption that SIEM infrastructure itself becomes a high-value target. The design therefore emphasizes:

- Minimizing implicit trust between services
- Treating telemetry as untrusted input
- Validating and normalizing data before detection logic consumes it
- Avoiding secrets in repositories, logs, containers, and sample artifacts
- Maintaining audit trails for analyst and system actions
- Designing safe defaults for self-hosted deployments
- Keeping public documentation separate from private implementation details

## Detection Engineering Approach

The detection engine is planned around a combination of Sigma-style rules and custom YAML rules. The goal is to make detection logic understandable, reviewable, and mappable to attacker behavior.

Planned detection metadata includes:

- Rule ID
- Rule title
- Description
- Severity
- Data source
- MITRE ATT&CK tactic
- MITRE ATT&CK technique
- Required fields
- Detection condition
- False-positive notes
- Triage guidance
- Suggested response actions

Example detection categories:

- Brute-force authentication attempts
- Suspicious PowerShell execution
- Unusual outbound network connections
- Port scanning behavior
- Potential credential access activity
- Endpoint process anomalies
- Threat intelligence indicator matches

## Threat Modeling Approach

The threat model focuses on realistic abuse cases for a SIEM platform, including:

- Forged or malformed telemetry
- Compromised endpoint agents
- Unauthorized API access
- JWT misuse or token theft
- Detection rule tampering
- Alert suppression or manipulation
- Poisoned threat intelligence feeds
- Sensitive data exposure through logs or samples
- Container and deployment misconfiguration
- CI/CD pipeline compromise

Each threat is intended to be documented with:

- Asset affected
- Threat scenario
- Potential impact
- Existing or planned mitigation
- Residual risk
- Follow-up implementation work

## CI/CD Security Direction

The project is intended to use GitHub Actions and GHCR for validation and container publishing. The public case study documents the planned security controls without exposing private workflows or secrets.

Planned CI/CD security checks include:

- Dependency scanning
- Static analysis
- Secret scanning
- Container image scanning
- Linting and formatting checks
- Unit and integration test gates
- Rule validation for detection content
- Infrastructure/deployment configuration checks
- Signed or controlled release artifacts as a future improvement

## Free and Resource-Constrained Development

A major challenge in building Sunbird is that it is being developed as a self-directed project without startup funding, paid cloud infrastructure, dedicated threat intelligence feeds, commercial data sources, or managed AI features.

That creates real constraints:

- No always-on cloud environment for large-scale testing
- Limited ability to simulate enterprise telemetry volume
- No paid threat intelligence enrichment pipeline
- No commercial endpoint telemetry dataset
- No dedicated QA, detection research, or infrastructure team
- No proprietary AI triage or alert summarization features
- Limited capacity to benchmark against funded SIEM vendors

These constraints shape the project. Sunbird is being designed around self-hosted deployment, open-source infrastructure, careful documentation, and practical security engineering tradeoffs rather than expensive managed services.

This also makes the project a useful engineering exercise: it requires prioritizing fundamentals such as telemetry quality, detection logic, trust boundaries, auditability, deployment safety, and analyst workflow before adding advanced features.

## Next Implementation Priorities

The next planned areas of work are:

1. Finalize the public case study documentation structure
2. Add architecture and trust-boundary diagrams
3. Document the initial telemetry schema for endpoint and network events
4. Add sanitized sample alerts and case records
5. Define a small set of Sigma-style detection rules
6. Map initial detections to MITRE ATT&CK
7. Document the alert triage workflow from detection to case closure
8. Expand the abuse-case threat model
9. Add CI/CD security validation notes
10. Create sanitized screenshots or UI mockups for analyst workflows
11. Document deployment assumptions for Docker Compose
12. Outline the Helm chart concept without exposing private enterprise implementation details

## What Is Not Included

This repository intentionally does not include:

- Private Sunbird source code
- Proprietary implementation details
- Secrets, credentials, tokens, or keys
- Production telemetry
- Customer data
- Private configuration files
- Internal deployment manifests
- Unredacted logs
- Claims that the system is production-deployed

## Intended Resume Positioning

This project can be summarized on a resume as:

> Designed and documented a self-directed SIEM case study covering endpoint and network telemetry, Go/Python ingestion and detection services, Sigma-style rules, MITRE ATT&CK mapping, alert triage, case management, and CI/CD security validation.

A stronger security-focused version:

> Built a public SIEM design case study documenting security-first architecture, telemetry pipelines, detection engineering workflows, abuse-case analysis, trust boundaries, and SOC triage processes while keeping proprietary implementation code private.

## Repository Disclaimer

This repository is a public case study for learning, documentation, and portfolio purposes. It is not a production security product, managed service, or commercial SIEM offering.

The private Sunbird source repository is separate and is available upon request.
