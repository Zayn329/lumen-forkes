# Repository Exploration Report: Lumen

This report provides a practical overview of the `lumen` repository architecture, dependencies, conventions, and key execution flows. It uses Simplified Technical English principles (ASD-STE100) to ensure clarity, directness, and precision.

---

## 1. Project Purpose

Lumen is an open-source, declarative framework and AI agent platform for data analysis, visualization, and dashboard creation.

### Key Capabilities
- **Declarative Data Pipelines**: Define data sources, transformations, filters, and views using YAML configurations or Python objects.
- **AI Agent System**: Use natural language with Large Language Models (LLMs) to query data, construct SQL pipelines, generate charts (via hvPlot, Vega-Lite, PyDeck), summarize documents, and build interactive dashboards.
- **Interactive UI**: Serve dynamic dashboards and conversational chat applications built on top of Panel, Bokeh, and Param.

---

## 2. Tech Stack and Dependencies

### Primary Language
- **Python**: Requirements specify `python >= 3.11`.

### Core Frameworks & Core Libraries
- **Panel (`>= 1.9.0`)**: Powers the web user interface, layout engine, and server execution.
- **Bokeh (`>= 3.9.0`)**: Provides low-level rendering and visualization primitives.
- **Param (`>= 2.2.1, < 2.5.0`)**: Provides reactive parameter handling and type-validated declarative classes.
- **HoloViews / hvPlot**: Generates interactive data visualizations.

### AI & LLM Stack
- **Instructor (`>= 1.14.3`) & Pydantic (`>= 2.8.0`)**: Structured data extraction and validated schema outputs from LLMs.
- **OpenAI / LiteLLM / Ollama / Anthropic / Google GenAI**: Integration with LLM providers.
- **Narwhals (`>= 2`)**: Unified DataFrame compatibility layer (Pandas, Polars, PyArrow).
- **DuckDB (`>= 1.2.0`) & SQLGlot**: Local analytical SQL processing and query translation.

### Package & Environment Management
- **Pixi (`pixi.toml`)**: Package and environment management.
- **Hatchling (`pyproject.toml`)**: Build backend system.

---

## 3. Coding Practices and Conventions

### Code Style and Formatting
- **Ruff**: Configured in `pyproject.toml` with line length limit 165. Enforces rules for bugbear (`B`), pyflakes (`F`), pycodestyle (`E`/`W`), numpy (`NPY`), print statements (`T20`), and pyupgrade (`UP`).
- **Isort**: Code import ordering with forced grid wrap and trailing commas.
- **Pyright**: Static type checking configured specifically for `lumen/ai`.

### Object Model Conventions
- **Param-based Design**: Classes inherit from `param.Parameterized` or `lumen.base.Component`.
- **Declarative Specifications**: Class attributes are defined as `param.Parameter` descriptors with automatic validation and change notifications.
- **Serialization**: Base classes implement `.to_spec()` and `.from_spec()` for JSON/YAML schema conversion.

---

## 4. Entry Points and Command Reference

### Package CLI Commands
As defined in `pyproject.toml`:
- `lumen`: Alias for `lumen.command.ai:main`. Starts the AI chat and data exploration interface.
- `lumen-ai`: Directly runs `lumen.command.ai:main`.
- `lumen-spec`: Runs `lumen.command:main` to serve YAML spec-based dashboards.

### Common Development Tasks (Pixi Tasks)
- **Start AI Server**: `pixi run serve-ai <data_file>`
- **Run Tests**: `pixi run test`
- **Run Spec Tests**: `pixi run test-spec`
- **Run AI Tests**: `pixi run test-ai`
- **Lint Code**: `pixi run lint`

---

## 5. Major Components and Responsibilities

| Directory | Responsibility | Key Files |
| :--- | :--- | :--- |
| `lumen/` | Core package root and component registry | `base.py`, `dashboard.py`, `pipeline.py`, `state.py` |
| `lumen/sources/` | Data connectors for SQL databases, DuckDB, files, and remote APIs | `base.py`, `duckdb.py`, `sqlalchemy.py` |
| `lumen/transforms/` | Data transformations operating on DataFrames or SQL queries | `base.py`, `sql.py` |
| `lumen/filters/` | UI and data filtering components | `base.py` |
| `lumen/views/` | Visualization renderings (hvPlot, Bokeh, Table views) | `base.py` |
| `lumen/variables/` | Reactive reactive state variables | `base.py` |
| `lumen/ai/` | LLM agents, coordinators, vector stores, tools, and prompts | `actor.py`, `llm.py`, `agents/`, `coordinator/`, `tools/` |
| `lumen/command/` | Command-line interface entry points | `ai.py`, `builder.py`, `validate.py` |
| `lumen/ui/` | Panel-based graphical user interface for building dashboards | `main.py`, `builder.py` |

---

## 6. Repository Map

