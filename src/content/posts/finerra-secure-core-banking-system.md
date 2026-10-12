---
title: "Finnera: designing a secure core-banking and fintech platform"
excerpt: A GIKI Final Year Project combining secure microservices, core-banking integration, KYC/AML, fraud detection, and blockchain-backed audit integrity.
pubDate: 2026-10-12
topics:
  - Projects
  - Cybersecurity
  - Fintech
  - Secure Architecture
externalUrl: https://github.com/rooshanriaz/Finerra-Secure-Core-Banking-System
---

## The project

Finnera is a secure core-banking and fintech platform built as my Final Year
Project at Ghulam Ishaq Khan Institute of Engineering Sciences and Technology
(GIKI). The project explores how banking operations, security controls,
compliance workflows, fraud detection, and audit integrity can work together in
a distributed system.

The aim was not to build a single banking screen around a database. Finnera was
designed as a modular platform with independently deployable services, explicit
security boundaries, and integrations for identity, core banking, compliance,
fraud analysis, and immutable auditing.

The complete source code and setup documentation are available in the
[Finnerra Secure Core Banking System repository](https://github.com/rooshanriaz/Finerra-Secure-Core-Banking-System).

## Recognition

Finnera won **2nd place at the Industrial Open House at GIKI** in the
Department of Computer Science & Engineering. The project was evaluated by
faculty members, industry professionals, and leadership, including the Federal
Secretary for IT, among several other professionals holding key positions.

That recognition was meaningful because the project had to communicate both its
technical depth and its relevance to real financial systems: security controls,
operational workflows, compliance, and the practical constraints of deploying a
large distributed platform.

## System architecture

Finnera uses a distributed microservices architecture. The API gateway provides
a central entry point for routing, rate limiting, request mediation, and API
policy enforcement, while the domain services own focused areas of the platform.

![Finnera system architecture and methodology](https://raw.githubusercontent.com/rooshanriaz/Finerra-Secure-Core-Banking-System/main/Methodology.png)

[View the original architecture diagram in the GitHub repository](https://github.com/rooshanriaz/Finerra-Secure-Core-Banking-System/blob/main/Methodology.png).

The main modules are:

- **API Gateway:** centralized routing, rate limiting, request mediation, and policy enforcement
- **Auth Service:** identity lifecycle, MFA, JWT validation, RBAC, and authorization policies
- **Core Banking Connector:** secure integration with Apache Fineract APIs
- **Transaction Service:** transaction orchestration and maker-checker approval workflows
- **Fraud Detection Service:** risk scoring, anomaly detection, and alert management
- **Audit Service:** event tracking, integrity verification, and blockchain anchoring
- **KYC/AML Service:** onboarding, AML screening, and decentralized identity workflows
- **ML Fraud Model:** machine-learning inference for transaction risk prediction
- **Frontend:** role-aware dashboards for banking and compliance operations
- **Fabric Network:** Hyperledger Fabric network and smart-contract assets

## Identity and access management

Identity is treated as a platform capability rather than something implemented
separately inside every service. Finnera uses Keycloak with OAuth2 and OpenID
Connect, JWT validation, multi-factor authentication, and role-based access
control.

The platform also supports policy-driven authorization and role-aware workflows
for different users, including:

- Super administrators
- Compliance officers
- Bank managers
- Loan officers

Centralizing authentication and authorization helps services make consistent
security decisions while keeping domain logic focused on banking operations.

## Core-banking operations

The core-banking connector provides a controlled integration boundary over Apache
Fineract. Instead of exposing the core system directly to every client or
frontend workflow, Finnera orchestrates requests through its own services and
security policies.

The documented operations include:

- Customer onboarding
- Account creation and management
- Deposits and withdrawals
- Inter-account transfers
- Loan management
- Role and permission management

This boundary also creates a place for validation, authentication, rate limits,
audit events, and business workflow checks before requests reach the core banking
system.

## Transaction security and maker-checker workflows

Financial transactions need more than authentication. Finnera includes
transaction orchestration and maker-checker approval workflows so that sensitive
operations can move through review and authorization stages instead of being
treated as a single unchecked request.

The transaction layer also connects to fraud scoring and audit services. That
allows suspicious activity to be evaluated before completion and gives
investigators a record of the decision path afterward.

## Fraud detection and risk scoring

Finnera combines rule-based checks with a dedicated machine-learning fraud model.
The fraud detection service supports real-time risk analysis, threshold-based
anomaly detection, transaction flagging, alerts, and investigation workflows.


## KYC, AML, and identity workflows

The KYC/AML service coordinates customer onboarding, document handling, AML
screening, risk assessment, and decentralized identity support. Personally
identifiable information is protected through encrypted storage and controlled
service access.


## Blockchain-backed audit integrity

Auditability was a core design goal. The audit service records security-relevant
events and supports integrity verification through blockchain anchoring with
Hyperledger Fabric.

The Fabric network includes audit and DID chaincode, with CouchDB used for world
state persistence. The design supports:

- Tamper-resistant audit records
- Transaction integrity verification
- DID and verifiable-credential workflows
- Exportable compliance reports
- Separation between application audit processing and ledger anchoring

The application can run without Fabric for local development using mock DID and
audit behavior, while the documented Fabric setup enables on-chain anchoring
when the full network is required.

## Security architecture

The platform applies layered controls across the edge, services, data, and audit
layers:

- Zero-trust service communication
- OAuth2 and OpenID Connect authentication
- JWT validation and token introspection
- RBAC and policy-driven authorization
- API gateway enforcement and rate limiting
- MFA and secure credential lifecycle management
- Encrypted sensitive customer data
- Environment-based secret handling
- Maker-checker transaction approval
- Fraud and anomaly monitoring
- Immutable, blockchain-backed audit records
- Optional strict TLS and mutual TLS deployment mode

These controls are more useful together than in isolation. For example, MFA
reduces account takeover risk, the gateway limits and mediates requests, the
transaction service applies workflow controls, fraud scoring evaluates behavior,
and the audit service preserves a record of what happened.

## Testing strategy

Finnera includes a testing strategy spanning multiple layers:

- Unit testing
- Integration testing
- API testing
- Security validation testing
- Fraud simulation scenarios
- Blockchain integrity verification
- End-to-end workflow checks

This approach reflects the platform's scope. Testing only individual service
methods would not be enough to validate authentication, transaction approval,
fraud scoring, audit recording, and cross-service behavior together.

## What I learned

Finnera made the connection between application security and system architecture
particularly concrete. A secure banking platform needs controls at every handoff:
from identity to gateway, from gateway to domain service, from transactions to
fraud analysis, and from audit events to integrity verification.

The project also reinforced the value of clear service boundaries. Separating
authentication, core banking integration, transaction processing, KYC/AML,
fraud analysis, and audit integrity makes the system more complex, but it also
makes security responsibilities easier to reason about and test.

## Repository

Explore the implementation, architecture diagram, deployment instructions, and
service code in the [Finnerra Secure Core Banking System repository](https://github.com/rooshanriaz/Finerra-Secure-Core-Banking-System).