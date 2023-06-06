# Azure Network Protocols
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" height="50%" width="50%" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Observing Traffic Between Azure Virtual Machines</h1>
In this tutorial, we will observe various network traffic to and from Azure Virtual Machines with Wireshark. We will also experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Wireshark 
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>Step-by-step guide</h2>
 
<h3>Step 1: Creating two different virtual machines</h3>

 We will first create a Windows 10 virtual machine in Azure. I have provided a step-by-step guide on how to do this in a previous tutorial, which can be found [here.](https://github.com/JArias337/Creating-Virtual-Machines-in-Azure) 
 
Once you create your first virtual machine, you will create your second virtual machine. The main difference is that you wil select an Umbuntu server. You also want to make sure that you are using the same resource group and region as your resource group. 

<p align="center">
<img src="https://i.imgur.com/BO5EU6u.png"/>
</p>

Under "Administrator Account", which is next to "Authentication type" select the "Password" option instead of "SSH public key." You can use the same username and password that you selected for the Windows 10 VM that you made, then click on "Review + create" and after validation click on "Create."

<p align="center">
<img src="https://i.imgur.com/Zwl5LJK.png"/>
</p>

<h3>Step 2: Installing and using Wireshark in the Windows 10 VM</h3>

Once the VMs are made, we are going to log into our Windows 10 VM by way of Remote Desktop Connection. You can follow "Step 3" from the step-by-step guide on how to do this, which can be found [here.](https://github.com/JArias337/Creating-Virtual-Machines-in-Azure) 

Now that we are in our Windows VM, we can observe its network traffic using a software called Wireshark. You can do a search for "Wireshark," click on the first link and select Windows Installer (64-bit). Once the dowloan is complete, you can click "Open file."

<p align="center">
<img src="https://i.imgur.com/YY4rkAS.png" />
</p>

Do a search for "Wireshark" in your Windows search bar and the program should open up. You can observe the network traffic by clicking on the blue icon on the top left corner.  

<p align="center">
<img src="https://i.imgur.com/3r4Acnn.png" />
</p>

<h3>Step 3: Running different network protocols and observing its traffic by using Wireshark</h3>

<h2 align="center">Utilizing ICMP protocol</h2>


We can observe connectivity between our two virtual machines by pinging our Ubuntu VM from our Windows 10 VM. To do this we first need to filter our network traffic by typing in ICMP at the top bar (ICMP is the protocol used by ping) and you'll notice that there isn't any traffic displayed in Wireshark.


<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b13.png" height="70%" width="85%"/>
</p>

In order to ping to our Ubuntu VM we need to retrieve its private IP address which can be found in our Azure account and clicking on our Ubuntu VM in Virtual machines. After copying the private IP address, go back to your Windows VM, open Powershell from the Windows search bar and type in "ping (private IP address)" and press Enter. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b14.png" />
</p>

You'll notice that Wireshark will display traffic between two IP addresses under Source and Destination column, here 10.0.0.4 is the Windows VM and 10.0.0.5 is the Ubuntu VM thus proving connectivity between the VMs. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b15.png" />
</p>

We can also see connectivity between the Windows VM and a website for instance. To test this go into Powershell, type in ping and the website you would like to test connectivity to and hit Enter. Here you can see that 10.0.0.4 is the Windows VM and 172.253.115.136 is YouTube proving connectivity.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b16.png" />
</p>

We can set the Windows VM to ping to Ubuntu VM perpetually by typing in "ping (private IP address) -t" and hitting Enter; notice Wireshark is constantly displaying network traffic between the virtual machines nonstop. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b17.png" />
</p>

<h2 align="center">Utilizing Network Security Groups</h2>

While the Windows VM is pinging our Ubuntu VM, we can alter the Firewall rules of the Ubuntu VM to prevent ICMP traffic coming through thus preventing connectivity between the two VMs. Basically we are going to create a new firewall rule that will deny any incoming ICMP traffic. To do this, go into Azure, type in "Network Security Group" in the search bar (which is essentially the virtual machine's Firewall), select the Ubuntu VM network security group, click on "Inbound security rules" and click "+ Add." 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b18.png" />
</p>

Here select "ICMP" under Protocol and "Deny" under Action. Change the Priority value above SSH's 300 so that the rule we made will be the first rule enact, here I've chosen 200 as the rule's Priority value and then click Add.
And once you're done adding this rule, notice the network traffic in Wireshark and Powershell, it basically shows that the Windows VM is pinging the Ubuntu VM yet there is no connection between them.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b19.png" />
</p>

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b20.png" />
</p>

To allow connectivity again between the two VMs, just go back to the Inbound security rule, select "Allow" and "Save" and you should notice correspondence between the virtual machines again. Hit Ctrl + C in Powershell to stop pinging.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b21.png" />
</p>

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b22.png" />
</p>

<h2 align="center">Utilizing SSH protocol</h2>

We can access Ubuntu VM from the Windows VM by the Powershell command line using SSH. In Wireshark, filter the traffic by typing in "ssh" in the top bar and clicking on the green fin icon to refresh. Then in Powershell, type  in "ssh (Ubuntu VM username)@(private IP address)" and hit Enter.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b23.png" />
</p>

You will receive a prompt if you want to continue so type "yes." Then you will need to enter the password that you made for your Ubuntu VM under "Administrator account" and when you type in your password you'll notice that there's no visible text appearing, there is nothing wrong with Powershell you just need to type it in correctly. Hit Enter after you've typed your password, you successfully logged in Ubuntu's VM marked by green text virtual machine name.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b24.png" />
</p>

You can list the directories (folders) of Ubuntu VM by typing in "ls -lasth" and press Enter and as you do you will notice the traffic displayed in Wireshark. You can exit out of Ubuntu VM by typing in "exit" and pressing Enter.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b25.png" />
</p>


<h2 align="center">Utilizing RDP protocol</h2>

We can observe RDP (Remote Desktop Protocol) traffic by its protocol and port number in Wireshark. We can do this by filtering RDP traffic by typing in "tcp.port == 3389" in the top bar and seeing a continuous display of traffic.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b28.png" />
</p>

<h2 align="center">Utilizing DHCP protocol</h2>

We can observe DHCP traffic in Wireshark by re-issuing the IP address of our Windows VM via the ipconfig /renew command in Powershell. First filter DHCP traffic by typing "dhcp" in the top bar and refresh. Then type in "ipconfig /renew" in the Powershell and observe the traffic in Wireshark. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b26.png" />
</p>


<h2 align="center">Utilizing DNS protocol</h2>


We can also observe DNS traffic by the nslookup command in Powershell, this command gives us the IP address of a website. Filter traffic by DNS in Wireshark then type in "nslookup (your choice of website)" into Powershell and observe the network traffic in Wireshark. 

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b27.png" />
</p>

We're almost finished, all we have to do now is to delete our resource groups in Azure so that it won't charge us more for using its services unnecessarily. So go into Azure, type in "Resource groups", click on the Resource group(s) shown, select "Delete resource group", copy the resource group's name and paste it below and then hit "Delete." Do this for each resource group and that's it for this tutorial.

<p align="center">
<img src="https://github.com/Mwajiduddin/Mwajiduddin/blob/main/images/b29.png" />
</p>
