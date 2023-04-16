# Active Directory Environment in VirtualBox

## Introduction

For this project, I utilized Oracle's VirtualBox to create an Active Directory environment consisting of four virtual machines.  

<br />

## The Architecture of the Lab

![AD ARCH](https://i.imgur.com/ynBUfaH.png)

<br />

## Technologies and Components Utilized:

- Active Directory
- AD Domain Service
- NAT
- RAS
- DNS
- Networking
- Oracle VirtualBox
- Windows Server 2019
- Windows 10 Pro
- Ubuntu 22.04.1

<br />

## Windows Server 2019 Setup

<br />

## Connecting Windows 10 Pro to AD

<br />

## Connecting Ubuntu 22.04.1 to AD

I wanted to expand the lab to include systems other than Windows so I decided to connect Ubuntu to AD. This was by far the most challenging portion of the lab. One is led to believe that while installing Ubuntu you can simply choose to use AD in the intial setup and all will be fine. I never got it to succeed and always ended up getting a failed message. After searching the web it appears to be common issue and it took several YouTube videos, official documentation, and various web sites to develope a fix that works for me. 

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

In this project, I utilized VirtualBox to create three virtual machines. The first VM was Windows Server 2019 that served as the Domain Controller, DHCP, and Active Directory. Once the networking configurations were complete on the server I then created second VM using Windows 10 Pro. Lastly, I added a personal objective to create a third VM using Ubuntu and connect it to active directory also. 

<br />

## Credits

This project was based on YouTube content created by [Josh Madakor](https://www.youtube.com/watch?v=MHsI8hJmggI&t=8s).


