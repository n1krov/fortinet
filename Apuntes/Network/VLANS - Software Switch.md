---
Tema: "[[network]]"
---

Embellecé y organizá mis apuntes de fortinet en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

que es software switch

![[Pasted image 20260112114057.png]]

nos permite agrupar multiples interfaces fisicas y wireless dentro de una interfaz de software.

esto actua como un switch de capa 2
solo soporta modo NAT

las interfaces comparten la misma red y pertenecees al mismo dominio de broadcast

IMAGEN DE EJEMPLO

![[Pasted image 20260112114656.png]]

es como si estuvieran conectados a un switch pero hecho en fortigate

CREACION

para crear un software switch es ir en  network>interface

y en el tipo poner Software switch y colocar las interfaces que van a pertenecer a ese sw switch

el rol tipo lan
la red in