

#Guide to Configure a Network in Packet Tracer  

##Network Topology  
Below is the topology we will use, including its respective **IPs**, **names** , and **services**:  

> ![Topology](./topológia.png)

---

# IP Address Configuration
The first step will be to assign the different **IPs**  to the router's interfaces.

---

### 1. Access the Router Terminal 
Open the device terminal in Packet Tracer from the CLI tab on the router.  

### 2. Enter Privileged EXEC Mode
Type the following command:  
`enable`  

### 3. Enter Global Configuration Mode
Type the following:  
`configure terminal`

### 4. Access the Interface
Choose the interface you want to configure. For example, for interface fa0/0, use:  
`interface fa0/0`  

### 5. Assign an IP Address  
You must assign a valid IP address. If the network address is 190.190.16.0, the first available IP will be 190.190.16.1, which will serve as the default gateway for end devices.
Use the following command to assign the IP with its subnet mask (/24):  
`ip address 190.190.16.1 255.255.255.0`  

### 6. Enable the Interface  
By default, interfaces are turned off. We need to enable them, and once activated, the interface will appear green in the topology. Use:  
`no shutdown`  

>_Use a consistent subnet mask throughout the topology; in this case, we will only use the /24 mask._  

>_Continue configuring each interface individually using different IP addresses. (If on one end we configure "x.x.x.1," on the other end, it will be "x.x.x.2")_   

>_Do not assign IPs to interfaces where no IPs are specified in the topology; VLANs will be configured later._  

### 7. Verify the configuration  
To view the active configuration, type:
`show running-config`

---

# Server Services Configuration  

---

In this step, we will configure services that servers can provide, such as DHCP, DNS, TFTP, and HTTP.  

## Steps to Follow  

### 1. Assign IPs to the Servers  
Open the "Desktop" tab and, within Desktop, the "IP Configuration" tab. Assign an IP to each server.
_For example, if the network is 190.190.32.0, the default gateway will be 190.190.32.1, and we will assign IPs starting from there._ 

### 2. Activate the Service   
After assigning an IP, activate the service by going to the "Services" tab and selecting the desired service.  

### 3. Configure the Service  
In this step, we will configure multiple services:  

> **HTTP:**  To configure the HTTP service, enter the HTTP service tab and ensure it is set to "on." The files you see are the page files that will be displayed when accessing your HTTP or HTTPS service.  

> **DNS:**  To configure the DNS service, access the DNS tab and add a name (we will use `www.dyla.com`) in the "Name" field and the IP of the HTTP server in the "Address" field, which in our case is (190.190.32.4). This allows `www.dylan.com` to be translated to the HTTP server's IP and displays the previously mentioned webpage.  

