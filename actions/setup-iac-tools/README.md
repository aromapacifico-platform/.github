# Setup IaC Tools Composite Action

GitHub Composite Action that sets up and caches the toolchain required for Infrastructure as Code (IaC) verification and deployments in the Aroma Pacifico platform.

## Tools Installed

1. **HashiCorp Terraform CLI**: Configured via `hashicorp/setup-terraform`.
2. **TFLint**: Installed via `terraform-linters/setup-tflint` with automatic AWS ruleset plugin initialization (`tflint --init`) and plugin caching in `~/.tflint.d/plugins`.
3. **Aqua Security Trivy**: Installed via `aquasecurity/setup-trivy` with database caching for security and misconfiguration scanning.
4. **Infracost CLI**: Installed via `infracost/actions/setup` for cloud cost estimation and FinOps governance.

---

## Inputs

| Name | Description | Required | Default |
|---|---|---|---|
| `terraform-version` | Version of Terraform CLI to install | No | `1.10.5` |
| `tflint-version` | Version of TFLint to install | No | `v0.55.0` |
| `trivy-version` | Version of Trivy scanner to install | No | `v0.58.2` |
| `infracost-version` | Version of Infracost CLI to install | No | `v0.10.39` |
| `install-tflint` | Whether to install and initialize TFLint | No | `true` |
| `install-trivy` | Whether to install Trivy scanner | No | `true` |
| `install-infracost` | Whether to install Infracost CLI | No | `true` |
| `infracost-api-key` | Optional Infracost API key to configure | No | `''` |
| `tflint-init` | Whether to execute `tflint --init` after setup | No | `true` |
| `tflint-config-path` | Path to `.tflint.hcl` configuration file | No | `iac/.tflint.hcl` |

---

## Usage Example

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Setup IaC Tools
    uses: ./.github/actions/setup-iac-tools
    with:
      terraform-version: '1.10.5'
      install-infracost: 'true'
      infracost-api-key: ${{ secrets.INFRACOST_API_KEY }}
```

