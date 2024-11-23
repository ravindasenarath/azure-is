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

 - Virtualization environments
 - Lightweight and designed to scale
 - Azure container instances : PaaS service
 - Azure container Apps : Similar to container instances + ability to incorporate load balancing and scaling
 - AKS(Azure Kubernetes Service) : Manged Kubernetes service

### Azure functions

 - Event driven serverless compute option
 - Even driven, short ( within seconds or less )
 - Charged for CPU time
 - Can be stateful(Durable Functions) or stateless

### App hosting options: Azure App Service

 - webapps, mobile back-ends, RESTful API's
 - Automatic scaling and high availability
 - Linux / Windows
 - Automated deployment from GitHub, Azure DevOps
 - Support .NET, .NET core, Java, Ruby, Node.js, PHO or Python
 - Handles infrastructure decisions in hosting web accessible apps
   - Deployment and management
   - Endpoints can be secured
   - Scale with traffic
   - Built in load balancing and traffic manager provide high availbility
  
  ### Asure virtual networking

 - Isolation and segmentation : Created isolated virtual networks. Allocate parts of ip addresses to subnet
 - Internet communication : Assign a public IP to azure resource or put resource behind a load balancer
 - Communicate between azure resources
    - Virtual networks can connect to other resources
    - Service endpoints can connect to other azure resource types 
 - Communicate with on prem resources
    - Point-to-point : Coputer outside org back into corporate network
    - Site-to-site : Link on-prem VPN device or gateway to Azure VPN gateway in virtual network.
    - Azure ExpreseRoute : Dedicated private connectivity(Not over internet)
 - Route netowrk traffic
    - Route tables : Define rules
    - Border Gateway Protocol(BGP) : Works with Azure VPN gateways, Azure Route Server or ExpressRoute go connect on prem to Azure virtual network 
 - Filter network traffic
    - Network security gruops : Mange inbound/outbound rules
    - Network virtual appliances : Specialized VMs with hardened network appliance.
 - Connect Virtual networks
 
 Virtual network peering: connect two network VN's, Use Azure intranet. Use defined routes(UDR) allow to control routing tables between routes.

## VPN

  - Use an encrypted tunnel
  - Connect two or more trusted networks over untrusted network
  - Traffic is encrypted

### VPN Gateways

Deployed in dedicated subnet of the virtual network end enable

 - Connect on-prem datacenters to virtual networks through a site-to-site connection
 - Connect individual devices to virtual networks through a point-to-point connection
 - Connect virtual networks to other virtual networks

Features of VPN gateway

 - Data transfer is encerypted in a private tunnel
 - Only one gateway per virtual network
 - One gateway can connect to multiple locations

Types of VPN Gateway

 - Policy based VPN - Sepcify statically the IP address of packets that should be encrypted
 - Route based VPN - IP sec tunnels are modeled as a network interface or virtual tunnel. Decide which tunnel to use for each packet. Preferred connection method for on-prem devices. More resilient to topology changes.

Use Route based VPN gateway for

 - COnnections between VNs
 - Point to point connections
 - Multisite connections
 - Coesistence with Azure ExpreeRoute gateway

High available scenarios

 - Active/standby : Two connections, one stand by which activate on disruption. Connections are interrupted during the failover(few seconds for planned and within 90 for unplanned disruptions)
 - Actuve/active : High availability
 - ExpressRoute failover : Configure a VPn gateway as a secure failover path for ExpressRoute connecitons
 - Zone -redundant gateways : Deploy VPN gateways and ExpressROute gateways in  a zero redundant configuration. 

### Azure ExpressRoute

 - Extend on-prem network to MS cloud via a private connection
 - Connection called "Express circuit"

Features and benefits using ExpressRoute between Azure and on-prem

 - Connectivity to MS cloud services across all regions in the geopolitical region
 - Global connectivity to MS services across all regions wih the ExpressRoute Global Reach
 - Dynamic routing between your network and MS via BOrder Gateway Protocol(BGP)

ExpressRoute enables direct access to the following services in all regions

 - MS Office 365
 - MS Dynamics 365
 - Azure compute services
 - Azure cloud services

Global connectivity - ExpressRoute Global Reach to exchange data across on-prem sites by connecting ExpressRoute circuits
Dynamic routing - GGP protocol is used.
Built in redundency - Can use multiple circuits

