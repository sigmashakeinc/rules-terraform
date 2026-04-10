# rules-terraform

Terraform and OpenTofu governance rules for AI coding agents. Prevents hardcoded credentials, public S3 buckets, wildcard IAM policies, missing encryption, disabled CloudTrail, and accidental `terraform destroy` — blocking infrastructure security misconfigurations before they reach `plan` or `apply`.

**11 rules · 2 files**

![rules-terraform — AI agent Terraform IaC governance demo](demo.cast)

> [▶ Watch interactive demo on SigmaShake Hub](https://hub.sigmashake.com/ruleset/rules-terraform)


## Install

```bash
ssg hub pull rules-terraform
```

Available on the [SigmaShake Hub](https://hub.sigmashake.com) — the open registry for AI agent governance rules. Compatible with Claude Code, GitHub Copilot, Cursor, Windsurf, Aider, and any AI coding agent using the `ssg` hook protocol.

## Rules

### tf_write_safety.rules — Credentials & resource safety (5 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-hardcoded-credentials` | DENY | error | Blocks credentials in .tf files |
| `no-public-s3-bucket` | DENY | error | Blocks public-read S3 buckets |
| `no-unencrypted-storage` | ASK | warning | Requires encryption on RDS/EBS |
| `no-open-ingress-all-traffic` | DENY | error | Blocks `0.0.0.0/0` on port 0 security groups |
| `ask-terraform-destroy` | ASK | error | Confirms before `terraform destroy` |

### tf_write_security.rules — IAM and network security (6 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-iam-star-action` | DENY | error | IAM policy granting all actions (`*`) |
| `no-iam-star-resource` | DENY | error | IAM policy applying to all resources (`*`) |
| `no-cloudtrail-disabled` | DENY | error | CloudTrail logging disabled |
| `no-rds-without-deletion-protection` | ASK | warning | RDS without `deletion_protection = true` |
| `no-security-group-all-egress` | ASK | warning | Security group allowing all outbound traffic |
| `no-s3-versioning-disabled` | LOG | warning | S3 bucket without versioning enabled |

## Why this matters

AI agents writing Terraform can produce infrastructure configurations with critical security misconfigurations that are invisible at authoring time but catastrophic in production. Hardcoded credentials in `.tf` files get committed to version control; wildcard IAM policies (`Action: "*", Resource: "*"`) violate the principle of least privilege; and a stray `terraform destroy` in automation can delete production infrastructure in minutes.

These rules enforce cloud security best practices at the moment the agent writes the Terraform file — before `git commit`, `terraform plan`, or `terraform apply` runs.

## Compatible with

- Terraform 0.14+ / OpenTofu
- AWS provider (rules are AWS-focused; adapt for GCP/Azure)
- Works alongside tfsec, Checkov, and Terraform Sentinel — these rules operate at the agent tool-call level, not at plan time

## About

Part of the [SigmaShake Hub](https://hub.sigmashake.com) — open-source governance rules for AI coding agents.
Install the `ssg` CLI to enforce these rules: `npm install -g @sigmashake/ssg`
