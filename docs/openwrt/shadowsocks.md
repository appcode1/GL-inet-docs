# GL Router Shadowsocks(SS) Setup Manual

You will learn how to set up shadowsocks server and client on the mini router in this guide.



## 1.SSH to the router	

In order to set up ss server, you need to have basic tools to ssh to the server.
You don't need to do this if you are setting up ss client on the router.

### 2.1. Windows User: 

#### 2.1.1. Download and install a PuTTY for windows user:

Go to the following webpage to download the latest PuTTY version：  

https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

#### 2.1.2 Install PuTTY for windows step by step

![52216437600](images/1522164376006.jpg)



![52216450379](images/PuTTY-Install-2.png)



![52216454338](images/PuTTY-Install-3.png)



![52216458386](images/PuTTY-Install-4.png)

####2.1.3. Launch PuTTY in Windows 

Click **PuTTY** in Start Menu 

![52216484291](images/1522164842915.png)



You will see the following Configuration Window: 

![52216510388](images/Setup-PuTTY-1.png)

**Input** Host Name (or IP address) "**192.168.8.1**", Keep Port as default "**22**", select connection type: "**SSH**",

**Input** "**your session**" in saved sessions, click **<u>S</u>ave** at right side.

![52216521565](images/Setup-PuTTY-2.png) 

**Click "<u>O</u>pen"** at the bottom

![52216531483](images/Setup-PuTTY-3.png)

A security alert will pop-up, click "**Yes**". 

![52219965352](images/SSH-in-root.png)

login as：**root**

![52216538050](images/SSH-in-1.png)

​	

​	root@192.168.8.1‘s password: **You need to use your password which you set up the router for the first time**



![52219922225](images/SSH-in-2.png)

When you see above picture, that means you are now ssh login the router, 

this picture shows we login the router (GL-AR750 Model) as root user.

### 2.2 Ubuntu User:

![buntu Logi](images/Ubuntu-Login.png)

Click "**Terminal**"

![52220472612](images/SSH-Login.png)

Input the following command: 

`user name@your computer name:~S SSH root@192.168.8.1` 

If you have ever connected to another router, host key verification failed may displayed as follow:

![52220528680](images/remove-ssh-keygen.png)

input the red highlight command:

`ssh-keygen -f "/home/username/.ssh/known_hosts" -R "192.168.8.1"`

![52220545833](images/Removed-Host-keygen.png)

you will see the known_hosts updated. 

retry the ssh login command: 

`user name@your computer name:~S SSH root@192.168.8.1` 

![52220556186](images/Ubuntu-sshin-router-1.png)

Type "**yes**"

![52220561601](images/Ubuntu-sshin-router-2.png) 	

Input your router password: (you can set this password when you first connect to your router)

![52220589633](images/1522205896331.png)

Final, you login the router when the above message displayed. 

## 3. Setup Shadowsocks Server

### 3.1 Edit Shadowsocks-server.json file

Input the following command to edit the configuration file "**shadowsocks-server.json**“ 

`root@GL-AR750:~# vi /etc/shadowsocks-sever.json` 

![52223877786](images/1522238777867.png)



switch to edit mode by press `i` on your keyboard, then you can change parameter in the configure file: 

