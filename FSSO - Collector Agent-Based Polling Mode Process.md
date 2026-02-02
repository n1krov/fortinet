---
Tema:
---

Como se muestra en la imagen, 

![[Captura de pantalla_20260130_103235.png]]
en este modo:
1. el usuario se autentica con el DC (domain controller)
2. el colector ahce la extraccion en el DC para recolectar eventos de login
3. el colector luego redireccion a los logins al fortigate
4. el usuario no necesita autenitcarse

> nota que el dc agent, no esta instalado, esto es sin dc agent, en el servidor esta instalado solo el agente de collector

el collector agent trabaja en el 445 por TCP
y el forti trabaja por el 8000 por TCP
el collector agent envia al forti
- nombre de usuario
- nombre de host
- Direccion IP
- grupos de usuario
