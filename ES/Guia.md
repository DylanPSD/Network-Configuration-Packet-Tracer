# Guía para Configurar una Red en Packet Tracer

## Topología de la Red  
A continuación, se muestra la topología que vamos a utilizar, incluyendo sus respectivas **IPs**, **nombres** y **servicios**:  

> ![Topología](./topológia.png)

---

# Configuración de Direcciones IP  
El primer paso será asignar las diferentes **IPs** a las interfaces del router.  

---

## Pasos a seguir:  

### 1. Entrar en el terminal del router  
Accede al terminal del dispositivo en Packet Tracer desde la pestaña de CLI en el router.

### 2. Acceder al modo EXEC privilegiado  
Escribe el siguiente comando:  
`enable ` 

### 3. Acceder al a interfaz 
Elige la interfaz que deseas configurar. Por ejemplo para la iterfaz fa0/0  utiliza:  
```interface fa0/0```  

### 4. Asignar una direccion IP  
Debes asignar una IP adecuada. Si la direccion de red es 190.190.16.0, la primera IP disponible será la 190.190.16.1, que funcionara como default gateway para los dispositivos finales.  
Usa el siguiente comando para asignar la IP con su mascara de subred (/24):  
`ip address 190.190.16.1 255.255.255.0`  

### 5.Activar la interfaz  
Por defecto las interfazes estan apagadas, debemos activarlas, cuando lo hagamos en la topologia la interfaz aparecera en verde. Usando:  
`no shutdown`  

>_Usa una mascara de subred consistente en toda la topología en este caso solo utilizaremos la mascara /24_  

>_Sigue configurando cada interfaz de manera individual utilizando direcciones IPs distintas.  (Si en un extremo configuramos la "x.x.x.1", en el otro sera la "x.x.x.2")_  

>_No Asignar IPs en las interfaces donde no hay IPs escritas en la topología, se configurarán VLanss mas tarde_  
  
---
  
# Creación de VLANs  
El siguiente paso será la creación de las VLAN. Estas tendrán que ser creadas en switchs de capa de enlace de datos(2) y switch de capa de red(3).  

---

## Pasos a seguir:  

### 1. Entrar en el terminal de configuracion del switch de capa 2
Entrar desde la pestaña de CLI del menu del switch capa 2  

### 2. Acceder al modo EXEC privilegiado  
Escribe el siguiente comando:  
`enable`  

### 3. Crear las VLAN
En los switches deben estar creadas todas las VLAN de su misma red para identificar a que dispositivo enviar el paquete. para crear las VLAN  10, 50, 20, 30, 40 y 60. Las VLAN 20 y 40 son VLANs para la telefonía IP por lo que se configurarán diferentes.  
_"Recordemos que los routers dividen redes"_   
Para crearlas escrirbe:  
__switch1__  
`vlan 10`  
`vlan 50`  
__switch2__  
`vlan 20`  
`vlan 30`  
__switch3__  
`vlan 40`  
`vlan 60`  

### 4. Mode Trunk  
Esto es una opción que permite que todas las vlan viajen por un medio sin que tenga que formar parte de la VLAN, esto se aplica en las interfaces entre switches o a las que van a los routers para permitir viajar a loas paquetes con:  
`switchport mode trunk`

### 5. Asignar VLAN a los puertos   
Para que los dispositivos finales tengan una VLAN asignada por ejemplo el pc0 con la VLAN 10:  
`switchport mode access`  
`switchport access vlan 10`  

### 6. Asignar VLAN de telefonía  
Habiendo asigando alguna VLAN previamente para poder configurar una VLAN de telefonía esta debería ir configurada después de la VLAN, si no, asignar de la siguiente manera (por ejemplo para el primer teléfono):  
`switchpor voice vlan 20`    

---

# Configuración de Direcciones IP en VLANs  

---

## Pasos a seguir:

### 1. Asignar direcciones IP a las VLAN  
Las direcciones IP en este caso se deberan configurar desde el switch de capa 3 y desde el router.  

__Desde el terminal de un switch de capa 3 debes escribir lo siguiente:__  
`interface vlan 10`  
`ip address 10.0.0.1 255.255.255.0`  
>_Donde "10.0.0.1" es la IP que deseamos asignar y 255.255.255.0 es la mascara de subred que en este caso es /24_  

__Desde el terminal de un router deberías escribir:__  
`interface fa0/0.60`  
`encapsulation Dot1q 60`  
`ip address 60.0.0.1 255.255.255.0`  
>_Donde "fa0/0.60 se utiliza para referirse a la subinterfáz que deseamos crear._  

>_La parte de "encapsulation Dot1q permite que un router gestione tráfico de múltiples VLANs en una única interfáz física"_   

>_Para una VLAN de telefonía IP se haría igual_  

---

# Configuración de enrutamiento

---

## Pasos a seguir:

#### 1. Enrutamiento OSPF   
En este caso utilizaremos OSPF como protocolo de enrutamiento, lo que va a hacer que los dispositivos finales e intermedios se puedan comunicar sin tener que estar en la misma red escribiendo lo siguinte:  

Desde el terminal de un switch de capa 3:  
`ip routing`  
`router ospf 100`  
`network 10.0.0.0 0.0.0.255 area 0`  
>_Donde "ip routing" se utiliza para habilitar el enrutamiento en un switch de capa 3._   

>_La parte de "100" es el identifiacor del proceso OPSF dentro del router para distinguir entre diferentes posibles procesos puede ir desde 1 a 65535_  

>_La parte de "0.0.0.255" es la wildcard esta se utiliza para identificar la parte de la red y la parte de host. Aquí un link para aprender a calcular una wildcard[https://hacks4geeks.com/hacks/calcular-wildcards-de-mascaras-de-red/]_

>_La parte del código de "area 0" sirve para asociar las redes con el área principal de OSPF que suele se la "0" (Backbone Area)_  

__Desde el terminal de un router:__  
`router ospf 100`  
`network 60.0.0.0 0.0.0.255 area 0`  
>_La parte de "100" es el identificador del proceso OPSF dentro del router para distinguir entre diferentes posibles procesos puede ir desde 1 a 65535_  

>_La parte de "0.0.0.255" es la wildcard esta se utiliza para identificar la parte de la red y la parte de host. Aquí un link para aprender a calcular una wildcard[https://hacks4geeks.com/hacks/calcular-wildcards-de-mascaras-de-red/]_

>_La parte del código de "area 0" sirve para asociar las redes con el área principal de OSPF que suele se la "0" (Backbone Area)_  