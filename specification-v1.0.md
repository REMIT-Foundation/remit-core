# REMIT
## Runtime Enforcement & Monitoring of Intelligent Tasks

**The Open Specification for Responsible AI Agent Behaviour**

Version 1.0 | Published June 2026 | REMIT Foundation

Licensed under Creative Commons Attribution 4.0 International (CC BY 4.0).
Anyone may use, adapt, and build upon this specification freely, provided attribution is given.

---

## Foreword

AI agents are no longer a curiosity. They are already booking appointments, processing claims, writing code, sending emails and making decisions on behalf of millions of people every day. And yet, in almost every organisation deploying them, the honest answer to the question "what exactly did that agent do, and why?" is: we are not entirely sure.

That is a problem. Not because AI agents are inherently dangerous, but because autonomy without accountability is a risk that no responsible organisation should accept. When a human employee acts outside their authority, there are processes, audit trails and consequences. When an AI agent does the same, there is often nothing.

REMIT was written to change that. It is not a product, a vendor, or a certification body with commercial interests. It is a shared language, a set of principles and a technical baseline that any organisation can adopt, adapt and build upon. It exists because the community of people building and deploying AI agents needed something to point to, something that said: this is what responsible agent behaviour looks like.

This first version of the specification is deliberately practical. It does not require perfection. It does not demand that you rearchitect your entire system overnight. It asks that you know what your agents are supposed to do, that you watch what they actually do, and that you can demonstrate both.

We hope this becomes a living document, shaped by the people who use it. If you find it useful, implement it. If you find it wrong, tell us. The goal is not compliance for its own sake. The goal is trust.

**The REMIT Foundation**
June 2026

---

## Contents

1. Introduction
2. Scope and Purpose
3. Definitions
4. The Seven Principles of REMIT
5. Technical Requirements
6. Integration Approaches
7. Audit and Evidence
8. Certification Levels
9. Regulatory Alignment
10. Governance and Amendments

Appendix A: Example Allowed Action Profiles
Appendix B: Sample Audit Log Schema
Appendix C: Mapping to OWASP Agentic AI Top 10

---

## 1. Introduction

The use of autonomous AI agents is growing faster than the frameworks designed to govern them. Across finance, healthcare, legal services, government and technology, organisations are deploying software that can reason, plan and act without direct human instruction at each step. This is genuinely powerful. It is also genuinely new, and the absence of agreed standards for how agents should behave is becoming a meaningful risk.

REMIT, which stands for Runtime Enforcement and Monitoring of Intelligent Tasks, is an open specification that establishes what responsible AI agent behaviour looks like in practice. It defines a set of principles, technical requirements and evidence standards that organisations can use to govern the agents they build and deploy.

REMIT is vendor neutral. It does not require any particular technology stack, programming language or commercial product. It is designed to work alongside existing security, privacy and compliance frameworks rather than replace them. And it is open: any organisation, team or individual may implement it, reference it, or contribute to its development.


### 1.1 Why This Specification Exists

Several things are true at once about AI agents today. They are increasingly capable. They are increasingly deployed in high stakes contexts. And the tooling to understand, audit and constrain their behaviour has not kept pace with the speed of adoption.

Existing security frameworks such as OWASP, NIST and ISO 27001 provide valuable guidance, but they were not designed with autonomous agents in mind. They do not address the specific challenge of an agent that receives a task, selects its own tools, executes actions across multiple systems and produces real world effects, all without a human in the loop at each decision point.

REMIT fills that gap. It is specifically designed for agentic AI: systems that act, not just systems that respond.


### 1.2 Who This Specification Is For

REMIT is written for anyone involved in building, deploying, procuring or overseeing AI agents. This includes:

- Engineering teams building AI agents or the platforms that run them.
- Security and compliance teams responsible for assessing the risk of agentic systems.
- Procurement teams evaluating AI products that include autonomous agent capabilities.
- Regulators and policy makers seeking a technical reference for responsible agent deployment.
- Organisations wishing to demonstrate to customers, partners or regulators that their agents operate responsibly.


### 1.3 Relationship to Other Standards

REMIT is designed to complement, not replace, existing frameworks. It maps directly to several regulatory obligations and voluntary frameworks:

