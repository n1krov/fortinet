---
Tema: "[[IPsec]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

La funcion ipsec aggregate tiene por objetivo aprovechar las interfaces disponibles en el caso de que caigan algunas de las interfaces. 

![[Captura de pantalla_20260123_092551.png]]

como se ve en la imagen. los equipos a y b estan conectados con 1 - 1 en interfaz y el 2 - 2
por lo que hay 2 vpn.
pero que pasa si en A cae la interfaz 1 y en B la interfaz 2?
el resultado es que no va a habaer conexion, para resolver este problemas se hacen las combinaciones de enlaces posibles en este caso 4. (11,12,21,22)

por lo que si pasa el caso de que cae 1 en A y 2 en b, el 2 en A y el 1 en B puede funcionar y esto se logra con ipsec aggregate.

La idea es crear los tuneles de la misma forma que hicimos en [[Crear Tunel]] con la diferencia que agregaremos esta opcion

![[Captura de pantalla_20260123_093943.png]]

en la parte de advanced poner en `Enabled` la opcion de `Aggregate member`

> Importante: tener en cuenta que en las rutas estaticas las distancias deben ser ***IGUALES***

Luego crear en VPN > IPsec  Tunnels