1. ”Server“：**0.0.0.0** (default, don't modify)
2. **server_port”: 443**
3. "password":  ***your password***
4. "method": ***rc4-md5*** is default encrypt method, no need to modify it. 

when you finish all above modification, you can click "***Esc***" to exit edit mode, then click "***:***", type`wq` to ***write*** the modification into the configuration files and ***quit***. 

![52222196458](images/ssh-wq.png) 

 

### 3.2 Edit ss-server init file

Type `vi /etc/init.d/ss-server` in the command line

![52224765700](images/ss-server-config.png)

When you open ss-server configure file, you can see the following configuration

![VI-ss-verver](images/VI-ss-verver.png)

Press `i` to switch to edit mode, remove the "**#**" before "/usr/bin/ss-server - C /etc/shadowsocks-server.json -u &", then click "**Esc**", to exit edit mode, and click“**:**”, type`wq` to save and quit the configuration file.

### 3.3 Start SS-Server services

Input `/etc/init.d/ss-server start` , then the ss-serer services start on your router. 

![52302947938](images/1523029479389.png)

After you start ss-server, the above information will display. 

Don't start the server multiple times.

### 3.4 Open Port on the mini Router

![52291627764](images/1522916277644.png) 



Click "**Traffic Rules**" Tab to active external port forwarding in your network. 

![52291649689](images/1522916496892.png) 

Scroll down to the "Open ports on router" and input information as following: 

Name: "**your customs name**"

Protocol: "**TCP+UDP**"

External Port: "**443**"

Click "**Add**" after filling relate information and scroll down to the bottom, then click "**save&apply**" 

Now the port forwarding shall be activated and also the Shadowsocks Server is ready to use. 


### 3.5 Setup Port Forward via Management Page

You don't need to set up port forward if you are using the GL router as the main router. 

But if connect the mini router to your main router as a client, you need to set up port forward in your main router.

**Note, the following is set up port forwarder in another GL router which is the main router**

#### 3.5.1 Login web management page - advanced settings.

![52291532196](images/1522915321961.png) 

![52222434301](images/Advance-Root-Login.png)

A window will pop-up warning you to separately login as a root user into advanced setting. 

Click "***OK***"

![52222445742](images/1522224457420.png) 
Login with your password as a root user

![52222450124](images/1522224501240.png)  

Advance Setting Page Review 

#### 3.5.2 Enable Port Forwarding in Firewall Setup

![52291523169](images/1522915231698.png) 

Select "**Firewall**" in Network Pull-down Menu.



![52291540915](images/1522915409158.png) 



Select "**Port Forwards**" Tab, and the following tab will display. please input port forwards information. in the "New port forward:", fill following information. 

Name: "**Your customize Name**"

Protocol: "**TCP+UDP**"  

External Zone: "**wan**"

External Port: "**443**"

Internal Zone: "**lan**"

Internal IP address: "**192.168.1.130**" (Suppose your mini router's IP is 1.130)

Internal Port: "**443**" 

Then click "**add**",



![52291562679](images/1522915626797.png) 

A saved Port Forwarding item with **Enable** ticked will displayed 

![52291589794](images/1522915897942.png) 

Click "**Save & Apply**" to apply the port forwarding in this router.


## 4. Using Shadowsock in PC or smartphone

### 4.1 Find and Download the clients of your OS platform: 

https://shadowsocks.org/en/download/clients.html

### 4.2 Setup your client on different devices

Install the Shadowsocks Client on your device, then setup the following information:

Host: **your Public IP address** (you checked in step **4.2**)

Port: **443**

Password: **yourpassword** (same as you setup in ss-server)

Encryption: **rc4-md5** (same as you select in ss-server)

### 4.3 Check your public IP address

You can use any of your PC, laptop, tablet or smartphone to connect your Wi-Fi, then open a web browser (IE, Chrome, Safari, Firefox etc.)

Open any IP address checking website, the following websites are for your options: 

1. www.myipaddress.com
2. www.checkip.org 
3. https://www.whatismypublicip.com/
4. https://www.showmyipaddress.eu/
5. http://ip.w69b.com/

The webpage will detect and show your public IP address, record it. 


### 4.4 Start using Private Shadowsocks Services

After setup, you just start your shadowsocks on your devices, enjoy it. 

You can test or check whether it's workable by open a web browser on your smartphone(use 3G/4G data but not WiFi), then go to a IP address checking website to check if the IP address is same as your SS-server public IP address. 

# 3. Shadowsocks Client Setup on the mini router



![52290505328](images/1522905053283.png) 

Select "**Shadowsocks**" in the services pull-down menu. 

Click "**Servers Manage**" tab to setup SS-Client for GL-AR750 Router

![52303103540](images/1523031035400.png) 

Click "**Edit**", fill the following information: 

Server Address: "Your server's public IP"

Server Port: "443"

Password: "Your Password"

Encrypt Method: "RC4-MD5"

Click "**Save&Supply**", 


![	52303110376](images/1523031103769.png)

Now all devices that connected to the mini router can use shadowsocks directly.