- The EU AI Act, particularly the requirements for high risk AI systems around transparency, human oversight and auditability.
- The UK ICO's guidance on automated decision making and accountability under UK GDPR.
- The NIST AI Risk Management Framework, specifically the Govern and Monitor functions.
- OWASP Agentic AI Top 10, which catalogues the most significant vulnerabilities in agentic systems. REMIT does not publish its own risk list. Instead, it maps each OWASP risk to the principles and technical requirements in this specification. The full mapping is in Appendix C.
- The REMIT Foundation Agent Passport v1.0, which defines signed capability manifests, cryptographic agent identity, attenuated capability tokens and local passport authority for binding allowed profiles to non-bypassable enforcement layers.
- The REMIT Enforcement Profile v1.0, which describes reference architectures (SDK, proxy, MCP, service mesh, sandbox) and conformance criteria for interception layers.

Where a requirement in REMIT maps directly to a regulatory obligation, this is noted in Section 9.

---

## 2. Scope and Purpose


### 2.1 What REMIT Covers

REMIT applies to any software system that exhibits all three of the following characteristics:

1. It receives a goal or task from a human or another system.
2. It selects and executes actions autonomously to pursue that goal, including calling external tools, APIs, databases or services.
3. It can produce real world effects as a result of those actions, such as sending communications, modifying data, making transactions or controlling systems.

Systems that merely generate text or recommendations without taking actions of their own are outside the scope of this specification, though many of the principles will still apply as good practice.


### 2.2 What REMIT Does Not Cover

REMIT is not a specification for:

- The safety or accuracy of the underlying AI model. Model safety is an important and separate concern addressed by the model developer.
- The content of agent outputs in terms of bias, fairness or accuracy. These are addressed by frameworks such as the EU AI Act's requirements for high risk systems.
- Multi agent orchestration protocols, which are addressed by emerging standards such as MCP, A2A and ACP.

REMIT focuses specifically on the runtime behaviour of agents: what they do, whether that is within their sanctioned scope, and whether there is a record of it.


### 2.3 Purpose of This Specification

> The purpose of REMIT is to ensure that AI agents do what they are supposed to do, nothing more and nothing less, and that there is always evidence to prove it.

This means three things in practice. First, every agent must have a defined scope of permitted actions before it begins. Second, every action the agent takes must be evaluated against that scope in real time. Third, a complete, tamper evident record of every action and every evaluation must be maintained and made available for audit.

---

## 3. Definitions

The following terms are used throughout this specification with the meanings given below.

| Term | Meaning |
|---|---|
| Agent | An AI system that receives a task and autonomously executes one or more actions to complete it, including interactions with external tools, APIs or data sources. |
| Task | The goal or instruction provided to an agent at the start of a session. The task defines the intended scope of the agent's activity. |
| Action | Any discrete operation performed by an agent, including calling a tool, reading or writing data, sending a communication, making an API request, or executing code. |
| Allowed Profile | A structured definition of the tools, data sources and action types that an agent is permitted to use in the context of a given task. |
| Session | A bounded period of agent activity, beginning when a task is received and ending when the task is completed, abandoned or terminated. |
| Verdict | The outcome of evaluating an action against the allowed profile: Allow, Flag or Block. |
| Audit Log | A tamper evident, chronological record of every action taken by an agent during a session, including the verdict assigned to each action and the reasoning for that verdict. |
| Operator | The organisation or individual responsible for deploying and governing an agent. |
| Implementer | A vendor, developer or platform that builds a product or service designed to help operators achieve REMIT compliance. |
| Scope Violation | Any action taken by an agent that falls outside its allowed profile for the current task. |
| Runtime Enforcement | The real time evaluation and, where necessary, blocking of agent actions based on the allowed profile. |
| Intent Extraction | The process of deriving the allowed profile from the task description, typically using automated reasoning. |

---

## 4. The Seven Principles of REMIT

REMIT is grounded in seven principles. These principles are not aspirational statements: each one has specific technical requirements attached to it, described in Section 5. The principles are the why; the requirements are the how.

