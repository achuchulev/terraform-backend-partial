# Test usage of ["remote"](https://www.terraform.io/docs/language/settings/backends/remote.html) backend partial configuration with Terraform Cloud (TFC)

### Introduction
When using Terraform Cloud (TFC) as a "remote" backend to manage multiple environments (e.g. dev, test, stage, prod, etc.) that share the same Terraform configuration, you need to have an option to specify the environment where Terraform will provision resources.



### The Problem
Currently, the Terraform "remote" backend does not support interpolations within its configuration block. This means it is not allowed to have variables as values when defining the "organization" and the "name" of the workspace.



Solution
The "remote" backend supports partial configuration, as shown below, that allows Terraform to be initialized with a dynamically set backend configuration.

```
terraform {
  backend "remote" {}
}
```

In order to configure Terraform Cloud workspaces with different environments, create backend configuration files in .hcl format (for example: `dev.hcl, test.hcl, prod.hcl`, etc.) in the root of the tracked configuration directory containing the following (replacing `"ORG"` and `"WORKSPACE"` with appropriate values):

```
organization = "ORG"
workspaces { name="WORKSPACE"}
```

Then, in Terraform Cloud, set the `TF_CLI_ARGS_init` Environment Variable with the name of the respective configuration file as value in the format `-backend-config=dev.hcl` to configure the `dev` workspace as an example.


This way you can have Terraform Cloud workspaces that share the same Terraform codebase configured to manage different environments.
