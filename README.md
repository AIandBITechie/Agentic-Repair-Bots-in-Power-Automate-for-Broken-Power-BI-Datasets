Agentic Repair Bots in Power Automate for Broken Power BI Datasets
Author: Mohith Reddy Patlolla — Senior Business Intelligence Architect

Repository: Agentic Repair Bot — Autonomous Diagnosis & Repair for Power BI Pipelines

Abstract :
Business intelligence ecosystems increasingly depend on automated data pipelines and distributed cloud services such as Microsoft Power BI and Power Automate. 
Despite their scalability, these systems are susceptible to operational failures—including authentication issues, schema drift, 
and connection timeouts—that create data downtime and increase Mean Time to Resolution (MTTR).
This repository provides a functional implementation of the Agentic Repair Bot architecture described in the research paper Agentic Repair Bots in Power Automate for Broken Power BI Datasets. 
The system integrates an OpenAI-powered diagnostic engine with Power Automate and the Power BI REST API to autonomously detect, diagnose, and remediate dataset refresh failures.
The implementation demonstrates how LLM-powered autonomous agents, when combined with event-driven orchestration in LCNC platforms, can significantly reduce operational overhead, 
improve system observability, and enhance data reliability across Power BI environments.


Keywords:
Agentic AI, Autonomous Agents, Power Automate, Power BI, LLM Diagnostics, Data Observability, Schema Drift, MTTR Reduction, LCNC Systems

1. Introduction:
Modern data environments experience dozens of monthly operational incidents, many of which stem from fragile data pipelines and external service dependencies.
Research shows that data teams spend nearly 50% of their time resolving breakages instead of delivering analytical insights [1].

Common failures include:
	•	Authentication & credential expiration
	•	Schema drift due to upstream changes
	•	Gateway / network timeouts
	•	Misconfigured refresh schedules

Power BI provides basic notification mechanisms, but manual intervention is still required for nearly all remediation tasks—resulting in slow response times and increased MTTR.
🔍 Why Agentic Repair Bots?
Traditional monitoring tools detect failures, but they do not fix them.
Agentic Repair Bots introduce a new capability:

Autonomous self-healing for broken Power BI datasets.
This repository operationalizes the agentic design proposed in the research paper by embedding an LLM reasoning agent directly into Power Automate workflows.

2. Background & Related Work
2.1 Agentic AI Systems
Agentic AI expands traditional LLMs by enabling autonomous decision-making, planning, and tool execution [2], [4].
These systems excel at interpreting unstructured data (like error logs) and producing executable remediation steps.

2.2 Data Observability
Data observability platforms detect anomalies but depend on humans to perform repairs. The lack of automated remediation remains a major gap in enterprise BI ecosystems [1].

2.3 Low-Code/No-Code (LCNC) Systems
Microsoft Power Automate provides a secure environment for orchestrating workflows but historically lacked advanced reasoning capabilities. Injecting an LLM agent into LCNC enables safe, controlled, and explainable automation [3].


3. System Architecture
The Agentic Repair Bot architecture consists of:
	1.	Power BI (Event Source)
	2.	Power Automate (Event Router & Trigger Engine)
	3.	LLM Diagnostic Engine
	4.	Autonomous Remediation Engine
	5.	Power BI REST API Handler
	6.	Human-in-the-Loop (HITL) Oversight

3.1 Architecture Diagram (ASCII Inline)
                ┌───────────────────────────────────────────┐
                │          Power BI Service                 │
                │  (Dataset Refresh Events + Error Logs)    │
                └───────────────────────────────────────────┘
                                 │
                                 ▼
         ┌────────────────────────────────────────────────────────┐
         │                Power Automate (Event Router)           │
         │  - Monitors refresh history                            │
         │  - Detects failures                                    │
         │  - Sends logs to Agentic LLM Endpoint                  │
         └────────────────────────────────────────────────────────┘
                                 │ HTTP POST
                                 ▼
        ┌──────────────────────────────────────────────────────────┐
        │            LLM Diagnostic & Reasoning Engine             │
        │  - Log interpretation                                    │
        │  - Pattern matching (Auth / Drift / Timeout)             │
        │  - Failure classification                                │
        │  - Generates remediation plan                            │
        └──────────────────────────────────────────────────────────┘
                                 │ JSON
                                 ▼
        ┌──────────────────────────────────────────────────────────┐
        │             Autonomous Remediation Engine                │
        │  - Trigger refresh                                       │
        │  - Update credentials                                    │
        │  - Identify schema drift                                 │
        └──────────────────────────────────────────────────────────┘
                                 │
                                 ▼
        ┌──────────────────────────────────────────────────────────┐
        │             Human-in-the-Loop Escalation                 │
        └──────────────────────────────────────────────────────────┘

