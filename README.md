# Terraform code to deploy three-tier architecture on azure


## Challenge #1

A 3-tier environment is a common setup. Use a tool of your choosing/familiarity create these resources. Please remember we will not be judged on the outcome but more focusing on the approach, style and reproducibility.


## What is three-tier architecture?
Three-tier architecture is a well-established software application architecture that organizes applications into three logical and physical computing tiers: the presentation tier or user interface; the application tier, where data is processed; and the data tier, where the data associated with the application is stored and managed.
The main advantage of a three-tier architecture is that because each layer works with its own infrastructure, each layer can be developed simultaneously by a separate team of developers and updated or expanded as needed without affecting other levels.

## What is terraform?
Terraform is an open-source infrastructure as code software tool created by HashiCorp. Users define and provision data center infrastructure using a declarative configuration language known as HashiCorp Configuration Language, or optionally JSON.


## Solution

Steps to follow:

1. Browse to the Azure portal (https://portal.azure.com/)
2. If necessary, log in to your Azure subscription and change the Azure directory. Make sure you are owner or Contributor to the subscription.
3. Open Cloud Shell.
4. Select PowerShell from the drop down
5. Verify the default Azure subscription where you want to create resources with below command 

	az account show
	
6. To use a specific Azure subscription, run az account set. 

	az account set --subscription "<subscription_id_or_subscription_name>"
	
7. Clone the terraform repo from GITHUB with below command 

	git clone https://github.com/umar112farook/azurethreetier.git

8. Change working directory now with below command.

	cd azurethreetier
	
9. Now initialize terraform with below command.

	terraform init
	
10. Now verify the execution terraform plan with below command.

	terraform Plan
	
11. Now do apply to create the resources in the subscription with below command.

	terraform apply.
	
12. When prompted press YES



##Following the above steps will give you below resources

1. availability_set == "app_availabilty_set"
2. availability_set == "web_availabilty_set"
3. network_interface == "app-net-interface"
4. network_interface == "web-net-interface"
5. virtual_machine == "app-vm"
6. virtual_machine == "web-vm"
7. sql_database == "db"
8. sql_server == "primary"
9. subnet == "app-subnet"
10. subnet == "db-subnet"
11. subnet == "web-subnet"
12. virtual_network == "vnet01"
13. resource_group == "azure-stack-rs"
14. network_security_group == "app-nsg"
15. network_security_group == "db-nsg"
16. network_security_group == "web-nsg"
17. subnet_network_security_group_association == "app-nsg-subnet"
18. subnet_network_security_group_association == "db-nsg-subnet"
19. subnet_network_security_group_association == "web-nsg-subnet"



Explanation:

One resource Group and inside that you will have:

1. One virtual network tied in three subnets.
2. Web and App subnet will have one virtual machines each.
3. First virtual machine -> allow inbound traffic from internet only.
4. Second virtual machine -> entertain traffic from first virtual machine only and can reply the same virtual machine again.
5. App can connect to database and database can connect to app but database cannot connect to web.


### The Terraform resources will consists of following structure

```
├── main.tf                   // The primary entrypoint for terraform resources.
├── vars.tf                   // It contain the declarations for variables.
├── output.tf                 // It contain the declarations for outputs.
├── terraform.tfvars          // The file to pass the terraform variables values.
```

### Module

A module is a container for multiple resources that are used together. Modules can be used to create lightweight abstractions, so that you can describe your infrastructure in terms of its architecture, rather than directly in terms of physical objects.

For the solution, we have created and used five modules:
1. resourcegroup - creating resourcegroup
2. networking - creating azure virtual network and required subnets
3. securitygroup - creating network security group, setting desired security rules and associating them to subnets
4. compute - creating availability sets, network interfaces and virtual machines
5. database - creating database server and database

All the stacks are placed in the modules folder and the variable are stored under **terraform.tfvars**

To run the code you need to append the variables in the terraform.tfvars

Each module consists minimum two files: main.tf, vars.tf

resourcegroup and networking modules consists of one extra file named output.tf
