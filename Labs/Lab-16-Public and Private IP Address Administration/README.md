
# azure-admin-labs
AZ-104 lab portfolio | identities, networking, compute, storage, monitoring, governance (scripts, screenshots, cleanup)

# Lab 16 - Azure Public and Private IP Address Administration

## Goal
Manage and validate public and private IP addresses in Azure, configure a Linux virtual machine, test network connectivity, and establish a secure SSH connection from my Windows computer.

- To understand the difference between public and private IP addresses.
- To investigate how a public IP, network interface, and Network Security Group work together.
- To understand SSH authentication, private keys, and host-key fingerprints.
- To troubleshoot connection problems and document how I resolved them.

## What I Did

### Exercise 1 - Creating and Configuring the Azure VM
- Continued working with my existing resource group **rg-az104-networking**.
- Used my existing Virtual Network **vnet-contoso-lab**.
- Created and configured an Ubuntu Linux virtual machine **vm-ip-lab01**.
- Worked with a network interface, public IP address, private IP address, Network Security Group, and SSH key.
- Investigated deployment problems and existing network resources.
- Confirmed that the VM was running and ready for connection.

## Network Configuration
Virtual Network: **vnet-contoso-lab**
- Resource Group: **rg-az104-networking**
- VM Name: **vm-ip-lab01**
- Operating System: **Ubuntu Linux**
- Public IP Address: **20.84.109.43**
- Private IP Address: **10.20.1.5**
- Linux Administrator: **azureuser**
- SSH Port: **22**
- Authentication Method: **SSH Private Key**
- Private Key File: **vm-ip-lab01_key.pem**
- computer Private IP: **Restricted**

## IP Address Validation and Troubleshooting

### Test 1 - Identifying My Public IP Address
I wanted to know which public IP address my computer was using to connect to the internet.
I opened:

https://api.ipify.org

**Result:** The website displayed the public IP address of my internet connection.

The public IP observed during the exercise was **Restricted**.

**Technical Explanation:**
- A public IP address is used for communication over the internet. A private IP address is used within a private network.
- got the private IP address of my laptop by running:
- Test-NetConnection 20.84.109.43 -Port 
- Another way one could check the private IP is to run **ipconfig** on powershell
- The Azure VM had a private IP address of 10.20.1.5.

- I also learned that adding /32 to an IPv4 address identifies one specific address.

For example: **Restricted**/32

This is useful when restricting SSH access to a trusted source IP address.

### Test 2 - Configuring Network Security Group Access
I investigated how to allow my computer to connect to the Azure VM through SSH.

Configuration:
- Direction: **Inbound**
- Source: **My public IP address /32**
- Protocol: **TCP**
- Destination Port: **22**
- Action: **Allow**

**Result:** The NSG configuration allowed SSH traffic from the permitted source address.

**Technical Explanation:**
A Network Security Group controls inbound and outbound network traffic using security rules.
- Port 22 is the standard port used by SSH.

Allowing traffic through an NSG does not automatically mean that a user can log in. The correct SSH username and authentication key are still required.

### Test 3 - Investigating VM Deployment and Public IP Problems
During the VM configuration, I encountered deployment and networking problems.

I investigated the resources created in the resource group and checked the public IP and network interface associations.

**Problem:** A public IP address was already associated with another network interface.

**Result:** I investigated the existing association and corrected the network configuration so that the VM could use the required public IP.

**Technical Explanation:**
A network interface connects a VM to an Azure Virtual Network.

The private IP address is assigned through the network interface. A public IP can also be associated with the interface's IP configuration.

A public IP cannot be associated with two different network interface IP configurations at the same time.

I also learned that a failed Azure deployment can leave resources behind. These resources must be checked before trying another deployment.

### Test 4 - Installing OpenSSH Client on Windows
I wanted to connect to the Azure VM directly from my Lenovo computer.

Initially, the SSH command was not working because OpenSSH Client was not available in PowerShell.

I opened Windows Settings, checked Optional Features, installed OpenSSH Client, and restarted the computer.

After restarting, I ran:

    ssh -V

**Result:** PowerShell displayed OpenSSH_for_Windows_9.5p2.

**Technical Explanation:**
SSH stands for Secure Shell. It allows me to connect securely to another computer over a network.

