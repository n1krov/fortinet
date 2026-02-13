---
Tema: "[[VPN SSL]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

en todo este tema siempre se trato el modo cliente. se hizo un apunte a parte para tratar el modo tunel.

aqui se van a explicar algunas cosas qeu se pueden aplicar de seguridad sobre el modo cliente


se configura normal el modo cliente en [[VPN - SSL_VPN Settings]] y en el [[VPN - SSL_VPN Portals]]

y nos encontramos con las siguientes opciones:
- Host Check
- Restrict to Specific OS Versions
- Web Mode
- FortiClient Download

#### Host check

![[Captura de pantalla_20260213_101053.png]]

si esto se activa, un sistema linux no va a poder conectarse nunca.

al momento de conectarte por medio del forti client. para este ejemplo se requiere una version antigua de fortiCLient

![[Captura de pantalla_20260213_102020.png]]

host check creo que funciona siempre y cuando sea version 6 forticlient

