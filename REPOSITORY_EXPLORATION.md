# Lumen Repository Exploration Report

## 1. Project Purpose and Primary Capabilities

Lumen is a declarative, framework-agnostic data processing, dashboard, and AI agent framework built on Panel, Bokeh, and Param.
It enables engineers and data scientists to build complex data applications using simple YAML specifications or Python APIs.

Key capabilities include:
- **Data Sources (`lumen.sources`)**: Connects to diverse data backends including DuckDB, SQL, Pandas, Intake, REST APIs, and file formats (CSV, Parquet, JSON).
- **Data Pipelines (`lumen.pipeline`)**: Declarative data processing pipelines supporting transforms, filters, aggregations, and streaming.
- **Views & Panels (`lumen.views`)**: Visualization components wrapping Panel/Holoviews/Bokeh layouts and interactive widgets.
- **AI Agents (`lumen.ai`)**: Dynamic LLM-driven agents and tools (`SourceAgent`, `SQLAgent`, `ExplorerAgent`, `FunctionTool`) for autonomous data analysis and natural language querying.
- **Dashboard Deployment**: CLI and runtime tools (`lumen serve`, `lumen build`) to serve declarative web applications.

## 2. Programming Languages and Key Dependencies

- **Primary Language**: Python (>= 3.11)
- **Core Dependencies**:
  - `panel` >= 1.4.0 (UI layouts and widgets)
  - `param` >= 2.0.0 (Declarative parameter management and reactive state)
  - `bokeh` (Plotting and rendering backend)
  - `pandas` / `polars` / `duckdb` (Data handling and SQL processing)
  - `pyyaml` (Declarative YAML configuration parsing)
- **Build System**: `hatchling` with `hatch-vcs`
- **Environment Management**: `pixi` (`pixi.toml`) and `pyproject.toml`

## 3. Major Directories and Component Responsibilities

- `lumen/`: Core Python package directory.
  - `lumen/sources/`: Data source connectors (`BaseSource`, `DuckDBSource`, `SQLAlchemySource`, `PandasSource`, `JoinedSource`, etc.).
  - `lumen/transforms/`: Data transformations (`SQLTransform`, `FilterTransform`, `GroupbyTransform`, etc.).
  - `lumen/filters/`: Interactive dashboard filter controls (`WidgetFilter`, `ParamFilter`).
  - `lumen/views/`: Visualizations and dashboard component wrappers (`ParamView`, `hvPlotView`, `Table`).
  - `lumen/pipeline/`: Data flow orchestration connecting Sources -> Transforms -> Filters -> Views.
  - `lumen/ai/`: AI components, LLM agents, agent tools, memory, and chat UI.
  - `lumen/target.py` & `lumen/dashboard.py`: Higher-level layout and multi-view application containers.
  - `lumen/command/`: CLI entrypoints (`lumen serve`, `lumen builder`).
- `lumen/tests/`: Comprehensive test suite grouped by module (`tests/sources/`, `tests/ai/`, `tests/transforms/`, etc.).
- `docs/`: Documentation sources (MkDocs / Zensical).
- `examples/`: Sample dashboards, YAML specifications, and tutorials.

## 4. Architectural Relationships & Data Flows

1. **Source Layer**: `Source.get(table, filter, transform)` fetches raw or query-filtered data into DataFrames/DataArrays.
2. **Pipeline Layer**: `Pipeline` binds to a `Source`, applies sequential `Transform` operations and interactive `Filter` criteria.
3. **View Layer**: `View` objects subscribe to `Pipeline` updates to render interactive UI components using Panel/Bokeh.
4. **AI Layer**: `SourceAgent` inspects `Source` metadata, constructs schema prompts for LLMs, and executes actions via `Tools` (e.g., `SQLTool`, `FunctionTool`).

## 5. Entry Points and Command Reference

- **CLI Commands**:
  - `lumen serve dashboard.yaml`: Serve a declarative YAML dashboard.
  - `lumen builder`: Launch the interactive dashboard builder.
- **Testing**:
  - `pytest lumen/tests/`: Run full test suite.
  - `pytest lumen/tests/sources/`: Run specific module tests.
- **Linting & Code Quality**:
  - `ruff check lumen`
  - `pre-commit run --all-files`

## 6. Development Conventions

- Use Param `Parameterized` classes for reactive attributes and validation.
- All public components expose standard parameter interfaces and declarative YAML schema bindings.
- Test files mirror module structures inside `lumen/tests/`.
