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

## Configure resources with Azure Resource Manager templates

### Review Azure Resource Manager template advantages

`Azure Resource Manager template` precisely defines all the Resource Manager resources in a deployment.

 - Templates improve consistency : Resource Manager templates provide a common language for you and others to describe your deployments.
 - Templates help express complex deployments : Templates enable you to deploy multiple resources in the correct order
 - Templates reduce manual, error-prone tasks : Manually creating and connecting resources can be time consuming, and it's easy to make mistakes.
 - Templates are code : can be shared, tested, and versioned similar to any other piece of software
 - Templates promote reuse : Template can contain parameters that are filled in when the template runs
 - Templates are linkable : You can link Resource Manager templates together to make the templates themselves modular.
 - Templates simplify orchestration : You only need to deploy the template to deploy all of your resources.

### Azure Resource Manager template schema

```
{
    "$schema": "http://schema.management.​azure.com/schemas/2019-04-01/deploymentTemplate.json#",​
    "contentVersion": "",​
    "parameters": {},​
    "variables": {},​
    "functions": [],​
    "resources": [],​
    "outputs": {}​
}
```
 - schema : Location of the schema
 - contentVersion : Version of the template
 - parameters : Values for deployment to customize
 - variables : Values that are used as JSON framgments in the template to simplify templatelanguage expressoins
 - functoins : User defiend functions that are available in the template
 - resources : Resource types that are deployed or updated in a resource group
 - output : Values that are returned

### Azure Resource Manager template parameters

```
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
        "description": "<description-of-the parameter>"
        }
    }
}
```
example
```
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "Username for the Virtual Machine."
    }
  },
  "adminPassword": {
    "type": "securestring",
    "metadata": {
      "description": "Password for the Virtual Machine."
    }
  }
}
```

### Bicep templates

Azure Bicep is a domain-specific language (DSL) that uses declarative syntax to deploy Azure resources. It provides concise syntax, reliable type safety, and support for code reuse.

![How Bicep works](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-resources-arm-templates/media/bicep.png)

 - Simpler syntax: Bicep provides a simpler syntax for writing templates.
 - Modules: You can break down complex template deployments into smaller module files 
 - Automatic dependency management: In most situations, Bicep automatically detects dependencies between your resources.
 - Type validation and IntelliSense: The Bicep extension for Visual Studio Code features rich validation and IntelliSense for all Azure resource type API definitions.


## Core Architectural components of Azure

### Regions, region pairs, sovereign regions
 - Regions : Geographical area on the planet that contains at least one datacenters that are connected via low-latency network
 - Region pairs : Most regions are pairs with another region within the same geography. ( West India and Brazil South paired in only one direction )
 - Soverieign regions : Isolated from the main instance of Azure.( Compliance or legal purposes )
### Availability Zones
 - Physically sepearte datacenters within a region
 - Categories
   - Zonal services : Pin the resource to a specific zone
   - Zone reduntant service : Platform replicate automatically across zones
   - Non-regional services : Available from Azure geographies 
### Azure datacenters
### Azure resources and Resource Groups
 - Resource : Building block of Azure (  Anything create, provisoin, deploy etc )
 - Resource Groups : Grouping of resources
   - Resource available in one group at a time
   - Groups's cannot be nested
### Subscriptions
 - Unit of management, billing and scale
 - Logically organize resource groups and facilitate billing
 - Subscription links to an azure account
 - Account can have multiple subscriptions but only need one
 - Can use to define boundaries around azure products
 - Access management applies at subscription level
 - Types of subscriptions
   - Billing boundary : To determine how an account is billed
   - Access control boundary : To reflect different organizational structures ( eg different departments )
### Management groups
 - Level of scope above subscriptions
 - Subscriptions within management group inherit settings from the group

## Azure compute and networking services

### Virtual Machines
 - Total control over the OS
 - Ability to run custom software
 - Custom hosting configurations
### Virtual Machine Scale sets
 - Create and manage a group of identical load-balanced VMs
### Virtual Machine availability sets
 - Ensure VM's stagger updates and have varied power and network preventing losing VM's with network or power failure

- Update domain - Apply updates while knowing only one update domain grouping is offline at a time
- Fault domain - Group VMs by common power source and network switch.  Default slipt VMs across three fault domains.

### Azure Containers