ExpressRoute connectivity models

 - CloudExchange colocation -
 - Point-to-point Ethernet connection -
 - Any-toany connection -
 - Directly from ExressRoute sites - Connect directly into MS global network at a peering location. Express route Direct provides 100 Gbps or 10 Gbps connectivity with Active/Active support

### Azure DNS

 - Reliability and performance : Hosted on Azure's global network of DNS name servers, use anycast for performance
 - Security : Based on Azure resource manager
   - Azure role-based access control(Azure RBAC) to control who as access to specific actions for organisation
   - Activity logs to monitor how a user in organisation modified a resource or find errors when troubleshooting
   - Resource locking to lock subscription, resource group, or resource. Locking prevent accidental deleting or modifying critical resources
 - Easy to Use : Integrated in Azure portal and use same credentials, support contact and billing as azure services.
 - Customize virtual networks : Support private DNS domains. Allow to use your own domain names in private virtual networks, rather than stuck with azure provided names.
 - Alias records : Can use to refer to Azure resoruce. If IP address of underlying resource change. Alias points to service instance, service instance associate with and IP address

## AZ-104: Manage identities and governance in Azure

### MS Entra ID

 - Part of the PaaS offerin and operates as a Microsoft managed directory service in cloud
 - Not part of the core infrastructure that customer own and manage
 - Less control
 - Get access to Multi-factor authenticatoin, identify protection and self service password reset

Usages of Entra ID

 - Configure access to applications
 - Configure SSO to cloud based SaaS apps
 - Manage user groups
 - Provisioning users
 - Enabling federation between organisations
 - Provideing an identify management solution
 - Identify irregular sign-in activities
 - Multi factor auth
 - Extend onprem AD implementation to Entra ID
 - Configure application proxy for cloud and local apps
 - Configure conditional access for users and devices

Entra Tenants

 - At any given moment, Azure subscription must be associated with one and only one Entra tenant(Can associate with multiple)
 - Each entra tenant assigned a default DNS derived from the name of the MS account used to create and Azure subscription followed by **onmicrosoft.com**

Entra Schema

 - Fewer object types
 - No definition of computer class
 - Can't use with Group Policy Objects(GPOs)
 - Doesn't include organisational unit(OU) class, so can't arrange into a hierarchy of custom containers. Can use organizing objects based on their group membership.
 - Objects of the application and servicePrinciple represent applications in Entra ID
 - Object in application class -> application definision, object in service principle -> constitues its instance in the current entra tenant

### Confiure user and group accounts

Account types

 - Cloud identity - Defined only in Entra ID. Includes administrator and users.
 - Directory synchronized Identity - Defined in on prem AD and synced to cloud via Entra connect. Source for accounts is Windows Server AD.
 - Guest user - Defined outside Azure. ( eg other cloud providers )

Things to consider

 - User profile data - Allow user to set profile info
 - Restore options for deleted accounts - Restore within a period
 - Gathered account data - Audit purpose
 - Consider naming conventions
 - Consider using initial passwords - Design an initial password and a way to communicate it securely

Group accounts

 - Security groups - Manage member and computer access to shared resources
 - MS 365 groups - Provide collaboration opportunities

Things to consider when add member to a group

 - Assigned - Each user have unique permissions
 - Dynamic user - To automatically add and remove group members
 - Dynamic device - To automatically add and remove devices in secruity groups

Administrative Units

Restrict administrator scope when organisation has independent divisions

Things to consider

 - Consider management tools
 - Consider role requirments in the Azure portal
 - Consider scope of administrative units

### Configuring subscriptions

 - An Azure subscription is a logical unit of Azure services that linked to an Azure account.
 - Azure account is an identity in MS Entra ID or a directory trusted by MS Entra ID such as a work or school account

![Azure Subscription](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-subscriptions/media/azure-subscriptions-e855533e.png)

 - Each subscription can have a different billing and payment configuration
 - Multiple subscription can be linked to same Azure account
 - Multiple Azure accounts can be linked to same subscription

Ways to obtain Azure subscription

 - Enterprise agreement
 - Microsoft reseller
 - Microsoft Partner
 - Personal free account

MS Cost Management

 - Shows organisational cost and usage patterns
 - Reports with usage based cost, internal and external costs. Help find spending anomalies
 - Use Azure management groups, budgets and recommendations to reduce costs
 - Can export or consume via API's

Cost Savings

 - Reservations - Pay ahead provide a billing discount
 - Azure Hybrid Benefits - Maximize the value of existing on prem Windows Server or SQL Server
 - Azure Credits - Monthly credit benefits
 - Azure regions - Pricing vary from one region to another
 - Budgets - Apply budgeting features
 - Price Calculator

