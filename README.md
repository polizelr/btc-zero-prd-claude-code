# Do Zero a Producao - Claude Code

> AI-powered invoice processing pipeline with autonomous monitoring, from requirements to production using Claude Code agentic development.

---

## Overview

### What This Is

This project demonstrates building a complete, production-ready AI invoice processing system using Claude Code as the primary development environment. It showcases modern agentic development practices where specialized AI agents collaborate throughout the entire software development lifecycle - from requirements gathering and architecture design to implementation, testing, and deployment.

### Why This Exists

Restaurant partner reconciliation currently consumes 3 FTEs spending 80% of their time on manual data entry from delivery platform invoices, causing R$45,000+ in reconciliation errors quarterly. This automated pipeline uses Google's Gemini 2.0 Flash for document extraction with 96.5% accuracy at a fraction of the cost (~R$0.002 per invoice).

Beyond solving the immediate business problem, this project serves as a comprehensive reference implementation for:

- **Agentic Development:** Leveraging Claude Code's custom agent system for domain-specific development tasks
- **LLMOps Best Practices:** Structured output validation with Pydantic, observability with LangFuse, and multi-provider fallback strategies
- **Cloud-Native Architecture:** Event-driven serverless pipelines on GCP with Infrastructure as Code
- **Autonomous Operations:** Self-monitoring DataOps with CrewAI agents for triage, investigation, and reporting

### Who This Is For

- **Data Engineers** building AI-powered extraction pipelines
- **Platform Engineers** implementing serverless event-driven architectures
- **ML Engineers** operationalizing LLM-based document processing
- **Teams** exploring agentic development workflows with Claude Code

---

## Quick Start

### Prerequisites

- Claude Code CLI installed and configured
- Google Cloud Platform account with billing enabled
- Python 3.11+
- Terraform 1.5+
- gcloud CLI authenticated

### 60-Second Setup

```bash
# Clone the repository
git clone https://github.com/your-org/btc-zero-prd-claude-code.git
cd btc-zero-prd-claude-code

# Initialize Claude Code with project context
claude

# Explore the codebase using the exploration agent
> /agent codebase-explorer
> "Give me an executive summary of this project"

# Start building with domain-specific agents
> /agent extraction-specialist
> "Help me implement the invoice extraction function"
```

### Using Custom Agents

This project includes 40+ specialized agents. Access them with `/agent <name>`:

```bash
# Architecture and planning
> /agent the-planner          # Strategic planning and roadmaps
> /agent pipeline-architect   # Data pipeline design

# Implementation
> /agent extraction-specialist # LLM extraction with Pydantic
> /agent function-developer    # Cloud Run function development
> /agent infra-deployer       # Terraform/Terragrunt deployment

# Quality and review
> /agent code-reviewer        # Security and quality review
> /agent test-generator       # Automated test creation
```

---

## Features

### AI-Powered Document Extraction

- **Gemini 2.0 Flash** for high-accuracy invoice parsing (96.5% accuracy)
- **Pydantic validation** for structured output with strict schema enforcement
- **OpenRouter fallback** when primary provider is unavailable
- **Vendor-specific prompts** optimized for UberEats, DoorDash, and Grubhub invoices

### Event-Driven Serverless Architecture

- **Cloud Run functions** for stateless, auto-scaling compute
- **Pub/Sub messaging** for decoupled, reliable event processing
- **GCS buckets** with lifecycle policies for input, processing, archive, and failed documents
- **BigQuery** as the analytical data warehouse

### LLMOps Observability

- **LangFuse integration** for cost tracking, latency monitoring, and quality scoring
- **Structured logging** with Cloud Logging export
- **Custom metrics dashboards** for extraction accuracy and pipeline health

### Autonomous Operations (Phase 2)

- **CrewAI multi-agent system** for self-monitoring pipelines
- **Triage Agent** for log monitoring and anomaly detection
- **Root Cause Agent** for automated investigation
- **Reporter Agent** for Slack notifications and status tracking

### Claude Code Agent Ecosystem

- **40+ specialized agents** for every development task
- **7 knowledge base domains** with validated patterns
- **MCP integration** for real-time technology validation

---

## Architecture

### High-Level Pipeline Architecture

