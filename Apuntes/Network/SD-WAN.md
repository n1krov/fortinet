---
Tema: "[[network]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

### Que es SD-WAN

> acceso a internet definido por software

![[Pasted image 20260106084028.png]]

es una interfaz virtual que se compone de diferentes interfaces que estan conectados a diferentes clientes

>[!note]
>esto se podria decir tambien que es la evolucion del balanceo [[ECMP - Balanceo]]

- Para configurar una sd wan las interfaces deben estar libres, es decir, sin uso


---


### Crear una zona y miembro sdwan


aqui se puede ver la opcion donde crear un miembro para una zona sdwan dada

![[Pasted image 20260106090750.png]]

nota que debes colocar
- interface que este libre
- la zona sdwan
- gateway coorrespondiente
	- en el caso del gateway debes poner la puerta de enlace de aquella que lleve hacia internet, en este caso, el **microtik**


despues tendras que crear las [[Rutas Estáticas]] y asignarle ahi la sdwan, lo bueno es que ya configuraste todo eso aqui, por lo que no necesitaras configurar una a una  cada ruta estatica si tienes sdwan configurada

tambien deberas [[Crear Politica]]s 

![[Pasted image 20260106092635.png]]
> politica de ejemplo





---


Sdwan trae ademas, una zona de politicas que es para definir como o por que interfaz enviar y recibir paquetes, y manejar mas facil el balanceo de carga.

