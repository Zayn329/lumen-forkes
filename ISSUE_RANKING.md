# Issue Ranking Report: Lumen Upstream Issues

- **Date of Investigation**: 2026-10-18
- **Upstream Repository**: https://github.com/holoviz/lumen (branch: `main`)
- **Verified Upstream SHA**: `a838017d9329f239d4f9079f3f649fdb4f9642d6`
- **Exploration Report**: [REPOSITORY_EXPLORATION.md](REPOSITORY_EXPLORATION.md)

---

## 1. Search Method and Discovery Scope
We queried the GitHub REST API for open issues in `holoviz/lumen` and evaluated candidate issues. We cross-referenced open pull requests to check for ongoing work or existing fixes.

---

## 2. Evaluation Criteria and Weighting
1. **Reproducibility and Clarity of Problem** (20%)
2. **Feasibility and Manageable Scope** (20%)
3. **Expected Impact and Usefulness** (20%)
4. **Availability of Tests and Verification Path** (15%)
5. **Root-Cause Investigation Path** (10%)
6. **Integration Risk** (10%) *(Higher score = lower risk)*
7. **Maintainability and Acceptance** (5%)

---

## 3. Candidate Evaluation and Weighted Scoring

### Candidate 1: Issue #2133 - `Source.metadata descriptions are ignored by get_metadata`
- **State**: Open
- **URL**: https://github.com/holoviz/lumen/issues/2133
- **Related PRs**: None found.
- **Scores**:
  - Reproducibility & Clarity: 5/5 (100% reproducible with Python snippet in `lumen/sources/base.py`)
  - Feasibility & Scope: 5/5 (Isolated to dictionary merging logic in `Source.get_metadata()`)
  - Expected Impact: 5/5 (Ensures LLMs receive user-provided table and column descriptions)
  - Tests & Verification: 5/5 (Simple unit test additions in `lumen/tests/sources/test_base.py`)
  - Root-Cause Path: 5/5 (Directly identified line range `lumen/sources/base.py:717-721`)
  - Integration Risk: 5/5 (Low risk; purely fixes metadata dict overlay)
  - Maintainability & Acceptance: 5/5 (High alignment with project metadata handling)
- **Weighted Score**: **100 / 100**

---

### Candidate 2: Issue #2130 - `Notebook export does not include FunctionTool calls`
- **State**: Open
- **URL**: https://github.com/holoviz/lumen/issues/2130
- **Related PRs**: None found.
- **Scores**:
  - Reproducibility & Clarity: 4/5
  - Feasibility & Scope: 4/5
  - Expected Impact: 4/5
  - Tests & Verification: 4/5
  - Root-Cause Path: 4/5
  - Integration Risk: 4/5
  - Maintainability & Acceptance: 4/5
- **Weighted Score**: **80 / 100**

---

### Candidate 3: Issue #2127 - `Data cleaning drops valid rows when an ID column contains 9999`
- **State**: Open
- **URL**: https://github.com/holoviz/lumen/issues/2127
- **Related PRs**: None found.
- **Scores**:
  - Reproducibility & Clarity: 4/5
  - Feasibility & Scope: 3/5
  - Expected Impact: 4/5
  - Tests & Verification: 4/5
  - Root-Cause Path: 3/5
  - Integration Risk: 3/5
  - Maintainability & Acceptance: 4/5
- **Weighted Score**: **72 / 100**

---

### Candidate 4: Issue #2134 - `SourceAgent derives table names that start with a digit`
- **State**: Open
- **URL**: https://github.com/holoviz/lumen/issues/2134
- **Related PRs**: PR #2139 (Fix already in open PR)
- **Status**: Blocked / Ineligible due to existing open PR #2139.

---

### Candidate 5: Issue #2126 - `SQLAlchemySource lists information_schema system tables on PostgreSQL`
- **State**: Open
- **URL**: https://github.com/holoviz/lumen/issues/2126
- **Related PRs**: PR #2142 (Fix already in open PR)
- **Status**: Blocked / Ineligible due to existing open PR #2142.

---

## 4. Selected Issue for Stage 2

- **Issue Number**: #2133
- **Title**: `Source.metadata descriptions are ignored by get_metadata`
- **URL**: https://github.com/holoviz/lumen/issues/2133
- **Problem Summary**: User-defined descriptions in `Source.metadata` (e.g. table-level and column-level descriptions) are ignored by `Source.get_metadata()`. Generated metadata initializes empty `'description'` keys, and the current merging logic (`if key not in metadata[table_name]`) fails to overwrite those empty values with user-provided descriptions.
- **Expected Behavior**: User-configured descriptions in `Source.metadata` override empty generated description strings for tables and columns.
- **Actual Behavior**: Generated empty string descriptions (`""`) take precedence over `Source.metadata` values.
- **Why Suitable**: Highly specific, fully reproducible, clear root cause, zero open PR conflict, straightforward unit testing path in `lumen/tests/sources/test_base.py`.
- **Preliminary Affected Files**: `lumen/sources/base.py`
- **Preliminary Testing Strategy**: Add unit tests in `lumen/tests/sources/test_base.py` verifying table-level and column-level metadata description overrides.
- **Integration Risks**: Minimal; needs to ensure non-empty generated metadata is handled according to specified precedence rules.
- **Questions for Stage 2**: Exactly how deep should dict merging operate for nested column metadata entries?

---

## 5. Next Action
Proceed to **Stage 2: Dependency Mapping**.
