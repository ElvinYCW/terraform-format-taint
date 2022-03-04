## TERRAFORM FORMAT AND TAINT COMMANDS

### Ensure that the Azure CLI is pre-installed, a valid Azure account is logged in through the CLI before attempting.

### 'terraform fmt':
We use this command to format or beautify the codes, whether the files are main.tf, variables.tf or contain other terraform codes. Just passed the command as it is 'terraform fmt'.

### 'terraform taint <resource-address>':
This command is passed together with the 'resource address' as per the terraform code. We use this command to taint the resource. When the resource is tainted, it is marked for deletion. This tainted resource will be deleted on the next 'terraform apply' command, and a new resource is going to be created in its place. So when or why do we use this commamd? When we want to re-run a provisoner or mimic a behavour which is only seen when a resource is created. Here is an example:

Upon executing the command 'terraform apply', the resource 'azurerm_linux_virtual_machine.myterraformvm' is created as can be seen in the state file 'terraform.tfstate':

  "resources": [
    {
      "mode": "managed",
  
      "type": "azurerm_linux_virtual_machine",
      "name": "myterraformvm",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "schema_version": 0,

Now, we shall pass the command 'terraform taint azurerm_linux_virtual_machine.myterraformvm', and the state file 'terraform.tfstate' will now show that the same resource has 'status: tainted':

  "resources": [
    {
      "mode": "managed",
  
      "type": "azurerm_linux_virtual_machine",
      "name": "myterraformvm",
      "provider": "provider[\"registry.terraform.io/hashicorp/azurerm\"]",
      "instances": [
        {
          "status": "tainted",
          "schema_version": 0,

(To see the state file, you may use 'cat terraform.tfstate' or 'vim terraform.tfstate' follow by ':q!' if vim is used or any text editor.)

After we taint the resource, we will run 'terraform apply' and the plan output will show it will add 1 new, change nothing and destroy 1 existing resource as follow:

      Plan: 1 to add, 0 to change, 1 to destroy.
      
      Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.
      
      Enter a value: 

We will continue the terraform apply execution by passing the value 'yes', and upon completion the result will be:

      Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

### do remember to delete all resources with 'terraform destroy'.