```text
                    INVOICE PROCESSING PIPELINE - COMPLETE ARCHITECTURE
+---------------------------------------------------------------------------------+
|                                                                                 |
|  INGESTION          PROCESSING                              STORAGE             |
|  ---------          ----------                              -------             |
|                                                                                 |
|  +-------+   +----------+   +----------+   +----------+   +----------+         |
|  | TIFF  |-->| TIFF->PNG|-->| CLASSIFY |-->| EXTRACT  |-->|  WRITE   |--> BQ   |
|  | (GCS) |   |          |   |          |   | (Gemini) |   |          |         |
|  +-------+   +----------+   +----------+   +----------+   +----------+         |
|      |           |              |              |              |                 |
|      +-----------+--------------+--------------+--------------+                 |
|                              |                                                  |
|                         Pub/Sub (events)                                        |
|                                                                                 |
|  -------------------------------------------------------------------------     |
|                                                                                 |
|  OBSERVABILITY                                                                  |
|  -------------                                                                  |
|                                                                                 |
|  +---------------+    +---------------+    +---------------+                    |
|  |   LangFuse    |    | Cloud Logging |    |Cloud Monitor. |                    |
|  |  (LLM calls)  |    |  (all logs)   |    |  (metrics)    |                    |
|  +---------------+    +---------------+    +---------------+                    |
|                               |                                                 |
|                               v                                                 |
|  -------------------------------------------------------------------------     |
|                                                                                 |
|  AUTONOMOUS OPS (CrewAI)                                                        |
|  -----------------------                                                        |
|                                                                                 |
|  +-------------+    +-----------------+    +-----------------+                  |
|  |   TRIAGE    |--->|   ROOT CAUSE    |--->|    REPORTER     |--> Slack        |
|  |   AGENT     |    |     AGENT       |    |     AGENT       |                  |
|  +-------------+    +-----------------+    +-----------------+                  |
|                                                                                 |
|  -------------------------------------------------------------------------     |
|                                                                                 |
|  CI/CD                                                                          |
|  -----                                                                          |
|                                                                                 |
|  GitHub --> CodeRabbit --> GitHub Actions --> Terraform --> GCP                 |
|                                                                                 |
+---------------------------------------------------------------------------------+
```

### Adapter Pattern for Cloud Portability

```text
                           ADAPTER PATTERN STRUCTURE
+--------------------------------------------------------------------------------+
|                                                                                |
|   INTERFACES                          GCP IMPLEMENTATION        FUTURE (TBD)   |
|   ----------                          ------------------        ------------   |
|                                                                                |
|   +------------------+               +------------------+     +--------------+ |
|   | StorageAdapter   | ------------->| GCSAdapter       |     | S3Adapter    | |
|   +------------------+               +------------------+     +--------------+ |
|                                                                                |
|   +------------------+               +------------------+     +--------------+ |
|   | MessagingAdapter | ------------->| PubSubAdapter    |     | SNSAdapter   | |
|   +------------------+               +------------------+     +--------------+ |
|                                                                                |
|   +------------------+               +------------------+     +--------------+ |
|   | LLMAdapter       | ------------->| VertexAdapter    |     | Bedrock      | |
|   +------------------+               +------------------+     +--------------+ |
|                                                                                |
|   RATIONALE: 10-15% upfront investment for future multi-cloud flexibility      |
|                                                                                |
+--------------------------------------------------------------------------------+
```

### Infrastructure Layout

```text
infrastructure/
+-- modules/
|   +-- cloud-run/       # Cloud Run function definitions
|   +-- pubsub/          # Pub/Sub topics and subscriptions
|   +-- gcs/             # GCS bucket configurations
|   +-- bigquery/        # BigQuery dataset and tables
|   +-- iam/             # Service accounts and permissions
+-- environments/
|   +-- dev/
|   |   +-- terragrunt.hcl
|   +-- prod/
|       +-- terragrunt.hcl
+-- terragrunt.hcl (root)
```

---

## Knowledge Base

The project includes a comprehensive knowledge base with MCP-validated patterns across 7 domains:

| Domain         | Description                                  | Key Patterns                                                           |
| -------------- | -------------------------------------------- | ---------------------------------------------------------------------- |
| **Pydantic**   | Python data validation for LLM output        | `llm-output-validation`, `extraction-schema`, `error-handling`         |
| **GCP**        | Serverless services for event-driven pipes   | `event-driven-pipeline`, `multi-bucket-pipeline`, `cloud-run-scaling`  |
| **Gemini**     | Multimodal LLM for document extraction       | `invoice-extraction`, `structured-json-output`, `openrouter-fallback`  |
| **LangFuse**   | LLMOps observability platform                | `python-sdk-integration`, `cloud-run-instrumentation`, `quality-loops` |
| **Terraform**  | Infrastructure as Code for GCP               | `cloud-run-module`, `pubsub-module`, `remote-state`                    |
| **Terragrunt** | Multi-environment orchestration              | `multi-environment-config`, `dry-hierarchies`, `dependency-management` |
| **CrewAI**     | Multi-agent orchestration for DataOps        | `triage-investigation-report`, `log-analysis-agent`, `slack-integration` |