### Configure Azure Policy

 - Service to create, assign, manage policies to control audit resources
 - Enforce rules over resource configurations so they stay compliant

Management groups

 - Provide a level of scope and control above subscriptions
 - containers to manage access, policy and compliance across subscriptions
 - By default all subscriptions are placed under the top-level management group or root group
 - All subscriptions within a management group inherit conditions from the group
 - Management group tree can support upto six levels of depth
 - Role based access control authorization for management group operations isn't enabled by default

![Management groups](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-azure-policy/media/management-groups-aa92c04a.png)

Implement Azure policies

 - Enforce rules and complience - Built-in policies or build custom policies. Support real-time policy evaluation and enforcement and periodic or on demand complience evaluation
 - Apply poicies at scale - Apply across organisation. Apply multiple policies and aggregate policy states with policy initiative. Define exclusion scope.
 - Perform remediation - Conduct real-time remediation
 - Exercise governance - Implement governance tasks

Create azure policies

![Steps to create policy definisions](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-azure-policy/media/implement-azure-policy-b4a4a47c.png)

 1. Create policy definitions - Condition to evaluate and the actions to perform when met.Can create or choose from built in. 
 2. Create an initiative definition - Set of policy definitions that help you track resources complience state.Can create or choose from built in. 
 3. Scope the initiative definition - How initiative definitions are applied to resources. Can limit the scope of an initiative definition of specific management group, suscription or resource group
 4. Determine compliance - Can evaluate the state of resources.

## AZ-104 : Configure and manage virtual networks for Azure administrators

### Configure Virtual Networks

Plan virtual networks

 - Logical isolocation of the Azure cloud resources
 - Can use to provision and manage virtual private networks(VPN)
 - Each virtual network has its own CIDR and can be linked to other virtual networks and on prem networks
 - Can link VPN to on prem to create a hybrid when CIDR do not overlap
 - Can control the DNS server settings for VPNs and segmentation of VPNs to subnets

Subnets

 - Logical divisions within VPN
 - Subnets contains a range of IP addresses that fall with in VPN CIDR
 - Address range for a subnet must be unique within VPN CIDR
 - Subnets can't overlap in Same VPN
 - Reserved addresses
   - 192.168.1.0 - Identifies the VPN
   - 192.168.1.1 - Default gateway
   - 192.168.1.2 / 192.168.1.3 - DNS IP
   - 192.168.1.255 - Broadcast address
  
Create VPN

 - Plan to use an IP address space that is not already in use in your organisation
   - Address space for VPN can be either on prem or cloud but not bot
   - Once created IP address space cannot be changed 
 - Define at least one subnet

### Configure network security groups

 - Define security rules to controlt traffic
 - Allow or Deny inbound/outbound traffic
 - Associated with a subnet or network interface
 - Can be used multiple times
 - DMZ(Demilitariazed zone) - act as a buffer between resource within virtual network and the internet
   - Restrict traffic flow to all machines that reside within the subnet
   - Each subnet can have a maximum of one associated network security group
 - By default `DenyAllInbound` and `AllowInternetOutbound`
 - Available settings
   - Name
   - Priority ( low priority, high presedence )
   - Port
   - Protocol
   - Source
   - Destination
   - Action
  - Inbound traffic - Associated network security groups rules first then associated network interface
  - Outbound traffic - NI first then assciated security groups
  - Application security groups(Logically group your virtual machines by workload)

### Configure Azure Virtual Network peering

 - Connect virtual networks in the same or different regions

![network peering](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-vnet-peering/media/network-peering-5beae28a.png)

 - Regional virtual network peering - In same region
 - Global virtual network peering - In different regions
 - Global peering in different Azure Government cloud regions ins't permitted
 - Traffic between networks is private(No public internet, gateways etc)
 - Use Azure infrastrucuture so low latency, high bandwidth
 - Azure VPN Gateway - Used as a transit point

![VPN gateway](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-vnet-peering/media/gateway-transit-173a51a0.png)

 - One gateway per virtual network
 - Support both regional and global peering
 - With VPN gateway transit virtual network can communicate to resources outside peering
   - Use site-to-site VPN to connect on prem
   - Use vnet-to-vnet to another virtual network
   - Use a point-to-site VPN to connect to a client
 - Allow share gateway and get access to resources ( No eed VPN gateway in peer virtual network )
 - Apply serucity groups in virtual network
 - Extend peering
   - Hub and spoke networks - Hub host infrastructure components like NVA or VPN gateway
   - User defined routes - Next hope in user defined route to be IP address of a virtual machine in peering network or a VPN gateway
   - Service chaning - Used to direct traffic from one virtual network to a virtual applience or gateway.

