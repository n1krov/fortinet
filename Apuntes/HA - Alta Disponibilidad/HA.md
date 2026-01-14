---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---
# HA - Alta disponibilidad

![[Pasted image 20260114085711.png]]

basicamenteson dos o mas fortigate en conjunto. un cluster de fortigates

la ventaja es que al tener un cluster de fortigates, si uno falla otro lo reemplaza

forti nos ofrece dos modos. 

el Activo-pasivo de alta disponibilidad
>  un equipo designado como primario procesa trafico, los demas son secundarios y esperan a que el primario falle
  
![[Pasted image 20260114085909.png]]


Activo - activo

> Todos los equipos procesan trafico, sin embargo se mantiene el esquema primario y secundario
 
![[Pasted image 20260114090027.png]]

debemos tener un balanceador de carga entre **los equpos de la red y el fortigate** pero fortinet nos ofrece este modo y es el equipo primario. 
el equipo primario recibe las peticiones de los usuaris y el las distribuye entre el primario y los secundarios

todo es posible gracias al [[Fortigate Clustering Protocol - FGCP]]

### Requerimientos

Como se ve en la imagen

![[Pasted image 20260114094105.png]]

- Dos o cuatro Identicos dispositivos fortigate
	- Igual lincencias en un mismo miembro de cluster
- Un link (preferiblemente dos o mas) entre los dispositivos fortigate
- Mismas interfaces en cada fortigate conectados al mismo dominio de broadcast
- DHCP y PPPoE 