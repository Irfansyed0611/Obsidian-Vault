# Semantic versioning

## **What is Semantic Versioning (SemVer)?**

Semantic Versioning is a versioning scheme that uses a three-part number format: **MAJOR.MINOR.PATCH** (e.g., 2.4.1)

### **Version Format: X.Y.Z**

- **X (MAJOR)**: Breaking changes, incompatible API changes
- **Y (MINOR)**: New features, backward-compatible additions
- **Z (PATCH)**: Bug fixes, backward-compatible fixes

### **Examples:**

- `1.0.0` → `2.0.0`: Breaking change
- `1.0.0` → `1.1.0`: New feature added
- `1.0.0` → `1.0.1`: Bug fix

## **Semantic Versioning in Terraform Contexts**

### **1. Terraform Module Versioning**

### **Publishing Modules with Git Tags**

```bash
# Tag your module repository
git tag -a "v1.0.0" -m "Initial stable release"
git tag -a "v1.1.0" -m "Added support for multiple AZs"
git tag -a "v2.0.0" -m "Breaking: Changed variable names"
git push --tags

```

### **Consuming Versioned Modules**

```hcl
# main.tf
module "vpc" {
  source  = "git::<https://github.com/company/terraform-vpc.git?ref=v1.2.0>"
  # Or using Terraform Registry
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"  # Allows 5.x updates but not 6.0

  cidr = "10.0.0.0/16"
}

# Version constraints
module "database" {
  source  = "company/rds/aws"
  version = ">= 2.0.0, < 3.0.0"  # Any 2.x version
}

```

### **2. Version Constraints in Terraform**

### **Constraint Operators**

```hcl
# versions.tf
terraform {
  required_version = ">= 1.0"  # Terraform CLI version

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"     # >= 5.0.0, < 6.0.0
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = ">= 2.23.0"  # 2.23.0 or higher
    }
    helm = {
      source  = "hashicorp/helm"
      version = "!= 2.11.0"  # Any version except 2.11.0
    }
  }
}

```

### **Version Constraint Syntax**

```hcl
version = "1.2.0"         # Exact version
version = ">= 1.2.0"      # Minimum version
version = "<= 1.2.0"      # Maximum version
version = "~> 1.2.0"      # Pessimistic constraint (>= 1.2.0, < 1.3.0)
version = "~> 1.2"        # Allows 1.x where x >= 2
version = ">= 1.0, < 2.0" # Range

```

### **3. Module Development Best Practices**

### **Module Structure with Versioning**

```
terraform-aws-webapp/
├── README.md
├── CHANGELOG.md        # Document version changes
├── VERSION            # Current version file
├── main.tf
├── variables.tf
├── outputs.tf
├── versions.tf        # Provider version constraints
└── examples/
    ├── basic/
    └── advanced/

```

### [**CHANGELOG.md](http://changelog.md/) Example**

```markdown
# Changelog

## [2.0.0] - 2024-01-15
### Breaking Changes
- Renamed variable `instance_count` to `desired_capacity`
- Removed deprecated `enable_monitoring` variable

### Added
- Support for ARM-based instances
- New output `cluster_arn`

## [1.3.0] - 2024-01-01
### Added
- Auto-scaling configuration
- CloudWatch alarms

## [1.2.1] - 2023-12-20
### Fixed
- Security group rule ordering issue

```

### **4. Automated Version Management**

### **Using Semantic Release with Terraform Modules**

```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    branches: [main]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Semantic Release
        uses: cycjimmy/semantic-release-action@v3
        with:
          semantic_version: 19
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

### **.releaserc.json Configuration**

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/github",
    [
      "@semantic-release/git",
      {
        "assets": ["CHANGELOG.md", "VERSION"],
        "message": "chore(release): ${nextRelease.version} [skip ci]"
      }
    ]
  ]
}

```

### **5. Infrastructure Versioning Strategy**

### **Environment-Specific Versioning**

