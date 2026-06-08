# azure-admin-labs
az-104 lab portfolio: identity, networking, compute, storage, monitoring, governance (scripts, screenshots, cleanup)
# Lab 13 -Azure DNS and Traffic Manager Lab Progress

## Goal 
Build a hybrid-style DNS lab using:
- Azure Private DNS Zones
- Autoregistration
- VNet links
- Azure DNS Private Resolver
- Forwarding rulesets

## What I did
## Build a hybrid-style Azure DNS lab using a hub-and-spoke network architecture
- Created three virtual networks:
    - Hub-VNet-10.0.0.0/16
    - Spoke-APP-10.1.0.0/16
    - Spoke-Data-10.2.0.0/16

- Created the required subnets for the Azure DNS Private Resolver inside Hub-VNet:
    - DnsInboundSubnet - 10.0.1.0/28
    - DnsOutboundSubnet - 10.0.2.0/28

- Configured VNet peering between the hub and both spoke networks:
    - Hub-Vnet ~ Spoke-App
    - Hub-VNet ~ Spoke-Data

- Deployed three virtual machines for DNS testing:
    - One Windows VM in Spoke-App
    - One Linux VM in Spoke-App
    - One Linux VM in Spoke-Data

## Private DNS Zone and Autoregistration
- Created the Azure Private DNS zone corp.internal
- Linked corp.internal to Spoke-App with autoregistration enabled
- Linked corp.internal to Spoke-Data with autoregistration disabled
- Verified that Azure automatically created DNS records for the Windows and Linux VMs in Spoke-App
- Confirmed that the LinuxVM in Spoke-Data did not receive an automatic DNS record because autoregistration was disabled for that VNet link
- Created a manual A record for the Linux in Spoke-Data
- Used nslookup from the virtual machines to test private DNS resolution between the spoke networks

## Storage Account and Private Endpoint DNS
- Created an Azure Storage Accont
- Created a Private Endpoint for the Storage Account inside Spoke-Data
- Configured the private DNS zone private.blob.core.windows.net
- Linked the private DNS zone to the required virtual networks
- Tested the Storage Account FQDN from a VM in Spoke-App
- Verified that the Storage Account resolved to the Private Endpoint IP address instead oa a public IP address

## Azure DNS Private Resolver
- Deployed Azure DNS Private Resolver inside Hub-VNet
- Created an inbound endpoint in DnsInboundSubnet
- Created an outbound endpoint in DnsOutboundSubnet
- Created a DNS forwarding ruleset
- Added a forwarding rule for the domain suffix lab.partner.local
- Linked Ms in both spoke networks using nslookup
- Removed the ruleset link from Spoke-Data and repeated the test to observe the effect of the missing VNet link

## DNS Resolution Behaviour
- Confirmed tht queries for corp.internal were answered by the linked Azure Private DNS zone
- Confirmed that queries for lab.partner.local were proessed through the DNS forwarding ruleset
- Observed the difference between DNS records created automatically through autoregistration and records created manually
- Verified that DNS resolution defends on the DNS suffix, zone links, ruleset links, and the resources connected to each virtual network 



## Evidence
- ![](screenshots/RG3.png)
- ![](screenshots/RG2.png)
- ![](screenshots/RG1.png)
- ![](screenshots/VNets.png)
- ![](screenshots/SpokeApp_VNet.png)
- ![](screenshots/Spokedata_VNet.png)
- ![](screenshots/Hub_VNet_Topology.png)
- ![](screenshots/VMs.png)
- ![](screenshots/WindowVM_connect.png)
- ![](screenshots/ConnectLinux_viaSSH.png)
- ![](screenshots/LinuxVM_Spoke.png)

- ![](screenshots/private_dnszone_setup.png)
- ![](screenshots/private_dnszones.png)
- ![](screenshots/corp.internal.png)
- ![](screenshots/SpokeVMDNS.png)

- ![](screenshots/storageAccount.png)
- ![](screenshots/privateLink_blobStorageEndpoint.png)
- ![](screenshots/storageFQDNto_Endpoint.png)

- ![](screenshots/DNSResolver.png)
- ![](screenshots/ForwardedDNSresolution_Linux.png)
- ![](screenshots/private_dnsresolution_fromLinux.png)

- ![](screenshots/peering.png)
- ![](screenshots/Securityrules_Windows.png)
