# Configure the network for your virtual machines

 ## What is Azure virtual networking?

 Azure virtual networks enable Azure resources to communicate with: each other, users on the internet, and on-premises client computers and provide key networking capabilities:

- Isolation and segmentation
- Internet communications
- Communicate between Azure resources
- Communicate with on-premises resources
- Route network traffic
- Filter network traffic
- Connect virtual networks

## What is an Azure Virtual Network (VNet)?

An Azure Virtual Network (VNet) is a representation of your own network in the cloud. It is a logical isolation of the Azure cloud dedicated to your subscription. 

VNets can be used in many ways

- Create a dedicated private cloud-only VNet. Sometimes you don't require a cross-premises configuration for your solution. When you create a VNet, your services and VMs within your VNet can communicate directly and securely with each other in the cloud. You can still configure endpoint connections for the VMs and services that require internet communication, as part of your solution.

- Securely extend your data center With VNets. You can build traditional site-to-site (S2S) VPNs to securely scale your datacenter capacity. S2S VPNs use IPSEC to provide a secure connection between your corporate VPN gateway and Azure.

- Enable hybrid cloud scenarios. VNets give you the flexibility to support a range of hybrid cloud scenarios. You can securely connect cloud-based applications to any type of on-premises system such as mainframes and Unix systems.

## Subnets 

Subnets provide logical divisions within your network and a VNet can be segmented into one or more subnets. 

Each subnet contains a range of IP addresses that fall within the VNet address space. 

- Each subnet contains a range of IP addresses that fall within the virtual network address space. 
- The range must be unique within the address space for the virtual network.
- The range can't overlap with other subnet address ranges within the virtual network.
- The address space must be specified by using the Classless Inter-Domain routing (CIDR) notation.

## Demo

Let's create two virtual machines, and use a vnet to connect the machines and to internet. 

1. Create a resource group

```sh
    az group create -g vnetdemo-rg -l eastu2
```

2. Create a vnet and a subnet

```sh
    az network vnet create --name MySubnet --resource-group vnetdemo-rg --address-prefixes 10.0.0.0/24
```

3. Create VM1

``` sh
    az vm create -g vnetdemo-rg -l eastus2 --image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest --vnet-name demovnet1 -n vm1 --subnet MySubnet
```

Since we are using a Windows VM, port 3389 is opened automatically when we create it. 

4. Create VM2

``` sh
    az vm create -g vnetdemo-rg -l eastus2 --image MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest --vnet-name demovnet1 -n vm2 --subnet MySubnet
```

5. Remove the public IP from vm2

```sh
    az network nic ip-config update --name ipconfigvm2 -g vnetdemo-rg --nic-name vm2VMNIC --remove PublicIpAddress
```

You can use this command if you need to get the NIC of your VM.

```sh
    az vm nic list -g vnetdemo-rg --vm-name vm2
```

You can use this command if you need to get the ip-config of your nic VM

```sh
    az network nic ip-config list --nic-name vm2VMNIC -g vnetdemo-rg -o table
```