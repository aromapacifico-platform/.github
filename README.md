# Aroma Pacifico Platform - GitHub Automation and Governance

This repository (`aromapacifico-platform/.github`) serves as the central automation, CI/CD pipeline registry, composite actions library, and organization-wide health configuration for the Aroma Pacifico coffee traceability platform (Federacion Nacional de Cafeteros / Universidad Icesi).

---

## 1. Directory Structure

```text
.github/
|-- .gitignore                  # Exclusion list for Python bytecode and caches
|-- CODEOWNERS                  # Code ownership declarations for platform assets
|-- PULL_REQUEST_TEMPLATE.md    # Standard PR template with epic checklists
|-- README.md                   # This master documentation
|-- dependabot.yml              # Automated dependency updates (Actions, Python, TF)
|-- infracost.yml               # Multi-environment FinOps configuration
|-- pytest.ini                  # Canonical configuration for pytest test suites
|-- actions/
|   `-- setup-iac-tools/        # Reusable composite action for Terraform, TFLint, Trivy, Infracost
|       |-- action.yml
|       `-- README.md
|-- ISSUE_TEMPLATE/
|   |-- bug_report.md           # Defect submission template
|   |-- feature_request.md      # Feature request template
|   `-- incident_override.md    # Emergency harvest window bypass request
|-- profile/
|   `-- README.md               # Organization public profile page
|-- scripts/
|   |-- check_harvest_window.py # Production harvest freeze gatekeeper (05:00-18:00 UTC-5)
|   |-- test_harvest_window.py  # Comprehensive test suite (49 pytest test cases)
|   |-- requirements-test.txt   # Test dependencies
|   `-- README.md               # Technical script documentation and CLI guide
`-- workflows/
    |-- ci-scripts.yml          # Automated CI for Python scripts and zero-emoji audits
    |-- iac-ci-pr.yml           # IaC Pull Request validation (fmt, validate, lint, Trivy, Infracost, plan)
    |-- iac-cd-platform.yml     # Multi-environment CD (dev -> test -> harvest gate -> prod approval)
    |-- iac-drift-detection.yml # Scheduled nightly drift detection (terraform plan -detailed-exitcode)
    `-- iac-reusable-tf-pipeline.yml # Parameterized workflow call per environment
```

---

## 2. Workflows Catalog

| Workflow | Trigger | Scope & Actions |
|---|---|---|
| `ci-scripts.yml` | Push & PR on `scripts/**`, `pytest.ini` | Executes pytest across Python matrix (3.11, 3.12) and audits zero emojis |
| `iac-ci-pr.yml` | PR against `main` affecting `iac/**` | Validates syntax, format, TFLint AWS rules, Trivy scan, Infracost diff, and OIDC speculative plans |
| `iac-cd-platform.yml` | Push to `main` affecting `iac/**` | Sequential promotion `dev` -> `test` -> Harvest Window Gate -> `prod` (manual approval required) |
| `iac-reusable-tf-pipeline.yml` | `workflow_call` | Modular execution of Terraform `init`, `plan`, and `apply` for a target environment |
| `iac-drift-detection.yml` | Schedule (`07:00 UTC` / `02:00 COT`) | Non-destructive drift audit across `dev`, `test`, `prod` using `plan -detailed-exitcode` |

---

## 3. Core Architectural Policies

1. **Zero-Emoji Mandate**:
   - Strictly enforced across code, workflows, comments, commit messages, and documentation.
   - Verified automatically via audit scripts in CI.
2. **AWS OIDC Authentication Only**:
   - Zero long-lived AWS keys stored in repository secrets.
   - Workflows assume least-privilege IAM roles via `aws-actions/configure-aws-credentials` using `id-token: write`.
3. **Harvest Window Production Freeze**:
   - Deployments to `prod` are prohibited between 05:00:00 and 18:00:00 (America/Bogota, UTC-5) during harvest seasons in Cauca, Huila, and Tolima.
   - Emergency bypass is permitted only with explicit flags (`--emergency`), approved labels (`incident`, `hotfix`), or documented incident tags.
4. **FinOps Cost Governance**:
   - Infracost analyzes pull requests to project monthly cloud expenditure impact before changes merge to `main`.
5. **Manual Push Invariant**:
   - Engineers manually verify and execute `git push`. Automated tools prepare atomic commits locally but do not push unattended.

---

## 4. Local Development and Testing

```bash
# Clone the repository
git clone https://github.com/aromapacifico-platform/.github.git
cd .github

# Install test dependencies
pip install -r scripts/requirements-test.txt

# Run gatekeeper test suite
pytest

# Verify zero-emoji compliance
python -c "import os, unicodedata; ..."
```

