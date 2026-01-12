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
