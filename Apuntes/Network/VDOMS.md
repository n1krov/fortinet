---
Tema: "[[network]]"
---
 
Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---
## VDOMs

permiten dividir un equpo en equipos virtuales

![[Pasted image 20260108105133.png]]

caada uno de estos vdoms tiene salida a internet

![[Pasted image 20260108111357.png]]
problema: cuando generas varios vdoms y necesitas tener salida a internet necesitas interfaces, si tenes 10 vdoms y tu equipo tiene 8 entonces 2 vdoms no van a tener acceso a internet por faltaa de interfaces, por lo que se hace un vdom central que sea el que se encargue de enrutar a los respectivos ISP's

![[Pasted image 20260108111903.png]]

estos se conectan al principal con algo llamado **Internet VDOM Link** pero en este modelo no pueden verse los VDOM1 VDOM2 y VDOM3

![[Pasted image 20260108112726.png]]


esto lleva a otro modelo el cual es **mesh vdom**

![[Pasted image 20260108112831.png]]
donde todos se conectan con todos.


hay dos modos de trabajo con vdoms. **Split** o **Multi VDOM**
![[Pasted image 20260108112931.png]]

Split
este crea dos vdoms uno para administracion y otro para manjear el trafico de la red

>[!IMPORTANT]
> El multiVDOM no permitido en la licencia de prueba.

#### Activar el modo multiple vdoms
###### interfaz grafica
system>settings

si no tiene es hacerlo por cli

###### [[CLI]]

config system global
set vdom-mode multi-vdom
end


algo importante a destacar el management vdom se encarga de actualizar fotigate, conectarse a dif servicios etc, debe tener acceso a internet