| # | Principle | What It Means in Practice |
|---|---|---|
| P1 | Defined Scope | Every agent must have an explicit, machine readable allowed profile before it begins executing actions. An agent must not act without a defined scope. |
| P2 | Real Time Enforcement | Every action must be evaluated against the allowed profile before it is executed. Evaluation must happen in real time, not retrospectively. |
| P3 | Least Privilege | An agent's allowed profile must be limited to the minimum set of tools and data sources genuinely necessary to complete the task. Agents must not be granted broad permissions as a convenience. |
| P4 | Complete Auditability | Every action, every evaluation and every verdict must be recorded in a tamper evident audit log. There must be no gaps in the record. |
| P5 | Human Oversight | Operators must be able to view, query and act upon agent activity in real time. Where an agent is blocked or flagged, a human must be notified promptly. |
| P6 | Transparency | Agents must not misrepresent their nature, identity or capabilities. Where an agent interacts with humans, those humans must be informed that they are interacting with an automated system. |
| P7 | Tenant Isolation | Where an agent platform serves multiple organisations, the data, audit logs and allowed profiles of each organisation must be strictly isolated. No data may be shared between tenants. |


### 4.1 Principle 1: Defined Scope

An agent that begins executing actions without a defined scope is, by definition, ungoverned. The allowed profile is the contract between the operator and the agent. It specifies what the agent is permitted to do, what data it may access, and what actions are explicitly forbidden regardless of context.

The allowed profile must be established before the session begins and must be associated with the specific task being undertaken. A general purpose profile that permits an agent to do anything is not compliant with this principle. The profile must reflect the actual task.

> **Example:** An agent tasked with "summarise the sales data for Q1" should have an allowed profile that permits reading from the sales database and generating text output. It should not have permission to write to any database, send emails, or access HR records, even if those permissions exist elsewhere in the system.


### 4.2 Principle 2: Real Time Enforcement

Retrospective auditing is valuable but insufficient. An audit log that shows an agent accessed sensitive data ten minutes ago does not undo the access. REMIT requires that evaluation happens before execution: the action is checked against the allowed profile, and only then is it permitted or blocked.

This places a technical requirement on response time. The evaluation must not introduce latency that makes the agent unusable in practice. The target is that enforcement adds no more than 500 milliseconds to any action.


### 4.3 Principle 3: Least Privilege

The principle of least privilege is well established in information security, and it applies equally to AI agents. An agent that has access to every tool and every data source in an organisation is an agent that can cause maximum damage if it misbehaves, whether through a bug, a prompt injection attack, or an error in reasoning.

Operators must resist the temptation to create a single, maximally permissive profile and apply it to all agents. The profile must be derived from the task.


### 4.4 Principle 4: Complete Auditability

Every action must be logged. Not most actions. Not actions that seem significant. Every action. The audit log is the evidence that the agent operated within its remit, and gaps in that log are gaps in the evidence.

Logs must be tamper evident: it must be possible to detect if a log has been modified or deleted after the fact. Logs must also be retained for a period appropriate to the regulatory context in which the agent operates.


### 4.5 Principle 5: Human Oversight

Autonomy does not mean abandonment. Operators retain responsibility for the actions of their agents, and that responsibility requires the ability to see what is happening and to intervene when something goes wrong. REMIT requires that operators have real time visibility of agent activity and are notified promptly of any scope violations.


### 4.6 Principle 6: Transparency

People have a right to know when they are interacting with an automated system. This is both an ethical requirement and, in many jurisdictions, a legal one. REMIT requires that agents do not present themselves as human, do not obscure their automated nature, and do not claim capabilities they do not have.


### 4.7 Principle 7: Tenant Isolation

Where an agent platform is used by multiple organisations, the data and audit logs of each organisation are confidential to that organisation. A breach of tenant isolation, where the activity or data of one organisation becomes visible to another, is one of the most serious failures a platform can experience. REMIT treats tenant isolation as a hard requirement, not a best practice.

---

## 5. Technical Requirements

This section defines the specific technical requirements that must be met by any system claiming REMIT compliance. Requirements are categorised as Mandatory (must be implemented for any level of compliance) or Recommended (expected for higher certification levels, described in Section 8).


### 5.1 Session Management

**5.1.1 Session Initiation (Mandatory)**

- Every agent session must be assigned a unique, immutable session identifier before any actions are executed.
- The task must be recorded in full at session initiation.
- The allowed profile must be derived from the task and recorded before the first action is executed.
- The session must be associated with the operator's organisation identifier.

**5.1.2 Session Termination (Mandatory)**

