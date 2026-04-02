# Autonomous SLA Reconciliation Engine - VeraProb
> **Deterministic Multi-Agent System for B2B Contract Audit & Temporal Reconciliation**

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue)](https://python.org)
[![Architecture](https://img.shields.io/badge/Architecture-Event--Driven%20State%20Machine-orange)]()
[![Data Engine](https://img.shields.io/badge/Data-DuckDB%20%7C%20Polars-black)]()

Este motor é um sistema distribuído de Inteligência Artificial desenhado para resolver o problema de **Revenue Leakage** em operações B2B de alta complexidade (TI, Telecom, Logística). Ele substitui processos de auditoria manual por um pipeline determinístico que cruza restrições jurídicas não-estruturadas com telemetria transacional de alto volume.

---

## Escopo do Sistema & Valor de Negócio (The "Why")

Em contratos Enterprise, os Acordos de Nível de Serviço (SLAs) possuem condicionais complexas (ex: *janelas de manutenção, multas exponenciais, exceções de feriados*). RAGs tradicionais falham miseravelmente nisso porque carecem de raciocínio lógico-temporal e precisão matemática.

**Este motor garante:**
1. **Tradução Jurídica para Código:** Converte cláusulas textuais (PDF) em schemas fortemente tipados (Pydantic V2).
2. **Reconciliação Temporal:** Realiza *joins* complexos entre regras contratuais e logs de séries temporais (Time-Series Telemetry).
3. **Auditabilidade Absoluta:** O sistema não gera "respostas", ele gera **Dossiês de Infração**, citando a página do contrato, a linha do log, e o cálculo matemático da multa.

---

##  Arquitetura do Grafo (State Machine & HITL)

O núcleo operacional não é um LLM isolado, mas uma **Máquina de Estados Finita** orquestrada via LangGraph. O fluxo garante recuperação de falhas (Fault Tolerance) e governança humana.

### O Ciclo de Vida da Auditoria:
1. **Ingestão Estruturada (Docling):** Quebra do PDF jurídico mantendo a integridade topológica de tabelas e cláusulas aninhadas.
2. **Dynamic Schema Generation:** Um LLM atua como extrator, gerando um JSON Schema com as regras de SLA.
3. **Temporal SQL Synthesis:** Um agente especializado converte o JSON Schema em *Queries SQL Dinâmicas*.
4. **OLAP Execution (DuckDB + Polars):** A query é executada *in-memory* contra a base de logs operacionais (suporta milhões de linhas em milissegundos).
5. **Chain of Verification (CoV):** Um agente "Crítico" revisa o output do SQL contra o texto original para evitar falsos positivos.
6. **Human-in-the-Loop (HITL) Gate:** Se uma infração é detectada e a penalidade calculada excede o limite (Threshold) de risco, o estado do grafo é congelado. O sistema aguarda o *callback* de um operador humano para aprovar a notificação ao fornecedor.

---

## Stack Tecnológica & Decisões de Engenharia

| Componente | Tecnologia | Trade-off / Justificativa |
| :--- | :--- | :--- |
| **Orquestração de Estado** | `LangGraph` | Substitui pipelines lineares (LangChain) para permitir grafos cíclicos, persistência de estado (SQLite/Postgres) e interrupções HITL. |
| **Parsing Documental** | `Docling` (IBM) | Supera PyPDF e Tesseract ao entender hierarquias de documentos complexos, vital para não perder o contexto de anexos de SLA. |
| **Motor Analítico** | `DuckDB` + `Polars` | Elimina a necessidade de Data Warehouses em nuvem (Snowflake/BigQuery) para processamento OLAP local de baixo custo e altíssima velocidade. |
| **Validação e Guardrails** | `DeepEval` + `Pydantic` | LLMs são probabilísticos; a auditoria exige determinismo. Pydantic força a estrutura, e DeepEval aplica testes unitários nas saídas semânticas. |

---

##  Estratégia de MLOps e Validação

Para assegurar confiabilidade em nível de produção, este repositório adota testes rigorosos:
* **Synthetic Telemetry Generation:** Como dados reais são protegidos por NDA, o módulo `src/data_gen` cria milhares de logs sintéticos de uptime/downtime com anomalias propositais para testar a acurácia do agente.
* **LLM-as-a-Judge:** Pipelines CI/CD rodam métricas de *Faithfulness* (Fidelidade ao PDF) e *Answer Relevancy* a cada novo PR.

---
    
## Setup Hermético (Desenvolvimento)

O projeto utiliza `Poetry` para isolamento e reprodutibilidade do ambiente.

```bash
# Clone o repositório
git clone [https://github.com/Batista94/veraprob-ai-engine](https://github.com/Batista94/veraprob-ai-engine.git)

# Inicialize o ambiente hermético
cd sla-reconciliation-engine
poetry install

# Rode a suíte de testes de sanidade
poetry run pytest tests/