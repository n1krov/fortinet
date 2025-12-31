---
Tema: "[[network]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

las politicas basadas en rutas requieren de [[Rutas Estáticas]] es requerido. loq ue se hace en esta pestaña es agregar filtros y demas de seguridad

![[Pasted image 20251231085242.png]]

para leer lo que dice la imagen debemos pensar lo siguiente: Suponiendo el escenario

![[Pasted image 20251231091547.png]]

1. En ese forti esta la politica
2. workstation
3. internet
4. forti site A
5. controlador de dominio windows server

>[!important]
>Cuando **2** quiera enviar trafico de por ejemplo TCP por el puerto 80 a **5**, si esta configurada la politica de enrutamiento, debera hacer un forward traffic hacia la ip del 5

