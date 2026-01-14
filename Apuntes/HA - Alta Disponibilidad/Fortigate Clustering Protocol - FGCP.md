---
Tema: "[[HA]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

fortigate clustering protocol

{que es esto}

un cluster usa FGCP para 
- descubrir que otros fortigates pertenecesn al mismo Grupo HA
- seleccionar el primario
- sincronizar datos y configuracion
- detectar cuando el fortigate falla

este protocolo tambien corre en el puerto 703 por tcp
- 0x8890 -> para NAT
- 0x8891 -> para transparente

en el puerto 23 por TCP 
- 0x8893 -> para configuracion y sincronizacion

