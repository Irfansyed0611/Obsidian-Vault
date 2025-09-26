---
tags:
---
### What is terraform state?
Terraform state is the source of truth for Terraform. It stores information about what resources exist in your infrastructure and how they correspond to your Terraform configuration, including metadata and resource attributes. This state is stored by default in a local file named "terraform.tfstate".

### Useful Terraform State commands

#### View current state

- `terraform state list` This list all the resource in the state file. This output can also be considered as the list of resources that are managed through terraform.

- `terraform state show aws_instance.web` This give detailed info about a specific resource.

- `terraform show`  Show all resources with details
- `terraform show -json > state.json` -> Show state in JSON format

#### Removing Resources from State

- Remove resource from state (doesn't destroy AWS resource)
`terraform state rm aws_instance.web`

- Remove multiple resources
`terraform state rm aws_instance.web aws_security_group.web_sg`

- Remove resources with pattern matching
`terraform state rm 'aws_instance.web[*]'`


#### Importing Existing AWS Resources

Resource that are created through AWS console can be imported to terraform state file to manage the it from the terraform end. To do this in the `main.tf` empty resource block as shown below:
```hcl

resource "aws_s3_bucket" "imported_bucket" {
	//You configuration goes here
}
```
Then in the terminal run this command.

`terraform import aws_s3_bucket.imported_bucket <bucket_name_from_AWS>`

Note: This command only adds the resource and its meta data to the state file. You still have to write your configuration file. The best practice would be to run the CLI commands and get all the configurations related to the resource (s3 bucket in this case) and use chatGPT write the configuration. Make sure when your so `terraform init` your configurations match the state file details. If not careful you'll delete the existing resource and recreate a new one based on your new configuration.
`terraform state show aws_s3_bucket.imported_bucket` can also be use to get details of the imported resource and write the config in the `main.tf`.

### Remote backend.
- A **remote backend** is a method of configuring a backend storage and locking system for Terraform.
- - It allows the **Terraform state file** to be:
    - Stored in a central, remote location.
    - Locked during operations like `terraform apply` to avoid concurrent modifications.
#### Why Do We Need a Remote Backend?
- **Local state files**:
    - Are suitable for individual development but **not ideal for team collaboration**.
    - Can cause issues if multiple users apply changes independently.
- **Remote backends** solve this by:
    - Providing a **centralized storage** accessible by all collaborators.
    - Enabling **state locking** to prevent concurrent modifications and reduce the risk of corruption.

#### Why Not Store State in GitHub?
- **Reason 1: Out-of-sync State**
    - A user might **forget to pull the latest state file** before making changes.
    - If they apply without the latest state, they may unknowingly **overwrite the correct infrastructure**.
-  **Reason 2: Sensitive Information**
    - The Terraform state file can **contain secrets** (e.g., passwords, tokens).
    - **Security best practices** dictate that such sensitive data **should not be pushed to GitHub or outside AWS**.
#### Recommended AWS Services for Remote Backend

-  **Amazon S3**:
    - Stores the **Terraform state file** securely.
    - Supports versioning and encryption.
- **Amazon DynamoDB**:
    - Used for **state file locking**.
    - Ensures only **one user can modify the state at a time**c.

#### **Why State File Locking is Important**?

- Prevents **race conditions** when multiple users run `terraform apply` concurrently.
- Helps **avoid conflicts and corruption** of the Terraform state file.
- Enforces **safety and coordination** in team environmentsckjj.
![[Pasted image 20250630004337.png]]

### How to configure remote backend?
In your `terraform.tf` add the following block.
```

```


### Terraform workspace
![[Pasted image 20250705194509.png]]

![[Pasted image 20250705194336.png | 400]]

