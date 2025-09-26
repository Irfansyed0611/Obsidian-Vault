#### What is terraform and how is it different from Ansible
Terraform is a cloud resource provisioning tool which creates resources using IaC (Infrastructure as a code). You write a script to provision resources. Ansible on the other hand is a CMT (configuration management tool). Ansible is used to automate installation and modifications of configurations and applications in the cloud resources.

#### General syntax of HCL

The terraform code is written in HCL (HashiCrop configuration Language). The general syntax of HCL.

```tf
block  parameters  {
.............
............. Arguments
.............
}
```

##### block_type
The block_type in terraform file tell what you are trying to create through that script. If it's a resource then you write resource, if it's a variable then you write variable and so on. 
Some common `block_type` are:
- `resource`
- `variable`
- `output`
- `provider`
- `data`
- `locals`
- `module`
- `terraform` (a special top-level block)
- And many nested block types like `ingress`, `egress`, `lifecycle`, `filter`, etc.

![[01 Introduction 2025-06-16 00.05.59.excalidraw]]

##### Parameters
The `parameters` you've indicated represent the **labels** that follow the `block_type` keyword. These labels serve to uniquely identify or name a specific instance of that block. The number of labels can vary depending on the `block_type`.
**Syntax**: `block_type "label_1" "label_2" { ... }`
- **`"label_1"`**: The first label often represents the _type_ of thing being configured within that block (e.g., `aws_instance`, `aws_s3_bucket` for a `resource` block, or the name of a variable for a `variable` block).
- **`"label_2"` (optional)**: The second label (if present) is typically a _local name_ that you give to this specific instance of the block. This local name must be unique within that configuration file (or module) for that block type and first label. It allows you to refer to this specific configuration later in your code.

#### arguments
The curly braces `{}` define the **body** of the block. Inside these braces, you place the **arguments** (and sometimes nested blocks) that configure the behavior or properties of the block.

**Example**:
```tf
resource "local_file" "my_file" {
	filename = "automate.txt"
	content = "This is my first main.tf file"
}
```

In the above example:
- resource is the block_type
- local_file and my_file are parameter label-1 and lable-2.
- filename and contents in {} are arguments.

