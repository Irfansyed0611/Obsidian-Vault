---
tags:
---
### Using count
#### main.tf
```hcl
# Create S3 buckets using count
resource "aws_s3_bucket" "environment_buckets" {
  count  = length(var.environments)
  bucket = "irfan-checking-count-${var.environments[count.index]}"

  tags = {
    Name        = "irfan-checking-count-${var.environments[count.index]}"
    Environment = var.environments[count.index]
    CreatedBy   = "Terraform"
  }
}


```

#### Variables.tf
```hcl
# Define the environments as a variable
variable "environments" {
  description = "List of environments for S3 buckets"
  type        = list(string)
  default     = ["dev", "prod"]
}

```

#### Outputs.tf
```hcl
# Outputs
output "bucket_names" {
  description = "Names of all created S3 buckets"
  value       = aws_s3_bucket.environment_buckets[*].bucket
}

output "bucket_arns" {
  description = "ARNs of all created S3 buckets"
  value       = aws_s3_bucket.environment_buckets[*].arn
}

output "individual_bucket_details" {
  description = "Individual bucket details"
  value = {
    for i, bucket in aws_s3_bucket.environment_buckets :
    var.environments[i] => {
      name = bucket.bucket
      arn  = bucket.arn
      id   = bucket.id
    }
  }
}
```

When staging was removed from the array, terraform destroyed the prod and recreated it at index[1] to maintain indexing order.
![[Pasted image 20250624020307.png]]

----

### Using for_each
#### main.tf
```hcl
resource "aws_s3_bucket" "environment_buckets" {
  for_each = toset(var.environments)
  bucket   = "irfan-checking-foreach-${each.value}"

  tags = {
    Name        = "irfan-checking-foreach-${each.value}"
    Environment = each.value
    CreatedBy   = "Terraform"
  }
}
```

#### variables.tf
```hcl
# Define the environments as a variable
variable "environments" {
  description = "List of environments for S3 buckets"
  type        = list(string)
  default     = ["dev", "prod"]
}
```

#### outputs.tf
```hcl
# Outputs
output "bucket_names" {
  description = "Names of all created S3 buckets"
  value       = values(aws_s3_bucket.environment_buckets)[*].bucket
}

output "bucket_arns" {
  description = "ARNs of all created S3 buckets"
  value       = values(aws_s3_bucket.environment_buckets)[*].arn
}

output "individual_bucket_details" {
  description = "Individual bucket details"
  value = {
    for env, bucket in aws_s3_bucket.environment_buckets : 
    env => {
      name = bucket.bucket
      arn  = bucket.arn
      id   = bucket.id
    }
  }
}
```

By using `for_each`, when we update or remove resources from the middle only that resource will be affected.jjj
![[Pasted image 20250624171547.png]]