4. Technical Components
4.1 LLM Diagnostic Engine
	•	Analyzes Power BI refresh logs
	•	Classifies error patterns
	•	Performs chain-of-thought reasoning
	•	Produces structured remediation instructions

4.2 Diagnostics Pattern Engine
Detects known failure signatures:
Pattern                     Classification
“Invalid credentials”       authentication_error
“Column not found”          schema_drift
“Timeout”                   connection_timeout

4.3 Schema Drift Engine

Identifies missing or changed columns and prepares HITL recommendations.

4.4 Remediation Engine

Executes automated fixes using Power BI REST API:
	•	Refresh dataset
	•	Update credentials
	•	Generate schema reconciliation reports

4.5 Event Router (Power Automate)
Triggers diagnostic/repair flow for each failed refresh cycle.

5. LLM Diagnostic Pipeline (ASCII)
      ┌────────────────────┐
      │  Raw Error Log     │
      └─────────┬──────────┘
                ▼
 ┌─────────────────────────────────┐
 │ Diagnostics Pattern Engine      │
 │  - Regex / Signature Detection  │
 └─────────┬───────────────────────┘
           ▼
 ┌────────────────────────────────────┐
 │  LLM Diagnostic Engine             │
 │  - Chain-of-Thought reasoning      │
 │  - Identify root cause             │
 │  - Generate remediation plan       │
 └─────────┬──────────────────────────┘
           ▼
 ┌──────────────────────────────────────┐
 │ Structured Remediation Plan          │
 └──────────────────────────────────────┘

6. Schema Drift Handling

When upstream sources change, refresh failures occur.
This engine:
	•	Extracts missing columns
	•	Generates schema adjustment recommendations
	•	Escalates when breaking changes are detected

ASCII diagram included in /docs/diagrams/schema_drift_light.png.



7. Human-in-the-Loop (HITL)
To ensure responsible AI, HITL is activated when:
	•	Unknown error signatures appear
	•	Schema drift affects critical reports
	•	High-risk actions are required
	•	Multi-step remediation fails

Diagram: /docs/diagrams/hitl_light.png

8. Evaluation — MTTR Reduction Framework
Evaluation focuses on:

TTD — Time To Detect:
Automated detection reduces delay from minutes → seconds.

TTDg — Time To Diagnose:
LLM reasoning eliminates manual log analysis.

TTR — Time To Repair:
Automation replaces manual intervention.

TTV — Time To Verify:
Automated verification refresh completes the repair loop.

PNG diagram: /docs/diagrams/mttr_light.png

9. Repository Structure
src/
  agent/
  powerbi/
  utils/
power_automate/
deployment/
docs/
  diagrams/
tests/
README.md
LICENSE

10. Getting Started
Install Dependencies:
pip install -r src/requirements.txt

Run the Bot Locally:
python src/main.py

Environment Variables:
OPENAI_API_KEY=your_key_here
PBI_ACCESS_TOKEN=your_token_here

11. References (IEEE Style)
[1] C. Gravina, “AI and Data Governance: The Power Duo Reshaping Business Intelligence,”
    Database Trends and Applications Magazine, 2025.

[2] Y. Jiang et al., “A Hybrid Deep Learning Model for Multi-Station Classification and Passenger Flow Prediction,”
    Applied Sciences, vol. 13, no. 5, p. 2899, 2023.

[3] B. Joy and B. A. Elly, “Explainable AI for Interpretable Predictive Models in Critical Business Decisions,”
    International Journal of Advanced Data Mining, vol. 15, no. 4, 2025.

[4] R. Patel and S. Kumar, “Power BI's AI Capabilities for Predictive Analytics,”
    International Conference on Big Data & Analytics, 2023.

[5] J. H. Park, S. S. Lee, and B. C. Kim, “Reducing MTTR with AIOps,” ResearchGate, 2025.

[6] Y. Wu et al., “LLM-Based Data Science Agents: A Survey,” ResearchGate, 2025.

[7] L. Li, Z. Wang, and B. Chen, “From LLM Reasoning to Autonomous AI Agents,” ResearchGate, 2025.

[8] C. A. Johnson, “The Rise of Autonomous AI Agents,” IJAI4S, 2025.

[9] D. L. Martin, “Unlock New Levels of Data Governance Efficiency With Agentic AI,” Techstrong.ai, 2025.

[10] R. S. Singh et al., “Design and Evaluation of a Scalable Data Pipeline,” arXiv, 2025.

[11] K. R. Singh, “10 Best AI ETL Tools for 2025,” Matillion Blog, 2025.
