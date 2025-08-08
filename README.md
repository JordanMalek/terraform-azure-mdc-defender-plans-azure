# terraform-azure-mdc-defender-plans-azure

> **NOTE:** When running the module, your subscription should not already be onboarded to MDC. If you have already completed the onboarding process, please refer to the [Onboarded Azure Subscription](#onboarded-azure-subscription) section.

\~> **NOTE:** Deletion of the resource will reset the pricing tier to `Free`.

## Notice on breaking changes

Major version updates (e.g., from 1.0.0 to 2.0.0) may include breaking changes that could impact your infrastructure. Review these changes carefully before upgrading.

You may need to update your Terraform code to accommodate changes in the new version. Check the changelog and migration guide for details.

* [Notice on Upgrade to v2.x](./NoticeOnUpgradeTov2.0.md)

Upgrading to a major version should always be tested in a safe environment before applying to production.

## Onboarding to Microsoft Defender for Cloud (MDC) plans in Azure

This Terraform module activates Microsoft Defender for Cloud (MDC) plans.

Supported onboarding types:

1. **Single Subscription**: Onboard MDC plans for a single subscription.
2. **Chosen Subscriptions**: Onboard MDC plans for a selected list of subscriptions.
3. **All Subscriptions**: Onboard MDC plans for all subscriptions where your account holds owner permissions.
4. **Management Group**: Onboard MDC plans for all subscriptions within a management group.

### Terraform and provider version requirements

* Terraform core: v1.x
* terraform-provider-azurerm: v3.x

## Usage

### Enable plans

#### Single Subscription

1. Navigate to `examples\single_subscription` folder.
2. Run `terraform apply`.
3. Onboarding will be applied to the current subscription.

#### Chosen Subscriptions / All Subscriptions / Management Group

1. Navigate to the relevant folder under `examples`.
2. Run `terraform apply`.
3. A new `output` directory will be created.
4. Modify `main.tf` in that directory to suit your needs.
5. Run `terraform apply` again.

### Disable plans

* Disable all plans: `terraform destroy`
* Disable a specific plan: remove it from `mdc_plans_list` and run `terraform apply`

### Onboarded Azure Subscription

If already onboarded, options include:

* **Portal**: Manually disable MDC plans in the Azure portal.
* **Terraform**:

  * Start fresh by destroying existing resources and reapplying.
  * Import existing resources using [Terraform import](https://developer.hashicorp.com/terraform/cli/import).
  * Manage multiple Terraform states.

## Telemetry Collection

This module uses `terraform-provider-modtm` to collect minimal telemetry data such as tags, a unique module identifier, and the operation type (Create/Update/Delete/Read). No sensitive data is collected.

Telemetry is non-blocking and will not interrupt Terraform operations.

To disable telemetry:

```hcl
provider "modtm" {
  enabled = false
}
```

## Contributing

### Configurations

[Configure Terraform for Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/terraform-install-configure)

Set the following environment variables:

**Linux/MacOS**

```bash
export ARM_SUBSCRIPTION_ID="<azure_subscription_id>"
export ARM_TENANT_ID="<azure_subscription_tenant_id>"
export ARM_CLIENT_ID="<service_principal_appid>"
export ARM_CLIENT_SECRET="<service_principal_password>"
```

**Windows PowerShell**

```powershell
$env:ARM_SUBSCRIPTION_ID="<azure_subscription_id>"
$env:ARM_TENANT_ID="<azure_subscription_tenant_id>"
$env:ARM_CLIENT_ID="<service_principal_appid>"
$env:ARM_CLIENT_SECRET="<service_principal_password>"
```

### Pre-Commit, PR Checks & E2E Tests

We provide a Docker image for pre-commit checks:
`mcr.microsoft.com/azterraform:latest`

**Pre-Commit**

```bash
docker run --rm -v $(pwd):/src -w /src mcr.microsoft.com/azterraform:latest make pre-commit
```

**PR Checks**

```bash
docker run --rm -v $(pwd):/src -w /src mcr.microsoft.com/azterraform:latest make pr-check
```

**E2E Tests**

```bash
docker run --rm -v $(pwd):/src -w /src \
  -e ARM_SUBSCRIPTION_ID -e ARM_TENANT_ID -e ARM_CLIENT_ID -e ARM_CLIENT_SECRET \
  mcr.microsoft.com/azterraform:latest make e2e-test
```

This project follows the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).

## License

[MIT](LICENSE)