```hcl
# environments/prod/terraform.tfvars
environment    = "production"
app_version    = "2.1.0"
infra_version  = "1.5.0"

# environments/staging/terraform.tfvars
environment    = "staging"
app_version    = "2.2.0-beta.1"
infra_version  = "1.6.0-rc.1"

```

### **Tagging Resources with Versions**

```hcl
# main.tf
locals {
  common_tags = {
    Environment     = var.environment
    ManagedBy      = "Terraform"
    TerraformVersion = "1.6.0"
    ModuleVersion  = "2.1.0"
    LastUpdated    = timestamp()
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.latest.id
  instance_type = var.instance_type

  tags = merge(local.common_tags, {
    Name    = "web-server"
    Version = var.app_version
  })
}
```

### **6. Terraform Registry Module Publishing**

### **Module Metadata for Registry**

```hcl
# terraform-aws-modules/my-module/metadata.yaml
name: my-module
version: 1.2.3
description: "AWS module for deploying scalable web applications"
source: "<https://github.com/terraform-aws-modules/my-module>"
```

### **Version Upgrade Path Documentation**

```markdown
# Upgrade Guide

## From 1.x to 2.0

### Breaking Changes:
1. Variable `num_instances` renamed to `instance_count`
   ```hcl
   # Old (1.x)
   num_instances = 3

   # New (2.0)
   instance_count = 3

```

1. Output changes:
    - `instance_ips` is now `instance_private_ips`
    - Added new output `instance_public_ips`

````

### **7. Dependency Lock File**
```hcl
# .terraform.lock.hcl (auto-generated)
provider "registry.terraform.io/hashicorp/aws" {
  version     = "5.31.0"
  constraints = "~> 5.0"
  hashes = [
    "h1:WwgMbMOhZblxZTdjHeJf9XB2/hcSHHmpuywLxuTWYw0=",
    "zh:0cdb9c2083bf8011c9b8e65c5b2e5e1c6e4b7e5f6f8e1e0e8d38fedf93f8e22c",
    # ...
  ]
}

````

### **8. Version Pinning Strategy**

### **Development vs Production**

```hcl
# Development - Allow minor updates
module "dev_vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"  # Accept patches and minor updates
}

# Production - Pin exact version
module "prod_vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "= 5.1.2"  # Exact version only
}

```

### **9. Makefile for Version Management**

```makefile
VERSION := $(shell cat VERSION)
NEXT_PATCH := $(shell echo $(VERSION) | awk -F. '{print $$1"."$$2"."$$3+1}')
NEXT_MINOR := $(shell echo $(VERSION) | awk -F. '{print $$1"."$$2+1".0"}')
NEXT_MAJOR := $(shell echo $(VERSION) | awk -F. '{print $$1+1".0.0"}')

release-patch:
	echo $(NEXT_PATCH) > VERSION
	git add VERSION
	git commit -m "chore: bump version to $(NEXT_PATCH)"
	git tag -a v$(NEXT_PATCH) -m "Release version $(NEXT_PATCH)"

release-minor:
	echo $(NEXT_MINOR) > VERSION
	git add VERSION
	git commit -m "feat: bump version to $(NEXT_MINOR)"
	git tag -a v$(NEXT_MINOR) -m "Release version $(NEXT_MINOR)"

release-major:
	echo $(NEXT_MAJOR) > VERSION
	git add VERSION
	git commit -m "feat!: bump version to $(NEXT_MAJOR)"
	git tag -a v$(NEXT_MAJOR) -m "Release version $(NEXT_MAJOR)"

```

## **Best Practices for SemVer in Terraform:**

1. **Always use version constraints** in production
2. **Document breaking changes** clearly in CHANGELOG
3. **Test version upgrades** in non-production first
4. **Use pessimistic constraints** (`~>`) for flexibility with safety
5. **Pin exact versions** for critical production infrastructure
6. **Automate version bumping** with CI/CD tools
7. **Tag all releases** in version control
8. **Include migration guides** for major version changes

This approach ensures predictable, manageable infrastructure changes while maintaining compatibility and stability across your Terraform deployments.