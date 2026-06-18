# ACME Cloud Utilities Collection

[![Ansible Collection](https://img.shields.io/badge/collection-acme.cloud__utils-blue)](https://github.com/Automation-Portal-ACME/acme-cloud-utils)

An Ansible collection maintained by the **ACME Corp Cloud Platform Team** providing reusable roles for day-2 cloud operations across AWS, Azure, and GCP. These roles standardize resource management tasks that every team needs but nobody wants to rewrite.

## Requirements

- Ansible Core >= 2.15
- `amazon.aws` collection (for AWS roles)
- `azure.azcollection` collection (for Azure roles)
- Appropriate cloud credentials configured on the controller

## Included Roles

### `acme.cloud_utils.resource_tagger`

Scans cloud resources and applies ACME's mandatory tagging policy. Identifies untagged or non-compliant resources and either reports or remediates them. Supports AWS EC2 instances, RDS databases, S3 buckets, Azure VMs, and resource groups.

**Variables:**

| Variable | Default | Description |
|----------|---------|-------------|
| `tagger_cloud_provider` | `aws` | Target cloud provider (`aws`, `azure`, `gcp`) |
| `tagger_region` | `us-east-1` | Cloud region to scan |
| `tagger_required_tags` | `[owner, cost_center, environment]` | Tags that must be present on all resources |
| `tagger_remediate` | `false` | When true, applies missing tags automatically |
| `tagger_default_owner` | `""` | Default owner tag value for untagged resources |

### `acme.cloud_utils.snapshot_manager`

Manages EBS volume and Azure disk snapshots with retention policies. Creates scheduled snapshots, enforces retention windows, and cleans up expired snapshots to control storage costs.

**Variables:**

| Variable | Default | Description |
|----------|---------|-------------|
| `snapshot_cloud_provider` | `aws` | Target cloud provider (`aws`, `azure`) |
| `snapshot_region` | `us-east-1` | Cloud region |
| `snapshot_retention_days` | `30` | Number of days to retain snapshots |
| `snapshot_tag_filter` | `{}` | Only snapshot volumes matching these tags |
| `snapshot_delete_expired` | `true` | Automatically remove snapshots past retention |

## Usage

```yaml
# Audit tagging compliance (report only)
- hosts: localhost
  roles:
    - role: acme.cloud_utils.resource_tagger
      tagger_cloud_provider: aws
      tagger_region: us-east-1
      tagger_remediate: false

# Manage snapshots with 14-day retention
- hosts: localhost
  roles:
    - role: acme.cloud_utils.snapshot_manager
      snapshot_retention_days: 14
      snapshot_tag_filter:
        environment: production
```

## License

Proprietary - ACME Corp Internal Use Only

## Author

ACME Corp Cloud Platform Team
