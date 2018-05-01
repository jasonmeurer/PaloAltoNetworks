# Deploy Palo Alto Networks Firewalls to Existing VNET

**This template is not supported by Palo Alto Networks.** 
</br>


[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmattmclimans%2FPaloAltoNetworks%2Fmaster%2Fazure%2Fexisting-environment%2Fadd-multiple-firewalls%2FazureDeploy.json)

### Diagram
This template deploys a selected number of Palo Alto Networks VM-300 Series firewalls into an existing VNET. 
</br>
<p align="center"> 
<img src="https://raw.githubusercontent.com/mattmclimans/tools/master/images/img-3.png">
</p>

### Template Deployments
1.  Palo Alto Networks VM-300 Series Firewall(s)
      - Select between 1-5 firewall(s) to deploy.
      - Select to deploy firewall(s) with public IP on the management interface.
2.  Storage Account
      - Select to deploy Managed Storage, New Unmanaged Storage Account, or Existing Unmanaged storage account.
			- Select storage type for the Firewall(s) OS disk.
3.  Availability Set
      - Firewalls will be placed in an availability set.
			- Select to use new or existing availability set.
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

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Firewall&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Description |
| ------ | ------ |
| Firewall Name | The name of the firewall to deploy.  This name will have a number appended to it.  For example, if the parameter is **AcmeFW** and 3 firewalls are deployed, the three firewalls will have the names **AcmeFW0**, **AcmeFW1**, and **AcmeFW2**. |
| Number Of Firewalls | Indicates how many firewalls to deploy.  Entry must be an integer between 1-5. |
| Virtual Machine Size | Sets the Azure Virtual Machine size for the firewall(s) being deployed.  |
| Image Sku | Sets the license type for the firewalls.  **BYOL** (bring-your-own-license) requires a separate license from Palo Alto Networks.  **Bundle1** (pay-as-you-go license) includes Threat Prevention and Premium Support subscriptions.  **Bundle2** (pay-as-you-go license) includes Threat Prevention, URL Filtering, WildFire, GlobalProtect, and Premium Support subscriptions.|
| Image Version | Sets the PAN-OS version for the firewall(s) being deployed. |
| Admin Username | Sets the local username for the firewall(s).  Do not use admin or administrator. |
| Admin Password | Sets the local password for the username. |

</br>
</br>

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Storage&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description |
| ------ | ------ |
| Storage Account Type | Sets the type of storage account to use for the OS disk(s) of the firewall(s).  **New Unmanaged Storage Account** creates a new storage account in the defined resource group.  **Existing Unmanaged Storage Account** uses an existing storage account in the defined resource group.  **Managed Storage** uses Azure's managed disks. |
| Storage Account Resource Group | Applies only to new/existing unmanaged storage.  If creating a **New Unmanaged Storage Account**, enter the resource group of where you want to deploy the new storage account.  If using an **Existing Unmanaged Storage Account**, enter the name of the resource group that contains it. |
| Storage Account Name | Applies only to unmanaged storage. If creating a **New Unmanaged Storage Account**, this will be the name of the new storage account.  If using an **Existing Unmanaged Storage Account**, enter the name of the existing storage account. |
| Os Disk Storage Type | Sets the storage type for OS disk of the firewall(s). |

</br>
</br>

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Availability&nbsp;Set&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description |
| ------ | ------ |
| New Or Existing Availability Set | Select **Create a new availability set** to create a new availability set for the firewall(s).  Select **Add firewall(s) to existing availability set** to add the firewalls to an existing availability set. |
| Availability Set Name | Enter the name of the availability set to deploy the firewall(s).  If creating a new availability set, enter the name of the new availability set.  If using an existing availability set, enter the name of the existing availability set. |
| Availability Set Update Domain Count | Applies only to a new availability set.  This value sets the update domain count for the new availability set.  Default value: 5 |
| Availability Set Fault Domain Count | Applies only to a new availability set.  This value sets the fault domain count for the new availability set. Default value: 3 |

