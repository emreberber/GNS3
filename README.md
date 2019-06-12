## GNS3 Notes

<img src="gns3-logo.png" alt="gns3-logo">

###### Content :

- [02.1 Creating Loopback Interface in Ubuntu](#02.1)
- [02.2 Router Configuration](#02.2)
- [02.3 Router Telnet Configuration](#02.3)

- [03.1 Cisco IOS Command Line Modes](#03.1)
- [03.2 Basic Router Configuration](#03.2)

- [04.1 Router SSH Configuration](#04.1)

<hr>

##### 02.1 Creating Loopback Interface in Ubuntu {#02.1}

```
$ sudo apt install uml-utilities
$ sudo modprobe tun
$ sudo tunctl 
Set 'tap0' persistent and owned by uid 0


$ sudo ifconfig tap0 192.168.2.10 netmask 255.255.255.0 up
tap0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.2.10  netmask 255.255.255.0  broadcast 192.168.2.255
        ether 76:d4:97:d1:8d:db  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


# set default gateway address for tap network
$ sudo ip route add default via 192.168.2.1 dev tap0


$ sudo ifconfig tap0 down
$ sudo ifconfig tap0 up


$ sudo tunctl -d tap0
Set 'tap0' nonpersistent
```

##### 02.2 Router Configuration {#02.2}

```
R3# conf t
R3(config)# interface fastEthernet 0/0
R3(config-if)# ip address 192.168.2.1 255.255.255.0
R3(config-if)# no sh


*Jun  7 00:30:28.379: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up
*Jun  7 00:30:29.379: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up


R3(config-if)# end
R3#
*Jun  7 00:31:33.147: %SYS-5-CONFIG_I: Configured from console by console


R1# ping 192.168.2.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.2.10, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 8/11/16 ms
```

##### 02.3 Router Telnet Configuration {#02.3}

```
R1# conf t
R1(config)# line vty 0 4
R1(config-line)# password 1234
R1(config-line)# login
R1(config-line)# exit
R1(config)# enable secret 5678
```

```
$ telnet 192.168.2.1
Trying 192.168.2.1...
Connected to 192.168.2.1.


User Access Verification


Password: (1234)


R1>en   
Password: (5678)
R1#
```

##### 03.1 Cisco IOS Command Line Modes {#03.1}

```
1.User Mode
Router>


2.Privileged mode
Router#


3.Global Configuration Mode
Router(config)#


3.1 Interface mode (Router physical interface configuration mode)
Router(config-if)#


3.2 Subinterface mode (Router sub-interface configuration mode)
Router(config-subif)#


3.3 Line mode (Router line configuration mode - console, vty etc.)
Router(config-line)#


3.4 Router configuration mode (Routing protocols configuration mode.)
Router(config-router)#
```

```
Router>enable
Router# 


Router#disable
Router>


Router#configure terminal
Router(config)#


Router(config)#exit
Router#


Router(config)#interface fa0/0
Router(config-if)#


Router(config-if)#exit
Router(config)#


Router(config-if)#end
Router#
```

##### 03.2 Basic Router Configuration {#03.2}

```
R1#disable
R1>


R1> en
R1#


R1#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#


# Hostname
R1(config)#hostname MyRouter
MyRouter(config)#


MyRouter(config)#enable password 1234
MyRouter(config)#enable secret 5678


# Console
MyRouter(config)#line console 0
MyRouter(config-line)#password 2468
MyRouter(config-line)#login
MyRouter(config-line)#exit
MyRouter(config)#


# Telnet 
MyRouter(config)#line vty 0 4
MyRouter(config-line)#password 1357
MyRouter(config-line)#login


MyRouter(config)#banner motd "Welcome Emre Berber's Router"


# Assign IP Adress 
MyRouter(config)#interface fastEthernet 0/0
MyRouter(config-if)#ip address 192.168.2.1 255.255.255.0
MyRouter(config-if)#no sh
```

```
# Telnet Connection
$ telnet 192.168.2.1
Trying 192.168.2.1...
Connected to 192.168.2.1.
Escape character is '^]'.
Welcome Emre Berber's Router




User Access Verification




Password: (1357)
MyRouter>


MyRouter>en
Password: (5678)
MyRouter#
```

```
# ?
MyRouter#clock ?
  read-calendar    Read the hardware calendar into the clock
  set              Set the time and date
  update-calendar  Update the hardware calendar from the clock




MyRouter#clock set ?
  hh:mm:ss  Current Time




MyRouter#clock set 03:14:30 ?
  <1-31>  Day of the month
  MONTH   Month of the year




MyRouter#clock set 03:14:30 08 jun 2019 
MyRouter#




MyRouter#show clock
03:15:19.055 UTC Sat Jun 8 2019
```

```
R1#show running-config 
Building configuration...




Current configuration : 1554 bytes
!
! Last configuration change at 17:03:39 UTC Sat Jun 8 2019
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$b6Li$rmsOlhTPAwTFfwdGVfDiP.
!
no aaa new-model
no ip icmp rate-limit unreachable
!         
!         
!         
!         
!         
!         
no ip domain lookup
ip cef    
no ipv6 cef
!         
multilink bundle-name authenticated
!         
!         
!         
!         
!         
!         
!         
!         
!         
!         
redundancy
!         
!         
ip tcp synwait-time 5
!         
!         
!         
!         
!         
!         
!         
!         
!         
!         
interface FastEthernet0/0
 ip address 192.168.2.1 255.255.255.0
 duplex half
!         
interface GigabitEthernet1/0
 no ip address
 shutdown 
 negotiation auto
!         
interface Serial2/0
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/1
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/2
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/3
 no ip address
 shutdown 
 serial restart-delay 0
!         
ip forward-protocol nd
no ip http server
no ip http secure-server
!         
!         
!         
no cdp log mismatch duplex
!         
!         
!         
control-plane
!         
!         
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!         
mgcp profile default
!         
!         
!         
gatekeeper
 shutdown 
!         
!         
line con 0
 exec-timeout 0 0
 privilege level 15
 password 2468
 logging synchronous
 login    
 stopbits 1
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
line vty 0 4
 password 1357
 login    
 transport input all
!         
!         
end       


          
R1#conf t
R1(config)#service password-encryption 


# show running-config run on config mode
R1(config)#do show running-config 
Building configuration...




Current configuration : 1567 bytes
!
! Last configuration change at 17:03:39 UTC Sat Jun 8 2019
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$b6Li$rmsOlhTPAwTFfwdGVfDiP.
!
no aaa new-model
no ip icmp rate-limit unreachable
!         
!         
!         
!         
!         
!         
no ip domain lookup
ip cef    
no ipv6 cef
!         
multilink bundle-name authenticated
!         
!         
!         
!         
!         
!         
!         
!         
!         
!         
redundancy
!         
!         
ip tcp synwait-time 5
!         
!         
!         
!         
!         
!         
!         
!         
!         
!         
interface FastEthernet0/0
 ip address 192.168.2.1 255.255.255.0
 duplex half
!         
interface GigabitEthernet1/0
 no ip address
 shutdown 
 negotiation auto
!         
interface Serial2/0
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/1
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/2
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/3
 no ip address
 shutdown 
 serial restart-delay 0
!         
ip forward-protocol nd
no ip http server
no ip http secure-server
!         
!         
!         
no cdp log mismatch duplex
!         
!         
!         
control-plane
!         
!         
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!         
mgcp profile default
!         
!         
!         
gatekeeper
 shutdown 
!         
!         
line con 0
 exec-timeout 0 0
 privilege level 15
 password 7 101C5D4F5D
 logging synchronous
 login    
 stopbits 1
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
line vty 0 4
 password 7 0257570E5C  # -> http://www.ifm.net.nz/cookbooks/passwordcracker.html
 login    
 transport input all
!         
!         
end       
          
R1(config)#
```

```
R1#copy running-config startup-config
Destination filename [startup-config]? 
Warning: Attempting to overwrite an NVRAM configuration previously written
by a different version of the system image.
Overwrite the previous NVRAM configuration?[confirm]
Building configuration...
[OK]
R1#
```


```
R1#show ip interface 
FastEthernet0/0 is up, line protocol is up
  Internet address is 192.168.2.1/24
  Broadcast address is 255.255.255.255
  Address determined by setup command
  MTU is 1500 bytes
  Helper address is not set
  Directed broadcast forwarding is disabled
  Outgoing access list is not set
  Inbound  access list is not set
  Proxy ARP is enabled
  Local Proxy ARP is disabled
  Security level is default
  Split horizon is enabled
  ICMP redirects are always sent
  ICMP unreachables are always sent
  ICMP mask replies are never sent
  IP fast switching is enabled
  IP fast switching on the same interface is disabled
  IP Flow switching is disabled
  IP CEF switching is enabled
  IP CEF switching turbo vector
  IP CEF turbo switching turbo vector
  IP multicast fast switching is enabled
  IP multicast distributed fast switching is disabled
  IP route-cache flags are Fast, CEF
  Router Discovery is disabled
  IP output packet accounting is disabled
  IP access violation accounting is disabled
  TCP/IP header compression is disabled
  RTP/IP header compression is disabled
  Policy routing is disabled
  Network address translation is disabled
  BGP Policy Mapping is disabled
  Input features: MCI Check
  IPv4 WCCP Redirect outbound is disabled
  IPv4 WCCP Redirect inbound is disabled
  IPv4 WCCP Redirect exclude is disabled
GigabitEthernet1/0 is administratively down, line protocol is down
  Internet protocol processing disabled
Serial2/0 is administratively down, line protocol is down
  Internet protocol processing disabled
Serial2/1 is administratively down, line protocol is down
  Internet protocol processing disabled
Serial2/2 is administratively down, line protocol is down
  Internet protocol processing disabled
Serial2/3 is administratively down, line protocol is down
  Internet protocol processing disabled
R1#
```

##### 04.1 Router SSH Configuration {#04.1}

```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#


R1(config)#hostname MyRouter
MyRouter(config)#interface fastEthernet 0/0
MyRouter(config-if)#ip address 192.168.2.1 255.255.255.0
MyRouter(config-if)#no sh
MyRouter(config-if)#exit
MyRouter(config)#


MyRouter(config)#ip domain-name emre.com
MyRouter(config)#cryp
MyRouter(config)#crypto key gene
MyRouter(config)#crypto key generate rsa
The name for the keys will be: MyRouter.emre.com
Choose the size of the key modulus in the range of 360 to 4096 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.




How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...
[OK] (elapsed time was 2 seconds)


MyRouter(config)#username emre password 1234
MyRouter(config)#enable password en1234




MyRouter(config)#line vty 0 4 
MyRouter(config-line)#trans
MyRouter(config-line)#transport inpu
MyRouter(config-line)#transport input ssh
MyRouter(config-line)#login local
MyRouter(config-line)#end
MyRouter#copy runn
MyRouter#copy running-config start
MyRouter#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
MyRouter#
```

- * https://ma.ttias.be/ssh-error-unable-negotiate-ip-no-matching-cipher-found/

```
$ ssh -c aes256-cbc emre@192.168.2.1


MyRouter>en
Password: 


MyRouter#show ip ssh 
SSH Enabled - version 2.0
Authentication methods:publickey,keyboard-interactive,password
Authentication timeout: 120 secs; Authentication retries: 3
Minimum expected Diffie Hellman key size : 1024 bits
IOS Keys in SECSH format(ssh-rsa, base64 encoded):
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQCXEOq7tLvpLy/w6M75cXAyW5uykVMen6vbUtS/0uYq
u34fPkZGHfFx7KaovFdSb+lz/3MGxBDpzThU70LS9cDGBBv+UR1hk0p7HBNaRbNgaXQNFMz5P1ZOutzN
Mo/vfLvWA9fYk960E2AK4WflTpqqS8nXl2dYWarWXnisONu8sQ== 


MyRouter#show ssh
Connection Version Mode Encryption  Hmac     State                   Username
0          2.0     IN   aes256-cbc  hmac-sha1    Session started       emre
0          2.0     OUT  aes256-cbc  hmac-sha1    Session started       emre
%No SSHv1 server connections running.




MyRouter#show running-config 
Building configuration...




Current configuration : 1689 bytes
!
! Last configuration change at 21:56:29 UTC Sat Jun 8 2019
upgrade fpd auto
version 15.3
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname MyRouter
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
enable secret 5 $1$b6Li$rmsOlhTPAwTFfwdGVfDiP.
enable password 7 01160855095852
!
no aaa new-model
no ip icmp rate-limit unreachable
!         
!         
!         
!         
!         
!         
no ip domain lookup
ip domain name emre.com
ip cef    
no ipv6 cef
!         
multilink bundle-name authenticated
!         
!         
!         
!         
!         
!         
!         
!         
!         
username emre password 7 025756085F
!         
redundancy
!         
!         
ip tcp synwait-time 5
ip ssh version 2
!         
!         
!         
!         
!         
!         
!         
!         
!         
!         
interface FastEthernet0/0
 ip address 192.168.2.1 255.255.255.0
 duplex half
!         
interface GigabitEthernet1/0
 no ip address
 shutdown 
 negotiation auto
!         
interface Serial2/0
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/1
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/2
 no ip address
 shutdown 
 serial restart-delay 0
!         
interface Serial2/3
 no ip address
 shutdown 
 serial restart-delay 0
!         
ip forward-protocol nd
no ip http server
no ip http secure-server
!         
!         
!         
no cdp log mismatch duplex
!         
!         
!         
control-plane
!         
!         
mgcp behavior rsip-range tgcp-only
mgcp behavior comedia-role none
mgcp behavior comedia-check-media-src disable
mgcp behavior comedia-sdp-force disable
!         
mgcp profile default
!         
!         
!         
gatekeeper
 shutdown 
!         
!         
line con 0
 exec-timeout 0 0
 privilege level 15
 password 7 101C5D4F5D
 logging synchronous
 login    
 stopbits 1
line aux 0
 exec-timeout 0 0
 privilege level 15
 logging synchronous
 stopbits 1
line vty 0 4
 password 7 0257570E5C
 login local
 transport input ssh
!         
!         
end   
```
