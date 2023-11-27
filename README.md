<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com/watch?v=YNPDCHiFTDM)

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

- Create Virtual Machines through Microsoft Azure
- Remote Desktop to Virtual Machine, Install Wireshark
- Test Connections between VM1, and VM2. Adjust Permissions
- Utilize Wireshark to Analyze SSH, DHCP, DNS, and RDP Traffic

<h2>Actions and Observations</h2>

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_1.png"/>
</p>
<p>
To begin, we will create the first virtual Machine "Jagilmor-VM-1", using a premade Resource Group "Jagilmor-Prof1". It is imperative that the Region of this virtual machine match the second virtual machine, as it will also share the same vnet.
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_2.png"/>
</p>
<p>
I've renamed the VM1 dns server to jagilmorvm1. This will make connecting to the remote desktop easier to remember, instead of the public IP address 20.163.25.135
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_3.png"/>
</p>
<p>
As we are now able to connect to the remote desktop, you can see the username JagilmorLab1 will the the primary login name for the VM1. At the top of the screenshot you can see that we are connected to the same IP 20.163.25.135 as referenced in the previous shot.
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_6.png"/>
</p>
<p>
Within the VM1, I will run an ipconfig through the windows command line to show that JagilmorLab1 is using private IP address 10.0.0.4
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_7.png"/>
</p>
<p>
We will now open up wireshark to begin looking at various activity
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_8.png"/>
</p>
<p>
Without filters, there is an extroardinary amount of information to observe. It is mostly impossible to gather any useful information as it channels through the primary Network Adapter
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_9.png"/>
</p>
<p>
Now we will begin to look at the second virtual machine to begin the traffic practice. Note the computer name is "JagilmorVM2", running on Linux (ubuntu 20.04) with a private IP address of 10.0.0.6, and is using the JagilmorVM1-vnet 
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_10.png"/>
</p>
<p>
We can see here that our first Virtual Machine "JagilmorVM1" is operating on Windows 10 Pro, and has a 10.0.0.4 private IP address, and is also using the JagilmorVM1-vnet
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_11.png"/>
</p>
<p>
Our first practice of network activity is to ping VM1 to VM2 by using the commmand "ping -t 10.0.0.6" this will send continuous network traffic from VM1 to VM2
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_12.png"/>
</p>
<p>
We can observe the exchange of packets between 10.0.0.4, and 10.0.0.6 by viewing the icmp filter in Wireshark.
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_13.png"/>
</p>
<p>
Within Azure and the VM2 "Networking" setting, we can adjust access of the icmp traffic by blocking all incoming ICMP traffic. It is important to set this as a 200 priority as there are other rules that are implemented (with a 300 priority or lower.)
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_14.png"/>
</p>
<p>
We can now observe the lack of traffic between each VM1 and VM2. The continuous reply from 10.0.0.6 has stopped after the Azure rule to block ICMP traffic was implemented. We can see in wireshark that there is no longer a reciprocating exchange of packets, just a ping request from 10.0.0.4
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_15.png"/>
</p>
<p>
After returning to Azure, we can re-enable the ICMP rule that we placed in the VM2 "Networking" setting to re-allow ICMP traffic. Doing this will return us to our original state of the ping exchange between 10.0.0.4 and 10.0.0.6
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_16.png"/>
</p>
<p>
Now we can monitor the SSH traffic that is being transmitted between VM1 and VM2. As you can see in the left side command line I've SSH connected into VM2 from the VM1 windows command line. On the right window we can observe the ssh activity within Wireshark. Note: any time a keystroke is pressed, the transmission is sent between machines, regardless of whether a command has been pushed through.
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_18.png"/>
</p>
<p>
Now within Wireshark we can filter and monitor DHCP traffic. On the left window the command: "ipconfig /renew" was sent, and we can observe the traffic request and acknowledgement packets transmitted in Wireshark.
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_19.png"/>
</p>
<p>
The next type of traffic we can monitor is DNS traffic. In Wireshark we will filter "dns" and in the VM1 command prompt we can run: nslookup www.linkedin.com<br>
  Within Wireshark we can see all of the transmissions in that single executed command.
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_20.png"/>
</p>
<p>
We can continue to monitor DNS traffic, but use the appropriate port filter "udp.port == 53"<br>
  Here we can see the same type of traffic as before, just with a more specific filter type.
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_21.png"/>
</p>
<p>
To expand on the DNS traffic we can see the batch of queries and responses from the single windows command "nslookup wwww.monster.com"
</p>
<br />

<p>
<img src="https://github.com/jagilmorProf/azure-network-protocols/blob/main/Jagilmor-Azure-NP_22.png"/>
</p>
<p>
Now we will observe the Remote Desktop Protocol traffic. We will use the filter "rdp" within Wireshark, and observe the continous exchange of data between various servers.
</p>
<br />
