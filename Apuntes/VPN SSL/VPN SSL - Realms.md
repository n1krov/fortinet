---
Tema: "[[VPN SSL]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---
en vpn > ssl vpn realms > create new

![[Captura de pantalla_20260212_105415.png]]

tiene el limite de concurrencia de usuarios
y el de customizar la pagina de login


luego en `vpn > ssl-vpn settings` aparecera una columna de realms

![[Captura de pantalla_20260212_110138.png]]

ahi es donde se configura  y en el momento de elegir que grupos tienen acceso a que realm puedes aplicar una realm por grupo, en este caso el de legales tendra la realm /Legales

quedando la configuracion de la siguiente manera

![[Captura de pantalla_20260212_110348.png]]

para ingresar debes ir al navegardor 
`https://ip:puerto_vpn/REALM`
- /Legales

