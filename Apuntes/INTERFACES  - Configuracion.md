---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
obligatorio Respetá enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

Trabajando con esta situacion de laboratorio

![[Pasted image 20251217115810.png]]

se puede ver qeu desde el forti en el site a y b pero en el site a para tomar de ejemplo tiene dos cables conectados a una nube que es el internet. esos dos estan conectados a una interfaz port2 y port3 respectivamente ya que port1 es la de administrador.

defini en port2 como `ISP 1` y port3 como `ISP 2` ambas con rol WAN

esos puertos simulan estar conectados en este lab a un mikrotik el cual maneja dos direcciones de red:
- `200.212.31.0/30`
	- El **Forti** tomara la `.1`
	- El **Mikrotik** tomara la `.2`
- `180.45.22.0/30`
	- El **Forti** tomara la `.1`
	- El **Mikrotik** tomara la `.2`

hoy una interfaz 4 (`port4`) que ese va conectado con el windows (`DomainController`)
en esa interfaz la ip para el forti es
- `10.0.1.254/24` 
	- Rol de LAN
	- Al acceder desde la maquina windows se puede tirar un ping y tendria que habilitar el acceso. sino tildar la opcion de ping en el Fortigat


> Para profundizar puedes ver las [[Interfaces del Mikrotik]]
## **RUTAS** - Desde ***FORTIGATE*** hacia ***MIKROTIK***

ir a Network>Static Routes

>[!note]
> {explicar brevemente que es static routes de manera senccilla}

el destino para todo `0.0.0.0/0.0.0.0`

el gateway debe ir para `port2 (ISP 1)` y otro para `port3 (ISP 2)`  que es colocar sus ips respectivamente

> AL `ISP 2` sel 



---

Desde la CLI


config system interface

show

edit port2

show

set ip 60.89.123.1/30

set alias "ISP 1"

set role wan

end

---

puerto 3

config system interface
edit port3

set ip 45.32.12.1/30
set alias "ISP 2"
set role wan

next
> Nota que `next` a diferencia de `end` lo que hace es volver al contexto anterior, y `end` te devuelve al inicio de la CLI. Por lo que resulta util utilizar next para seguir configurando las interfaces.


---

y por ultimo el puerto4 del `site B` que es la interfaz que forti va a tomar para que la windows del `site B` se pueda conectar

edit port4
show

set ip 10.0.2.254/24

set allowaccess ping
show

end


---

### Rutas Estaticas CLI

config router static
show

edit <- con edit vale para **editar** y para **crear**


dentro de (***router static***) ~por lo  que sé~ sepuede hacer `get` o `show`

`show` muestra las config o **cambios que se hicieron manualnete** a las opciones **por defecto**

y `get` muestra todo

