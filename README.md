
# AgentDebug v4.0 - README

## Overview

**AgentDebug v4.0** is a comprehensive, self-contained debugging framework for AI agent trajectories. It combines **fine-grained error analysis**, **critical error detection**, and **reinforcement learning (RL)-guided iterative debugging** to autonomously identify, prioritize, and correct failures in agent execution traces.

Built with **First Principles reasoning**, the system models agent behavior as modular steps (planning, tool use, memory, system) and uses a **Q-learning agent** to learn optimal debugging strategies over time. Performance is rigorously benchmarked across time, memory, success rate, and learning convergence.

---

## Key Features

| Feature | Description |
|-------|-----------|
| **Error Taxonomy** | Probabilistic error injection based on real-world failure modes |
| **Trajectory Modeling** | Structured representation of agent steps with state, action, module |
| **Fine-Grained Analysis** | Stochastic error detection with context-aware heuristics |
| **Critical Error Detection** | Prioritizes planning/tool failures (high-impact) |
| **RL-Guided Debugging** | Q-learning agent learns which modules to fix first |
| **Performance Benchmarking** | Time, memory, success rate, Q-value convergence |
| **Cross-Platform Memory Tracking** | `resource` (Unix) / `tracemalloc` (Windows) |
| **Extensible Tools & Agents** | Plug-in real tools (web search, code analysis) |
| **Unit Tested** | Full test suite with edge cases |

---

## System Architecture

```
[Agent Execution] → [Trajectory] → [Fine-Grained Analysis]
        ↓
[Critical Error Detection] → [RL Agent Chooses Module]
        ↓
[Correct Step] → [Update Q-Table] → [Repeat until Success]
```

### Core Components

1. **`Step`** – Atomic unit: `(state, action, module)`
2. **`Trajectory`** – Sequence of steps; evaluates success
3. **`DebuggingAgent`** – Q-learning agent over error states
4. **`Benchmarker`** – Measures time/memory/success across runs
5. **`SimpleAgent`** – Simulates real agent with tools + memory

---

## Error Taxonomy

| Module | Error Type | Probability |
|--------|------------|-----------|
| **planning** | `reasoning_loop` | 0.4 |
| | `incomplete_plan` | 0.6 |
| **tool** | `tool_selection_error` | 0.5 |
| | `invalid_tool_input` | 0.5 |
| **memory** | `retrieval_failure` | 0.7 |
| | `context_overflow` | 0.3 |
| **system** | `execution_timeout` | 0.6 |
| | `resource_limit_exceeded` | 0.4 |

> Errors are injected stochastically during simulation and analysis.

---

## Tools Included

| Tool | Function |
|------|--------|
| `web_search` | Simulates search: `"Web search results for: {query}"` |
| `code_analysis` | Returns: `"Code analysis results for: {query}"` |

> Easily extendable via `Tool(name, func, description)`

---

## Benchmark Scenarios

| Scenario | Trajectory Size | Query |
|--------|------------------|-------|
| Small | 5 | `"Find AI info"` |
| Medium | 100 | `"Find AI info"` |
| Large | 1000 | `"Complex query for AI debugging analysis"` |

- **10 runs per scenario**
- Metrics logged to `benchmark_results.txt`
- Includes **std dev**, **peak memory**, **Q-value change**

---

## Output Example

```text
Initial Trajectory Analysis:
+--------+----------+---------------------+---------+
| Step   | Module   | Action              | Error   |
+========+==========+=====================+=========+
| 1      | planning | reason_query        | None    |
| 2      | memory   | retrieve_context    | retrieval_failure |
| 3      | tool     | web_search          | None    |
| 4      | planning | generate_response   | None    |
| 5      | memory   | store_context       | None    |
+--------+----------+---------------------+---------+

Iteration 1: Refined feedback: Focused on memory. Critical error at step 2 (memory): retrieval_failure...
Trajectory successful after debugging!
```

---

## Files Generated

- `benchmark_results.txt` – Full benchmark logs
- Console output – Real-time trajectory + debugging steps

---

## How to Run

### 1. Execute Main Demo

```bash
python AgentDebugv4.0.txt
```

> Runs:
> - Single agent simulation
> - 3-step iterative debugging
> - Final trajectory evaluation

### 2. Run Full Benchmarks

```bash
python AgentDebugv4.0.txt
```

> Automatically runs:
> - 3 scenarios × 10 runs
> - Aggregated metrics
> - Logs to `benchmark_results.txt`

### 3. Run Unit Tests

```bash
python -m unittest AgentDebugv4.0.txt
```

> Or let the script run them automatically at the end.

---

## Requirements

- Python 3.8+
- Standard libraries only:
  - `random`, `logging`, `unittest`
  - `typing`, `collections`, `numpy`
  - `tabulate`, `time`, `platform`
  - `resource` (Unix), `tracemalloc` (Windows)

> **No external dependencies**

---

## Customization

### Add New Modules
```python
ERROR_TAXONOMY["new_module"] = {"error_a": 0.7, "error_b": 0.3}
```

### Add New Tools
```python
def my_tool(query): return f"Result: {query}"
new_tool = Tool("my_tool", my_tool, "Does something useful")
agent = SimpleAgent(tools=[..., new_tool])
```

### Tune RL Agent
```python
rl_agent = DebuggingAgent(
    modules=[...],
    learning_rate=0.2,
    discount_factor=0.95,
    epsilon=0.3
)
```

---

## Design Philosophy (First Principles)

1. **All agent failures are observable in trajectory**
2. **Critical errors (planning/tool) block success**
3. **Debugging is a sequential decision problem → RL**
4. **Measure everything: time, memory, learning**
5. **Self-contained, reproducible, extensible**

---

## Future Improvements

- [ ] Persistent Q-table across runs
- [ ] Multi-agent collaborative debugging
- [ ] Real LLM integration (via API)
- [ ] Visualization dashboard (Matplotlib/Plotly)
- [ ] Confidence scoring per correction

---

## Author

**numbnut** – Built with First Principles reasoning and better axioms.

> _"If you cannot debug it, you cannot trust it."_

---

## License

MIT License – Free to use, modify, and extend.

---

**AgentDebug v4.0** – *Because perfect agents don’t exist. Perfect debuggers do.*
