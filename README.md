<p align="center">
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQE8PstcPGgZTuEczosANbV4nyHV3wakk2yB2XObdv0Dg&s=10" />
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
In Part 1, we demonstrated how to use Active Directory to create users with PowerShell on the Domain Controller and manage client accounts. In Part 2, DNS will emphasize how it’s connected to Active Directory. 


<h2>Deployment and Configuration Steps</h2>

<p>
DNS is short for Domain Name System, which converts website names written in human-readable form into IP addresses that can locate the resource.

To begin, make sure that the virtual machines created in Part 1 are turned on. In this case, those are DC-1 and Client-1. There will be three sections to this DNS exercise: the A-Record, Local DNS Cache, and CNAME. A-Record will be the first exercise.

*A-Record*

Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)


<img width="713" height="578" alt="image" src="https://github.com/user-attachments/assets/f5aadbc2-a088-403d-a0f9-b210caa4134b" />


Connect/log into Client-1 as an admin (mydomain\jane_admin)


<img width="713" height="549" alt="image" src="https://github.com/user-attachments/assets/ed545a93-a14e-4f5b-a0d9-f38339a0edc9" />


 
From Client-1, go to PowerShell and try to ping “mainframe”. You’ll notice that it fails


<img width="975" height="931" alt="image" src="https://github.com/user-attachments/assets/52fe7cd5-ef1b-4c06-9490-eb683082d361" />
 

Nslookup “mainframe” and notice that it fails (no DNS record)


<img width="975" height="931" alt="image" src="https://github.com/user-attachments/assets/4eb025df-c25b-4fde-8229-e7ef06f4e0f0" />
 

Go to Windows Administrator Tools in DC-1 and go to DNS. Create a DNS A-record on DC-1 for “mainframe” and have it point to DC-1’s Private IP address. 

From there, go to mydomain.com and then right click for “New Host A-Record”

<img width="975" height="656" alt="image" src="https://github.com/user-attachments/assets/0762a933-9990-4980-bd9e-7eb1df71afe6" />
 
<img width="535" height="593" alt="image" src="https://github.com/user-attachments/assets/0392f075-6805-46fd-b13d-e646d5a3e009" />

 

Go back to Client-1 and try to ping it. Observe that it works


<img width="975" height="931" alt="image" src="https://github.com/user-attachments/assets/a4076369-361b-4e95-b90c-8d0fef02e2fd" />


*Local DNS Cache*

Go back to DC-1 and change mainframe’s record address to 8.8.8.8


<img width="625" height="711" alt="image" src="https://github.com/user-attachments/assets/f919abdf-873b-4cd4-ae86-9f80c30392cf" />
 


Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address


<img width="975" height="931" alt="image" src="https://github.com/user-attachments/assets/3f1c252a-90aa-4ebd-a601-edfbd1e77059" />
 


Observe the local DNS cache (ipconfig /displaydns)


<img width="975" height="523" alt="image" src="https://github.com/user-attachments/assets/816b7f28-1b39-4bab-8b56-fd7060665bb5" />
 

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