OpenSSH Client is needed on my computer because my computer is initiating the connection to the Azure Linux VM.

The -V option displays the installed SSH version.

### Test 5 - Testing Network Connectivity
I wanted to confirm whether my computer could reach the Azure VM on SSH port 22.

I opened Windows PowerShell and ran:

    Test-NetConnection 20.84.109.43 -Port 22

**Result:**

    TcpTestSucceeded : True

**Technical Explanation:**
Test-NetConnection checks network connectivity to a destination.

20.84.109.43 was the public IP address of my Azure VM.

-Port 22 instructed PowerShell to test TCP port 22.

The result True confirmed that my computer could establish a TCP connection to the VM on port 22.

However, it did not confirm that my SSH username or private key was correct.

### Test 6 - Checking the SSH Service on Linux
I wanted to confirm that the SSH service was running on the Azure VM.

I opened Azure Portal:

Virtual Machine > Run Command > RunShellScript

I ran:

    sudo systemctl status ssh --no-pager

**Result:**

    Active: active (running)

**Technical Explanation:**
The systemctl command is used to manage and inspect Linux services.

The status ssh command checks the SSH service.

The --no-pager option displays the output directly.

This confirmed that the SSH service was running on the VM.

### Test 7 - Investigating SSH Host-Key Fingerprints
During my first SSH connection attempt, PowerShell displayed a warning about the authenticity of the remote host.

I had not encountered an SSH fingerprint before, so I investigated what it meant.

The warning included information similar to:

    The authenticity of host '20.84.109.43' can't be established.
    ED25519 key fingerprint is SHA256:...
    Are you sure you want to continue connecting?

**Technical Explanation:**
An SSH fingerprint is a digital identifier calculated from a cryptographic key.

It is not a physical fingerprint like the one on my hand.

The fingerprint helps identify the SSH host key presented by the remote server.

ED25519 is a type of cryptographic key, while SHA256 is used to calculate the fingerprint.

I learned that:
- The server's host key helps identify the remote computer.
- The fingerprint is a shorter representation of that key.
- The known_hosts file stores trusted SSH host-key information.
- An unexpected change in a server's host key should be investigated.
- Accepting a host key without independently verifying it does not guarantee that the server is genuine.

To inspect the SSH files on the VM, I used Azure Run Command:

    sudo ls -la /etc/ssh/

I found SSH host-key files, including:

    ssh_host_ed25519_key.pub
    ssh_host_rsa_key.pub

The command for displaying the ED25519 host-key fingerprint is:

    sudo ssh-keygen -l -f /etc/ssh/ssh_host_ed25519_key.pub

**Result:** I confirmed that SSH host-key files existed on the VM and learned how their fingerprints can be checked.

I also learned that the server's host key is different from my downloaded private SSH key.

The host key helps me identify the server, while my private key helps the server authenticate me.

### Test 8 - Locating My SSH Private Key
I encountered problems because the SSH command could not find the private key file.

I needed to confirm the actual filename and location.

I opened PowerShell and ran:

    Get-ChildItem "$HOME\Downloads" -File | Select-Object Name

**Result:** I found the private key file:

    vm-ip-lab01_key.pem

**Technical Explanation:**
Get-ChildItem lists files and folders.

$HOME refers to my Windows user directory.

-File limits the results to files.

The pipe symbol | passes the result to another command.

Select-Object Name displays the filenames.

I learned that I must use the correct filename and location when specifying an SSH private key.

### Test 9 - Investigating SSH Authentication Errors
My initial SSH connection attempts were unsuccessful.

I first tried connecting using the VM name as the username:

    ssh vm-ip-lab01@20.84.109.43

**Result:**

    Permission denied (publickey)

**Technical Explanation:**
The VM name and the Linux administrator username are not necessarily the same.

The VM name was vm-ip-lab01, but the correct Linux administrator username was azureuser.

I confirmed the username using Azure Portal under the VM connection settings.

I also needed to specify the correct private key file.

This taught me that network connectivity and SSH authentication are two different things.

### Test 10 - Successful SSH Connection
After confirming the correct username and private key, I ran:

    ssh -i "$HOME\Downloads\vm-ip-lab01_key.pem" azureuser@20.84.109.43

**Result:** The connection was successful.

