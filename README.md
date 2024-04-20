# Multicloud-VPN-AWS-AZURE
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

2. Create a Virtual Network, subnet and Gateway subnet
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/b4fd1282-7baf-4eaa-bea0-d51bf5361c5d)
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/646ac098-9291-43cc-bcc8-6a3a2c2ea823)

3. Create the VPN Gateway
   ![Screenshot 2024-04-20 130151](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/3dd9ff6b-0b8e-4a0b-9de2-ffddacce3357)

Configuring AWS

4. Create the Virtual Private Cloud (VPC)
   ![Screenshot 2024-04-20 130602](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/ee79feab-eaa7-4335-9329-35d02c386448)

5. Create a subnet inside the VPC (Virtual Network)
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/c0813861-93cf-4b97-8c82-4efb57616857)

6. Create a customer gateway pointing to the public ip address of Azure VPN Gateway.
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/a919a118-421d-4569-b464-6633cc6f2ed4)

7. Create the Virtual Private Gateway then attach to the VPC
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/6a087ec6-2815-4de5-a88a-59b5392ce65e)

8. Create a site-to-site VPN Connection
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/bd4f692f-edbc-4f58-a1b9-689310fc11e7)
   NOTE: Set the routing as static pointing to the azure subnet-01 prefix (172.10.1.0/24)

9. Download the configuration file
   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/17755aad-6b4f-4410-ac41-ad761d77b6a8)
   
 Adding the AWS information on Azure Configuration

10. Now let’s create the Local Network Gateway
    The Local Network Gateway is an Azure resource with information to Azure about the customer gateway device, in this case the AWS Virtual Private Gateway
    ![Screenshot 2024-04-20 131842](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/c1335095-8186-4b6e-bcfe-7bfe4ff84768)

    Now you need to specify the public ip address from the AWS Virtual Private Gateway and the VPC CIDR prefix.
    Please note that the public address from the AWS Virtual Private Gateway is described at the configuration file you have downloaded.
    As mentioned earlier, AWS creates two IPSec tunnels to high availability purposes. I’ll use the public ip address from the IPSec Tunnel #1 for now.

11. Then let’s create the connection on the Virtual Network Gateway
    ![Screenshot 2024-04-20 132304](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/fe956ac0-9284-4532-a669-231285c78933)

   You should fill the fields according below. Please note that the Shared key was obtained at the configuration file downloaded earlier and In this case, I’m using the Shared Key for the Ipsec tunnel #1 created by AWS 
   and described at the configuration file.

   ![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/b22acf45-ba32-409c-908f-a343d1d14014)

   IMPORTANT NOTE: Whiile you configurin the connection may be you cann't see the pre-shared key option. In this senario you can create deafult level and edit the preshared key using azure cli. Please refer the link and 
   change the preshared key.
   https://learn.microsoft.com/en-us/cli/azure/network/vpn-connection/shared-key?view=azure-cli-latest

After a few minutes, you can see the connection established:

![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/28cf1cda-0ee1-4c88-bf73-37cc5af76240)

In the same way, we can check on AWS that the 1st tunnel is up:

![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/ed39c901-b638-4aee-8bc5-9f993f453d67)

Now let’s edit the route table associated with our VPC
And add the route to Azure subnet through the Virtual Private Gateway:

![image](https://github.com/Sanjaypramod/Multicloud-VPN-AWS-AZURE/assets/86740453/6ec0e63c-6ed8-4688-b7ca-cda978a98c5d)

12. If you need high availabilty you can create another tunnel for the same.
    Now we can create a 2nd connection to ensure high availability. To do this let’s create another Local Network Gateway which we will point to the public ip address of the IPSec tunnel #2 on the AWS.
    Then we can create the 2nd connection on the Virtual Network Gateway.
    In a few moments, you can check the boths cloud tunnel connetions.

13. Let’s test!
    Spin up a machine on both cloud sides and check the ping connection using the private IP.


THANKS FOR THE READING.










    







