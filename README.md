<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>
 
 - Observe DNS, DHCP, RDP & ICMP Traffic
 - Type commands into Powershell & Command Prompt, search for IP Adresses + Websites
 - Use Wireshark to capture packets, filter for different types of traffic
 - Initiate a perpetual/non-stop ping from PC to Private IP
 - Open Network Security Groups
 
 <h2>Actions and Observations</h2>
 
- Part 1 (Create our Resources)
  - Create a Resource Group
  - Create a Windows 10 Virtual Machine (VM)
  - While creating the VM, select the previously created Resource Group
  - While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet
  - Create a Linux (Ubuntu) VM
  - While create the VM, select the previously created Resource Group and Vnet
  - Observe Your Virtual Network within Network Watcher

- Part 2 (Observe ICMP Traffic)
  - Use Remote Desktop to connect to your Windows 10 Virtual Machine
  - Within your Windows 10 Virtual Machine, Install Wireshark
  - Open Wireshark and filter for ICMP traffic only (start capturing packets)
  - Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM
  - Observe ping requests and replies within WireShark
  - From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark (ping www.google.com -4)
  - Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM (Private IP)
  - Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic
Priority - 200
Name - DENY_ICMP_PING_FROM_ANYWHERE
For more info search for - Azure Network Security Group microsoft docs
  - Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
  - Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using
  - Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)
  - Stop the ping activity

- Part 3 (Observe SSH Traffic)
  - Back in Wireshark, filter for SSH traffic only (tcp.port == 22)
  - From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address, ssh labuser@privateubuntoIP)
  - Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark (id, uname -a, pwd, ls -lasth, exit)
  - Exit the SSH connection by typing ‘exit’ and pressing [Enter] 

- Part 4 (Observe DHCP Traffic)
  - Back in Wireshark, filter for DHCP traffic only 
  - From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew) 
  - nslookup www.google.com
  - Observe the DHCP traffic appearing in WireShark

- Part 5 (Observe DNS Traffic)
  - Back in Wireshark, filter for DNS traffic only (udp.port == 53)
  - Powershell - nslookup www.disney.com
  - From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are
  - Observe the DNS traffic being show in WireShark

- Part 6 (Observe RDP Traffic)
  - Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
  - Oserve the immediate non-stop spam of traffic? Why do you think it’s non-stop spamming vs only showing traffic when you do an activity?
  - Answer: because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted
  
  - Finish
![Wireshark](https://user-images.githubusercontent.com/128350155/227862713-4cdaa254-1cfe-4320-9fb8-09b0c1aff472.jpg)