### Configure network routing and endpoints

System routes

 - Used to direct network traffic between VMs, on prem networks and internet
 - Information recorded in a route table
 - Control traffic for VMs(Between VMs in same subnet, VMs in different subnets in same VN, VM to internet)
 - Route table contains set of rules(routes)
 - Route tables record information about the system routes, where the tables are associated to subnets
 - Each packet leaving a subnet is handled based on the associated route table
 - Packets matched to route using destination(IP, VN gateway, virtual appliance, internet)
 - If maching route not found packet is dropped

![Route table](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-network-routing-endpoints/media/system-routes-08992506.png)

User defined routes(UDR)

 - Control network traffic by defning routes that specify the next hop of the traffic flow
 - Next hop can be
   - Virtual network gateway
   - Virtual Network
   - Internet
   - Network virtual appliance(NVA)
 - Similar to system routes, UDRs also access route tables.
 - Each route table can be associated to multiple subnets.
 - Each subnet can be associated to one route table only.
 - There are no charges for creating route tables in Microsoft Azure.

![UDR](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-network-routing-endpoints/media/user-defined-routes-2417e693.png)

Service endpoint(virtual network endpoints)

 - Allows to access the services without the need for reserved public IP addresses
 - Extend virtual network identify to Azure services to secure resources
 - Secure Azure service resources to VN by network rules
 - Can remove public internet access to resources
 - Configured though the subnets.
 - Provide optimal routing ( forced-tunelling )

![Service endpoint](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-network-routing-endpoints/media/service-endpoint-addresses-a027197f.png)

Private Link

 - Provide private connectivity from a VN to Azure Paas, Customer owned or MS parter services.
 - Keep all traffic on MS global network. No public internet access
 - Privately deliver own services to custoemr in VNs

![Private Link](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-network-routing-endpoints/media/private-links-602b4a62.png)

### Configure Azure Load Balancer

![Load balancer](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-azure-load-balancer/media/load-balancer-4caf947b.png)

 - Can be used for inboud or outbound
 - Can be public or internal or both
 - Things to configure
   - Front-end IP configuration - Public or internal ip LB responds to
   - Back-end pools - Services or resources
   - Health probes - Ensure backend are healthy
   - Load-balancing rules - How traffic is distributed

![Internal LB](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-azure-load-balancer/media/internal-load-balancer-5ae85589.png)

- Stock Keeping Unit (SKU)
  - Basic - Upto 300 pools
  - Standard - Superset of basic load balancer. Expanded and more granular feature set than basic SKU, Upto 1000 pools
  - Gateway - High performance and availability
 