- Sessions must be explicitly terminated, either by the agent completing the task, by operator instruction, or by a timeout.
- The termination reason and timestamp must be recorded in the audit log.
- Actions must not be accepted on a terminated session.


### 5.2 Intent Extraction and Allowed Profile

**5.2.1 Profile Structure (Mandatory)**

The allowed profile must be a structured, machine readable document containing at minimum:

- A list of permitted tools or tool categories.
- A list of permitted data sources or data types.
- A list of explicitly forbidden tools or actions.
- A list of risk indicators: patterns of behaviour that should trigger elevated scrutiny even if not explicitly forbidden.

**5.2.2 Profile Derivation (Mandatory)**

- The allowed profile must be derived from the specific task, not from a generic template.
- The derivation process must be documented and reproducible.
- Where automated reasoning is used to derive the profile, the prompt and model response must be logged.

**5.2.3 Cryptographic Binding (Recommended)**

- Where high assurance is required, the allowed profile should be realised as a signed Agent Passport manifest bound to the session identifier and agent identity. See the REMIT Foundation Agent Passport v1.0 specification.
- Unsigned profiles remain valid for REMIT Levels 1 and 2. Signed passports are recommended for Level 3 and required for Level 4 (High Assurance) when published.


### 5.3 Action Interception and Evaluation

**5.3.1 Interception (Mandatory)**

- Every action attempted by the agent must pass through an interception layer before execution.
- The interception layer must capture the tool name, input parameters and timestamp of every action.
- Actions must not be executed before a verdict is returned.

**5.3.2 Evaluation (Mandatory)**

- Every action must be evaluated against the allowed profile.
- The evaluation must return one of three verdicts: Allow, Flag or Block.
- The evaluation must also return a risk score between 0.0 and 1.0, where 0.0 represents no risk and 1.0 represents a clear scope violation.
- The reasoning for the verdict must be recorded in human readable form.
- The evaluation must complete within 500 milliseconds in 95% of cases.

**5.3.3 Verdict Definitions (Mandatory)**

| Verdict | Definition |
|---|---|
| Allow | The action is consistent with the allowed profile and the task. The action may proceed. Risk score: 0.0 to 0.3. |
| Flag | The action is unusual or potentially outside scope, but may be legitimate. The action may proceed but must be highlighted in the audit log and reported to the operator. Risk score: 0.3 to 0.7. |
| Block | The action is outside the allowed profile or represents a clear scope violation. The action must not execute. The agent must be informed that the action was blocked. The operator must be notified immediately. Risk score: 0.7 to 1.0. |


### 5.4 Audit Logging

**5.4.1 Log Contents (Mandatory)**

Every entry in the audit log must contain:

- Session identifier.
- Organisation identifier.
- Action identifier (unique per action within the session).
- Tool name and input parameters.
- Verdict (Allow, Flag or Block).
- Risk score.
- Reasoning for the verdict in plain English.
- Timestamp (UTC, millisecond precision).

**5.4.2 Log Integrity (Mandatory)**

- Logs must be written in an append only manner. Entries must not be modified after writing.
- Logs must be protected against unauthorised deletion.
- Where technically feasible, logs should be signed or hashed to enable tamper detection.

**5.4.3 Log Retention (Mandatory)**

- Logs must be retained for a minimum of 90 days for non regulated contexts.
- Logs must be retained for a minimum of 7 years where the agent operates in a regulated context such as financial services, healthcare or legal services.
- Operators must be able to export logs in a standard machine readable format such as JSON or CSV.


### 5.5 Alerting (Mandatory)

- Any Block verdict must trigger an alert to the operator within 60 seconds.
- Any Flag verdict must be surfaced in the operator's monitoring interface within 5 minutes.
- Operators must be able to acknowledge alerts.
- Unacknowledged Block alerts must be escalated after a configurable timeout.


### 5.6 Multi Tenancy (Mandatory where applicable)

- Where a platform serves multiple organisations, each organisation's sessions, audit logs, allowed profiles and alerts must be isolated at the data layer.
- Row level security or equivalent must be enforced on all tenant data.
- No query, report or data export must be possible that returns data from more than one organisation.
- Each organisation must have its own authentication credentials. Credentials must not be shared across organisations.

---

## 6. Integration Approaches

