---
title: "Windows Server Terminal (RDS)"
discription: Windows Server
date: 2025-05-01T21:29:01+08:00 
draft: false
type: post
tags: ["Terminal","RDP","Windows Server","CAL"]
showTableOfContents: true
--- 


## Termenal for Free

![img0](images/0.webp)

### Add Terminal to  Domain

1. set DNS of DC server (if not work disable IPv6)
![img1](images/1.webp)

2. Change 
![img3](images/3.webp)

3. Choose domain my domain `dan.local`
![img4](images/4.webp)

4. Done !
![img6](images/6.webp)

### Troubleshooter Problems

1. If  doesn't connect to domain - disable ipv6
![img5](images/5.webp)

2. Network Level Authentication 
![img7](images/7.webp)

9. Run `sysdm.cpl` 
![img9](images/9.webp)

8. Disable NLA
![img8](images/8.webp)



### Create Terminal and Adds Roles 

10. Manage > Add Roles and Features
![img10](images/10.webp)

11. Next 
![img11](images/11.webp)

12. Select Role-based or feature-based installation and Next
![img12](images/12.webp)


13. Next 
![img13](images/13.webp)


14. [x] Remote Desktop Services , Next 
![img14](images/14.webp)

15. [x] NET. Framework 4.8 Features, Next
![img15](images/15.webp)

16. [x] Remote Desktop Licensing, Next 
![img16](images/16.webp)

17. Add Features
![img17](images/17.webp)

18. [x] Remote Desktop Licensing Diagnoser, Next 
![img19](images/19.webp)

19. Add Features
![img18](images/18.webp)

20. Install
![img20](images/20.webp)

21. After install Close
![img21](images/21.webp)

> Done !


### RD Licensing Activation for Free

1. Open RD Licensing Manager
![img22](images/22.webp)

2. Active Server, Next
![img23](images/23.webp)

3. Next
![img24](images/24.webp)

4. Open https://activate.microsoft.com/

5. Select Activate a license server > Next
![img25](images/25.webp)

6. Fill all and Next 
![img26](images/26.webp)

7. ID Product you can find here :
![img2](images/2.webp)



8. Next
![img27](images/27.webp)

9. Copy and save activatin key
![img28](images/28.webp)

10. Past Key here:
![img29](images/29.webp)

11. [x] and Next
![img30](images/30.webp)

12. Next
![img31](images/31.webp)

13. Open https://activate.microsoft.com/ and Choose `Install Client Access Licenses`
![img32](images/32.webp)

14. Past key and choose `Enterprise agreement` and Next
![img33](images/33.webp)

15. Choose per device or per user and Enterprise agreement Number (take random from google.com)
![img34](images/34.webp)

16. Difference:
![img35](images/35.webp)

17. Next
![img36](images/36.webp)

18. Copy New Licence Key
![img37](images/37.webp)

19. Past New Licence Key to Server
![img38](images/38.webp)

20. Finish
![img39](images/39.webp)

21. Done !
![img40](images/40.webp)

### Add GPO rules 

1. Run `gpedit.msc`


2. Go to Computer Configuration > Administrative Templates > Windows Components > Remote Desktop Services > Remote Desktop Session Host > Licensing and configure the following options:

3. **Use the specified Remote Desktop license servers** – specify the name or the IP address of the server where the RDS license is installed;
![img42](images/42.webp)

4. **Set the Remote Desktop licensing mode** – select the license type for RDS CALs.
![img41](images/41.webp)
