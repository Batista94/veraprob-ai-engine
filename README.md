# Autonomous SLA Reconciliation Engine — VeraProb
**Deterministic Multi-Agent System for B2B Contract Audit & Temporal Reconciliation**

[🇺S English](#english) | [🇧🇷 Português](#português)

---

<a name="português"></a>
## 🇧🇷 Português

### 🎯 Visão Geral & Valor de Negócio
Este motor é um sistema distribuído de Inteligência Artificial desenhado para resolver o problema de **Revenue Leakage** em operações B2B. Ele substitui auditorias manuais por um pipeline **determinístico** que cruza restrições jurídicas (PDF) com telemetria transacional (Logs).

**O diferencial**: Diferente de RAGs comuns, este sistema utiliza **Raciocínio Temporal** e **Injeção de Dependência** para garantir que a lógica de auditoria seja portável entre ambientes locais (DuckDB) e corporativos (Snowflake/Databricks).

### 🏗️ Diferenciais de Engenharia
- **Decoupled Action (MCP)**: Implementação do Model Context Protocol para separar o raciocínio do modelo da execução de ferramentas.
- **Adapter Pattern**: Abstração completa da camada de dados. O sistema é agnóstico ao banco de dados.
- **Human-in-the-Loop (HITL)**: Persistência de estado via SQLite para aprovação humana em infrações de alto valor.

<a name="english"></a>
## 🇺S English

### 🎯 Overview & Business Value
An AI-driven engine designed to solve **Revenue Leakage** in high-complexity B2B operations. It replaces manual audits with a **deterministic pipeline** that bridges the gap between Unstructured Legal Contracts and Structured Operational Telemetry.

**The Edge**: Unlike standard RAGs, this system employs **Temporal Reasoning** and **Dependency Injection** to ensure the audit logic is portable across local environments (DuckDB) and enterprise warehouses (Snowflake/Databricks).

### 🏗️ Engineering Highlights
- **Decoupled Action (MCP)**: Built using the Model Context Protocol to strictly separate model reasoning from tool execution.
- **Adapter Pattern**: Complete data-layer abstraction. The engine is database-agnostic.
- **Human-in-the-Loop (HITL)**: State persistence via SQLite, enabling human oversight for high-threshold financial penalties.

### 🛠️ Tech Stack

| Component      | Technology       | Senior Justification                          |
|----------------|------------------|-----------------------------------------------|
| Orchestration  | LangGraph        | Stateful, cyclic agentic workflows with native HITL |
| Data Engine    | DuckDB / Polars  | OLAP performance in-memory. Easily swappable via Adapters |
| Parsing        | Docling (IBM)    | Topology-aware PDF parsing (preserving complex SLA tables) |
| Protocol       | MCP              | Standardizes tool interaction, making the engine plug-and-play |
| Validation     | DeepEval         | Metric-based testing for Hallucination and Faithfulness |
| Infrastructure | Docker           | Multi-stage builds for hermetic and slim production images |

### 🚀 Setup & Execution
O projeto utiliza **Makefile** e **Poetry** para padronizar o ambiente de desenvolvimento.

```bash
# Install dependencies
make install

# Generate synthetic logs for testing
poetry run python src/data_gen/generate_logs.py

# Run the Audit Suite
make audit
```

#### Environment Variables (`.env`)
```bash
OPENAI_API_KEY=sk-...
LANGSMITH_API_KEY=ls-...
DATA_ENGINE=duckdb # options: duckdb, snowflake_adapter
```

### 📂 Project Structure
.
├── src/
│ ├── agents/ # LangGraph Nodes & State logic
│ ├── data_engine/ # Abstract Base Classes & Adapters (DuckDB, Snowflake)
│ ├── mcp_server/ # Model Context Protocol implementation
│ └── parsers/ # Docling & Layout analysis logic
├── tests/ # Unit, Integration & DeepEval (LLM-as-a-Judge)
├── ARCHITECTURE.md # Deep dive into engineering decisions
└── Makefile # Automation entry points


### 🛠️ Developer Experience (DX) & Quality Assurance
- **Local-First Development**: Setup completo em < 2 minutos via `make install`.
- **CI Ready**: Configurado para rodar `Ruff` (linting) e `Pytest` em pipelines de integração contínua.
- **Observability**: Integração nativa com LangSmith para debugging de traços de agentes em tempo real.

---

## 🗺️ Roadmap de Evolução / Evolution Roadmap
| Fase | Feature | Status |
|------|---------|--------|
| 1 | Extração de métricas de Uptime e Latência (MVP) / Uptime & Latency metrics extraction | [ ] |
| 2 | Multi-PDF context (Contrato Principal + Aditivos) / Multi-PDF context (Main + Addendums) | [ ] |
| 3 | Interface via MCP para Claude Desktop / MCP interface for Claude Desktop | [ ] |
| 4 | Exportação de evidências para logs imutáveis / Immutable audit trail export | [ ] |

---

[![stars](https://img.shields.io/github/stars/Batista94/veraprob-ai-engine?style=social)](https://github.com/Batista94/veraprob-ai-engine)
[![license](https://img.shields.io/github/license/Batista94/veraprob-ai-engine)](LICENSE)
[![python](https://img.shields.io/badge/Python-3.11%2B-blue)](https://www.python.org/)
[![poetry](https://img.shields.io/badge/Poetry-1.8%2B-9356fc)](https://python-poetry.org/)