REMIT does not mandate a specific technical integration method. Operators and implementers may choose the approach that best fits their architecture and risk profile. This section describes four recognised integration patterns, in order from highest to lowest code change requirement.


### 6.1 SDK Integration

The operator or developer integrates a REMIT compliant library directly into the agent code. Every tool call is wrapped with an interception call before execution. This provides the highest degree of control and the richest context for evaluation.

> Best suited for: teams building new agents from scratch, or agents where the codebase is owned and accessible.


### 6.2 Proxy Mode

A REMIT compliant proxy sits between the agent and its tools. The agent's outbound HTTP or API calls pass through the proxy, which intercepts, evaluates and either forwards or blocks them. No changes are required to the agent code itself.

> Best suited for: existing agents where the codebase cannot be modified, or agents built on third party platforms.


### 6.3 MCP Server Mode

For agents that use the Model Context Protocol, a REMIT compliant MCP server acts as the intermediary between the agent and its tools. The operator adds a single configuration entry to their agent's MCP settings. All tool calls routed through the REMIT MCP server are evaluated automatically.

> Best suited for: MCP native agents using frameworks such as Claude, LangChain or AutoGen with MCP support.


### 6.4 CI/CD Gate

A REMIT compliant check is added to the operator's continuous integration pipeline. The check scans agent code for unprotected tool calls and fails the build if any are found. This does not provide runtime enforcement on its own and must be combined with one of the above approaches for full compliance.

> Best suited for: DevSecOps teams who wish to enforce REMIT adoption as a condition of deployment.


### 6.5 Combining Approaches

Operators are encouraged to combine approaches. A typical high assurance deployment might use SDK integration for agents under active development, proxy mode for legacy agents, and a CI/CD gate to enforce that all new agents are instrumented before reaching production.

| Mode | Code Change Required | Setup Time | Control Level | Best For |
|---|---|---|---|---|
| SDK | Yes | 30 minutes | Highest | Teams building new agents |
| Proxy | No | 10 minutes | Medium | Existing agents, any language |
| MCP Server | No (config only) | 5 minutes | Medium | MCP native agents |
| CI/CD Gate | No | 5 minutes | Enforcement only | DevSecOps, mandating adoption |

---

## 7. Audit and Evidence


### 7.1 The Audit Trail as Evidence

The audit log produced by a REMIT compliant system is not merely a technical record. It is the primary evidence that an agent operated within its remit. Regulators, auditors, customers and legal teams may request access to audit logs, and operators must be prepared to produce them.

For this reason, REMIT places strong requirements on the completeness, integrity and accessibility of audit logs. An audit trail with gaps, or one that cannot be verified as unmodified, provides much weaker assurance than one that meets the full requirements of Section 5.4.


### 7.2 Evidence Required for REMIT Compliance

An organisation wishing to demonstrate REMIT compliance should be able to produce the following on request:

1. A sample of allowed profiles generated for recent sessions, demonstrating that profiles are task specific and appropriately scoped.
2. Audit logs for a representative sample of sessions, showing every action, every verdict and every reasoning statement.
3. Evidence that Block verdicts triggered operator alerts within the required timeframe.
4. A description of the integration approach used, including the technical architecture of the interception layer.
5. For multi tenant platforms: a description of how tenant isolation is enforced at the data layer.


### 7.3 Audit Log Access

Operators must retain the ability to access their own audit logs at any time. Where logs are held by an implementer on behalf of the operator, the implementer must provide a mechanism for the operator to export their complete log history in a standard format. The operator must not be locked out of their own audit data.

---

## 8. Certification Levels

REMIT defines three certification levels. These are designed to provide a meaningful progression from initial adoption to full compliance, recognising that organisations will have different starting points and timelines.

| Level | Name | Requirements |
|---|---|---|
| Level 1 | REMIT Monitored | Sessions are being created with a task and unique identifier. Actions are being intercepted and logged. An audit trail exists. Evaluation may be rule based rather than AI assisted. Alerting is in place for Block verdicts. |
| Level 2 | REMIT Verified | All Level 1 requirements, plus: allowed profiles are derived from the task using a documented, reproducible method. Risk scores are assigned to every action. Reasoning is recorded in human readable form for every verdict. Tenant isolation is in place where applicable. Logs are retained for the minimum required period. |
| Level 3 | REMIT Certified | All Level 2 requirements, plus: log integrity controls are in place (append only, tamper evident). Evaluation latency is within the 500ms target in 95% of cases. A third party assessment has been conducted by a REMIT Foundation approved assessor. OWASP and regulatory mapping documentation has been completed for the operator's applicable frameworks. |
| Level 4 | REMIT High Assurance | All Level 3 requirements, plus: Agent Passport v1.0 with signed manifests and capability tokens. Non-bypassable enforcement layer per REMIT Enforcement Profile v1.0. Local or hosted passport authority with documented revocation. Runtime attestation where required by operator risk assessment. |