PowerShell displayed the Ubuntu welcome message and the remote Linux terminal:

    azureuser@vm-ip-lab01:~$

**Technical Explanation:**
ssh starts a secure remote connection.

-i specifies the private key file used for authentication.

$HOME\Downloads\vm-ip-lab01_key.pem was the location of my private key.

azureuser was the Linux administrator username.

20.84.109.43 was the public IP address of the Azure VM.

The Linux terminal prompt confirmed that I had successfully connected from my Windows computer to the Azure Ubuntu VM.

Once connected, I could use Linux commands such as:

    hostname

To close the remote SSH connection, I could use:

    exit

## Troubleshooting Summary

| Problem | Cause | Solution |
|---|---|---|
| VM deployment problem | Part of the deployment failed | Investigated the deployment and existing resources |
| Public IP association problem | Public IP already associated with another NIC | Checked and corrected the network association |
| SSH command not recognized | OpenSSH Client unavailable | Installed OpenSSH Client and restarted Windows |
| Connection timed out | SSH connection received no response | Investigated the public IP, NSG, port 22, and SSH service |
| Host authenticity warning | Server host key was not yet trusted | Investigated SSH host-key fingerprints |
| Private key not found | Incorrect key filename or location | Located the correct PEM file in Downloads |
| Permission denied (publickey) | SSH authentication was rejected | Used the correct username and private key |
| No such file or directory | Incorrect Linux file path | Listed the SSH directory to find the correct filename |

## Operational Considerations

In a production environment, I would also review:

- Public and private IP address assignments.
- Whether public IP addresses are static or dynamic.
- NSG inbound and outbound security rules.
- Restricting SSH access to trusted source IP addresses.
- SSH private key security and access permissions.
- Host-key fingerprint verification.
- VM network interface and public IP associations.
- Network connectivity and authentication troubleshooting.
- Azure resource costs and unused resources.
- Resource cleanup after completing temporary exercises.

## Resource Cleanup

After completing the lab, I started deleting the temporary Azure resources.

These included the VM and resources created specifically for the SSH exercise, such as its network interface, disk, public IP, SSH key resource, and lab-only NSGs.

I kept the existing Virtual Network **vnet-contoso-lab** for future networking labs.

I also learned that deleting an Azure SSH key resource does not automatically delete the private PEM file stored on my computer.

The private key must remain secure and must not be uploaded to GitHub.

## Conclusion

This lab helped me understand how public and private IP addresses work in Azure and how they are used to connect to a virtual machine. I configured and investigated network resources, tested connectivity from my Windows computer, and learned how SSH keys and host-key fingerprints work.

I encountered several errors, including deployment problems, public IP association issues, connection timeouts, missing private key files, and SSH authentication failures. By checking the network configuration, SSH service, username, and private key, I was able to resolve the connection problems.

The most important achievement was successfully connecting from my computer to the Azure Ubuntu VM using SSH.

The main lesson is that network connectivity, server identity verification, and user authentication are separate parts of a secure remote connection.

## References

- [Microsoft Learn - Public IP Addresses](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses)
- [Microsoft Learn - Private IP Addresses](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/private-ip-addresses)
- [Microsoft Learn - Network Security Groups](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)
- [Microsoft Learn - Connect to Linux VMs with SSH](https://learn.microsoft.com/en-us/azure/virtual-machines/linux-vm-connect)
- [Microsoft Learn - Test-NetConnection](https://learn.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection)
- [Official AZ-104 Study Guide](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/az-104)
- [Official AZ-104 Hands-on Labs](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/master/Allfiles/Labs)

## Lab Screenshots and Results

- [Azure VM Overview](screenshots/vm-overview.png)
- [Public and Private IP Addresses](screenshots/ip-addresses.png)
- [Network Security Group](screenshots/network-security-group.png)
- [OpenSSH Client Verification](screenshots/openssh-version.png)
- [Network Connectivity Test](screenshots/test-netconnection.png)
- [SSH Service Status](screenshots/ssh-service-status.png)
- [SSH Private Key File](screenshots/ssh-private-key.png)
- [SSH Authentication Error](screenshots/ssh-authentication-error.png)
- [Successful SSH Connection](screenshots/ssh-success.png)
- [Resource Cleanup](screenshots/resource-cleanup.png)
