# Multicloud-VPN-AWS-AZURE
(Site to Site VPN) How to create a VPN between Azure and AWS.
In this Article I have gone through (Site to Site VPN) How to create a VPN between Azure and AWS.

The procedure is as follows.

Azure side
> Create virtual network
> Create gateway subnet
> creation of public IP
> Create virtual network gateway

AWS side
> creation of VPC
> Create subnet
> Create Internet gateway (optional)
> create the customer gateway statically
> Creating Virtual Private Gateway
> create a VPN connection statically
> download the configuration file

Azure side
> Create a local network gateway
> Create connection

AWS side
> add a virtual private gateway to the routing table

option
Azure side
> Setting up two connections


CONFIGURATION STEPS

1. Crate a resource group on Azure to deploy the resources on that
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/644a1718-4468-4da7-a735-a7dfe6f43a43)

