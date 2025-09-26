#### Core Terraform Workflow

The fundamental workflow in Terraform revolves around a few key command-line interface (CLI) commands:
1. **`terraform init`**: This command initializes the working directory, downloading necessary provider plugins (like the AWS provider) and preparing the project for further commands. During initialization, Terraform creates a `.terraform.lock.hcl` file for deterministic initialization and a hidden `.terraform` folder to store provider binaries. If you're switching to a remote backend, `terraform init` will also prompt you to copy your local state to the remote location.

2. **`terraform plan`**: Before making any changes, this command generates an execution plan, which is a preview of what Terraform will do. It compares the current state of your infrastructure (tracked in the state file) with the desired state defined in your `.tf` configuration files, listing all proposed actions (create, update, or delete resources). This step is crucial for review, analysis, and debugging.
	![[Pasted image 20250616163035.png | 400]]

3. **`terraform apply`**: This command executes the planned changes to provision or modify the infrastructure in your AWS account. Terraform will typically prompt for confirmation before proceeding, though this can be skipped with the `-auto-approve` flag. After a successful `apply`, Terraform updates its state file to reflect the new infrastructure.

4. **`terraform destroy`**: When resources are no longer needed, this command systematically tears down all infrastructure managed by the current Terraform configuration. It also presents a plan for destruction, which needs confirmation.

This sequence – `init`, `plan`, `apply`, `destroy` – forms the standard lifecycle for managing infrastructure as code with Terraform.