Certification is self declared for Levels 1 and 2. Level 3 certification requires a third party assessment. Level 4 certification requires a third party assessment with enforcement profile conformance evidence. The REMIT Foundation maintains a public register of Level 3 and Level 4 certified organisations and platforms.

---

## 9. Regulatory Alignment

REMIT has been designed with direct reference to existing and emerging regulatory requirements. This section summarises how REMIT compliance supports obligations under key frameworks. It is not legal advice, and operators should seek their own legal counsel for specific compliance questions.


### 9.1 EU AI Act

The EU AI Act classifies certain AI systems as high risk, including those used in employment, education, critical infrastructure and access to essential services. High risk systems must meet requirements including transparency, human oversight, record keeping and accuracy. REMIT supports compliance with these requirements as follows:

- The audit log satisfies the record keeping obligations for high risk systems under Article 12.
- Real time enforcement and alerting supports the human oversight requirements under Article 14.
- The allowed profile and transparency requirements in REMIT support the technical documentation obligations under Article 11.


### 9.2 UK GDPR and ICO Guidance

Under UK GDPR, organisations using automated decision making must be able to explain decisions and demonstrate that appropriate safeguards are in place. REMIT's audit log and reasoning requirements directly support this obligation. The ICO's guidance on AI and data protection encourages organisations to document the purpose, scope and outputs of AI systems, all of which are captured in REMIT's session and audit requirements.


### 9.3 NIST AI Risk Management Framework

The NIST AI RMF is organised around four functions: Govern, Map, Measure and Manage. REMIT aligns primarily with the Govern and Manage functions, providing the runtime controls and evidence that those functions require. Organisations using REMIT alongside NIST AI RMF should document the mapping between REMIT's requirements and their NIST profile.


### 9.4 OWASP Agentic AI Top 10

REMIT is not a parallel risk catalogue. It is a control framework that organisations can use to respond to the risks identified by OWASP. The OWASP Agentic AI Top 10 names the threats. REMIT names the runtime controls and the evidence you need to show they are working.

Many of the highest priority OWASP risks are exactly what REMIT was written for. When an agent pursues the wrong goal, abuses a tool it was permitted to use, or acts beyond its intended scope, REMIT's allowed profile, real time enforcement and audit trail are the practical response. That is particularly true for tool misuse, unexpected code execution and rogue agent behaviour, where REMIT provides strong, direct coverage.

Some OWASP risks sit partly outside REMIT's scope. Supply chain compromise, insecure inter agent communication and human trust exploitation often need controls beyond runtime monitoring: dependency management, secure messaging between agents, and training for the people who work alongside them. REMIT still helps here by ensuring that when something goes wrong, you have a complete record of what the agent did and why it was allowed or blocked.

Operators should treat Appendix C as a starting point, not a checklist. Review the full OWASP Agentic AI Top 10 for your deployment context, document where REMIT gives you coverage, and be honest about the gaps that need complementary controls. For threat modelling, use the REMIT Foundation's Agent Threat Modelling Standard (ATMS), which maps the same OWASP risks to a structured analysis process.

---

## 10. Governance and Amendments


### 10.1 The REMIT Foundation

REMIT is maintained by the REMIT Foundation, an independent, non profit body. The Foundation's responsibilities are to maintain and publish the specification, to develop and operate the Level 3 certification programme, to maintain the public register of certified organisations and platforms, and to manage the process for amendments and new versions.

The Foundation does not endorse or recommend any commercial product or vendor. Any product may claim REMIT compliance provided it meets the requirements of this specification.


### 10.2 Contributing to REMIT

REMIT is an open specification and welcomes contributions from anyone. Contributions may take the form of proposed amendments, issue reports, case studies or regulatory mapping documents. All contributions are reviewed by the Foundation's technical committee before incorporation into the specification.

