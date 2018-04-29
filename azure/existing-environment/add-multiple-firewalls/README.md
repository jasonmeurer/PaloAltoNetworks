# Deploy Palo Alto Networks Firewalls to Existing VNET


[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmattmclimans%2FPaloAltoNetworks%2Fmaster%2Fazure%2Fexisting-environment%2Fadd-multiple-firewalls%2FazureDeploy.json)

This template deploys a selected number of Palo Alto Networks VM-300 Series firewalls into an existing VNET. Internal Standard Load Balancer (with HA Ports) to an existing set Palo Alto Networks firewalls.  This template must be deployed through PowerShell or through the Azure CLI. Do not deploy this template through the Azure console (see this [guide](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-deploy) for more information).
</br>
<p align="center"> 
<img src="https://github.com/mattmclimans/tools/blob/master/images/img-2.png">
</p>

### Template Deployments
1.  Palo Alto Networks VM-300 Series Firewall(s)
      - Select the number of firewall(s) to deploy (1-5).
2.  Storage Account
      - Select to deploy Managed Storage, New Unmanaged Storage Account, or Existing Unmanaged storage account.
			- Select storage type for the Firewall(s) OS disk.
3.  Availability Set
      - Firewalls will be placed in an availability set.
			- Select whether to deploy into new availability set or deploy into an existing availability set.
4.  Internal Load Balancer
      - Select to deploy a new internal load balancer, use an existing internal load balancer, or do not use an internal load balancer.
			- If deploying a new internal load balancer, the load balancer will use Azure's Standard LB with HA Ports.
5.  Public Load Balancer
      - Select to deploy a new public load balancer, use an existing public load balancer, or do not use a public load balancer.

### Template Pre-requisites
- Existing VNET with subnets for Mgmt, Untrust, and Trust subnets.
- Trust NICs on the Palo Alto Networks firewalls for the internal load balancer's backend pool.
- Existing VNET with subnet to deploy the internal load balancer.

### Template Parameters
The table below describes each parameter in detail.

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Firewall&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Description |
| ------ | ------ |
| **Firewall Name** | The name of the firewall to deploy.  This name will have a number appended to it.  For example, if deploying two firewalls and the firewall name is Palo-Alto, the first firewall will be named Palo-Alto0, second firewall will be named Palo-Alto1. |
| **Number Of Firewalls** | This number indicates how many firewalls to deploy.  Entry must be an integer between 1-5. |
| **Virtual Machine Size** | Sets the Azure Virtual Machine size for the firewalls.  |
| **Image Sku** | Sets the license for the firewalls.  BYOL ("Bring your own license") requires a license from Palo Alto Networks.  Bundle1, is a pay-as-you-go license that contains Threat Prevention and Premium Support subscriptions.  Bundle2, is a pay-as-you-go license that contains Threat Prevention, URL Filtering, WildFire, GlobalProtect, and Premium Support subscriptions.|
| **Image Version** | Sets the PAN-OS the firewalls will be deployed with.  The current PAN-OS 8.0 recommended release is PAN-OS 8.0.7 (as of 04/25/18) |
| **Admin Username** | Sets the username for the firewall(s).  Do not use admin or administrator. |
| **Admin Password** | Sets the password the admin username entered. |

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Storage&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description |
| ------ | ------ |
| **Storage Account Type** | Sets the type of storage account to use for the Firewall's OS disks.  New-Unmanaged-Storage-Account creates a new storage account.  Existing-Unmanaged-Storage-Account uses an existing storage account.  Managed-Storage-Account uses Azure's managed storage disks. |
| **Storage Account Resource Group** | Used only if using Unmanaged disks.  If creating a New-Unmanaged-Storage-Account, enter the name of the resource group to deploy it to.  If adding OS disks to an Existing-Unmanaged-Storage-Account, enter the resource group of the the existing storage account. |
| **Storage Account Name** | Used only if using Unmanaged disks.  If creating a New-Unmanaged-Storage-Account, this will be the name of the new storage account.  If using an Existing-Unmanaged-Storage-Account, enter the name of the existing storage account. |
| **Os Disk Storage Type** | Sets the disk storage type for the firewall's OS. |

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Availability&nbsp;Set&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description |
| ------ | ------ |
| **New Or Existing Availability Set** | Selects to deploy firewall(s) to a new or existing availability set. |
| **Availability Set Name** | Enter the name of the availability set to deploy the firewall(s).  If creating a new availability set, enter the name of the new availability set.  If using an existing availability set, enter the name of the existing availability set. |
| **Availability Set Update Domain Count** | Applies only if creating a new availability set.  This value sets the update domain count for the new availability set.  Default value: 5 |
| **Availability Set Fault Domain Count** | Applies only if creating a new availability set.  This value specifies the fault domain count for the new availability set. Default value: 3 |
| **Mgmt Public Nic Name** | Applies only if adding public IP to the management interface.  This value sets the name of the public IP name for the firewall(s) management interface. |

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Network&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description |
| ------ | ------ |
| **Mgmt DNS Label Prefix** | Applies only if adding public IP to the management interface.  This value sets the DNS name for the firewall(s) management public IP. |
| **Mgmt Nic Name** | Sets the name for the firewall(s) private management NIC. |
| **Untrust Nic Name** | Sets the name for the firewall(s) private management NIC. |
| **Trust Nic Name** | Sets the name for the firewall(s) private management NIC. |
| **Virtual Network Resource Group** | Enter the resource group of the virtual network. |
| **Virtual Network Name** | Enter the name of the EXISTING virtual network.  This virtual network is where the firewall(s) will be deployed. |
| **Mgmt Subnet Name** | Enter the name of the EXISTING management subnet. |
| **Untrust Subnet Name** | Enter the name of the EXISTING untrust subnet. |
| **Trust Subnet Name** | Enter the name of the EXISTING trust subnet. |

