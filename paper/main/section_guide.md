# Paper Section Guide

---

## Section 2: Antecedentes y Trabajo Relacionado
**Maps to thesis**: Ch.2 (Revisión de Literatura) + Ch.3 (Marco Teórico)
**Write**: 2nd

### 2.1 Model Context Protocol (MCP)
- Origin: Anthropic Nov 2024, open protocol for LLM-tool communication
- Architecture: client-server, JSON-RPC, stdio/SSE transport
- Lifecycle: discovery → schema loading → invocation → response
- Adoption: Claude Desktop, AWS Strands, VS Code extensions, open-source ecosystem
- Key technical detail: tool schemas are injected into context window before user prompt

### 2.2 Ejecución de Código en Sistemas Agénticos
- The code execution pattern (Anthropic engineering blog): model writes Python that calls tools
- How it works: sandbox receives code → executes → returns only final result
- Sandboxing technologies: Docker (this work), E2B, Cloudflare Workers
- Key difference: tool schemas are NOT loaded into model context; model discovers them via code imports
- Reference implementation: Strands code_interpreter (AWS)

### 2.3 Arquitecturas Multi-Agente
- Topologies: orchestrator-worker, swarm, pipeline
- Orchestrator pattern (used in this work): main agent delegates to specialized sub-agents
- Frameworks: AutoGen (Microsoft), CrewAI, Strands (AWS)
- How tool calling scales differently in single vs multi-agent

### 2.4 Trabajo Relacionado
- **Tool-use benchmarks**: BFCL (Berkeley Function Calling Leaderboard), API-Bank, ToolBench
  - These measure *capability* (can the model call the right tool?) not *efficiency* (how many tokens?)
- **Multi-agent evaluations**: GAIA, AgentBench
  - Focus on end-to-end task completion, not execution mode comparison
- **Token efficiency studies**: if any exist, cite; otherwise highlight the gap
- **Our differentiation**: first systematic study comparing HOW tools are executed (direct vs code) across agent architectures, measuring efficiency metrics

---

## Section 3: Diseño Experimental
**Maps to thesis**: Ch.4 (Diseño Experimental) + Ch.5 (Metodología)
**Write**: 1st (most concrete - all components already built)

### 3.1 Matriz Factorial
Table: 2×2 matrix

|  | MCP Directo | Ejecución de Código |
|--|-------------|---------------------|
| Agente Único | A1 | A2 |
| Multi-Agente | A3 | A4 |

- A1: Single agent calls MCP tools directly via protocol
- A2: Single agent writes Python executed in Docker sandbox
- A3: Orchestrator + sub-agents, each calls MCP directly
- A4: Orchestrator + sub-agents, each uses code execution

### 3.2 Suite de Tareas (T1, T2, T3)

| Type | Description | # Tools | Servers |
|------|-------------|---------|---------|
| T1 - Simple | Single tool invocation | 1 | morse, cocktails |
| T2 - Chained | Sequential pipeline, output→input | 3-5 | nutrition, alphavantage, nasa, postgres |
| T3 - Complex | Many tools required | 27-60 | lastfm, devtoolkit |

For each server: list example tasks with expected outputs

### 3.3 Servidores MCP
Table of all 8 servers: name, tool count, task type, external API, dual implementation

Explain dual implementation:
- MCP version: FastMCP server in Docker, `@mcp.tool()` decorators
- Code version: Python files with TypedDict Input/Output, importable functions

### 3.4 Framework: OrchAIstra
- Architecture diagram: AgentFactory → Agent Types → MCP Servers / Docker Sandbox
- Execution flow of one experiment run
- SandboxImageBuilder: aggregates requirements, builds Docker image per server set
- MetricsCollector: tokens, latency, tool calls, traces

### 3.5 Métricas
Formal definitions:
- **Tokens**: input_tokens, output_tokens, total_tokens (from Bedrock API)
- **Latency**: total_latency_ms (wall clock, includes Docker overhead)
- **Precision**: LLM-graded score 0-100 (grading rubric in task files)
- **Tool calls**: count, success_rate
- **Failure modes**: wrong tool, wrong params, timeout, sandbox error

### 3.6 Hipótesis
- H1: Code execution reduces total tokens for T2 and T3 tasks
- H2: Direct MCP has lower latency for T1 tasks (no sandbox overhead)
- H3: Precision is equivalent between execution modes
- H4: There exists a crossover point in tool count where code execution becomes more efficient

### 3.7 Variables de Control
- LLM model: fixed (Claude via AWS Bedrock)
- Temperature: 0.2
- Repetitions per task: N runs for statistical significance
- Docker config: --network none --memory 512m --cpus 1 --read-only

---

## Section 4: Resultados
**Write**: After running experiments

### 4.1 Resultados por Arquitectura
- Tables: A1 vs A2 vs A3 vs A4 for each task type
- Aggregate metrics per configuration

### 4.2 Análisis de Tokens
- Bar charts: total tokens by configuration and task type
- Breakdown: input vs output tokens
- Key comparison: MCP direct vs code execution token ratios

### 4.3 Análisis de Latencia
- Execution times per configuration
- Docker overhead quantification
- Per-tool latency vs end-to-end

### 4.4 Análisis de Precisión
- Success rates by configuration and task type
- LLM grader score distributions
- Failure mode categorization

### 4.5 Análisis Estadístico
- Significance tests (paired t-test or Wilcoxon)
- Confidence intervals
- Effect sizes

---

## Section 5: Discusión
**Write**: After results

### 5.1 Interpretación de Resultados
- Map findings to H1-H4
- Unexpected patterns

### 5.2 Análisis de Modos de Fallo
- When does each approach fail and why?
- Error categorization

### 5.3 Implicaciones Prácticas
- Decision guidelines for developers
- When to use MCP direct vs code execution
- Cost-performance tradeoffs

### 5.4 Limitaciones
- Single LLM model
- Synthetic tasks
- Docker-specific sandbox
- Limited server diversity

---

## Section 6: Conclusiones
**Write**: Last

- Summary of key findings
- Contributions recap
- Future work: more models, more tasks, prompt optimization, decision model

---

## Writing Order Summary
1. **Section 3** (Diseño Experimental) - everything is built, just describe it
2. **Section 2** (Antecedentes) - frame the technical context
3. **Section 1** (Introducción) - DONE
4. **Sections 4-6** - after running experiments