> **TFTP:**  To configure the TFTP service, which will function as a storage or repository for firmware and configuration files (e.g., the router's NVRAM), access the TFTP section and ensure it is activated. [Here you can see how to use it](./Cheatsheet.md)  or refer to the guide later.  

> **DHCP:**  We will leave it for later to follow the guide step by step.

---

# VLAN Creation  
The next step is to create VLANs. These must be created on Layer 2 switches and Layer 3 switches.

---

## Steps to Follow:  

### 1. Access the Layer 2 Switch Configuration Terminal 
Open the CLI tab from the switch's menu.  

### 2. Enter Privileged EXEC Mode  
Type the following command:
`enable`

### 3. Enter Global Configuration Mode
Type the following:  
`configure terminal`  

### 4. Create VLANs  
In the switches, all VLANs in the same network must be created to identify which device to send the packet to. To create VLANs 10, 50, 20, 30, 40, and 60. VLANs 20 and 40 are for IP telephony, so they will be configured differently.
_"Remember that routers divide networks."_
To create them, type:
**switch1** 
`vlan 10`
`vlan 50`
**switch2** 
`vlan 20`
`vlan 30`
**switch3** 
`vlan 40`
`vlan 60`  

### 5. Configure Trunk Mode  
This option allows all VLANs to travel over a medium without being part of the VLAN itself. It is applied on interfaces between switches or those connected to routers, allowing packets to travel:  
`switchport mode trunk`  

### 6. Assign VLANs to Ports
To assign a VLAN to end devices, for example, PC0 with VLAN 10:
`switchport mode access`
`switchport access vlan 10`  

### 7. Assign Voice VLANs  
After assigning a VLAN, to configure a voice VLAN this must be configured after the common VLAN. if it doesn't exsists (for example, for the first phone):  
`switchport voice vlan 20`  

### 8. Verify the configuration  
To view the active configuration, type:
`show running-config`

---

# IP Address Configuration in VLANs  

---

## Steps to Follow:  

### 1. Assign IP Addresses to VLANs  
IP addresses must be configured from the Layer 3 switch and the router.  

__From the Layer 3 switch's global configuration mode, type:__   
`interface vlan 10`  
`ip address 10.0.0.1 255.255.255.0`  
>_Where "10.0.0.1" is the IP that we desire to assign and "255.255.255.0" is the subnet mask wich in this case is /24_

__From the router's global configuration mode, type:__  
`interface fa0/0.60`  
`encapsulation Dot1q 60`  
`ip address 60.0.0.1 255.255.255.0`  
>_Where "fa0/0.60" it's use to refer the subinterface wich we wanto to create._  

>_The "encapsulation Dot1q" part allows the router to handle traffic from multiple VLANs on a single physical interface._  

>_For an IP telephony VLAN, it would be done in the same way_  


---

# Routing Configuration  

---

## Steps to Follow:  

### 1. OSPF Routing  
In this case, we will use OSPF as the routing protocol, which will allow end and intermediate devices to communicate without being on the same network.  

- **From the Layer 3 switch's global configuration mode:**  
`ip routing`  
`router ospf 100`  
`network 10.0.0.0 0.0.0.255 area 0`  
> *Where "ip routing" is used to enable routing on a Layer 3 switch.*  

> *The "100" part is the identifier of the OSPF process within the router, used to distinguish between different possible processes, ranging from 1 to 65535.*  

> *The "0.0.0.255" part is the wildcard, which is used to identify the network portion and the host portion. Here is a link to learn how to calculate a wildcard [LINK](https://hacks4geeks.com/hacks/calcular-wildcards-de-mascaras-de-red/)*  

> *The "area 0" code section is used to associate networks with the main OSPF area, which is usually "0" (Backbone Area).*

- **From the router's global configuration mode:**  
`router ospf 100`  
`network 60.0.0.0 0.0.0.255 area 0`  

>*The "100" part is the OSPF preccess identifier withi the router, used to distinguish between different possible process. It can range from 1 to 65535*  

>*The "0.0.0.255" part is the wildcard, usedo to identify the network portion and the host portion. Here's a link to learn how to calcculate a wildcard [LINK](https://hacks4geeks.com/hacks/calcular-wildcards-de-mascaras-de-red/)*  

>*The "area 0" code section is used to associate networks with the main OSPF area, wich is usually "0" (Backbone Area)*  

### 2. Verify the Routing table
To view the routing table, enter:  
`show ip route`

---

# DHCP Configuration  

---

## Steps to Follow:

### 1. DHCP Address Pool Creation   
We need to create two DHCP address pools one for phones and one for network devices. We will need to create two types of DHCP address pools on the Leon router and the Alicante router as follows   

**For the DHCP address pool for devices :**  
From the global configuration mode of a router or a layer 3 switch, enter the following:   
`ip dhcp pool equipos`  
`network 60.0.0.0 255.255.255.0`  
`default-router 60.0.0.1`  
`dns-server 190.190.32.2`  
>*Where "equipos" is the name we will assign to DHCP address pool*  

>*"network 60.0.0.0 255.255.255.0" defines the network range in wich the IPs will be assigned*  

>*"default-router" specifies the default gateway*  

>*"dns-server" specifies wich DNS server will be assigned along with the IPs*


**For phones:**   
From the global configuration mode of a router or a layer 3 switch, enter the following:  
`ip dhcp pool telefonos`  
`network 40.0.0.0 255.255.255.0`  
`default-router 40.0.0.1`  
`option 150 ip 40.0.0.1`   
>*Where "telefonos" is the name we are assigning to the DHCP address pool.*  

>*network 40.0.0.0 255.255.255.0" defines the network range within which the IPs will be assigned.*  
  
>*default-router" specifies the default gateway.*  

>*option 150 ip 40.0.0.1" is used to automate the configuration of the phones, and the IP defines the TFTP server. In this case, the router will act as the TFTP server, containing firmware repositories, configuration files, etc.*  

---

# IP Telephony Configuration   

---


## Steps to Follow: 

### 1. Configure the Router as Call Manager Express (CME) 
Once the DHCP set is created, we are ready to configure CME, which allows the router to act as a telephony server. From the global configuration mode of a router, use the following commands:
*(In this case, we will use the router "Alicante" as an example)*  

Enable the telephony functionality with:


`telephony-service `

`max-ephones 4`
`max-dn 4`
`ip source-address 40.0.0.1 port 2000` 
`auto assign 1 to 3`

> *"telephony-service" is used to enter the telephony submode.*  
  
> *"max-ephones" specifies the maximum number of ephones that can be configured in CME.*  


> *"max-dn" specifies the maximum number of directory numbers (DN) that can be configured in CME.*  


> *"ip source-address 40.0.0.1 port 2000" specifies the gateway used for communication by the phones and the port they will use.*  


### 2. Configure the Phones 
To assign a number to the phones, from the global configuration mode of the router configured earlier, use the following:  

**Phone 1**   
`ephone-dn 1`  
`number 2000`
  

**Phone 2**   
`ephone-dn 2`  
`number 2001`
  

**Phone 3**  
`ephone-dn 3`  
`number 2002`
  

> *"ephone-dn" is used to enter the phone configuration menu, in this case, for phones 1, 2, and 3.*  

> *"number" is used to assign a number to the phone.*  

### 3. Configure Call Routing 

To enable the phones to communicate with others outside their network, from the global configuration mode of the router, enter:

```sql
dial-peer voice 1 voip  
destination-pattern 1...  
session target ipv4:190.190.0.201
```
**You will need to repeat this configuration on the "León" router to enable telephony communication.**   

> *"dial-peer voice 1 voip" is used to route voice calls, where "1" is the identifier and "voip" specifies VoIP call routing.*  

> *"destination-pattern 1..." specifies the range of numbers to communicate with. "1..." means any number starting with "1" and containing four digits.*  

> *"session target ipv4:190.190.0.201" specifies the destination IP address for the call. Calls matching the 1... pattern will be sent to the IP 190.190.0.201.*  

### 4. Verify Configuration 
To view the active configuration, type:

`show running-config`


---


# DHCP Configuration 


---


## Steps to Follow: 

To assign IPs dynamically, we need DHCP. In this case, we will configure DHCP using a router and a server.

### From a Server 
---
### 1. Open the Menu 

Click on the server, open the "Services" tab, and select DHCP.

#### Configure the Scope  
> To configure the DHCP scope, you need to enter a **pool name**.  

> You will also need a **Default Gateway** , which will be the gateway of the network you want to manage IPs for, in this case, 190.190.16.1.  

> You also need a **DNS server** , which in our case is 190.190.32.2.  

> Just below, you'll find the **"Start IP Address"**  field. Enter the IP address where the assignment should start; this field is used for "exclusions." We will start at 190.190.16.11 with the subnet mask 255.255.255.0.  

### 2. Configure the Interface  
To enable IP assignment to end devices, configure the router interface, in this case, "CACERES," from global configuration mode:
`ip helper-address 190.190.32.3`  

> This designates the server with IP "190.190.32.3" as the DHCP server. *Do the same for the other interface.*   

#### *To continue the guide, you must create another DHCP scope for the network 190.190.100.0.*  



### From a Router  
---


#### In this example, we will use a router. It differs slightly from server configuration, and we will create a scope to assign IPs to the "TOLEDO" network, 190.190.200.0. 

### 1. Create a DHCP Pool 

From the global configuration mode, type:

```arduino
ip dhcp pool toledo  
network 190.190.200.0 255.255.255.0  
default-router 190.190.200.1  
dns-server 190.190.32.2
```

> *"toledo" is the name we assign to the DHCP address pool.*  

> *"network 190.190.200.0 255.255.255.0" defines the range of IPs and the subnet mask to be assigned.*  

> *"default-router" specifies the default gateway.*  

> *"dns-server" specifies the DNS server to assign along with the IPs.*  

> *Since the 190.190.200.0 network is connected to the "TOLEDO" router, there is no need for "ip helper-address" as it recognizes itself as the DHCP server.*

---

# Copying a Router's Configuration to a TFTP Server 

---


## Steps to Follow: 

### 1. Identify the Configuration to Copy 

To select the correct configuration to copy, you need to understand the different memories of a router. Wich we had explained in[Concepts](./Concepts.md).

### 2. Copy the Configuration to Memory 

Once the configuration to copy is identified and the TFTP server is active, from privileged EXEC mode, type:

`copy running-config startup-config`  
> This copies the active configuration to non-volatile memory.  

### 3. Copy the Memory to the TFTP Server 

To copy the memory to the TFTP server, type:

`copy startup-config tftp`  
`190.190.32.2`  

> *"copy startup-config tftp" indicates the memory to copy and the storage destination.*  
>*"190.190.32.2" is the IP address of the TFTP server.*

---

# Standard ACL Configuration 

---

**_For the configuration of extended ACLs, we have created two copies of the topology to avoid interference with the configuration of different ACLs and with IP address translation, which we will address later._**  
> To configure an ACL, it is necessary to know where it should be applied. This is explained in [Concepts](./Concepts.md) . We will use the scenario [Statement](./enunciado.md)  as an example to explain ACLs.
## Steps to Follow: 

### 1. Create the Standard ACL 
 
- **Numbered Standard ACL** 
A numbered standard ACL is created from the global configuration mode, as shown in the example below:


```arduino
ip access-list 1 permit host 190.190.100.11  
ip access-list 1 permit host 190.190.100.12  
ip access-list 1 permit host 190.190.200.26  
ip access-list 1 permit host 190.190.200.27  
ip access-list 1 deny 190.190.100.0 255.255.255.0  
ip access-list 1 deny any
```

> *"ip access-list 1" creates an ACL with the number 1.*  

>*"permit 190.190.x.x" allows the device with that IP address to access the network of servers.*  

>*"deny 190.190.100.0 255.255.255.0" denies access from the 190.190.100.0 network to the server network.*  

>*"deny any" blocks all other traffic. Although traffic is implicitly denied when creating an ACL, we explicitly add this because it is required by the scenario.*   

- **Named Standard ACL** 
A named standard ACL is also created from the global configuration mode. For example:  
```arduino
ip access-list standard telnet  
permit 190.190.100.0 255.255.255.0  
deny any
```  

> *"ip access-list standard telnet" creates and enters the configuration menu for the "telnet" ACL.*  

>*"permit 190.190.100.0 255.255.255.0" allows the 190.190.100.0 network to access the router Madrid's VTY lines.*  

### 2. Apply the ACL to the Interface 
ACLs are applied to interfaces similarly. We will distinguish between using a numbered standard ACL for network interfaces and a named standard ACL for controlling the VTY lines on the router Madrid.  
 
- **Numbered Standard ACL**   
To assign the ACL to an interface, in this case on the "AVILA" router, enter the following commands in global configuration mode:  

`interface fa0/0`  
`ip access-group 1 out`  

> *"interface fa0/0" selects the interface.*  

>*"ip access-group 1 out" assigns ACL number 1 to the interface for outbound traffic.*   

- **Named Standard ACL for VTY** 
To assign the ACL to the VTY lines on the "MADRID" router, use the following commands in global configuration mode:  

`line vty 0 4`  
`access-class telnet in`  

>*"line vty 0 4" accesses the VTY lines (0 to 4, for five simultaneous connections).*  
>*"access-class telnet in" specifies that the "telnet" ACL is applied for incoming traffic.*  

### 3. Verify ACL Creation   
To display the configured ACLs, use the command:  
`show access-lists`

---

# Extended ACL Configuration 

---

> To configure an ACL, it is necessary to know where it should be applied. This is explained in [Concepts](./Concepts.md) . We will use the scenario [Statement](./enunciado.md)  as an example to explain ACLs.
## Steps to Follow: 

### 1. Create the Extended ACL  
In this example, we will create named extended ACLs with different transport protocols and services.  
 
- **On the "CACERES" Router:**

```r
ip access-list extended cáceres  
deny ip host 190.190.16.11 190.190.32.0 0.0.0.255  
deny ip host 190.190.100.14 host 190.190.32.5  
permit ip 190.190.16.0 0.0.0.255 190.190.32.0 0.0.0.255  
permit ip 190.190.100.0 0.0.0.255 190.190.32.0 0.0.0.255
```

> *"host" specifies a single host, so no wildcard mask is needed.*
*"ip" refers to the IP protocol being controlled by the ACL.* 
- **For Another Protocol Example:**


```arduino
ip access-list extended ping  
deny icmp host 190.190.16.14 host 190.190.200.30  
deny icmp host 190.190.16.14 190.190.100.0 0.0.0.255  
permit icmp any any
```

> *"icmp" refers to the ICMP protocol, used for pinging devices.*
*"any" specifies any host or network.* 
- **On the "LEON" Router:**


```arduino
ip access-list extended tftp  
permit udp host 192.192.0.193 host 190.190.32.5 eq tftp  
deny udp any any
```

> *"udp" specifies the UDP protocol.*
*"eq" means "equals," used to specify a specific port or protocol, in this case, TFTP.*
### 2. Apply the Extended ACL 
 
- **For the "CACERES" ACL:**

```kotlin
interface se/2/0  
ip access-group caceres out
```
 
- **For the "Ping" ACL:**

```kotlin
interface fa0/0  
ip access-group ping in
```
 
- **For the "TFTP" ACL:**

```kotlin
interface se0/0/1  
ip access-group tftp out
```

### 3. Verify ACL Creation 
Use the command:
`show access-lists`


---

# NAT and PAT 

---

## Steps to Follow: 
In this section, we will configure the translation of local IP addresses to global IP addresses. This will be done on the border router, which is the last router before the ISP—in our case, the router "MADRID." We will configure NAT and PAT step by step using the scenario outlined in the [ENUNCIADO](./enunciado).  

### 1. Create ACLs for Translation 
 To perform dynamic translations, whether NAT or PAT, we need to create standard ACLs to specify the allowed or denied networks or hosts to be translated. We will create two ACLs: one for dynamic NAT and one for port-based PAT. From the global configuration mode on the "MADRID" router, execute the following:  

- **Dynamic NAT ACL:**


```Arduino
ip access-list standard NATDIN  
permit 190.190.16.0 0.0.0.255  
permit 190.190.200.0 0.0.0.3
```
 
- **PAT ACL:**


```Arduino
ip access-list standard PATPU  
deny host 190.190.200.27  
permit 190.190.200.0 0.0.0.255
```

### 2. Create the NAT IP Pool  
To perform dynamic translations, we need to create IP address pools, which are the public IPs to which our local IPs will be translated. We will create only one pool because port-based PAT translates local IPs to public IPs configured on the interface assigned to the PAT translation. From the "MADRID" router, enter:  

```sql
ip nat pool NATPUB 195.195.195.20 195.195.195.45 netmask 255.255.255.0
```  
>*"ip nat pool NATPUB" creates the IP pool, in this case, a range, and assigns it the name "NATPUB.*  

>*With two IP addresses, it specifies the range from x.x.x.20 to x.x.x.45.*

### 3. Configure the Translation 
To configure the translation, we must specify an incoming ACL to define the IPs to be translated and a pool of public IPs to which the local IPs will be translated. We can also define a port for the translation, as is the case with PAT. On the "MADRID" router, enter:  

- **Dynamic NAT:**
```bash
ip nat inside source list NATDIN pool NATPUB
```  

>*"ip nat inside source list" is the command that prompts us to provide an ACL with IPs to be translated.*  

>*"pool" specifies a set of public IPs to which the ACL IPs will be translated.*


- **PAT:**

```bash
ip nat inside source list PATPU interface serial3/0 overload
```  
>*"interface serial3/0" specifies the port through which the PATPU ACL IPs will be translated.*  

>*"overload" is a PAT feature that enables multiple IPs to be translated to the same port or IP. This means multiple IPs can be translated to the same public IP or port.*  

### 4. Verify the IP Translation 

Send packets from the main interface in Packet Tracer and use the following command to verify:

`show ip nat translation`