| &nbsp;&nbsp;&nbsp;&nbsp;Internal&nbsp;Load&nbsp;Balancer&nbsp;&nbsp;&nbsp;&nbsp; Parameters | Description |
| ------ | ------ |
| **Create New Internal LB** | "Create-New-LB" creates a new internal standard load balancer.  "Use-Existing-LB" adds the firewall(s) to an existing internal load balancer.  "Do-Not-Use-LB" does not add the firewall(s) to an internal load balancer. |
| **Internal LB Name** | If Create-New-LB is selected, enter the name of the new internal load balancer.  If Use-Existing-LB is selected, enter the name of the existing internal load balancer. |
| **Internal LB Frontend Name** | Applies only if Create-New-LB is selected.  This value sets the new internal load balancer's front end name. |
| **Internal LB Subnet Name** | Applies only if Create-New-LB is selected.  This value must be the name of an existing subnet and this subnet's CIDR can hold the Internal LB Frontend IP.  |
| **Internal LB Frontend IP** | Applies only if Create-New-LB is selected.  This value is the internal load balancer's frontend IP address. |
| **Internal LB Backend Pool Name** | Applies only if Create-New-LB is selected.  This value sets the name of the new internal load balancer's backend pool. |
| **Internal LB Probe Name** | Applies only if Create-New-LB is selected.  This value sets the name of the health probe. |
| **Internal LB Probe Port** | Applies only if Create-New-LB is selected.  This is the port the health probe will use to monitor the health of the firewall(s).  Default: 80 |
| **Internal LB Rule Name** | Applies only if Create-New-LB is selected.  This value sets the internal load balancer's Load Balancing rule name.  This rule leverages HA ports. |

| &nbsp;&nbsp;&nbsp;Public&nbsp;Load&nbsp;Balancer&nbsp;Parameters&nbsp;&nbsp;&nbsp;| Description |
| ------ | ------ |
| **Create New Public LB** | "Create-New-LB" creates a new public load balancer.  "Use-Existing-LB" adds the firewall(s) to an existing public load balancer.  "Do-Not-Use-LB" does not add the firewall(s) to a public load balancer. |
| **Public LB Name** | If Create-New-LB is selected, enter the name of the new internal load balancer.  If Use-Existing-LB is selected, enter the name of the existing internal load balancer.  | 
| **Public LB Frontend Name** | Applies only if Create-New-LB is selected.  This value sets the frontend name for the new public load balancer. |
| **Public LB Public Ip Name** | Applies only if Create-New-LB is selected.  This value sets the name of the public IP address associated with the new public load balancer. |
| **Public LB Backend Pool Name** | Applies only if Create-New-LB is selected.  This value sets the name of the new public load balancer's backend pool.  |
| **Public LB Health Probe Name** | Applies only if Create-New-LB is selected.  This value sets the health probe's name for the new public load balancer. |
| **Public LB Health Probe Port** | Applies only if Create-New-LB is selected.  This value sets the port the health probe will use to monitor the healh of the firewall(s) untrust NICs. |
| **Public LB Rule Name** | Applies only if Create-New-LB is selected.  This value sets the new public load balancer's first load balancing rule. |
| **Public LB Rule Frontend Port** | Applies only if Create-New-LB is selected.  This value sets first load balancing rule's frontend port. |
| **Public LB Rule Backend Port** | Applies only if Create-New-LB is selected.  This value sets the first load balancing rule's backend port. |

### Diagram
The diagram below illustrates the features that can be deployed through this template.
