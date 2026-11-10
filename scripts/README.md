# Automation Scripts and Quality Gates

This directory contains utility scripts, policy gatekeepers, and verification test suites for the Aroma Pacifico coffee traceability platform.

## Contents

- `check_harvest_window.py`: Evaluates whether current system time or a given timestamp falls within the restricted harvest window (05:00:00 to 18:00:00 America/Bogota, UTC-5) during scheduled harvest seasons in Cauca, Huila, and Tolima.
- `test_harvest_window.py`: Comprehensive test suite verifying all operational paths, boundaries, bypass mechanisms, and timezone conversions.
- `requirements-test.txt`: Python package requirements for running the test suite.

---

## 1. Harvest Window Freeze Gatekeeper (`check_harvest_window.py`)

### Operational Rule
- **Restricted Window**: 05:00:00 to 18:00:00 (America/Bogota, UTC-5) inclusive during active harvest periods.
- **Harvest Calendar**: Months 4, 5, 6 (April to June) and 9, 10, 11 (September to November), representing the bimodal coffee harvest across Cauca, Huila, and Tolima.
- **Exit Codes**:
  - `0`: Deployment allowed (outside harvest season, outside restricted hours, or emergency bypass granted).
  - `1`: Deployment blocked (active harvest freeze window).
  - `2`: Configuration or invocation error.

### CLI Reference

```bash
# Evaluate current system time
python check_harvest_window.py

# Evaluate a specific timestamp (ISO-8601 or UTC)
python check_harvest_window.py --timestamp "2026-10-15T12:00:00-05:00"

# Output structured JSON
python check_harvest_window.py --timestamp "2026-10-15T12:00:00-05:00" --json

# Passive check (always exits with code 0)
python check_harvest_window.py --check-only

# Emergency bypass via CLI flag
python check_harvest_window.py --emergency

# Inspect PR / Issue labels
python check_harvest_window.py --labels "incident,hotfix"

# Inspect GitHub Actions event JSON
python check_harvest_window.py --github-event-path "$GITHUB_EVENT_PATH"
```

### Emergency Bypass Options

Deployments during restricted hours may proceed only if explicitly authorized via:
1. **CLI Flags**: `--emergency`, `--incident`, `--hotfix`, `--override`
2. **Environment Variables**:
   - `EMERGENCY_OVERRIDE=true`
   - `INCIDENT_FLAG=true`
   - `HOTFIX=true`
   - `DEPLOYMENT_OVERRIDE=true`
   - `HARVEST_BYPASS=true`
3. **Pull Request / Issue Labels**:
   - Labels containing `incident`, `emergency`, `hotfix`, `critical-fix`, or `harvest-bypass`.
4. **Commit Message Keywords**:
   - Commits containing `[emergency]`, `[incident]`, or `[hotfix]`.

---

## 2. Test Suite Execution (`test_harvest_window.py`)

Run the test suite using `pytest`:

```bash
# From repository root or .github directory
pytest

# Verbose output with timing
pytest -v --durations=10
```

The test suite covers:
- Exact boundary seconds (`04:59:59`, `05:00:00`, `18:00:00`, `18:00:01`).
- Bimodal harvest months vs off-season months.
- Timezone translations (`UTC-5` offset, `America/Bogota` localization).
- All emergency bypass channels (CLI, environment, labels, event JSON).
- Standard output, JSON generation, and GitHub Actions `GITHUB_OUTPUT` file writing.

