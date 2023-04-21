# Integrated Active Directory Environment in VirtualBox

## Introduction

For this project, Oracle VirtualBox was utilized to create an integrated environment that consisted of a Windows Server 2019 Virtual Machine (VM) serving as the Domain Controller (DC) with Active Directory (AD) service. A custom PowerShell script was then executed to populate AD with approximately 1000 fictional users. I then created a Windows 10 Pro VM and an Ubuntu 22.04.1 VM and integrated them into AD to create a centralized management system for user accounts, computers, and other network resources. 

<br />

## Project Architecture

![AD ARCH](https://i.imgur.com/JW4alB6.png)

<br />

## Technologies and Components Utilized:

- Active Directory
- AD Domain Service
- NAT
- DNS
- Networking
- PowerShell
- Oracle VirtualBox
- Windows Server 2019
- Windows 10 Pro
- Ubuntu 22.04.1

<br />

## Windows Server 2019 Setup

- Two network adapters were used to separate traffic between external and internal. <br> ![adapters](https://i.imgur.com/WQydx1Ml.png) <br>
- The following roles and services were configured. <br> ![roles](https://i.imgur.com/hDJ4vyfl.png)
- PowerShell script used to generate approximately 1000 fictional users. <br> ![ps scrip](https://i.imgur.com/dhNIRcql.png) <br> ![AD Users](https://i.imgur.com/CG0Pu8vl.png)
- After the configurations were completed I then took time to to explore AD by performing tasks such as: creating groups, organizational units, modifying user permissions, disabling users, deleting users, and so on.  <br> ![AD users and groups](https://i.imgur.com/2FVLj5ml.png)

<br />

## Connecting Windows 10 Pro to AD

- The creation of the Windows VM was fairly straightforward and I encounted no issues.
- During the install I placed the VM on the internal network and created a local account. 
- I then updated the system's name and joined it to my domain. <br> ![Change name and join domain](https://i.imgur.com/96BzAlim.png)
- From there I verified the VM was connected to AD by logging into several of the random users I had created. <br> ![Change name and join domain](https://i.imgur.com/ToE60Pgm.png)

<br />

## Connecting Ubuntu 22.04.1 to AD

I wanted to expand the lab to include systems other than Windows, so I decided to connect Ubuntu to AD. This was by far the most challenging portion of the lab. One is led to believe that while installing Ubuntu you can simply choose to use AD in the setup, and all will be fine. I never got it to succeed and always ended up getting a failed message. After searching the web, it appears to be a common issue and it took several YouTube videos, official documentation, and various websites to develop a workaround that would allow my VM to connect with AD. 

#### STEPS

Computer Name: LINUX <br>
Domain Name: mydomain.com <br>
Host Name: LINUX.mydomain.com <br>
Administrator Account: username

  1. Create your Ubuntu VM, update everything, and take a snapshot so you can easily go back if something goes wrong. Network settings will be the same as your  Windows 10 Pro VM.
  3. Open up the command line interface.
  5. Verify you can ping the DC and that the DC can ping back.
  6. Set the host name for the machine: <br> ```sudo hostnamectl set-hostname LINUX.mydomain.com```
  7. Verify the host name: <br> ```hostnamectl```
  8. Install the following: <br> ```sudo apt install sssd-ad sssd-tools realmd adcli```
  9. Discover the DC: <br> ```sudo realm -v discover mydomain.com```
  10. Install the following: <br> ```sudo apt-get install -y krb5.conf```
  11. Edit the krb5.conf file. (Capilization matters, verify default_realm is your DC and add rdns = false) <br> ```sudo nano /etc/krb5.conf``` <br> <br>
![.conf edit](https://i.imgur.com/uTKdqMWl.png)
  11. Obtain Kerberous ticket with an account that has admin priviledges: <br> ```kinit username```
  12. Connect the system to the DC: <br> ```realm join -v -U username mydomain.com```
  14. Verify you have can see random users in your AD: <br> ```id user@mydomain.com``` <br> <br> ![access to AD](https://i.imgur.com/vrfmAnDl.png)
  15. Now log off your primary account and pick a random user in AD. <br> ```user@mydomain.com``` <br> <br> ![Linux Logon](https://i.imgur.com/TAy4kSNl.png)
  16. Finally, once Ubuntu does the initial setup, open the command line interface and verify: <br> ```who``` <br> <br> ![who](https://i.imgur.com/s2djFZ3l.png)

## Conclusion

In this project, VirtualBox was used to create an integrated AD environment. The first VM was Windows Server 2019 that served as the DC, DHCP, and Active Directory populated with approximately 1000 users. Once the networking configurations were complete for the server, I then created two more VMs and connected them to an internal network using Active Directory. I found the initial setup for the two Windows machines straightforward; however, I did encounter difficulties by adding the additional goal of connecting an Ubuntu VM to Active Directory. After researching and testing I was able to find a solution that successfully fixed the problem and have created a project showing how AD can provide centralized management of user accounts, computers, and other network resources.   

![AD Computers](https://i.imgur.com/yGbJT3Il.png)
![DHCP Reservations](https://i.imgur.com/BHs2LzLl.png)

<br />

## Credits

This project was based off YouTube content created by [Josh Madakor](https://www.youtube.com/watch?v=MHsI8hJmggI&t=8s).


