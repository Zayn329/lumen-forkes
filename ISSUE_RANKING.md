# Candidate Issues Ranking and Selection Report — Lumen

- **Investigation Date**: 2026-10-14
- **Upstream Repository**: `https://github.com/holoviz/lumen`
- **Upstream Branch**: `main`
- **Verified Upstream Commit SHA**: `5344e21a98a7ba6bb7f1f0f7eb775332fc21a96c`
- **Exploration Report**: `REPOSITORY_EXPLORATION.md`

## 1. Search Method and Scope
We inspected open issues on `https://github.com/holoviz/lumen/issues` focusing on bugs, AI/agent enhancements, and data source behavior. Four major candidates were identified and evaluated.

---

## 2. Ranked Candidate Issues & Evaluation Rubric

| Criterion | Weight |
|---|---|
| Reproducibility and clarity of the problem | 20% |
| Feasibility and manageable scope | 20% |
| Expected impact and usefulness | 20% |
| Availability of tests and a verification path | 15% |
| Root-cause investigation path | 10% |
| Integration risk (higher score = lower risk) | 10% |
| Evidence of likely maintainability and acceptance | 5% |

---

### Candidate 1: Issue #2133 — `Source.metadata` descriptions are ignored by `get_metadata`

- **URL**: `https://github.com/holoviz/lumen/issues/2133`
- **State**: Open
- **Summary**: User-configured `Source.metadata` table and column descriptions are overwritten or ignored because `get_metadata` in `lumen/sources/base.py` defaults to auto-generated empty string values without recursively merging or prioritizing non-empty user metadata.
- **Scoring**:
  - Reproducibility & clarity: 5/5 (100% clear example provided in issue)
  - Feasibility & scope: 5/5 (Isolated logic inside `Source.get_metadata()` in `lumen/sources/base.py`)
  - Expected impact: 5/5 (Crucial for LLM schema context and metadata discovery across all sources)
  - Availability of tests: 5/5 (Existing source metadata tests in `lumen/tests/sources/`)
  - Root cause path: 5/5 (Lines 717–721 of `lumen/sources/base.py` clearly identified)
  - Integration risk: 5/5 (Zero external dependency risk, purely Python dict merging)
  - Maintainability & acceptance: 5/5 (High community alignment and direct bug fix)
- **Total Weighted Score**: **100 / 100**

---

### Candidate 2: Issue #2134 — `SourceAgent` derives table names that start with a digit

- **URL**: `https://github.com/holoviz/lumen/issues/2134`
- **State**: Open (PR #2139 submitted upstream by community)
- **Summary**: `SourceAgent._derive_table_name` places numeric arguments at the start of derived table names, causing DuckDB/SQL syntax errors.
- **Scoring**:
  - Reproducibility & clarity: 5/5
  - Feasibility & scope: 5/5
  - Expected impact: 4/5
  - Availability of tests: 4/5
  - Root cause path: 5/5
  - Integration risk: 4/5
  - Maintainability & acceptance: 2/5 (Conflicting PR #2139 already submitted upstream)
- **Total Weighted Score**: **87 / 100**

---

### Candidate 3: Issue #2127 — Data cleaning drops valid rows when an ID column contains 9999

- **URL**: `https://github.com/holoviz/lumen/issues/2127`
- **State**: Open
- **Summary**: SQLAgent data quality cleaning treats `9999` or `-9999` as invalid sentinel values and drops valid ID rows.
- **Scoring**:
  - Reproducibility & clarity: 4/5
  - Feasibility & scope: 4/5
  - Expected impact: 4/5
  - Availability of tests: 3/5
  - Root cause path: 4/5
  - Integration risk: 3/5 (Heuristic changes to data quality linter require tuning)
  - Maintainability & acceptance: 4/5
- **Total Weighted Score**: **78.5 / 100**

---

### Candidate 4: Issue #2130 — Notebook export does not include `FunctionTool` calls

- **URL**: `https://github.com/holoviz/lumen/issues/2130`
- **State**: Open
- **Summary**: Notebook export in `lumen/ai/export.py` omits `FunctionTool` execution cells.
- **Scoring**:
  - Reproducibility & clarity: 4/5
  - Feasibility & scope: 3/5
  - Expected impact: 3/5
  - Availability of tests: 3/5
  - Root cause path: 4/5
  - Integration risk: 4/5
  - Maintainability & acceptance: 4/5
- **Total Weighted Score**: **74.5 / 100**

---

## 3. Selected Primary Issue for Stage 2

**Selected Issue**: **#2133 — `Source.metadata` descriptions are ignored by `get_metadata`**

- **URL**: `https://github.com/holoviz/lumen/issues/2133`
- **Problem Summary**: When user-defined `Source.metadata` contains table or column descriptions, `get_metadata()` returns empty strings because auto-generated schema dictionaries populate default empty string fields (`description=""`) and `get_metadata` fails to merge user-provided metadata into existing key structures.
- **Expected Behavior**: User-defined table descriptions and column metadata should take precedence over default or empty auto-generated schema values.
- **Actual Behavior**: Auto-generated empty strings overwrite or block user-defined metadata.
- **Why Suitable**:
  - 100% reproducible with clear isolated logic in `lumen/sources/base.py`.
  - Highly impactful for all downstream AI tools and LLM agents that query metadata via `source.get_metadata()`.
  - Safe, concise, and verifiable via existing unit tests in `lumen/tests/sources/`.
- **Preliminary Affected Files**:
  - `lumen/sources/base.py` (`Source.get_metadata` method)
  - `lumen/tests/sources/test_base.py` or `lumen/tests/sources/test_sqlalchemy.py`
- **Questions for Stage 2**:
  - How should deep nested dictionary merging handle conflicting non-empty generated metadata vs user-defined metadata?
  - What default behavior should `JoinedSource` and child source classes inherit?
