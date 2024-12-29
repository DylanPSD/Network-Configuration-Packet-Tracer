

1. **Access the Router:**  
- **`enable`** 
Accesses the router's privileged EXEC mode.
 
- **`configure terminal`** 
Enters global configuration mode.


---

2. **Configure Router Interfaces:**  
- **`interface [interface]`** 
Enters the configuration mode for a specific interface.
 
- **`ip address [IP] [subnet_mask]`** 
Assigns an IP address and a subnet mask to the selected interface.
 
- **`no shutdown`** 
Activates the interface.


---

3. **Telephony Configuration Commands (CME):**  
- **`telephony-service`** 
Enables telephony on a router, allowing it to act as a Call Manager Express (CME).
 
- **`max-ephones <number>`** 
Sets the maximum number of phones.
 
- **`max-dn <number>`** 
Defines the maximum number of directory numbers.
 
- **`ip source-address <IP> port <port>`** 
Configures the IP address and port of the router to be used for VoIP communication.
 
- **`auto assign <range>`** 
Automatically assigns phone numbers within the specified range.
 
- **`ephone-dn <number>`** 
Configures a directory number (DN).
 
- **`number <number>`** 
Assigns a specific phone number.
 
- **`dial-peer voice <number> voip`** 
Creates a dial-peer to route VoIP calls.
 
- **`destination-pattern <pattern>`** 
Defines the number pattern to be routed through the dial-peer.
 
- **`session target ipv4:<IP>`** 
Sets the destination IP address to which calls should be sent.


---

4. **Verify Configuration:**  
- **`show running-config`** 
Displays the active configuration of the device.


---

5. **Configure DHCP:**  
- **`ip helper-address <IP>`** 
Configures the IP address of a DHCP server.
 
- **`ip dhcp pool <pool_name>`** 
Creates a DHCP address pool.
 
- **`network <network_address> <subnet_mask>`** 
Defines the range of IP addresses to be assigned from the DHCP pool.
 
- **`default-router <IP>`** 
Specifies the default gateway for the DHCP pool.
 
- **`dns-server <IP>`** 
Configures the DNS server for the DHCP pool.
 
- **`option 150 ip [IP]`** 
Specifies the TFTP server's IP address for devices receiving DHCP (commonly used for IP phones).


---

6. **Configure VLANs:**  
- **`vlan [VLAN_ID]`** 
Creates a VLAN with a unique identifier.
 
- **`switchport mode trunk`** 
Configures a switch port to carry traffic for multiple VLANs.
 
- **`switchport mode access`** 
Configures a switch port to be a member of a single VLAN.
 
- **`switchport access vlan [VLAN_ID]`** 
Assigns a switch port to a specific VLAN.
 
- **`switchport voice vlan [VLAN_ID]`** 
Assigns a specific VLAN for voice traffic.


---

7. **Configure IP Addresses on VLANs:**  
- **`interface vlan [VLAN_ID]`** 
Enters the configuration for a VLAN on a Layer 3 switch, allowing the assignment of an IP address to the VLAN.
 
- **`ip address [IP] [subnet_mask]`** 
Assigns an IP address to the VLAN interface.
 
- **`interface [interface.subinterface]`** 
Configures a subinterface on the router (e.g., `interface fa0/0.10` for configuring the subinterface for VLAN 10).
 
- **`encapsulation Dot1q [VLAN_ID]`** 
Configures VLAN encapsulation on a subinterface (e.g., `encapsulation Dot1q 10` for VLAN 10).


---

8. **Configure OSPF Routing:**  
- **`ip routing`** 
Enables routing on a device, such as a Layer 3 switch or router.
 
- **`router ospf [PROCESS_ID]`** 
Starts the OSPF routing process with a unique identifier.
 
- **`network [network] [wildcard_mask] area [area]`** 
Defines the networks participating in the OSPF process, using a wildcard mask to specify the address range (e.g., `network 192.168.1.0 0.0.0.255 area 0`).


---

9. **Verify the Routing Table:**  
- **`show ip route`** 
Displays the routing table, allowing you to view active routes and their information.


---

10. **Configure NAT:**  
- **`ip nat pool <pool_name> <start_IP> <end_IP> netmask <subnet_mask>`** 
Creates a pool of public IP addresses for Network Address Translation (NAT).
 
- **`ip nat inside source list <ACL_name> pool <pool_name>`** 
Configures dynamic NAT, using an ACL and a pool of public IP addresses to translate internal IP addresses.
 
- **`ip nat inside source list <ACL_name> interface <interface> overload`** 
Configures Port Address Translation (PAT), where multiple internal addresses share a single public IP address using an interface as the exit point.
 
- **`show ip nat translation`** 
Displays the NAT translation table, showing internal IP addresses and their corresponding public IP addresses.


---

11. **Configure Remote Access:**  
- **`line vty <0-4>`** 
Configures remote access through VTY lines (Telnet or SSH) on the device.
 
- **`access-class <ACL_name> <direction>`** 
Associates an ACL with the VTY lines, controlling who can access the device remotely.


---

12. **Save Configuration:**  
- **`copy running-config startup-config`** 
Copies the active configuration to the startup configuration file.
 
- **`copy startup-config tftp`** 
Copies the startup configuration file to a TFTP server.


---

13. **Access Control Lists (ACLs):**  
- **`ip access-list <type> <ACL_name>`** 
Creates an access control list (ACL), which can be either standard or extended.
 
- **`permit <network> <wildcard_mask>`** 
Allows traffic from a specific address or network in an ACL.
 
- **`deny <network> <wildcard_mask>`** 
Denies traffic from a specific address or network in an ACL.
 
- **`ip access-group <ACL_number/ACL_name> <direction>`** 
Associates a numbered or named ACL with a specific interface, controlling traffic entering or leaving that interface.


---
