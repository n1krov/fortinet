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

