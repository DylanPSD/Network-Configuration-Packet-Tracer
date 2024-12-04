# Conceptos necesarios para la configuración de una red en Packet Tracer

Estos son los conceptos básicos que debes conocer para configurar una red con esta guía.

---

## Paquetes de Red  
Formato en el que se manda la información en la **capa de red (capa 3)** del modelo OSI.

---

## Topología Física y Lógica  

- **Física**: Esquema de la distribución en el entorno físico de los PCs, servidores, armarios de racks, etc.  
- **Lógica**: Esquema lógico de la distribución de los routers, los tipos de medios y los servicios de los servidores.

---

## Medios  
Vías que utilizan los paquetes de red para viajar entre dispositivos intermedios hasta llegar a los dispositivos finales.

---  

## Routers  
Dsipositivos intermedios que separan y comunican redes, tamnbien enrutan paquetes y son los dispositivos en los que principalmente se va a basar la configuración de una red  

---

## Dispositivos Finales  

- **PC**: Cliente de servicios en la red.  
- **Servidor**: Proveedor de servicios como DHCP, DNS o TFTP.  

---  

## Modo EXEC privilegiado  
Este modo permite realiazar configuraciones y administra el sistema de manera completa, proporciona un conjunto amplio de comandos. A diferencia del modo EXEC básico o de usuario.  
>se indica con un "#" al final del nombre del dispositivo en la linea de comandos
---  

## Modo de configuración global  
En este modo se permite hacer configuraciones que afectan al dispositivo en su totalidad. Desde este modo se puede acceder a submodos para: configurar interfaces, protocolos...  
>Se identifica con un "(config)#" en la linea de comandos

---

## Direcciones y Protocolos  

- **Protocolo de Internet (IP)**: Dirección única que identifica un dispositivo final en una red.  
- **Máscara de Subred**: Número de 32 o 128 bits que identifica la parte de red y la parte de host en una IP.  
- **Máscara Wildcard**: Cadena de 32 bits utilizada por el router para determinar las partes de red y host, principalmente en protocolos como OSPF.  

---

## Puerta de enlace predeterminada (Default Gateway)
Esto se refiere a dispositivos o IP que se comportan como puntos de acceso para enviar datos fuera de su propia red.

---

## Redes Virtuales  

- **Red de Área Local Virtual (VLAN)**: Redes lógicas independientes dentro de una misma red física.  
- **Voz sobre IP (VoIP)**: Método que permite realizar llamadas a través de la red.  

---

## Protocolos de Enrutamiento  

- **OSPF (Open Shortest Path First)**: Protocolo de enrutamiento que dirige paquetes de red por los medios más óptimos para llegar a su destino.  

---

## Protocolos de Servicios  

- **DHCP (Dynamic Host Configuration Protocol)**: Asigna direcciones IP dinámicamente y de forma automática a los dispositivos finales.  
- **DNS (Domain Name System)**: Traduce nombres de dominio (ej. Google.com) en direcciones IP utilizables por las máquinas.  

---

## Access-Lists (ACLs)  

Permiten restringir o conceder acceso a servicios, medios o dispositivos finales.  
Se configuran en dispositivos de capa 3 y pueden ser:

1. **Estándar**: Configuradas cerca del destino.  
   - **Numeradas**: Identificadas con un número, no editables tras su configuración.  
   - **Nombradas**: Identificadas con un nombre, editables tras su configuración.  

2. **Extendidas**: Configuradas cerca de la fuente para un mayor control.  
   - **Numeradas**: Identificadas con un número, no editables tras su configuración.  
   - **Nombradas**: Identificadas con un nombre, editables tras su configuración. 

---

## Protocolos de Transferencia de Archivos  

- **TFTP (Trivial File Transfer Protocol)**: Permite la transferencia de archivos entre sistemas conectados a una red.  

---

## Protocolos de Comunicación  

- **Telnet**: Protocolo que permite acceder y configurar otro dispositivo dentro de la red mediante texto.  

---

## Traducción de Direcciones  

### NAT (Network Address Translation)  
Protocolo que traduce IPs privadas a IPs globales. Se configura en el router conectado al ISP o red externa.  

1. **NAT Estática**: Traduce una IP privada a una IP pública fija (útil para servidores, impresoras, etc.).  
2. **NAT Dinámica**: Traduce un rango de IPs privadas a un rango de IPs públicas mediante una **Access-List** y un "pool" de IPs públicas.

---

### PAT (Port Address Translation)  
Protocolo similar a NAT pero permite traducción de múltiples direcciones privadas a una sola IP pública, añadiendo información de puerto.  

1. **PAT Dinámico**: Traduce de manera similar a NAT dinámica, pero permite compartir IPs públicas mientras no están en uso.  
2. **PAT a Puerto**: Traduce un conjunto de redes a un puerto específico de la interfaz del router conectado al ISP.

---

Con este resumen de conceptos, estás listo para seguir esta guía de configuracion en **Packet Tracer**.
