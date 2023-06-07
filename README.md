# Azure Network Protocols
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" height="50%" width="50%" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Observing Traffic Between Azure Virtual Machines</h1>

<h2>Introduction</h2>

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

<h3>Step 3: Running a variety of network protocols and using Wireshark to observe its traffic</h3>

<h2 align="center">Implementing ICMP protocol</h2>


You are able to view connectivity between your two virtual machines by pinging the Ubuntu VM from your Windows 10 VM. First, you will need to filter your network traffic by typing in ICMP at the top (ICMP: protocol used by ping) and you will observe that there is no traffic.

<p align="center">
<img src="https://imgpile.com/images/9mfJt1.png"/>
</p>

You will need to retrieve your Ubuntu VM's private IP address so you can ping it, which can be found in your Azure account by accessing your Ubuntu VM in Virtual machines. After you copy the private IP address, go back to your Windows VM and type in Powershell from the Windows search bar. Once you open the program, type in "ping (Ubuntu private IP address)" and press enter. 

<p align="center">
<img src="https://imgpile.com/images/9mfPHL.png" />
</p>

You will observe that there is traffic between two IP addresses under Source and Destination column. 10.0.0.4 is the Windows VM and 10.0.0.5 is the Ubuntu VM, showing that there is connectivity between the VMs. 

<p align="center">
<img src="https://imgpile.com/images/9mfYSx.png" />
</p>

You can also observe connectivity between the Windows VM and a website of your choice. Go into Powershell, type in ping and the website you would like to test and hit Enter. In our example below, you can see that 10.0.0.4 is the Windows VM and 23.204.249.185 is amazon.com, showing that we have a connection.

<p align="center">
<img src="https://imgpile.com/images/9mfsbj.png" />
</p>

You can also have your Windows VM ping your Ubuntu VM indefinitely by typing "ping (Ubuntu private IP address) -t" and hitting Enter. You will obvserve that Wireshark is showing network traffic between the virtual machines with no end. 

<p align="center">
<img src="https://imgpile.com/images/9mfrY8.png" />
 </p>

<p align="center">
<img src="https://imgpile.com/images/9mffgC.png" />
</p>

<h2 align="center">Implementing Network Security Groups</h2>

As your Windows VM is pinging your Ubuntu VM, you can modify the Firewall rules of the Ubuntu VM to stop ICMP traffic, which will end this connection between the two VMs. In order to do this, you will create a new firewall rule that will prevent any incoming ICMP traffic. Go into Azure, type "Network Security Group" in the search bar (which functions as your virtual machine's Firewall), select the Ubuntu VM network security group, click on "Inbound security rules" and click "+ Add." 

Next, select "ICMP" under Protocol and "Deny" under Action. Adjust the Priority value above SSH's 300 so that the rule we made will be the first rule in place. In your case, you can select 200 as the rule's Priority value and then click Add.
Once this rule is in place, you will notice there is no connection between the virtual machines.

<p align="center">
<img src="https://imgpile.com/images/9mBg2w.png" />
</p>

<p align="center">
<img src="https://imgpile.com/images/9mF5cc.png" />
</p>

To enable connectivity again between the two VMs, go to your Inbound security rule, select "Allow" and "Save" and you will observe that the virtual machines are connecting again. You can hit Ctrl + C in Powershell to stop the pinging.

<p align="center">
<img src="https://imgpile.com/images/9mFdMP.png" />
</p>

<p align="center">
<img src="https://imgpile.com/images/9mFDCx.png" />
</p>

<h2 align="center">Implementing SSH protocol</h2>

You can access your Ubuntu VM from the Windows VM by the Powershell command line using SSH. Filter the traffic in Wireshark by typing in "ssh" in the top bar and clicking on the green icon so you can refresh. Next go to Powershell, type  in "ssh (your Ubuntu VM username)@(private IP address)" and hit Enter.

You will receive a message asking if you want to continue, which you will type "yes." Next, enter the password that you created for your Ubuntu VM under "Administrator account" and when you type in your password. When you type in your password, you will see that there's no visible text appearing, but that is ok. Just type in your password and rest assured that it will go through if you enter it correctly.

<p align="center">
<img src="https://imgpile.com/images/9mQ7yl.png" />
</p>

You can see the directories (folders) of your Ubuntu VM by typing in "ls -lasth" and pressing Enter. You will notice the traffic displayed in Wireshark. You can exit out of your Ubuntu VM by typing in "exit" and pressing Enter.

<p align="center">
<img src="https://imgpile.com/images/9mQwXF.png" />
</p>


<h2 align="center">Implementing RDP protocol</h2>

You can view RDP (Remote Desktop Protocol) traffic by its protocol/port number. You can do this by filtering RDP traffic by typing in "tcp.port == 3389" in the bar, which will allow you to see as constant display of traffic.

<p align="center">
<img src="https://imgpile.com/images/9mQTbu.png" />
</p>

<h2 align="center">Implementing DHCP protocol</h2>

You can view DHCP traffic by re-issuing the IP address of our Windows VM in Powershell. Filter DHCP traffic by typing "dhcp" in the bar and hit the green icon to refresh. Next type "ipconfig /renew" in Powershell and view the traffic in Wireshark. 

<p align="center">
<img src="https://imgpile.com/images/9mQCP2.png" />
</p>


<h2 align="center">Implementing DNS protocol</h2>


You can also view DNS traffic by the nslookup command in Powershell. Nslookup gives us the IP address of a website. You can filter traffic by DNS in Wireshark then type in "nslookup (your choice of website)" into Powershell and observe the network traffic in Wireshark. 

<p align="center">
<img src="https://imgpile.com/images/9mQxGX.png" />
</p>

Once you are done using your virtual machine, you will need to delete the resource group so you are not being charged when you are not using it. In order to do this, type "Resource groups", click on the Resource group that you would like to delete, select "Delete resource group", copy and paste the name of the resource group below and then hit "Delete." 

<p align="center">
<img src="https://imgpile.com/images/9mQKHh.png" />
</p>
