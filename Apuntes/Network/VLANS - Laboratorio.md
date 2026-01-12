---
Tema: "[[network]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---


Laboratiorio de prueba de como manejar las [[VLANS]] en fortigate

Para ello tenemos el siguiente escenario

![[Pasted image 20260112095526.png]]

para configurar las vlans estan los switches A y B de CISCO

se lo configura de la siguiente manera

#### Switch A
```sh
enable
configure terminal
vlan 101
name Legales
exit
vlan 102
name Contable
exit
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 101
exit
interface gigabitEthernet 0/2
switchport mode access
switchport access vlan 102
exit
interface gigabitEthernet 0/0
switchport trunk encapsulation dot1q
switchport mode trunk
end
```

#### Switch B

```sh
enable
configure terminal
vlan 103
name Sistemas
exit
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 103
exit
interface gigabitEthernet 0/0
switchport trunk encapsulation dot1q
switchport mode trunk
end
```


luego de copytastear podemos meter un 

```sh
show vlan
```

##### Config del fortigate

admin y la pass es en blanco y ahi despues debes poner una clave

##### habilitar http

```sh
config system interface
edit port1
append allowaccess http
```

Luego crear las interfaces en network>interface
hay 3 vlans 
101 -> port2 legales
102 -> port2 contable
103 -> port3 sistemas

luego para la habilitacion del trafico y comunicacion entre ellas se debe crear las firewall policys

aqui lo importante es las incomming y outgoing interfaces, esas deben tener configuradas las interfaces vlans, NO los puertos.



---

recordar que 

![[Pasted image 20260109120223.png]]

- los equipos de capa 2 (Swithces) tienen la capacidad de agragar o remover estos tags
- los equpos de capa 3 (fortigate) tienen la capacidad de sobreescribir estos tags, y esto lo hacen para poder enrutar todo el trafico correctamente

si el equipo de contables de la vlan 101 le quiere hacer un ping al equipo de la vlan 103
al pasar por el switch del sitio donde esata la vlan101 va a detectar el puerto que tiene asignada a la vlan101 y la inserta el vlan tag a las tramas provenientes de eese equipo

cuando pase por el router este va a reescribir esa parte de la trama reemplazando el 101 por 103 y se la envia al switch que entiende el vlan tag 103, este switch termina finalmente removiendo el vlan tag 103 porque ya no necesita y luego llegga a destino

los equipos finales no manejan vlan tags, solo equipos intermedio de capa 2 y 3, switches y routers