The specification is published on GitHub under a Creative Commons Attribution 4.0 licence. Pull requests, issues and discussions are open to the public.


### 10.3 Versioning

REMIT uses semantic versioning. The first digit indicates a major version, which may include breaking changes. The second digit indicates a minor version, which adds requirements but does not remove or modify existing ones. The third digit indicates a patch, which corrects errors or improves clarity without changing requirements.

Organisations implementing REMIT should document which version they are implementing. The Foundation will provide a minimum of 12 months' notice before deprecating any major version.


### 10.4 Contact

The REMIT Foundation can be contacted at:

- Specification questions: spec@remitframework.org
- Certification enquiries: certification@remitframework.org
- Press and partnerships: hello@remitframework.org

---

## Appendix A: Example Allowed Action Profiles

The following examples illustrate what a well formed allowed profile looks like for different task types. These are illustrative only.


### Example 1: Summarise Sales Data

| Field | Detail |
|---|---|
| Task | Summarise total revenue by product for Q1 2026 and produce a brief written summary. |
| Allowed Tools | database_query (read only), text_generation |
| Allowed Data | Sales transaction table, product catalogue |
| Forbidden Tools | database_write, send_email, file_write, web_search, code_execution |
| Risk Indicators | Any attempt to access tables other than sales or products. Any attempt to write or modify data. Any attempt to send communications. |


### Example 2: Book Travel

| Field | Detail |
|---|---|
| Task | Find the cheapest return flight from London Heathrow to Paris Charles de Gaulle on 14 July, departing after 08:00, and book it using the company travel account. |
| Allowed Tools | flight_search, flight_booking, calendar_read |
| Allowed Data | Travel account credentials, user's calendar availability |
| Forbidden Tools | email_send, file_read, web_browse (general), database_query, code_execution |
| Risk Indicators | Any search for destinations other than LHR and CDG. Any attempt to access files or email. Any booking using a payment method other than the company travel account. |


### Example 3: Process a Support Ticket

| Field | Detail |
|---|---|
| Task | Read the open support ticket with ID 84921, categorise it, and assign it to the correct team based on the ticket routing rules. |
| Allowed Tools | ticketing_read, ticketing_update, routing_rules_read |
| Allowed Data | Ticket 84921 only, routing rules document |
| Forbidden Tools | ticketing_delete, email_send, customer_data_read (beyond ticket), database_query |
| Risk Indicators | Any attempt to read tickets other than 84921. Any attempt to contact the customer directly. Any attempt to close or delete the ticket. |

---

## Appendix B: Sample Audit Log Schema

The following JSON schema represents a single audit log entry that meets the REMIT requirements of Section 5.4. Implementers may extend this schema but must not omit any of the required fields.

```json
{
  "action_id": "act_01j9zk7m3p4q5r6s7t8u9v0w",
  "session_id": "sess_01j9zk2a3b4c5d6e7f8g9h0i",
  "org_id": "org_01j9za1b2c3d4e5f6g7h8i9j",
  "tool_name": "read_file",
  "parameters": {
    "path": "/etc/ssh/id_rsa"
  },
  "verdict": "block",
  "score": 0.97,
  "reasoning": "The agent attempted to read an SSH private key file. This action is entirely unrelated to the stated task of summarising sales data and represents a clear scope violation. No legitimate summarisation task requires access to SSH credentials.",
  "timestamp": "2026-06-23T14:32:07.841Z",
  "session_task": "Summarise total revenue by product for Q1 2026.",
  "allowed_profile_id": "profile_01j9za9k0l1m2n3o4p5q6r7s",
  "passport_id": "ppt_01k0abc123def456",
  "token_id": "tok_01k0def789ghi012",
  "capability": "read_file",
  "enforcement_decision": "deny"
}
```

When Agent Passport is implemented, `passport_id`, `token_id`, `capability` and `enforcement_decision` are required extensions. See Agent Passport v1.0 §12.1.

The reasoning field must be written in plain English and must be comprehensible to a non technical reader. It is not sufficient to record only the risk score. The reasoning is the explanation that a regulator, auditor or legal team will read when reviewing agent behaviour.

---

## Appendix C: Mapping to OWASP Agentic AI Top 10

