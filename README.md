
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Wireshark (Protocol Analyzer)
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
  -  DNS - Domain Name System
  -  ICMP - Internet Control Message Protocol
  -  SSH - Secure Shell
  -  RDP - Remote Desktop Protocol



<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>List of Prerequisites</h2>

- Mircosoft Azure Subscription, sign up for a free account. 
- Access to Microsoft Remote Desktop
    - For Mac user check out this video on how to remote desktop connect ( https://tinyurl.com/yhvrs3mt )
  


<h2>Actions and Observations</h2>

<p>
</p>


![image](https://github.com/JosiahAllen03/test/assets/147882549/61094283-0a98-440a-829a-2b454b08f3f1)

<p>
-Create a resource group. Make sure you take note of what region your resource group is in as you will want the region to be in the same for your Virtual Machines (VM)
  
</p>
<br />


![image](https://github.com/JosiahAllen03/test/assets/147882549/00a33447-a806-4e03-b063-5be80ee9c4ee)

- Create Windows 10 VM. Make sure the size is 2vcpus $91.98/month. Before Finalizing our virtual machine you will go to the networking tab, and In the network allow Virtual Machine 1 or VM-1 to create a new Virtual network or Vnet and Sub network or Subnet.
</p>


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/1a6dbfdc-5b6a-4ff0-9431-c38d8a4ed6e2)

 - create Vm-2 with Linux. Make sure it's in the same Vnet and it's using 2 vcpus. Scroll down that page select Admin account and allow for a password. Let VM-2 deploy then double-check to make sure they are in the same Vnet. If they are not in the same Vnet, delete VM-2 and repeat step 2.
   
<p>

</p>
<br />


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/8e75c999-a66b-4e08-aa04-0eb17bcea885)

 - Next use a Remote desktop to connect to Vm-1. Use the public IP from VM-1 and sign in.
  
<p>

  
![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/52394a92-7752-434c-ae63-e04358ac047c)

- Within the Windows VM-1 download and install wireshark. After installed feel free to open the application.
</p>
<p>

</p>
<br />


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/627a6de8-e8d3-406c-b2ff-faebbc672fca)

- open wireshark and filter at the top for ICMP traffic


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/7361e783-fee3-4e18-a4f6-04a02dba038d)

- Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM.
  -  Observe ping requests and replies within WireShark.


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/176c82c7-cbbe-4ac8-9e5d-06a4ab5ce91b)

- From The Windows 10 VM, open the command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in Wireshark. 


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/e110a374-038c-42f2-849f-94b3c5b38a50)

- When pinging you will open up wireshark which should be filtered to only show ICMP traffic. You should be able to see the ping request and the ping reply


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/746b41fd-3e92-4843-b248-1e2739a599c6)

- Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM. You can do this style of ping by adding  (-t) to your command


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/f5651073-901f-4018-b09c-7852143f822a)

- Head to the Network Security Groups tab the select VM-2 


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/c9d63b07-9b4e-4b55-9e05-9deea699baa2)

- We then want to add an Inbound security rule. To further test the connection between the machines we will now block ICMP traffic.


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/370e5656-c235-4484-b281-52511b410ac6)

- By adding this, our ping on VM-1 should start to fail. After checking go back and remove this rule from Vm-2 and stop the ping activity.


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/d9232bc4-1431-4f4f-a89b-7ee386f9cfbe)

- Head Back in Wireshark, and filter for SSH traffic only. We will now test for SSH connection. 


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/6912efed-06d7-472c-85cb-d212d8b0188e)

- SSH sign into your VM-2 which is on Linux.
    - To SSH into our Virtual Machine 2 we we need our VM-2 private IP address, as well as the username and password we used to create the virtual machine at the start.
        - An example command should look like this ( ssh Labuser@10.0.0.5 ) After you enter your password. 


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/ca5be365-1e58-4808-81d6-42f20cc4b806)

- After signing in feel free to enter some commands to see the connection between the two machines
    - Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
    - Exit the SSH connection by typing ‘exit’ and pressing [Enter]


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/06e68437-0aaa-4d9e-baed-aa00fe5b161c)

- Back in Wireshark, filter for DHCP traffic only. We will now observe the DHCP traffic.


![image](https://github.com/JosiahAllen03/Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols/assets/147882549/14c8ac3c-e9cf-4d08-916e-bbb09d936f81)

- From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
  Observe the DHCP traffic appearing in WireShark

##### Lab Clean-up #####

- After finishing the lab, close your remote desktop connection.
- Delete the resource group(s) you created at the start of the lab
- Verify that the resource group/Virtual machines are all deleted
