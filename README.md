<p align="center">
<img src="<img width="358" height="358" alt="image" src="https://github.com/user-attachments/assets/5c62a3a3-f122-46fc-b746-b03c39b85a39" />
" alt="Generic DNS logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure) - Part 2 (DNS)</h1>
This tutorial outlines the implementation of DNS within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Recap</h2>
In Part 1, we demonstrated how to use Active Directory to create users with PowerShell on the Domain Controller, as well as manage client accounts. For Part 2, DNS will emphasize how it’s connected to Active Directory. 


<h2>Deployment and Configuration Steps</h2>

<p>
DNS is short for Domain Name System, which converts website names written in human-readable form into IP addresses that can locate the resource.

To begin, make sure that the virtual machines created in Part 1 are turned on. In this case, those are DC-1 and Client-1. There will be three sections to this DNS exercise: the A-Record, Local DNS Cache, and CNAME. A-Record will be the first exercise.

A-Record

Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)
 

Connect/log into Client-1 as an admin (mydomain\jane_admin)
 

From Client-1, go to PowerShell and try to ping “mainframe”. You’ll notice that it fails
 

Nslookup “mainframe” notice that it fails (no DNS record)
 

Go to Windows Administrator Tools in DC-1 and go to DNS. Create a DNS A-record on DC-1 for “mainframe” and have it point to DC-1’s Private IP address. 
From there, go to mydomain.com and then right click for “New Host A-Record”
 

 

Go back to Client-1 and try to ping it. Observe that it works
 

Local DNS Cache

Go back to DC-1 and change mainframe’s record address to 8.8.8.8
 

Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address
 

Observe the local dns cache (ipconfig /displaydns)
 

Flush the DNS cache (ipconfig /flushdns)
 

Observe that the cache is empty (ipconfig /displaydns)
 

Attempt to ping “mainframe” again. Observe the address of the new record is showing up
 

CNAME

Go back to DC-1 and create a CNAME record within the DNS Manager that points the host “search” to www.google.com
 

Go back to Client-1 and attempt to ping “search”, observe the results of the CNAME record
 

On Client-1, nslookup “search”, observe the results of the CNAME record. This will reveal the ipv6 and ipv4 results
 

This officially wraps up Part 2 of this tutorial. Part 3 will conclude the Active Directory series on Network File Shares and Permissions


</p>
<br />
