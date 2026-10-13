# Repository Exploration Report: Lumen

## 1. Project Purpose and Primary Capabilities
Lumen is an open-source, declarative framework and agentic system within the HoloViz ecosystem for chatting with data, performing retrieval-augmented generation (RAG), creating dynamic data transformation pipelines, generating charts, and building interactive dashboards.

Key capabilities include:
- Declarative data sources, targets, transforms, views, filters, and variables.
- AI Agents (`SourceAgent`, `SQLAgent`, `hvPlotAgent`, `ValidationAgent`, etc.) for natural language data processing and visualization.
- Integration with Panel and HoloViews for UI rendering and server deployment.

## 2. Programming Languages and Important Dependencies
- **Language**: Python >= 3.11 (tested on Python 3.12).
- **Core Dependencies**: `panel`, `holoviews`, `hvplot`, `param`, `bokeh`, `pandas`, `pydantic`.
- **Optional / Extension Dependencies**: `duckdb`, `sqlalchemy`, `xarray`, `sqlglot`, `openai`, `polars`, `pyarrow`.

## 3. Major Directories and Component Responsibilities
- `lumen/`: Core Python package source code.
  - `lumen/ai/`: Agent implementations, tool bindings, LLM interface adapters, prompts, and data quality modules.
  - `lumen/sources/`: Data source integrations (e.g., `DuckDBSource`, `SQLAlchemySource`, `FileSource`, `IntakeSource`).
  - `lumen/transforms/`: Data transformation logic (SQL and Python-based transforms).
  - `lumen/views/`: Visualization view engines (Panel, HoloViews, hvPlot, Vega-Lite).
  - `lumen/filters/`: Interactive and static data filtering abstractions.
  - `lumen/tests/`: Unit and integration test suite structured parallel to the main module architecture.
- `docs/`: Documentation sources and website materials.
- `examples/`: Sample apps and configuration specs.

## 4. Architectural Relationships and Execution Flows
1. **Data Ingestion Flow**: `Source` subclasses encapsulate connectors to external data stores, returning schemas and tables (`get_metadata()`, `get()`).
2. **AI Agent Flow**: User requests are dispatched to specialized agents in `lumen.ai.agents`. Agents utilize tools (`lumen.ai.tools`) to query source metadata, generate SQL/Python code, execute transforms, and produce visualization specifications.
3. **UI & Dashboard Flow**: Views and components bind to sources/transforms and render reactive Panel layouts.

## 5. Coding Conventions and STE Compliance
- Simplified Technical English (STE): Direct statements, factual claims, clear distinctions between verified behavior and preliminary assumptions.
- Typing: Extensive type hinting (`typing`, `pydantic`).
- Code Style: Formatted using standard HoloViz linting rules (`ruff`, `pre-commit`).
