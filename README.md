
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>


<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

In this tutorial, we will analyze network traffic between Azure Virtual Machines using Wireshark and experiment with Network Security Groups to understand how different network protocols interact and how NSGs control traffic.

<h2>Environments and Technologies Used</h2>

	•	Microsoft Azure (Virtual Machines)
	•	Remote Desktop
	•	Command-Line Tools (SSH, CMD)
	•	Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
	•	Wireshark (Protocol Analyzer)

<h2>Operating Systems Used</h2>

	•	Windows 10 (21H2)
	•	Ubuntu Server 20.04

<h2>Lab Steps</h2>

Step 1: Setting Up the Environment

	•	Create two Virtual Machines (VMs) in Azure:
	•	Linux VM (Ubuntu Server 20.04)
	•	Windows 10 VM
	•	Ensure both VMs are in the same Virtual Network (VNet) and are provisioned with 2 vCPUs each.
	•	On the Windows 10 VM, download and install Wireshark:
Download Wireshark

Step 2: Capturing ICMP Traffic with Wireshark

	1.	Open Wireshark on the Windows 10 VM.
	2.	Set the Wireshark Filter to capture only ICMP traffic.

icmp


	3.	Ping the Linux VM’s Private IP from the Windows VM using the command:

ping <Linux_VM_Private_IP>


	4.	Observe the ICMP packets being captured in Wireshark. Each packet represents an echo request and echo reply exchange between the two VMs.

<p align="center">
<img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Wireshark ICMP Capture"/>
</p>


Step 3: Inspecting Individual Packets

	•	Click on any captured ICMP packet in Wireshark to see detailed information about the packet, including the source and destination IP, time-to-live (TTL), and packet size.

<p align="center">
<img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Inspecting ICMP Packets"/>
</p>


Step 4: Testing Network Security Group Rules with ICMP

	1.	Continuous Ping:
Initiate a continuous ping from the Windows VM to the Linux VM using the command:

ping -t <Linux_VM_Private_IP>


	2.	Block ICMP Traffic:
Go to Azure Portal, navigate to the Linux VM’s Network Security Group, and block inbound ICMP traffic by creating a new NSG rule.
	3.	The continuous ping on the Windows VM should now show Request Timed Out messages, indicating that ICMP is being blocked.
	4.	Allow ICMP Traffic Again:
Remove the NSG rule that blocks ICMP traffic to restore connectivity.

<p align="center">
<img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Blocking ICMP Traffic"/>
<img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="ICMP Traffic Blocked"/>
</p>


Step 5: Capturing SSH Traffic with Wireshark

	1.	Set the Wireshark Filter to capture SSH packets only:

ssh


	2.	From the Windows VM, initiate an SSH connection to the Linux VM using the command:

ssh labuser@<Linux_VM_Private_IP>


	3.	Observe SSH packets being captured in Wireshark, showing encrypted communication between the two VMs.

<p align="center">
<img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="SSH Traffic Capture"/>
</p>


Step 6: Capturing DHCP Traffic

	1.	Set the Wireshark Filter to capture DHCP packets:

dhcp


	2.	On the Windows 10 VM, open Command Prompt and run the following command to request a new IP address:

ipconfig /renew


	3.	Wireshark should capture DHCP Discover, Offer, Request, and Acknowledge messages as the DHCP server assigns a new IP address.

<p align="center">
<img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="DHCP Traffic Capture"/>
</p>


Step 7: Capturing DNS Traffic

	1.	Set the Wireshark Filter to capture DNS packets:

dns


	2.	Run the following command on Windows Command Prompt:

nslookup www.google.com


	3.	Wireshark will capture DNS queries and responses as the DNS server resolves the IP address of www.google.com.

<p align="center">
<img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="DNS Traffic Capture"/>
</p>


Step 8: Capturing RDP Traffic

	1.	Set the Wireshark Filter to capture RDP packets:

tcp.port==3389


	2.	Observe continuous RDP traffic being captured, as Remote Desktop Protocol (RDP) is used to connect to the Azure Virtual Machine.

<p align="center">
<img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="RDP Traffic Capture"/>
</p>


<h2>Summary</h2>

	•	We used Wireshark to analyze various network protocols, including ICMP, SSH, DHCP, DNS, and RDP.
	•	We configured Network Security Groups to control traffic flow and verified the effects using Wireshark captures.
	•	This lab demonstrated how different protocols operate and how NSGs can secure and filter specific types of network traffic.

This lab provides a hands-on approach to understanding network protocols and their interaction in an Azure environment. Let me know if you need any further assistance or modifications!

