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

### VDOM - INTERNET (Configuracion)

como se muestra en la imagen la idea es tener vdoms y que uno solo se encargue de enrutar a internet los diferentes vdoms a suis respectivas ISPs

![[Pasted image 20260108112726.png]]

> el vdom de internet estara por encima ya que el se encargara de dar salida a internet  a los demas vdoms

si se llama INTERNET, por CLI y acceder a su configuracion seria

config vdom
edit INTERNET

ahi ya nos paramos en el vdom de internet


ahora a esos vdoms para enrutarlos tenemos que estar parado en la VDOM global
y configurar las interfaces y usar **VDOM Link**

![[Pasted image 20260109085314.png]]

Luego se configuran dos patas del cable virtual

![[Pasted image 20260109090530.png]]

Resta configurar del lado del VDOM LEGALES

###### VDOM LEGALES

en Static Routes 

![[Pasted image 20260109091009.png]]

Luego por su Firewall policy

![[Pasted image 20260109091116.png]]

nota que la configuracion nat esta desactivada porque eso lo tiene que hacer VDOM INTERNET, y esta va enrutada

###### VDOM INTERNET
ahora ambos se conoocen pero INTERNET probablemente no sepa que red tiene el equipo que se conecte por lo cual hay que configurarl

![[Pasted image 20260109091803.png]]

en ese caso la 10.10.10.1 estaria siendo Dentro del virtual link, el vdom de legales

luego se configura su firewall policy y nota que aqui si tiene el NAT ACTIVADO

![[Pasted image 20260109092046.png]]

#### MESHED VDOMS - Conectar Distintos VDOMs y que se vean entre si

la idea recordar qeu es llegar a esto

![[Pasted image 20260108112831.png]]



en Global VDOMS > interfaces  creamos el vdom LINK