![SKU features](https://github.com/user-attachments/assets/33d1c159-0017-4e96-b9f9-20db14b143c9)

 - Health Probes
    - HTTP(every 15s, if respond within 31s with 200OK) or TCP(if specified listener exists)
    - Port : Backend port
    - URI : URI to request health status
    - Interval : Default 15s
    - Unhealthy threshold : No of failures to consider unhealthy

 - Load balancing rules
   - Session persistence - None(Any can handle), Client IP(Successive from same ip goes to same backend), Client IP and protocol
   - Load balancing rules can be used in combination with NAT rules 

### Configure Azure Application Gateway

 - Load balancer for web traffic

![Application Gateway](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-azure-application-gateway/media/application-gateway-cb3392f4.png)

 - Application layer routing - Route based on the URL of request
 - Round robin load balancing - Use RR algorithem
 - Session stickiness
 - Protocols - HTTP/HTTPS/HTTP/2 or Websocket
 - Firewall protection
 - Encryption
 - Load autoscaling

 - Methods of routing
   - Path based routing - different url paths
   - Multi site routing - More than one web application on same application gateway instance
 - Can redirect traffic
 - Rewrite HTTP headers
 - Custom error pages

![Path based routing](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-azure-application-gateway/media/path-based-routing-15bcef5f.png)

![Multi site routing](https://learn.microsoft.com/en-gb/training/wwl-azure/configure-azure-application-gateway/media/site-based-routing-e686b605.png)

Application Gateway components

 - Front-end IP address
 - Web Application Firewall
 - Listeners
 - Routing rules - A rule specifies how to interpret the hostname and path elements in the URL of a request. A routing rule also has an associated set of HTTP settings. These HTTP settings indicate whether (and how) traffic is encrypted between Application Gateway and the back-end servers. Other configuration information includes protocol, session stickiness, connection draining, request timeout period, and health probes.
 - Back-end pools
 - Health probes - When the server returns an HTTP response with a status code between 200 and 399, the server is considered healthy.

### Design and IP addressing schema for your Azure deployment

 - Number of possible addresses in a submet (2^n)-5
 - Non routable IP ranges
   - 10.0.0.0 to 10.255.255.255
   - 172.16.0.0 to 172.31.255.255
   - 192.168.0.0 to 192.168.255.255 
 - Smallest subnet use /29 mask - 8 ip addresses
 - Largest subnet use /3 mask - 1073741824 addresses
 - Types of IP addresses
   - Public - For Public facing services
   - Private
 - Allocation methods
   - Dynamic - Assing for the lifespan of the resource. Default method.
   - Static - Static allocation
 - SKUs for public IP
   - Basic
   - Standard
  
Both have by default
 - Inbound originated flow idle timeout 4 min, adjustable upto 30min
 - Fixed outbound idle timeout of 4min
  
Basic SKU

 - Available for static or dynamic allocation
 - Are open, NSG are recommended
 - Available for inbound only traffic
 - Available when using instance meta data service (IMDS)
 - Don't support Availability Zones
 - Don't support routing preferences

Standard SKU

 - Always use static allocation
 - Always secure, thus close to inbound, Must enable by NSG
 - Zero redundant and optional zonal
 - Can assigned to network interfaces, Standard public LB, Applicatoin gateways or VPN gateways
 - Can utilize routing prefreence
 - Can used as anycast forntend IPs for cross-region LB

Public IP address prefix

 - Zone redundant or associated with a specil availability zone
 - Can specify firewall rules for a known range of IP addrsses
 - Use Azure Traffic Manager to balance region-specific instances
 - Can bring own public ip from on prem via Custom address prefix
 - Can't move between regions

Private IP addresses

 - Dynamic ( DHCP lease) - chnage over lifespan of resource
 - Static ( DHCP reservatoin) - Don't change.
 - Internet Assigned Numbers Authority(IANA) IP ranges
    - 10.0.0.0/8
    - 172.16.0.0/12
    - 192.168.0.0/16
 - For subnets first three IP's are reserved by default
 - First and last IP addresses are also reserved
 - .1, .2, .3 aren't visible for configurable

### Distribute your services across Azure virtual networks and integrate them by using Virtual network peering

### Host your domain on Azure DNS

 - Provide name resolution
 - DNS record types
    - A - Host record, maps domain or host name to IP
    - CNAME - Canonical Name record, create an alias from one domain name to another
    - MX - Main exchange record, maps request to mail server
    - TXT - Text record. Use to associate text strings with a domain name.
 - Additional types
    - Wildcards
    - CAA ( Certificate Authority )
    - NS ( Name server )
    - SOA ( Start of Authority )
    - SPF ( Sender policy framework )
    - SRV ( Server locations )
  - SOA and NS records are automatically created when NCS zone is created in Azure DNS
  - Record sets : Assign multiple resources to single record
  - SOA and CNAME can't contain record sets
  - Why Azure DNS
    - Improve security
    - Easy to use
    - Private DNS domains
    - Alias record sets
  - Role based access control
  - Activity logs
  - Resource locking
  - Private zones - Name resolution for VMs within a virtual network. Allow to use own custom domain names rather than azure provided ones
  - Private DNS zones have the following benefits:
    - DNS zones are supported as part of the Azure infrastructure, so there's no need to invest in a DNS solution.
    - All DNS record types are supported: A, CNAME, TXT, MX, SOA, AAAA, PTR, and SRV.
    - Host names for VMs in your virtual network are automatically maintained.
    - Split-horizon DNS support allows the same domain name to exist in both private and public zones. It resolves to the correct one based on the originating request location.
   - Alias record sets - Can point to an Azure resource. Support A, AAAA and CNAME
   - A record - Point to a IP address
   - CNAME - When different domain names need to go to same website
   - Alias records support
     - A Traffic Manager profile
     - Azure Content Delivery Network endpoints
     - A public IP resource
     - A front-door profile
   - Alias records support A, AAAA and CNAME

### Manage and control traffic flow in your Azure deployment with routes

### Improve application scalability and resiliency by using Azure Load Balancer
