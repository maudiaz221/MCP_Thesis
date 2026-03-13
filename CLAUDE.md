# MCP Thesis - CLAUDE.md

## Research Purpose

This thesis evaluates **MCP Code Execution vs Direct MCP Tool Calls** in agentic AI systems. The core question: when should an LLM-powered agent call tools directly via MCP protocol versus generating Python code that executes those same tools in a sandboxed container?

We test this across **single-agent** and **multi-agent** architectures, producing a 2x2 factorial design.

## Experimental Design

### Agent Types (2x2 Factorial)

| ID | Architecture | Execution Mode | Description |
|----|-------------|----------------|-------------|
| A1 | Single Agent | Direct MCP | Agent calls MCP server tools directly via protocol |
| A2 | Single Agent | Code Execution | Agent writes Python code executed in Docker sandbox |
| A3 | Multi-Agent | Direct MCP | Orchestrator delegates to sub-agents, each calls MCP directly |
| A4 | Multi-Agent | Code Execution | Orchestrator delegates to sub-agents using code execution |

### Task Types

| ID | Type | Description | Representative Server |
|----|------|-------------|----------------------|
| T1 | Single Tool | One tool invocation | morse (1 tool), cocktails (1 tool) |
| T2 | Chained Tools | Sequential pipeline, output of one feeds into next | nutrition (5 tools), alphavantage (4 tools) |
| T3 | Many Tools | Complex tasks requiring 30+ tools | devtoolkit (60 tools), lastfm (27 tools) |

### 8 MCP Servers

Each server exists in two forms: as an MCP server (for A1/A3) and as Python tool files (for A2/A4).

| Server | Tools | Type | API |
|--------|-------|------|-----|
| morse | 1 | T1 - Single | None (local) |
| cocktails | ~5 | T1/T2 | TheCocktailDB |
| nutrition | 5 | T2 - Chained | None (local) |
| alphavantage | 4 | T2 - Chained | Alpha Vantage API |
| nasa | 4+ | T2 - Chained | NASA Open APIs |
| postgres | 3 | T2 - Chained | PostgreSQL |
| lastfm | 27 | T3 - Many | Last.fm API |
| devtoolkit | 60 | T3 - Many | None (local) |

### Metrics Collected

- **Token usage**: input_tokens, output_tokens, total_tokens
- **Latency**: total_latency_ms, per-tool latency
- **Precision**: LLM-graded task correctness (0-100)
- **Tool calls**: count, success_rate
- **Failure modes**: errors, timeouts, wrong tool selection

### Hypotheses

- **H1**: Code execution reduces total token consumption for chained (T2) and many-tool (T3) tasks
- **H2**: Direct MCP has lower latency for single-tool (T1) tasks
- **H3**: Task precision is equivalent between modes (no accuracy penalty)
- **H4**: There exists a crossover point in tool count where code execution becomes more efficient

## Repository Structure

```
MCP_Thesis/
├── paper/                  # LNCS paper (Spanish, ~12-15 pages)
│   ├── samplepaper.tex     # Main paper file
│   ├── llncs.cls           # Springer LNCS class
│   ├── splncs04.bst        # Bibliography style
│   └── fig1.eps            # Placeholder figure
├── structure/
│   └── outline.md          # Full thesis chapter outline (9 chapters)
├── notes/
│   ├── experiment.md       # Experiment design notes
│   └── resources.md        # Reference links
└── CLAUDE.md               # This file
```

## Connection to OrchAIstra

The experimental framework lives at `/workspaces/research/OrchAIstra/agent/`:

```
OrchAIstra/agent/
├── agent/                  # Core framework
│   ├── types/              # A1-A4 agent implementations
│   ├── base.py             # BaseAgent
│   ├── config.py           # AgentConfig
│   ├── factory.py          # AgentFactory
│   └── metrics.py          # RunResult, ExecutionTrace
├── servers/                # Python tool files (for CODE_MODE A2/A4)
├── mcp_servers/            # MCP server Docker containers (for A1/A3)
├── mcp-code-sandbox/       # Docker sandbox for code execution
├── tasks/                  # LLM-graded task definitions (task_*.md)
└── test_agents/            # Benchmark scripts per server
```

**Key pattern**: Each server has two implementations:
- `mcp_servers/<name>/server.py` — FastMCP server with `@mcp.tool()` decorators (A1/A3)
- `servers/<name>/<tool>.py` — Python files with `Input`/`Output` TypedDicts and functions (A2/A4)

## Writing Guidance

### Paper (paper/samplepaper.tex)
- **Language**: Spanish
- **Format**: LNCS (Springer Lecture Notes in Computer Science)
- **Length**: 12-15 pages
- **Class file**: llncs.cls (already in paper/)
- **Bibliography**: splncs04.bst style, numeric citations

### Writing Order
1. **Section 3 (Diseno Experimental)** — most concrete; A1-A4, T1-T3, servers, metrics defined
2. **Section 2 (Antecedentes)** — MCP protocol, code execution, multi-agent, benchmarks
3. **Section 1 (Introduccion)** — flows from technical content
4. **Sections 4-6** — after running experiments

### Thesis (future, 9 chapters)
- **Language**: Spanish
- **Chapters**: See structure/outline.md for full breakdown
- **Paper maps to thesis**: Sec 1 → Ch.1, Sec 2 → Ch.2+3, Sec 3 → Ch.4+5, Sec 4 → Ch.6, Sec 5 → Ch.7+8, Sec 6 → Ch.9

### What goes in paper vs thesis
- **Paper includes**: 2x2 design, 3 task types, all hypotheses, quantitative results, architecture diagrams
- **Thesis adds**: Decision model (Ch.7), full server implementations, complete task lists, Docker security details, extended lit review, appendices with source code
