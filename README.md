# Microsoft Certified: Azure Administrator Associate

## Use Azure Resource Manager

 - Enable user to work with the resources(VM, Storage, Virtual network, web app, database, etc) in the solutions group.
 - Deploy, update or delete all resources for solution in a single coordinated operation
 - Template for deployment ( use in different env eg-dev, staging, production )

### Consistent management layer

 - Consistent management layer to perform tasks through Azure Powershell, Azure CLI, Azuer portal, REST API and client SKDs.

![Diagram](https://learn.microsoft.com/en-us/training/wwl-azure/use-azure-resource-manager/media/resource-manager-016a1bac.png)

### Benefits

 - You can deploy, manage, and monitor all the resources for your solution as a group, rather than handling these resources individually.
 - You can repeatedly deploy your solution throughout the development lifecycle and have confidence your resources are deployed in a consistent state.
 - You can manage your infrastructure through declarative templates rather than scripts.
 - You can define the dependencies between resources so they're deployed in the correct order.
 - You can apply access control to all services in your resource group because Role-Based Access Control (RBAC) is natively integrated into the management platform.
 - You can apply tags to resources to logically organize all the resources in your subscription.
 - You can clarify your organization's billing by viewing costs for a group of resources sharing the same tag.

## Review Azure resource terminology

 - resource - A manageable item that is available through Azure. Some common resources are a virtual machine, storage account, web app, database, and virtual network, but there are many more.
 - resource group - A container that holds related resources for an Azure solution. The resource group can include all the resources for the solution, or only those resources that you want to manage as a group. You decide how you want to allocate resources to resource groups based on what makes the most sense for your organization.
 - resource provider - A service that supplies the resources you can deploy and manage through Resource Manager. Each resource provider offers operations for working with the resources that are deployed. Some common resource providers are Microsoft.Compute, which supplies the virtual machine resource, Microsoft.Storage, which supplies the storage account resource, and Microsoft.Web, which supplies resources related to web apps.
 - template - A JavaScript Object Notation (JSON) file that defines one or more resources to deploy to a resource group. It also defines the dependencies between the deployed resources. The template can be used to deploy the resources consistently and repeatedly.
 - declarative syntax - Syntax that lets you state "Here is what I intend to create" without having to write the sequence of programming commands to create it. The Resource Manager template is an example of declarative syntax. In the file, you define the properties for the infrastructure to deploy to Azure.

## Create resource groups

 - Can deploy to any new or existing resource group
 - Deployment to a resource group becomes a job where template execution can be tracked
 - If deployment fails output of the job describe why
 - Deployments are incremental

### Rules for resource groups
 - Resources can only exist in one resource group ( cannot be shrared between groups )
 - Resource groups cannot be named
 - Resource groups can have resources of many different types
 - Resource groups can have resources from different regions

### Creating resource groups
 - All resources in group should share same life cycle
 - Can move resources between groups but limitations apply
 - Resourse groups can be used to scope acess control for administrative actions
 - Resources can interact with resources in other resource groups
 - Need to provide a location

### Create Azure Resource Manager locks

 - Allow organizations to put a structure in place that prevents the accidental deletion of the resources
 - Can associaete with a subscription/resource group or resource
 - Locks are inherited by child resources
 - Only the owner and User Access Administrator roles can create/delete management locks
 - Lock types
  - Read-Only locks - No changes can be done
  - Delete locks - Prevent deletion 

### Reorganize Azure resources

- When moving resources both source and target groups are locked during the operation
- Write and delete operations are locked ( can't add, update or delete )

### Remove resources and resource groups

- Powershell, use `Remove-AzResourceGroup` command ( eg `Remove-AzResourceGroup -Name "RES001"` )

### Determine resource limits

 - The limits shown are the limits for your subscription.
 - When you need to increase a default limit, there is a Request Increase link.
 - All resources have a maximum limit listed in Azure limits.
 - If you are at the maximum limit, the limit can't be increased.

## Introduction to Azure Cloud Shell

 - Browser associate cloud shell
 - Bash / Powershell
 - Double encryption at rest by default
 - Cloud storage for ssh keys, sripts etc

### When should you use Azure Cloud Shell?

Shouldn't use Azure Cloud Shell if
 - Long running scripts ( more 20 min )
 - Need admin permission
 - Install tools that aren't supported
 - Need storage from different regions
 - Open multiple instances at once

### Powershell

Share from traditional command line

 - Buit in help system
 - Pipeline
 - Aliases

Differs

 - Operates on objectsover text
 - Has cmdlets, takes objects and return objects. Core cmdlets built in .NET Core
 - Has many types of commands - native executables, cmdlets, functions, scripts or aliases
 - cmdlet -> [Verb]-[Noun]
 - Search with `Get-Command -Nount alias*` or `Get-Command -Verb Get -Noun alias*`
