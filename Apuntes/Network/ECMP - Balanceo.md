---
Tema: "[[network]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

situacion

![[topologia.png]]

en la imagen se puede ver que amboos forti tienen dos interfaces conectadas al mikrotik
en la configuracion dde las rutas estaticas se añadio para la segunda interfaz una distancia mas lejana que la de por defecto.
pero que pasa cuando tienen la misma distancia y el enrutamiento se dirige al mismo destino, tienen la misma prioridad y todo???

{explicar qeu es un ECMP}

Condiciones

- las distancias entre ambas interfaces por ej la del forti A, sus dos interfaces que apuntan al mikrotik deben ser iguales 
- las distancias tambien deben ser iguales

con estas condiciones se produce un balanceo ECMP
el inconveniente es que desde la web interface no podemos modificar pero desde la [[CLI]] es posible

#### Ver la configuracion del ECMP `v4-ecmp-mode`

`config system settings ?`

`get | grep ecmp`

el que hay que notar es el `v4-ecmp-mode` 

las opciones que tiene esta configuracion es
- source-ip-based (default)
- weight-based
- usage-based
- source-dest-ip-based

#### source ip based
dada una sesion, forti hace una suerte de elegir qeu interfaz usar para dirigir el trafico hacia el mikrotik
en esa sesion hace un sorteo de interfaz le toco y siempre usara esa por toda la sesion activa

#### source-dest-ip-based
esta balancea segun la fuente y el destino. si navegas o se envia por ejemplo una traza ICMP a google.com este tomara una interfaz y usara esa para google.com, pero para yahoo.com puede tomar otra y asi
#### weight-based


#### usage-based