</br>
</br>

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Network&nbsp;Parameters&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description |
| ------ | ------ |
| Virtual Network Resource Group | Enter the resource group of the existing virtual network. |
| Virtual Network Name | Enter the name of the existing virtual network.  This virtual network is where the firewall(s) will be deployed. |
| Mgmt Subnet Name | Enter the name of the existing management subnet. |
| Untrust Subnet Name | Enter the name of the existing untrust subnet. |
| Trust Subnet Name | Enter the name of the existing trust subnet. |
| Mgmt Nic Name | Sets the name for the firewall(s) private management NIC(s). The name of the firewall(s) will be appended to this parameter.  For example, if the parameter entry is **mgmt-nic** the final NIC name will be **mgmt-nic**[-Firewall Name#] |
| Untrust Nic Name | Sets the name for the firewall(s) private untrust NIC(s). The name of the firewall(s) will be appended to this parameter.  For example, if the parameter entry is **untrust-nic** the final NIC name will be **untrust-nic**[-Firewall Name#]  |
| Trust Nic Name | Sets the name for the firewall(s) private trust NIC(s).  The name of the firewall(s) will be appended to this parameter.  For example, if the parameter entry is **trust-nic** the final NIC name will be **trust-nic**[-Firewall Name#] |
| Apply Public Ip To Mgmt Nic | Select **Yes** to apply a public IP to management NIC(s) of the firewall(s).  Select **No** to omit applying public IP to management NIC(s) of the firewall(s)|
| Mgmt Public Nic Name | Applies only if adding public IP to the management NIC(s).  This value sets public IP name for the management NIC(s) of the firewall(s).  The name of the firewall(s) will be appended to this parameter.  For example, if the parameter entry is **mgmt-public-ip**, the final NIC name will be **mgmt-public-ip**[-Firewall Name#] |
| Mgmt DNS Label Prefix | Applies only if adding public IP to the management NIC(s).  This value sets the DNS name for the firewall(s) management public IP. |

</br>
</br>

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Internal&nbsp;Load&nbsp;Balancer&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Parameters | Description |
| ------ | ------ |
| Create New Internal LB | **Create new internal load balancer** creates a new internal standard load balancer and applies the trust NIC(s) as the backend pool.  **Use existing internal load balancer** uses an existing internal load balancer and applies the trust NIC(s) as the backend pool.  **Do not use load balancer** will omit applying the trust NIC(s) to a load balancer.  |
| Internal LB Name | If **Create new internal load balancer** is selected, enter the name of the new internal standard load balancer.  If **Use existing internal load balancer** is selected, enter the name of the existing internal load balancer. |
| Internal LB Frontend Name | Applies only if creating a new internal load balancer. This parameter sets the new internal load balancer's front end name. |
| Internal LB Subnet Name | Applies only if creating a new internal load balancer.  This parameter must be the name of an existing subnet and this subnet's CIDR can hold the Internal LB Frontend IP.  |
| Internal LB Frontend IP | Applies only if creating a new internal load balancer.  This parameter is the internal load balancer's frontend IP address. |
| Internal LB Backend Pool Name | Applies only if creating a new internal load balancer.  This parameter sets the name of the new internal load balancer's backend pool. |
| Internal LB Probe Name | Applies only if creating a new internal load balancer.  This parameter sets the name of the health probe. |
| Internal LB Probe Port | Applies only if creating a new internal load balancer.  This is the port the health probe will use to monitor the health of the firewall(s).  Default: 80 |
| Internal LB Rule Name | Applies only if creating a new internal load balancer.  This parameter sets the internal load balancer's Load Balancing rule name.  This rule leverages HA ports. |

</br>
</br>

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Public&nbsp;Load&nbsp;Balancer&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Parameters| Description |
| ------ | ------ |
| Create New Public LB | **Create new public load balancer** creates a new internal standard load balancer and applies the trust NIC(s) as the backend pool.  **Use existing public load balancer** uses an existing internal load balancer and applies the trust NIC(s) as the backend pool.  **Do not use load balancer** will omit applying the trust NIC(s) to a load balancer. |
| Public LB Name | If Create-New-LB is selected, enter the name of the new internal load balancer.  If Use-Existing-LB is selected, enter the name of the existing internal load balancer.  | 
| Public LB Frontend Name | Applies only if creating a new public load balancer.  This parameter sets the frontend name for the new public load balancer. |
| Public LB Public Ip Name | Applies only if creating a new public load balancer.  This parameter sets the name of the public IP address associated with the new public load balancer. |
| Public LB Backend Pool Name |Applies only if creating a new public load balancer.  This parameter sets the name of the new public load balancer's backend pool.  |
| Public LB Health Probe Name | Applies only if creating a new public load balancer.  This parameter sets the health probe's name for the new public load balancer. |
| Public LB Health Probe Port | Applies only if creating a new public load balancer.  This parameter sets the port the health probe will use to monitor the health of the firewall(s) untrust NICs. |
| Public LB Rule Name | Applies only if creating a new public load balancer.  This parameter sets the new public load balancer's first load balancing rule. |
| Public LB Rule Frontend Port | Applies only if creating a new public load balancer.  This parameter sets the load balancing rule's frontend port. |
| Public LB Rule Backend Port | Applies only if creating a new public load balancer.  This parameter sets the  load balancing rule's backend port. |
