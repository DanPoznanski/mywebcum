---
title: "Windows Server"
discription: Windows Server 
date: 2024-12-13T21:29:01+08:00 
draft: false
type: post
tags: ["Windows Server"]
showTableOfContents: true
--- 




![img1](images/logo.svg)



First i need change on server dns on localhost

![img2](images/2.png)


After change the name of server and restart

![img3](images/3.png)


## Add DC 

![img4](images/4.png)

Next
![img5](images/5.png)

Next
![img6](images/6.png)

Next
![img7](images/7.png)

Next 
![img8](images/8.png)

Next
![img9](images/9.png)

Next
![img10](images/10.png)

Next 
![img11](images/11.png)

Install
![img12](images/12.png)

Wait
![img13](images/13.png)

Promote this server to a domain controller
![img14](images/14.png)

dan.local name of my dc 
![img15](images/15.png)

Next
![img16](images/16.png)

Next
![img17](images/17.png)

Next 
![img18](images/18.png)

Next
![img19](images/19.png)

Next
![img20](images/20.png)

Install
![img21](images/21.png)

Wait
![img22](images/22.png)

Done !

### DNS Reverse IP

![img23](images/23.png)


DNS > Reverse Lookup Zone > New Zone

![img24](images/24.png)

Next
![img25](images/25.png)

Next
![img26](images/26.png)

Next
![img27](images/27.png)


Enter Zone IP my its `192.168.1`
![img28](images/28.png)

Next
![img29](images/29.png)

Next
![img30](images/30.png)

Finish
![img31](images/31.png)

Done!
![img32](images/32.png)

### Create AD User

Tools > Active Directory Users and Computers
![img33](images/33.png)

Users > New > Users
![img34](images/34.png)

Next
![img35](images/35.png)

Next
![img36](images/36.png)

Finish
![img37](images/37.png)

Done
![img38](images/38.png)


### Connect User to Domain Windows 11

![img39](images/39.png)

dns of dc (dont forget disable dnsv6)
![img40](images/40.png)


![img41](images/41.png)