### Knowledge Base Structure

```text
.claude/kb/
+-- _index.yaml              # Machine-readable registry
+-- _templates/              # Document templates
+-- pydantic/
|   +-- index.md
|   +-- quick-reference.md
|   +-- concepts/            # Core concepts
|   +-- patterns/            # Implementation patterns
|   +-- specs/               # Schema specifications
+-- gcp/
+-- gemini/
+-- langfuse/
+-- terraform/
+-- terragrunt/
+-- crewai/
```

---

## Agent System

### Agent Categories

| Category             | Agents                                                                                       | Purpose                              |
| -------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------ |
| **Exploration**      | `codebase-explorer`, `kb-architect`                                                          | Codebase analysis, KB design         |
| **AI/ML**            | `ai-data-engineer`, `llm-specialist`, `genai-architect`, `ai-prompt-specialist`              | LLM development, prompt engineering  |
| **Data Engineering** | `medallion-architect`, `spark-specialist`, `lakeflow-*`, `spark-*`                           | Data pipeline architecture           |
| **Code Quality**     | `code-reviewer`, `code-cleaner`, `test-generator`, `python-developer`                        | Quality assurance, testing           |
| **Communication**    | `the-planner`, `adaptive-explainer`, `meeting-analyst`                                       | Planning, documentation              |
| **Domain**           | `extraction-specialist`, `pipeline-architect`, `function-developer`, `infra-deployer`        | Project-specific implementation      |
| **Workflow**         | `brainstorm-agent`, `define-agent`, `design-agent`, `build-agent`, `iterate-agent`, `ship-agent` | Development lifecycle stages     |
| **AWS**              | `aws-deployer`, `aws-lambda-architect`, `lambda-builder`, `ci-cd-specialist`                 | AWS-specific development             |
| **Dev**              | `dev-loop-executor`, `prompt-crafter`                                                        | Development iteration                |

### Featured Agents

**extraction-specialist** - LLM extraction expert for invoice processing

```bash
> /agent extraction-specialist
> "Design the extraction prompt for UberEats invoices"
```

- Gemini vision prompt design
- Pydantic schema creation
- LangFuse instrumentation
- Error handling with retries

**pipeline-architect** - Data pipeline design specialist

```bash
> /agent pipeline-architect
> "Design the event-driven architecture for invoice processing"
```

- Event-driven patterns
- Pub/Sub topic design
- Cloud Run function decomposition
- Data flow optimization

**infra-deployer** - Infrastructure deployment expert

```bash
> /agent infra-deployer
> "Set up Terraform modules for the GCP infrastructure"
```

- Terraform module patterns
- Terragrunt environment management
- IAM and security configuration
- CI/CD pipeline setup

---

## Timeline and Milestones

```text
January 2026                    February 2026                    March 2026                      April 2026
------------------------------------------------------------------------------------------------------------
|                               |                               |                               |
| +-------------------------+   | +-------------------------+   | +-------------------------+   | +---------+
| | Planning & Design       |   | | MVP Development         |   | | Testing & Validation    |   | | Launch  |
| | (6 meetings)            |   | | (4 functions + infra)   |   | | (LangFuse + accuracy)   |   | | + Ops   |
| +-------------------------+   | +-------------------------+   | +-------------------------+   | +---------+
|                               |                               |                               |
   Jan 15     Jan 22   Jan 27     Feb 3    Feb 10   Feb 17      Mar 1    Mar 15                  Apr 1    Apr 30
   Meeting 1  Meeting 2 Meeting 3  Meeting 4 Meeting 5 Meeting 6  Testing   Validation           PROD     AutoOps
```

### Key Milestones

| Date             | Milestone                      | Owner         |
| ---------------- | ------------------------------ | ------------- |
| Jan 17, 2026     | GCP project created            | Joao Silva    |
| Feb 1, 2026      | Pub/Sub infrastructure ready   | Pedro Lima    |
| Feb 7, 2026      | All 4 functions implemented    | Joao + Ana    |
| Feb 15, 2026     | First end-to-end test          | Joao Silva    |
| Feb 28, 2026     | MVP demo to stakeholders       | Marina Santos |
| Mar 15, 2026     | Accuracy validation complete   | Ana Costa     |
| **Apr 1, 2026**  | **Production launch**          | Pedro Lima    |
| Apr 30, 2026     | CrewAI pilot complete          | Joao Silva    |