```
lumen-forked-contrib/
├── docs/                      # Documentation source files
├── examples/                  # Example datasets and dashboards
├── lumen/                     # Main Python package source code
│   ├── ai/                    # AI subsystem (agents, prompts, tools, coordinators)
│   │   ├── agents/            # Specialized domain agents (SQL, hvPlot, VegaLite, etc.)
│   │   ├── controls/          # UI widgets for AI interaction and ingestion
│   │   ├── coordinator/       # Multi-agent coordination and planning logic
│   │   ├── prompts/           # Jinja2 prompt templates
│   │   └── tools/             # Agent tools (vector lookup, metadata, SQL tools)
│   ├── command/               # CLI implementations (lumen-ai, lumen-spec)
│   ├── filters/               # Interactive UI filter components
│   ├── sources/               # Data source connector classes
│   ├── transforms/            # Data transformation step definitions
│   ├── ui/                    # Visual builder interface components
│   ├── variables/             # Reactive state and variable handlers
│   ├── views/                 # Visual display components
│   ├── base.py                # Core Component base class
│   ├── dashboard.py           # Dashboard orchestration engine
│   ├── pipeline.py            # Data pipeline specification and runtime
│   └── state.py               # Global application state management
├── scripts/                   # Utility and maintenance scripts
├── pixi.toml                  # Pixi package and task definitions
├── pyproject.toml             # Python build, dependencies, and linter settings
└── README.md                  # Project overview and quickstart guide
```

---

## 7. Architectural Map

```
                                  +-----------------------+
                                  |    CLI / User Interface|
                                  |  (lumen-ai / Panel UI)|
                                  +-----------+-----------+
                                              |
                                              v
                                  +-----------------------+
                                  |  AI Coordinator /     |
                                  |  Actor System         |
                                  +-----------+-----------+
                                              |
                     +------------------------+------------------------+
                     |                                                 |
                     v                                                 v
        +-------------------------+                       +-------------------------+
        | LLM Agents & Tools      |                       | Declarative Engine      |
        | (SQL, hvPlot, VegaLite) |                       | (Dashboard / Pipeline)  |
        +------------+------------+                       +------------+------------+
                     |                                                 |
                     v                                                 v
        +-------------------------+                       +-------------------------+
        | Output Specifications   |---------------------->| Sources & Transforms    |
        | (SQL / Views / Configs) |                       | (DuckDB / Narwhals)     |
        +-------------------------+                       +-------------------------+
```

---

## 8. Important Dependency Relationships

1. **`lumen.ai` -> `lumen.core`**: The AI subsystem generates declarative specifications (`Pipeline`, `Source`, `View`) that feed directly into Lumen core components.
2. **`lumen.views` -> Panel / HoloViews / Bokeh**: Views convert processed data into interactive Panel objects and Bokeh figures.
3. **`lumen.sources` -> DuckDB / Narwhals / SQLAlchemy**: Sources retrieve raw data and standardize it into Narwhals-compatible DataFrames or SQL tables.
4. **`lumen.ai.agents` -> Instructor / Pydantic / LLM APIs**: Agents send system prompts to LLMs and receive strictly typed Pydantic models back.

---

## 9. Major Data and Execution Flows

### A. Spec Dashboard Execution Flow
1. User invokes `lumen-spec dashboard.yaml`.
2. `lumen.command.main()` parses the YAML specification.
3. `Dashboard.from_spec()` instantiates `Source`, `Pipeline`, `Filter`, and `View` objects.
4. Data flows from `Source` -> `Transforms` -> `Filters` -> `View`.
5. Panel serves the resulting interactive dashboard.

### B. Conversational AI Data Exploration Flow
1. User submits a natural language prompt via `lumen-ai`.
2. `Actor` receives user input and consults `Coordinator` and `Planner`.
3. `Coordinator` selects specialized agents (e.g., `SQLAgent`, `hvPlotAgent`).
4. Agents run tools (e.g., table stats, metadata lookup, query generation).
5. Output code/specifications are validated and rendered live in the Panel UI chat interface.

---

## 10. Areas for Deeper Investigation

- **`lumen/ai/coordinator/`**: Internal routing logic for dynamic agent selection and tool execution loops.
- **`lumen/ai/code_executor.py`**: Execution sandbox boundaries and security guardrails for generated Python code.
- **`lumen/sources/`**: Optimization layer for large-scale SQL query pushing and lazy evaluation across disparate databases.

---

## 11. Facts vs. Assumptions

### Observed Facts
- Dependencies, entry points, and linters are explicitly configured in `pyproject.toml` and `pixi.toml`.
- The codebase uses `param.Parameterized` as the base class for declarative primitives.
- Code style rules are enforced using `ruff` and `isort`.

### Inferences / Assumptions
- `pixi` is the standard tool used by maintainers for local development and test execution.
- LLM interactions rely on `instructor` for schema validation across different vendor models.
