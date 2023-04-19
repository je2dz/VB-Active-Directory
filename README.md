# Integrated Active Directory Environment in VirtualBox

## Introduction

For this project, I utilized VirtualBox to create an Active Directory (AD) environment consisting of three virtual machines (VM). The first VM consisted of Windows Server 2019 which acted as the Domain Controller (DC) and AD provider. A custom PowerShell script by Josh Madakor, was then executed to populate AD with approximately 1000 users. Two more VMs (Windows 10 Pro & Ubuntu) were created and integrated into AD to create a centralized management system for user accounts, computers, and other network resources. 

<br />

## The Architecture of the Lab

![AD ARCH](https://i.imgur.com/JW4alB6.png)

<br />

## Technologies and Components Utilized:

- Active Directory
- AD Domain Service
- NAT
- RAS
- DNS
- Networking
- PowerShell
- Oracle VirtualBox
- Windows Server 2019
- Windows 10 Pro
- Ubuntu 22.04.1

<br />

## Windows Server 2019 Setup

Key points to remember:

- When creating the VM you need two NICs. <br> ![int nw](https://i.imgur.com/ITXfdqSl.png) <br> ![Internet nw](https://i.imgur.com/1pGAsAil.png)
- During the setup sequence, be sure to choose the "Desktop Experience" so you have GUI: <br> ![server os](https://i.imgur.com/XyC9XDKl.png) 
- When you get to the logon screen it asks you to hit Ctrl+Alt+Del which ends up only affecting your host. For VirtualBox there are two options. You can click Input > Keyboard > Insert Ctrl+Alt+Del. Or there is an option to use Host+Del which is the default setting of RCtrl+Del.
-

<br />

## Connecting Windows 10 Pro to AD

Key points to remember:

- When creating the VM you only need one NIC and it needs to be attached to the internal network. <br> ![int nw](https://i.imgur.com/ITXfdqSl.png)
- During the install ensure you only create a local account. Don't create or login to a Microsoft account.
- Change the system's name. Don't click the standard "Rename this PC" button. Instead scroll down and click "Rename this PC (advanced)". You can now change the name and join the domain. <br> ![Change name and join domain](https://i.imgur.com/96BzAlim.png)
- After you join the domain you can choose one of the random users you created earlier to logon. <br> ![Change name and join domain](https://i.imgur.com/ToE60Pgm.png)
- Configuring the adapters:





<br />

## Connecting Ubuntu 22.04.1 to AD

I wanted to expand the lab to include systems other than Windows, so I decided to connect Ubuntu to AD. This was by far the most challenging portion of the lab. One is led to believe that while installing Ubuntu you can simply choose to use AD in the setup, and all will be fine. I never got it to succeed and always ended up getting a failed message. After searching the web, it appears to be a common issue and it took several YouTube videos, official documentation, and various websites to develop a fix that works for my project. 

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
![AD Users](https://i.imgur.com/CG0Pu8vl.png)

<br />

## Credits

This project was based off YouTube content created by [Josh Madakor](https://www.youtube.com/watch?v=MHsI8hJmggI&t=8s).