---

## Team

| Name                 | Role                   | Responsibilities                                               |
| -------------------- | ---------------------- | -------------------------------------------------------------- |
| **Marina Santos**    | Product Manager        | Project leadership, requirements, stakeholder management       |
| **Joao Silva**       | Senior Data Engineer   | Architecture, data pipeline, Cloud Run functions, CrewAI       |
| **Ana Costa**        | ML Engineer            | LLM selection, prompt engineering, extraction logic, LangFuse  |
| **Pedro Lima**       | Platform/DevOps Lead   | Infrastructure, CI/CD, Terraform, security                     |
| **Carlos Ferreira**  | Business Stakeholder   | Requirements validation, test data, business acceptance        |

---

## Success Metrics

| Metric                         | Target   | Method                    |
| ------------------------------ | -------- | ------------------------- |
| Extraction accuracy (overall)  | >= 90%   | Ground truth comparison   |
| Processing latency P95         | < 30s    | Cloud Monitoring          |
| Pipeline availability          | > 99%    | Uptime monitoring         |
| Cost per invoice               | < $0.01  | LangFuse + billing        |
| Manual processing reduction    | > 80%    | FTE hours tracking        |
| Time to detect issues          | < 5 min  | CrewAI metrics            |

---

## Technology Stack

| Layer                 | Technology                     | Purpose                              |
| --------------------- | ------------------------------ | ------------------------------------ |
| **Cloud**             | Google Cloud Platform          | Primary infrastructure               |
| **Compute**           | Cloud Run                      | Serverless function execution        |
| **Messaging**         | Pub/Sub                        | Event-driven communication           |
| **Storage**           | GCS                            | File storage (input, processed, etc) |
| **Data Warehouse**    | BigQuery                       | Extracted invoice data storage       |
| **LLM**               | Gemini 2.0 Flash (Vertex AI)   | Document extraction                  |
| **LLM Fallback**      | OpenRouter (Claude 3.5/GPT-4o) | Backup extraction                    |
| **LLMOps**            | LangFuse                       | LLM observability and monitoring     |
| **Schema Validation** | Pydantic                       | Structured output validation         |
| **IaC**               | Terraform + Terragrunt         | Infrastructure provisioning          |
| **CI/CD**             | GitHub Actions                 | Automated testing and deployment     |
| **Code Review**       | CodeRabbit                     | AI-powered PR review                 |
| **Autonomous Ops**    | CrewAI                         | AI agents for self-healing           |
| **Alerts**            | Slack                          | Team notifications                   |

---

## Documentation

| Document                                                       | Description                                        |
| -------------------------------------------------------------- | -------------------------------------------------- |
| [Requirements Document](.claude/notes/summary-requirements.md) | Consolidated requirements from 6 planning meetings |
| [Knowledge Base Index](.claude/kb/_index.yaml)                 | Registry of all KB domains and patterns            |
| [Agent System](.claude/agents/)                                | 40+ specialized agents for development             |

---

## Cost Estimates

| Component          | Monthly (Dev)         | Monthly (Prod)          |
| ------------------ | --------------------- | ----------------------- |
| Cloud Run          | $5-10                 | $10-30                  |
| Pub/Sub            | ~$1                   | ~$2                     |
| GCS                | ~$2                   | ~$5                     |
| BigQuery           | ~$5                   | ~$10                    |
| Gemini 2.0 Flash   | ~$4 (2000 invoices)   | ~$7 (3500 invoices)     |
| LangFuse (Cloud)   | Free tier             | Free tier               |
| **Total**          | **~$20/month**        | **~$55/month**          |

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Use Claude Code agents for development (`/agent code-reviewer` before committing)
4. Commit your changes following conventional commits
5. Push to the branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request (CodeRabbit will automatically review)

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- **Anthropic** for Claude Code and the agentic development paradigm
- **Google Cloud** for Gemini 2.0 Flash and serverless infrastructure
- **LangFuse** for LLMOps observability
- **CrewAI** for multi-agent orchestration patterns

---

> *"Do Zero a Producao"* - From Zero to Production, powered by AI-assisted development.
