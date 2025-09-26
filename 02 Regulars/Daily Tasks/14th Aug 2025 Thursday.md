---
tags:
  - daily
---
--------
## Task to complete

- [ ] 
- [ ]   

-----
##  Action items from daily meeting

| Task | Further Action item |
| ---- | ------------------- |
|      |                     |


----

## Notes

# Terraform Resources Creation Guide: A Comprehensive Training Manual


![Terraform module architecture diagram showing how modules work together | 400](https://user-gen-media-assets.s3.amazonaws.com/gpt4o_images/9283581f-cb9b-4685-8256-f709b2e6499c.png )
Terraform module architecture diagram showing how modules work together

![[14th Aug 2025 Thursday 2025-08-14 19.34.04.excalidraw]]

## Understanding Terraform Modules

**Terraform modules** are reusable and encapsulated collections of Terraform configurations that simplify infrastructure management. Think of modules as **building blocks** - instead of writing the same infrastructure code repeatedly, you create a module once and reuse it across different environments and projects.[kodekloud+2](https://kodekloud.com/blog/understanding-terraform-modules/)

## Why Modules Are Important

Modules provide several critical benefits that make them essential for scalable infrastructure management:[kodekloud+2](https://kodekloud.com/blog/understanding-terraform-modules/)
- **Reusability**: Write once, use everywhere - modules eliminate code duplication across projects and environments
- **Abstraction**: Complex infrastructure setups are simplified into easy-to-use interfaces with clear inputs and outputs
- **Encapsulation**: Related resources are grouped together, making infrastructure easier to understand and maintain
- **Consistency**: Standardized deployments ensure all environments follow the same patterns and best practices
- **Maintainability**: Changes made to a module automatically propagate to all environments using that module

![Terraform project directory structure visualization | 400](https://user-gen-media-assets.s3.amazonaws.com/gpt4o_images/1c2b2a5c-0ca7-403c-978e-3e8e94202c74.png)
Terraform project directory structure visualization

## Module Types and Structure

Your project demonstrates the **standard module structure** recommended by HashiCorp. There are three main types of modules:[developer.hashicorp+2](https://developer.hashicorp.com/terraform/tutorials/modules/module)

**Root Module**: The main directory where you run Terraform commands. In your case, this would be each environment directory like `iovance-mfg-dev`.[developer.hashicorp](https://developer.hashicorp.com/terraform/tutorials/modules/module)

**Child Modules**: Reusable modules called by other configurations. Your `modules/` directory contains child modules like `CW-Alarms` and `sns_topic`.[developer.hashicorp](https://developer.hashicorp.com/terraform/tutorials/modules/module)

**Published Modules**: Modules shared via registries or repositories for broader community use.[spacelift](https://spacelift.io/blog/what-are-terraform-modules-and-how-do-they-work)

## Your Project's Directory Structure Explained

Your directory structure follows infrastructure as code best practices by separating concerns into logical groups:[env0+2](https://www.env0.com/blog/terraform-files-and-folder-structure-organizing-infrastructure-as-code)
### Bootstrap Backend Directory

The `bootstrap-backend/` directory serves a **critical foundational purpose** - it creates the remote backend infrastructure needed for team collaboration. Each subdirectory (`mfg-dev`, `mfg-prod`, `mfg-test`) contains:[scalr+2](https://scalr.com/learning-center/practical-guide-to-terraform-init-backend-config/)

- **backend.tf**: Configures where the Terraform state will be stored
- **main.tf**: Creates S3 buckets and DynamoDB tables for state storage and locking
- **providers.tf**: Specifies the required Terraform and provider versions
- **terraform.tfvars**: Environment-specific variable values
- **variables.tf**: Variable declarations

This separation ensures that **backend infrastructure is managed independently** from your application infrastructure, following the principle that backend resources should be stable and rarely change.[squareops+1](https://squareops.com/blog/terraform-state-management/)

### Environments Directory

The `environments/` directory contains **environment-specific configurations** that consume your reusable modules. Each environment (`iovance-mfg-dev`, `iovance-mfg-prod`, `iovance-mfg-test`) has its own:[env0+1](https://www.env0.com/blog/terraform-files-and-folder-structure-organizing-infrastructure-as-code)

- **Complete Terraform configuration** with environment-specific values
- **Independent state file** preventing cross-environment conflicts
- **Custom variable files** allowing different settings per environment

This structure enables **safe testing** in development environments before promoting changes to production.[spacelift+1](https://spacelift.io/blog/terraform-best-practices)

### Modules Directory

The `modules/` directory houses your **reusable infrastructure components**. Your modules are logically organized by function:[env0+1](https://www.env0.com/blog/terraform-modules)

**CW-Alarms/**: CloudWatch monitoring modules for different metric types

- `ec2_cpu_usage_alarm/`
- `ec2_disk_free_space_alarm/`
- `ec2_memory_utilization_alarm/`
- `ec2_status_check_alarm/`

**Infrastructure modules**: Core AWS services

- `dynamodb_table/`
- `s3_bucket/`
- `sns_topic/`

Each module follows the **standard structure** with `main.tf`, `variables.tf`, `outputs.tf`, and `README.md` files.[cloud.google+1](https://cloud.google.com/docs/terraform/best-practices/general-style-structure)

![Terraform backend and state management architecture | 400](https://user-gen-media-assets.s3.amazonaws.com/gpt4o_images/0de734dd-7390-41f6-b5d6-af9b9d893285.png)

Terraform backend and state management architecture

## Understanding Terraform Backend

A **Terraform backend** determines where and how Terraform's state data is stored and accessed. The state file is crucial because it maps your configuration to real-world resources and tracks metadata about your infrastructure.[squareops+4](https://squareops.com/blog/terraform-state-management/)
### Local vs Remote Backends

**Local Backend (Default)**: Stores state as a file on your local machine. While simple, this creates problems for team collaboration since others cannot access your state file.[spacelift+1](https://spacelift.io/blog/terraform-state)

**Remote Backends**: Store state in shared locations like AWS S3, Azure Blob Storage, or Google Cloud Storage. Remote backends enable:[scalr+2](https://scalr.com/learning-center/practical-guide-to-terraform-init-backend-config/)

- **Team Collaboration**: Multiple team members can work on the same infrastructure
- **State Locking**: Prevents concurrent modifications that could corrupt state
- **State Versioning**: Maintains history of infrastructure changes
- **Security**: Encrypted storage and access controls protect sensitive data

### Backend Configuration in Your Project

Your project uses **partial backend configuration**, allowing dynamic backend settings for different environments. The backend block in your configurations might look like:[scalr+1](https://scalr.com/learning-center/practical-guide-to-terraform-init-backend-config/)

```hcl
terraform {
  backend "s3" {
    bucket         = "iovance-mfg-dev-tf-bucket"
    key            = "TerraformState/EC2_Status_Check_Alarm.tfstate"
    region         = "us-east-1"
    dynamodb_table = "iovance-mfg-dev-tf-backend"
  }
}

```

But its a good practice to separate the `bucket`, `key`, `dynamodb_table` in a different file called `backend.hcl`.

```hcl
terraform {
  backend "s3" {
    region         = "us-east-1"
  }
}
```

```
bucket         = "iovance-mfg-dev-tf-bucket"
key            = "TerraformState/EC2_Status_Check_Alarm.tfstate"
dynamodb_table = "iovance-mfg-dev-tf-backend"
```

The actual values are provided via `terraform init -backend-config=<config-file>`, enabling the same code to work across environments with different backend settings.[env0+1](https://www.env0.com/blog/terraform-backends)

## Deep Dive: The for_each Meta-Argument

The `for_each` meta-argument is a powerful feature that creates **multiple instances of a resource** from a single configuration block. This eliminates code duplication and enables dynamic resource creation.[developer.hashicorp+2](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)

### How for_each Works

Your disk monitoring example perfectly demonstrates `for_each` usage:[developer.hashicorp+1](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)

```
module "free_disk_space" {
  source              = "../../modules/CW-Alarms/ec2_disk_free_space_alarm/"
  for_each            = var.disk_free_space_alarms
  alarm_name          = "${each.value.instance_name}-Disk Usage-${each.value.alarm_type}"
  # ... other parameters using each.key and each.value
}
```

![How for_each creates multiple resources from a single configuration block | 500](https://user-gen-media-assets.s3.amazonaws.com/gpt4o_images/b2954cbe-1b0d-4e0b-ae50-fd0b0565ccab.png)

How for_each creates multiple resources from a single configuration block

### Understanding the Variable Structure

Your `terraform.tfvars` configuration creates a **map of objects**:[developer.hashicorp+1](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)

```hcl
disk_free_space_alarms = {
  "USE1MD-AD-MGR_Warning" = {
    instance_name       = "USE1MD-AD-MGR"
    instance_id         = "i-000fc60a486c0fca3"
    threshold           = 20
    alarm_type          = "Warning"
    topic_key           = "Disk_Utilization_Warning"
    period              = 300
    evaluation_periods  = 1
    datapoints_to_alarm = 1
  },
  "USE1MD-AD-MGR_Critical" = {
    instance_name       = "USE1MD-AD-MGR"
    instance_id         = "i-000fc60a486c0fca3"
    threshold           = 10
    alarm_type          = "Critical"
    topic_key           = "Disk_Utilization_Critical"
    period              = 300
    evaluation_periods  = 1
    datapoints_to_alarm = 1
  }
}

```
### The each.key and each.value

When `for_each` iterates over this map:[developer.hashicorp+2](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)

- **each.key**: Contains the map key (e.g., "USE1MD-AD-MGR_Warning")
- **each.value**: Contains the entire object with all its properties
- **each.value.instance_name**: Accesses specific properties within the object

This creates **two separate CloudWatch alarms**:

1. A Warning alarm when disk usage exceeds 80%
2. A Critical alarm when disk usage exceeds 90%

Both monitor the same EC2 instance but trigger different SNS topics based on severity.

### Benefits of this Approach

Using `for_each` in your monitoring setup provides:[spacelift+2](https://spacelift.io/blog/terraform-for-each)

- **Scalability**: Easily add new instances or alarm types by updating the variable
- **Maintainability**: Changes to alarm logic only need to be made in the module
- **Flexibility**: Different thresholds and settings per instance without code duplication
- **Resource Tracking**: Terraform tracks each alarm by its unique key, enabling precise updates
## Complete Workflow: How Everything Works Together

Your project demonstrates a sophisticated **multi-layer approach** to infrastructure management that follows enterprise best practices.

### Step 1: Bootstrap Backend Setup

The bootstrap process creates the **foundation for state management**:[scalr+1](https://scalr.com/learning-center/practical-guide-to-terraform-init-backend-config/)
- S3 buckets for storing Terraform state files
- DynamoDB tables for state locking to prevent conflicts
- IAM roles and policies for secure access

### Step 2: Environment Configuration

Each environment directory contains **complete infrastructure definitions**:[env0+1](https://www.env0.com/blog/terraform-files-and-folder-structure-organizing-infrastructure-as-code)
- Calls to reusable modules with environment-specific parameters
- Variable files tailored to each environment's requirements
- Backend configuration pointing to environment-specific state storage

### Step 3: Module Execution

Modules encapsulate **infrastructure patterns and best practices**:[kodekloud+1](https://kodekloud.com/blog/understanding-terraform-modules/)
- CloudWatch alarm modules create monitoring infrastructure
- SNS topic modules set up notification channels
- S3 and DynamoDB modules provide data storage and state management

### Step 4: Resource Creation and State Management

Terraform orchestrates the **creation and tracking of AWS resources**:[zeet+1](https://zeet.co/blog/terraform-state-management)
- CloudWatch alarms monitor EC2 instance metrics
- SNS topics route notifications to appropriate channels
- State files track all resources and their relationships

## Real-World Example: CloudWatch Monitoring Implementation

Your CloudWatch disk monitoring setup demonstrates **production-ready infrastructure patterns**:youtube[dev+1](https://dev.to/chinmay13/getting-started-with-aws-and-terraform-setting-up-cloudwatch-alarms-for-cpu-utilization-on-aws-ec2-instances-with-terraform-14l2)

## Components Working Together

1. **EC2 Instances**: Target resources being monitored
2. **CloudWatch Agent**: Collects custom metrics like disk usage (requires separate installation)[nashtechglobal+2](https://blog.nashtechglobal.com/create-cloudwatch-alarms-for-memory-and-disk-usage/)
3. **CloudWatch Alarms**: Monitor metrics and trigger when thresholds are exceeded
4. **SNS Topics**: Route notifications to administrators via email, SMS, or other channels
5. **Terraform Modules**: Encapsulate this entire pattern for reuse across environments

## The for_each Advantage

Instead of creating separate alarm resources for each instance and threshold combination, your `for_each` approach:

- **Reduces configuration by 80%**: One module call replaces dozens of resource blocks
- **Enables dynamic scaling**: Add new monitored instances by updating variables
- **Maintains consistency**: All alarms follow the same configuration pattern
- **Simplifies maintenance**: Updates to alarm logic propagate to all instances automatically


## Recommended Development Workflow

1. **Start with Bootstrap**: Always set up remote backend infrastructure first
2. **Develop in Modules**: Create reusable modules for common infrastructure patterns
3. **Test in Development**: Use development environments to validate changes before production deployment
4. **Iterate with for_each**: Use dynamic resource creation for scalable infrastructure patterns
5. **Document Everything**: Maintain clear README files explaining module usage and requirements

This structure scales effectively from small projects to enterprise deployments managing hundreds of resources across multiple environments. The combination of modules, remote state, and dynamic resource creation provides a robust foundation for infrastructure automation that grows with your organization's needs.

1. [https://kodekloud.com/blog/understanding-terraform-modules/](https://kodekloud.com/blog/understanding-terraform-modules/)
2. [https://spacelift.io/blog/what-are-terraform-modules-and-how-do-they-work](https://spacelift.io/blog/what-are-terraform-modules-and-how-do-they-work)
3. [https://www.env0.com/blog/terraform-modules](https://www.env0.com/blog/terraform-modules)
4. [https://developer.hashicorp.com/terraform/tutorials/modules/module](https://developer.hashicorp.com/terraform/tutorials/modules/module)
5. [https://cloud.google.com/docs/terraform/best-practices/general-style-structure](https://cloud.google.com/docs/terraform/best-practices/general-style-structure)
6. [https://developer.hashicorp.com/terraform/language/modules/develop/structure](https://developer.hashicorp.com/terraform/language/modules/develop/structure)
7. [https://www.env0.com/blog/terraform-files-and-folder-structure-organizing-infrastructure-as-code](https://www.env0.com/blog/terraform-files-and-folder-structure-organizing-infrastructure-as-code)
8. [https://developer.hashicorp.com/terraform/tutorials/modules/pattern-module-creation](https://developer.hashicorp.com/terraform/tutorials/modules/pattern-module-creation)
9. [https://docs.aws.amazon.com/prescriptive-guidance/latest/terraform-aws-provider-best-practices/structure.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/terraform-aws-provider-best-practices/structure.html)
10. [https://scalr.com/learning-center/practical-guide-to-terraform-init-backend-config/](https://scalr.com/learning-center/practical-guide-to-terraform-init-backend-config/)
11. [https://www.env0.com/blog/terraform-backends](https://www.env0.com/blog/terraform-backends)
12. [https://spacelift.io/blog/terraform-s3-backend](https://spacelift.io/blog/terraform-s3-backend)
13. [https://squareops.com/blog/terraform-state-management/](https://squareops.com/blog/terraform-state-management/)
14. [https://spacelift.io/blog/terraform-state](https://spacelift.io/blog/terraform-state)
15. [https://spacelift.io/blog/terraform-best-practices](https://spacelift.io/blog/terraform-best-practices)
16. [https://devopscube.com/terraform-module-best-practices/](https://devopscube.com/terraform-module-best-practices/)
17. [https://developer.hashicorp.com/terraform/language/backend](https://developer.hashicorp.com/terraform/language/backend)
18. [https://spacelift.io/blog/terraform-backends](https://spacelift.io/blog/terraform-backends)
19. [https://zeet.co/blog/terraform-state-management](https://zeet.co/blog/terraform-state-management)
20. [https://developer.hashicorp.com/terraform/language/state](https://developer.hashicorp.com/terraform/language/state)
21. [https://developer.hashicorp.com/terraform/language/meta-arguments/for_each](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)
22. [https://spacelift.io/blog/terraform-for-each](https://spacelift.io/blog/terraform-for-each)
23. [https://kodekloud.com/blog/terraform-for_each/](https://kodekloud.com/blog/terraform-for_each/)
24. [https://www.env0.com/blog/terraform-for-each-examples-tips-and-best-practices](https://www.env0.com/blog/terraform-for-each-examples-tips-and-best-practices)
25. [https://www.youtube.com/watch?v=CwgOcUEn-d0](https://www.youtube.com/watch?v=CwgOcUEn-d0)
26. [https://dev.to/chinmay13/getting-started-with-aws-and-terraform-setting-up-cloudwatch-alarms-for-cpu-utilization-on-aws-ec2-instances-with-terraform-14l2](https://dev.to/chinmay13/getting-started-with-aws-and-terraform-setting-up-cloudwatch-alarms-for-cpu-utilization-on-aws-ec2-instances-with-terraform-14l2)
27. [https://blog.nashtechglobal.com/create-cloudwatch-alarms-for-memory-and-disk-usage/](https://blog.nashtechglobal.com/create-cloudwatch-alarms-for-memory-and-disk-usage/)
28. [https://www.linkedin.com/pulse/monitoring-ec2-disk-usage-cloudwatch-metrics-alarms-krishna-wattamwar](https://www.linkedin.com/pulse/monitoring-ec2-disk-usage-cloudwatch-metrics-alarms-krishna-wattamwar)
29. [https://www.easydeploy.io/blog/setup-ram-diskspace-ec2-instance-in-aws-cloudwatch/](https://www.easydeploy.io/blog/setup-ram-diskspace-ec2-instance-in-aws-cloudwatch/)
30. [https://dev.to/pwd9000/terraform-understanding-count-and-foreach-loops-c6i](https://dev.to/pwd9000/terraform-understanding-count-and-foreach-loops-c6i)
31. [https://spacelift.io/blog/terraform-for-loop](https://spacelift.io/blog/terraform-for-loop)
32. [https://k21academy.com/terraform-iac/what-are-terraform-modules-and-their-purposes/](https://k21academy.com/terraform-iac/what-are-terraform-modules-and-their-purposes/)
33. [https://zeet.co/blog/terraform-module](https://zeet.co/blog/terraform-module)
34. [https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/guides/remote-state-backend](https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/guides/remote-state-backend)
35. [https://www.youtube.com/watch?v=xBWmBPVTums](https://www.youtube.com/watch?v=xBWmBPVTums)
36. [https://stackoverflow.com/questions/75952647/why-is-terraform-init-backend-config-not-reading-my-backend-config-file](https://stackoverflow.com/questions/75952647/why-is-terraform-init-backend-config-not-reading-my-backend-config-file)
37. [https://www.reddit.com/r/Terraform/comments/150fna8/golden_standard_for_modules_structure/](https://www.reddit.com/r/Terraform/comments/150fna8/golden_standard_for_modules_structure/)
38. [https://spacelift.io/blog/terraform-files](https://spacelift.io/blog/terraform-files)
39. [https://kodekloud.com/blog/manage-terraform-state/](https://kodekloud.com/blog/manage-terraform-state/)
40. [https://scalr.com/learning-center/scalr-terraform-state-management-features-and-functionality/](https://scalr.com/learning-center/scalr-terraform-state-management-features-and-functionality/)
41. [https://www.youtube.com/watch?v=nMVXs8VnrF4](https://www.youtube.com/watch?v=nMVXs8VnrF4)
42. [https://www.reddit.com/r/Terraform/comments/1aqiga7/how_to_organize_terraform_files/](https://www.reddit.com/r/Terraform/comments/1aqiga7/how_to_organize_terraform_files/)
43. [https://www.firefly.ai/academy/the-complete-guide-to-terraform-state-management](https://www.firefly.ai/academy/the-complete-guide-to-terraform-state-management)
44. [https://docs.gruntwork.io/reference/modules/terraform-aws-messaging/sns/](https://docs.gruntwork.io/reference/modules/terraform-aws-messaging/sns/)
45. [https://stackoverflow.com/questions/74421553/setting-up-cw-alert-for-disk-usage-using-terraform](https://stackoverflow.com/questions/74421553/setting-up-cw-alert-for-disk-usage-using-terraform)
46. [https://docs.cloudposse.com/modules/library/aws/sns-topic/](https://docs.cloudposse.com/modules/library/aws/sns-topic/)
47. [https://stackoverflow.com/questions/57593432/cloudwatch-metric-alarm-using-terraform](https://stackoverflow.com/questions/57593432/cloudwatch-metric-alarm-using-terraform)
48. [https://github.com/cloudposse/terraform-aws-sns-topic](https://github.com/cloudposse/terraform-aws-sns-topic)
49. [https://github.com/terraform-aws-modules/terraform-aws-cloudwatch](https://github.com/terraform-aws-modules/terraform-aws-cloudwatch)
50. [https://stackoverflow.com/questions/61147346/terraform-module-to-create-aws-sns-topic-subscription](https://stackoverflow.com/questions/61147346/terraform-module-to-create-aws-sns-topic-subscription)
51. [https://stratusgrid.com/blog/cloudwatch-alarms-terraform-module](https://stratusgrid.com/blog/cloudwatch-alarms-terraform-module)
52. [https://github.com/cisagov/instance-cw-alarms-tf-module](https://github.com/cisagov/instance-cw-alarms-tf-module)
53. [https://registry.terraform.io/modules/terraform-aws-modules/sns/aws/latest](https://registry.terraform.io/modules/terraform-aws-modules/sns/aws/latest)
54. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_alarm](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_metric_alarm)
55. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
56. [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic)
57. [https://github.com/clouddrove/terraform-aws-cloudwatch-alarms](https://github.com/clouddrove/terraform-aws-cloudwatch-alarms)
58. [https://registry.terraform.io/modules/terraform-aws-modules/sns/aws/4.1.0](https://registry.terraform.io/modules/terraform-aws-modules/sns/aws/4.1.0)
59. [https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/016ac149ea8a36d04fade1726f9329dc/2c9f3969-1a00-42c1-a3e4-952cc679b4ae/07e209a3.json](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/016ac149ea8a36d04fade1726f9329dc/2c9f3969-1a00-42c1-a3e4-952cc679b4ae/07e209a3.json)





