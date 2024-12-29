

1. **Acceder al Router:**  
- **`enable`** 
Accede al modo EXEC privilegiado del router
 
- **`configure terminal`** 
Entra al modo de configuración global

---

2. **Configurar Interfaces del Router:**  
- **`interface [interfaz]`** 
Entra en el modo de configuración de una interfaz específica.
 
- **`ip address [IP] [máscara]`** 
Asigna una dirección IP y una máscara de subred a la interfaz seleccionada.
 
- **`no shutdown`** 
Activa la interfaz

---

3. **Comandos de Configuración de Telefonía (CME):**  
- **`telephony-service`** 
Habilita la telefonía en un router, permitiendo que actúe como un (CME) Call Manager Express.
 
- **`max-ephones <número>`** 
Establece el número máximo de teléfonos.
 
- **`max-dn <número>`** 
Define el número máximo de números de directorio 
 
- **`ip source-address <IP> port <puerto>`** 
Configura la dirección IP y el puerto del router que se utilizarán para la comunicación VoIP.
 
- **`auto assign <rango>`** 
Asigna automáticamente números de teléfono dentro del rango especificado.
 
- **`ephone-dn <número>`** 
Configura un número de directorio (DN) 
 
- **`number <número>`** 
Asigna un número de teléfono específico
 
- **`dial-peer voice <número> voip`** 
Crea un dial-peer para enrutar llamadas VoIP
 
- **`destination-pattern <patrón>`** 
Define el patrón de números que se van a enrutar mediante el dial-peer.
 
- **`session target ipv4:<IP>`** 
Establece la dirección IP de destino a la cual deben enviarse las llamadas


---

4. **Verificar Configuración:**  
- **`show running-config`** 
Muestra la configuración activa del dispositivo.

---

5. **Configurar DHCP:**  
- **`ip helper-address <IP>`** 
Configura la dirección IP de un servidor DHCP  

- **`ip dhcp pool <nombre_pool>`** 
Crea un conjunto de direcciones DHCP  

- **`network <dirección_red> <máscara_subred>`** 
Define el rango de direcciones IP que serán asignadas dentro del conjunto DHCP.
 
- **`default-router <IP>`** 
Especifica la puerta de enlace predeterminada para el conjunto DHCP
 
- **`dns-server <IP>`** 
Configura el servidor DNS para el conjunto DHCP
 
- **`option 150 ip [IP]`** 
Especifica la dirección IP del servidor TFTP para los dispositivos que reciben DHCP (usado comúnmente para teléfonos IP).

---

6. **Configurar VLANs:**  
- **`vlan [ID_VLAN]`** 
Crea una VLAN con un identificador único
 
- **`switchport mode trunk`** 
Configura un puerto del switch para que pueda transportar tráfico de múltiples VLANs.
 
- **`switchport mode access`** 
Configura un puerto del switch para que sea miembro de una única VLAN.
 
- **`switchport access vlan [ID_VLAN]`** 
Asigna un puerto del switch a una VLAN específica 
 
- **`switchport voice vlan [ID_VLAN]`** 
Asigna una VLAN específica para el tráfico de voz 

---

7. **Configurar Direcciones IP en VLANs:**  
- **`interface vlan [ID_VLAN]`** 
Entra en la configuración de una VLAN en un switch de capa 3, permitiendo asignar una dirección IP a la VLAN.
 
- **`ip address [IP] [máscara]`** 
Asigna una dirección IP a la interfaz VLAN.
 
- **`interface [interfaz.subinterfaz]`** 
Configura una subinterfaz en el router (por ejemplo, `interface fa0/0.10` para configurar la subinterfaz para la VLAN 10).
 
- **`encapsulation Dot1q [ID_VLAN]`** 
Configura la encapsulación de VLAN en una subinterfaz (por ejemplo, `encapsulation Dot1q 10` para la VLAN 10).


---

8. **Configurar Enrutamiento OSPF:**  
- **`ip routing`** 
Habilita el enrutamiento en un dispositivo, como un switch de capa 3 o router.
 
- **`router ospf [ID_PROCESO]`** 
Inicia el proceso de enrutamiento OSPF con un identificador único 
 
- **`network [red] [wildcard] area [area]`** 
Define las redes que participarán en el proceso OSPF, usando una máscara wildcard para especificar el rango de direcciones (por ejemplo, `network 192.168.1.0 0.0.0.255 area 0`).

---

9. **Verificar la Tabla de Enrutamiento:**  
- **`show ip route`** 
Muestra la tabla de enrutamiento, permitiendo ver las rutas activas y su información.

---

10. **Configurar NAT:**  
- **`ip nat pool <nombre_pool> <IP_inicio> <IP_fin> netmask <máscara_subred>`** 
Crea un pool de direcciones IP públicas para traducción de direcciones de red (NAT).
 
- **`ip nat inside source list <nombre_acl> pool <nombre_pool>`** 
Configura NAT dinámico, utilizando una ACL y un pool de direcciones públicas para traducir direcciones IP internas.
 
- **`ip nat inside source list <nombre_acl> interface <interfaz> overload`** 
Configura PAT (NAT con sobrecarga), donde varias direcciones internas pueden compartir una sola dirección IP pública, utilizando una interfaz como punto de salida.
 
- **`show ip nat translation`** 
Muestra la tabla de traducción NAT, con las direcciones IP internas y sus correspondientes direcciones IP públicas.


---

11. **Configurar Acceso Remoto:**  
- **`line vty <0-4>`** 
Configura el acceso remoto a través de las líneas VTY (Telnet o SSH) en el dispositivo.
 
- **`access-class <nombre_acl> <dirección>`** 
Asocia una ACL a las líneas VTY, controlando quién puede acceder al dispositivo de forma remota.


---

12. **Guardar Configuración:**  
- **`copy running-config startup-config`** 
Copia la configuración activa al archivo de configuración de inicio (startup-config).
 
- **`copy startup-config tftp`** 
Copia la configuración de inicio (startup-config) a un servidor TFTP.


---

13. **Listas de Control de Acceso (ACLs):**  
- **`ip access-list <tipo> <nombre_acl>`** 
Crea una lista de control de acceso (ACL), que puede ser estándar o extendida.
 
- **`permit <dirección_red> <wildcard>`** 
Permite el tráfico desde una dirección o red específica en una ACL.
 
- **`deny <dirección_red> <wildcard>`** 
Niega el tráfico desde una dirección o red específica en una ACL.
 
- **`ip access-group <número_acl/nombre_acl> <dirección>`** 
Asocia una ACL numerada o nombrada a una interfaz específica, controlando el tráfico que entra o sale de esa interfaz.


---
