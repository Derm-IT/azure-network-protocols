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
    * Start a new Wireshark packet capture, filtering for SSH traffic. Type `ssh` or `tcp.port==22` and hit enter into filter bar
    * From the Windows 10 VM, SSH'd into the Linux VM using its private IP address (`ssh adminuser@<private IP address>`).
    * It will spawn a command line that connects to the Linux VM
    * Type `touch note.txt` 
    * Observe the SSH traffic in Wireshark.
    * Type `ls` in powershell to observe that the file was created
    * Type `exit` to exit the SSH connection.
    ![image](https://github.com/user-attachments/assets/f82c7bd8-4c1e-46d6-b8f5-17072a439d1f)
    ![image](https://github.com/user-attachments/assets/295d7a8f-2e14-4fb9-b880-3d5daffa45a9)
6.  **Observe DHCP Traffic:**
    * Start a new Wireshark packet capture (capture ->restart->continue without saveing), filtering for DHCP traffic `dhcp` or `udp.port==67 || udp.port==68`.
    * From the Windows 10 VM, open notepad and type
      ```
      ipconfig /release
      ipconfig /renew
      ```
    * Save as dhcp.bat anywhere on the VM but `C:\Users\<username>?` is easiest and make save type `All files`
    ![image](https://github.com/user-attachments/assets/e2d24e21-9996-4368-a628-289d0568d430)
    * Open Powershell as admin and run the .bat script
    * You will lose connection as the ip has been released and once Azures DHCP server assigns a new one from /renew the connection will come back
    * Observe the DHCP traffic in Wireshark and note the 255.255.255.255 broadcast ip
    ![image](https://github.com/user-attachments/assets/7422a24c-2c4e-431e-9efc-abd88ef57aa8)
7.  **Observe DNS Traffic:**
    * Started a new Wireshark packet capture, filtering for DNS traffic, `dns` or `udp.port==53`
    * From the Windows 10 VM, used `nslookup` in Powershell to resolve the IP addresses of google.com and disney.com.
    * Observed the DNS traffic in Wireshark.
    ![image](https://github.com/user-attachments/assets/9bc1ad1f-cc0e-4e7d-80f1-604cab5f9464)
8.  **Observe RDP Traffic:**
    * Start a new Wireshark packet capture, filtering for RDP traffic (`tcp.port == 3389`).
    * Observe the continuous stream of RDP traffic.
    * The RDP protocol continuously streams data, resulting in constant traffic, as it provides a live view of the other machine.
    ![image](https://github.com/user-attachments/assets/a73807cb-0976-4467-bddc-72631bd2b462)


### Part 3: Configuring a Firewall (Network Security Group)

1.  **Disable ICMP Traffic:**
    * Initiate a perpetual ping from the Windows 10 VM to the Linux VM
    * Type `ping <linux private ip> -t` in Powershell in Windows VM
    ![image](https://github.com/user-attachments/assets/932b8636-d838-41f6-a295-638a3c490bc5)
    * Click the Network Security Group (ex. main-linux-vm-nsg) associated with the Linux VM in Azure
      ![image](https://github.com/user-attachments/assets/b5628d85-1269-44fe-a96e-ee32f08bdff6)
    * Go to Settings-> Inbound security rules and click Add
    ![image](https://github.com/user-attachments/assets/200b04f0-ea65-41ce-9c50-b5c28133d717)
    * Leave everything as is, but set port ranges to *, Protocol to ICMPv4, Action to Deny and Priority to 290
    ![image](https://github.com/user-attachments/assets/56bb01f6-3e04-4afe-a708-4438f6017a44)
    * *Note* the priority as a lower number gets seen first and * in port as ICMP doesnt use port and * indicates any port
    * Add the rule
    * Return to the Linux VM
    * Observe the ping activity in Wireshark(may take up to 60s to come into effect)
    ![image](https://github.com/user-attachments/assets/2f0c6614-d731-46a7-b9a1-df0e8574e5c6)

2.  **Re-enable ICMP Traffic:**
    * Re-enable ICMP traffic in the NSG.
    * Go back to Inbound Security Rules, select the rule and click delete
    ![image](https://github.com/user-attachments/assets/0aaa4c49-b8a3-42db-8f4e-20cb820d3a65)
    * Observe the ping activity in Wireshark (ping should resume).
    ![image](https://github.com/user-attachments/assets/2d58ca24-2ba3-4b3b-9745-64fd66a54ad3)
    * Stop the ping activity.
### Cleanup (if not using the VM or RG anymore)

1.  **Close Remote Desktop Connection:**
    * Close the Remote Desktop connection to the Windows 10 VM.
2.  **Delete Resource Group(s):**
    * Delete the Resource Group created at the beginning of the lab.
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
