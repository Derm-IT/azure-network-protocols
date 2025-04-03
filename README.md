# Azure Virtual Machines and Network Traffic Analysis

## Overview

This project documents my hands-on experience in deploying and configuring virtual machines (VMs) within the Azure cloud environment and analyzing network traffic using Wireshark. The project covers VM creation, basic network troubleshooting, firewall configuration, and the observation of various network protocols (ICMP, SSH, DHCP, DNS, RDP). This project demonstrates foundational cloud and networking skills.

## Prerequisites

* An active Azure subscription.
* Basic understanding of networking concepts (IP addressing, TCP/IP, ICMP, SSH, DHCP, DNS, RDP).
* Familiarity with remote desktop tools (Microsoft Remote Desktop, etc.).
* Basic command-line proficiency (PowerShell).

## Steps

### Part 1: Virtual Machine Creation

1.  **Create a Resource Group:**
    * Logged into the Azure portal and created a new Resource Group called <b>rg-main</b> to organize the VMs.
    * Set region to Canada Central
2.  **Create a Windows 10 Virtual Machine (VM):**
       ![image](https://github.com/user-attachments/assets/a43e7bc3-962f-4f10-a74f-30c5291aee99)
    * Created a Windows 10 VM in Azure
        *  Selected the previously created Resource Group, <b>rg-main</b>
        *  Named the VM "main-windows-vm"
        *  Set region to Canada Central (Same as RG)
        *  Chose image to be windows 10
        ![image](https://github.com/user-attachments/assets/24f9bc60-4d53-4189-b585-3bee1dd4372c)
        * Chose size as a standard 2vcpus and 8GiB of memory (enough for this project)
        * Set User and Password
        * Confirm licensing ***
        ![image](https://github.com/user-attachments/assets/ceed6e66-24da-4664-af09-ca559c0fc776)
        * Leave Disks default and head to Networking Tab
        * CREATE a new virtual network (to use for all VMs) , main-vnet
        * Leave everything else default, especially RDP Port (needed for accessing VM during testing)
        ![image](https://github.com/user-attachments/assets/ca669a08-6c96-4d6c-8605-af1ddb749040)
        * Skip to Review + Create
        * Click Create
        * Wait for Virtual Network to appear and move on
        ![image](https://github.com/user-attachments/assets/e4267407-0751-4575-a875-4532859e6fec)
    * Allowed Azure to create a new Virtual Network (VNet) and Subnet.
    * (Add screenshot of Windows 10 VM creation)
3.  **Create a Linux (Ubuntu) Virtual Machine (VM):**
    * Deploy an Ubuntu VM, ensuring it was placed in the same Resource Group and Virtual Network as the Windows 10 VM.
        * Create a new VM, name it (ex. main-linux-vm), select the same RG an region and choose the image to be Ubuntu 22 this time
        * Leave the size the same and set user and password (easiest to do the same for testing)
        * Same as before leave everything else before and skip to Networkng tab and choose same VNET as before
        ![image](https://github.com/user-attachments/assets/cf2624aa-cc9d-496b-b993-99e7ffb02d1b)
        * Select the vnet and select review and create and then create after validation
        ![image](https://github.com/user-attachments/assets/6779c119-c071-4cf7-9b51-75674c57d225)
    * Linux Virtual Machine Deployed
  
4. BOTH Virtual Machines should now appear on Virtual Machines in Azure
   ![image](https://github.com/user-attachments/assets/994567f9-039c-4d06-b2f2-bceda8cd070c)
5.  **Verify Network Connectivity:**
    * Search Virtual Networks in Azure
    ![image](https://github.com/user-attachments/assets/9dc86de6-62fe-4b94-acbe-e8d1911b77e1)
    * Select your VNET (main-vnet) -> Settings -> Connected Devices
    * Should see both VM's netowrk interface to verify both VMs are under the same VNET
    ![image](https://github.com/user-attachments/assets/5eb3c0ca-3444-4dc0-916b-c3a447f539ef)


### Part 2: Network Traffic Observation
|  Goal here is to check connectivity between the two VMs and view network traffic with Wireshark
1.  **Connect to your Windows 10 VM**
    * Retrieve the public IP of your Windows 10 VM 
        * Go to the Virtual Machines page and copy the Windows VM (main-windows-vm) Public IP
        ![image](https://github.com/user-attachments/assets/58151b26-3b58-40a0-bd20-4f2b3891c717)
        * On your Windows Computer (NOT VM) open Remote Desktop Connect or on Mac install and open Microsoft Remote Desktop
        * Enter the public IP address (if you can't see the form click show options) 
        ![image](https://github.com/user-attachments/assets/aecc5563-61d9-40e7-a3fb-082d3c5a5211)
        * Click connect and enter the user and password you chose when setting up the Windows VM (make sure to click more choices and use a different account)
        ![image](https://github.com/user-attachments/assets/0d54892a-1fb7-403a-81ae-d61c39d93c4e)
        * Click ok and then yes on the popup to initiate the Remote Desktop connection
        * You will be met with a lot of popups for personalization and network discover, just choose no for everything
3.  **Install Wireshark (Windows 10 VM):**
    * Open the browser in your VM and go to https://www.wireshark.org
    * Download Windows x64 installer and run it
    * Leave everything alone installer but check Ncap is checked
    ![image](https://github.com/user-attachments/assets/65c6726f-5ee8-45d0-b212-65b4e37e27c8)
    * NCAP is a next-generation capture file format used by Wireshark for efficient storage of network traffic data
    * Also install NCAP popup, leave everything as is, to continue Wireshark installtion 
4.  **Observe ICMP Traffic:**
    * Open Wireshark and start a packet capture by clicking the adapter with activity next to it (usually called Ethernet) and then clicking the shark icon 
    ![image](https://github.com/user-attachments/assets/1cdc2a5d-3b2b-4e36-8a82-297e8135bd62)
    * You will see a lot of movement as this all the network activity happening, which can be filterd to see specfic activity such as protocols or ports
    * To view activity between the two VMS we need the private IP address for the Linux VM
    * Retrieve the private IP address of the Linux VM
    ![image](https://github.com/user-attachments/assets/3bd29d10-ac49-435c-8297-6ae69ccbcb58)
    ![image](https://github.com/user-attachments/assets/94203553-0f65-4652-ac1e-9c28afedeea5)
    * Open Powershell in Windows VM and ping the Linux VM
      ```powershell
      ping 10.0.0.5
      ```
    * Observe the ping requests and replies in Wireshark by filtering the capture by typing icmp into the search bar
    ![image](https://github.com/user-attachments/assets/a930e3c7-f015-4f8d-ac0c-44e4a5def7c6)
    * Ping a public website (www.google.com) from the Windows 10 VM and observe the traffic in Wireshark.
    ![image](https://github.com/user-attachments/assets/f1334a46-837d-4feb-8e15-9e514c4f2ffd)
    * As observed the ping command sends 4 requests to the destination IP (which can be typed as an IP or as the domain name which is converted by DNS first) and recieves 4 replys
    *  Windows: `ping -t <ip-address>` creates a continuous ping useful for connectiviy of real time changes
5.  **Observe SSH Traffic:**
    * Started a new Wireshark packet capture, filtering for SSH traffic.
    * From the Windows 10 VM, SSH'd into the Ubuntu VM using its private IP address (`ssh labuser@<private IP address>`).
    * Typed commands into the Linux SSH connection and observed the SSH traffic in Wireshark.
    * Exited the SSH connection.
    * (Add screenshot of Wireshark SSH capture)
6.  **Observe DHCP Traffic:**
    * Started a new Wireshark packet capture, filtering for DHCP traffic.
    * From the Windows 10 VM, opened PowerShell as administrator and ran `ipconfig /renew`.
    * Observed the DHCP traffic in Wireshark.
    * (Add screenshot of Wireshark DHCP capture)
7.  **Observe DNS Traffic:**
    * Started a new Wireshark packet capture, filtering for DNS traffic.
    * From the Windows 10 VM, used `nslookup` to resolve the IP addresses of google.com and disney.com.
    * Observed the DNS traffic in Wireshark.
    * (Add screenshot of Wireshark DNS capture)
8.  **Observe RDP Traffic:**
    * Started a new Wireshark packet capture, filtering for RDP traffic (`tcp.port == 3389`).
    * Observed the continuous stream of RDP traffic.
    * **Observation:** The RDP protocol continuously streams data, resulting in constant traffic, as it provides a live view of the other machine.
    * (Add screenshot of Wireshark RDP capture)

### Part 3: Configuring a Firewall (Network Security Group)

1.  **Disable ICMP Traffic:**
    * Initiated a perpetual ping from the Windows 10 VM to the Ubuntu VM.
    * Opened the Network Security Group (NSG) associated with the Ubuntu VM and disabled incoming ICMP traffic.
    * Observed the ping activity in Wireshark and the command line.
    * (Add screenshot of NSG ICMP rule change)
2.  **Re-enable ICMP Traffic:**
    * Re-enabled ICMP traffic in the NSG.
    * Observed the ping activity in Wireshark and the command line (ping should resume).
    * Stopped the ping activity.
    * (Add screenshot of NSG ICMP rule restore)

### Cleanup (if not using the VM or RG anymore)

1.  **Close Remote Desktop Connection:**
    * Closed the Remote Desktop connection to the Windows 10 VM.
2.  **Delete Resource Group(s):**
    * Deleted the Resource Group created at the beginning of the lab.
3.  **Verify Resource Group Deletion:**
    * Verified that the Resource Group was successfully deleted.

## Takeaways

* Gained practical experience in deploying and configuring VMs in Azure.
* Learned how to use Wireshark to analyze network traffic.
* Understood the behavior of various network protocols (ICMP, SSH, DHCP, DNS, RDP).
* Configured Network Security Groups (NSGs) to control network traffic.
* Learned the importance of proper Azure resource clean up.
* Understanding network protocols is crucial for network troubleshooting.
```