This appendix maps each risk in the OWASP Top 10 for Agentic Applications (2026 edition) to the REMIT principles and technical requirements that address it. REMIT does not redefine or replace the OWASP list. It shows how implementing this specification helps you respond to risks OWASP has already identified.

Coverage is rated as follows:

| Rating | Meaning |
|---|---|
| Strong | REMIT requirements directly and substantially mitigate this risk for most deployments. |
| Partial | REMIT provides meaningful runtime controls, but additional measures are usually needed. |
| Limited | REMIT contributes audit evidence or boundary controls, but this risk is primarily addressed elsewhere. |

### OWASP to REMIT mapping

| OWASP Risk (2026) | REMIT Principles | Key REMIT Requirements | Coverage |
|---|---|---|---|
| ASI01: Agent Goal Hijack | P1 Defined Scope, P2 Real Time Enforcement | 5.2 Allowed Profile, 5.3 Action Interception and Evaluation | Partial |
| ASI02: Tool Misuse and Exploitation | P1 Defined Scope, P2 Real Time Enforcement, P3 Least Privilege | 5.2 Allowed Profile, 5.3 Evaluation, 5.4 Audit Logging | Strong |
| ASI03: Agent Identity and Privilege Abuse | P3 Least Privilege, P6 Transparency, P7 Tenant Isolation | 5.1 Session Management, 5.6 Multi Tenancy, 5.3 Evaluation | Partial |
| ASI04: Agentic Supply Chain Compromise | P4 Complete Auditability | 5.4 Audit Logging | Limited |
| ASI05: Unexpected Code Execution | P1 Defined Scope, P2 Real Time Enforcement, P3 Least Privilege | 5.2 Forbidden tools, 5.3 Block verdicts, 5.5 Alerting | Strong |
| ASI06: Memory and Context Poisoning | P1 Defined Scope, P4 Complete Auditability | 5.2 Risk indicators, 5.4 Audit Logging | Partial |
| ASI07: Insecure Inter Agent Communication | P4 Complete Auditability, P7 Tenant Isolation | 5.4 Audit Logging, 5.6 Multi Tenancy | Limited |
| ASI08: Cascading Agent Failures | P2 Real Time Enforcement, P5 Human Oversight | 5.1 Session Termination, 5.5 Alerting | Partial |
| ASI09: Human Agent Trust Exploitation | P5 Human Oversight, P6 Transparency | 5.5 Alerting, Principle P6 | Partial |
| ASI10: Rogue Agents | P1 to P5 | 5.1 to 5.5 (full runtime control stack) | Strong |

### Notes on selected risks

**ASI01: Agent Goal Hijack.** REMIT evaluates every action against an allowed profile derived from the stated task. If a hijacked goal leads the agent to call tools or access data outside that profile, the action is flagged or blocked before it executes. REMIT does not, on its own, validate whether the original task input was manipulated. Operators should combine REMIT with input validation and content trust boundaries, as described in ATMS.

**ASI02: Tool Misuse and Exploitation.** This is one of the clearest alignments between OWASP and REMIT. The allowed profile limits which tools an agent may use, evaluation happens before execution, and every tool call is logged with a verdict and reasoning. Least privilege ensures the agent cannot reach tools it does not need for the task at hand.

**ASI04: Agentic Supply Chain Compromise.** REMIT records what an agent did with the tools and models it used, which is valuable evidence after a compromise. It does not replace dependency scanning, model provenance checks or software bill of materials practices. Operators should use the REMIT Foundation AIBOM Standard alongside REMIT for supply chain assurance.

**ASI07: Insecure Inter Agent Communication.** Multi agent orchestration is outside the core scope of this specification (see Section 2.2). Where agents do communicate, REMIT still applies to each agent's individual actions and audit trail. Secure messaging protocols between agents require separate architectural controls.

**ASI10: Rogue Agents.** REMIT was designed for this class of failure. An agent operating outside its sanctioned scope is, by REMIT's definition, a scope violation. Real time enforcement blocks the action, alerting notifies the operator, and the audit log preserves the evidence.

For a threat modelling perspective on the same OWASP risks, see Appendix C of the Agent Threat Modelling Standard (ATMS), which maps OWASP risks to ATMS threat categories and mitigations.

---

*End of REMIT Specification v1.0*

*remitframework.org | spec@remitframework.org*
