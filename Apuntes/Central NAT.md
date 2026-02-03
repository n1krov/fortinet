---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

para habilitar el central nat es en 

system > central SNAT 

o por [[CLI]]

`config system settings`
`set central-nat enable` 
para deshabilitarla es lo mismo, llegar a setting sy darle `set central-nat disable`

aqui con esto, la configuracion del NAT se centraliza y la config pasa aconfigurarse como poilticas a parte. es muy similar a las firewall policys

![[Captura de pantalla_20260203_111910.png]]


---

CON Virtual IPs y si tenemos habilitados el Central NAT

cuando creemos una VIP imediatamente en el kernel del forti OS se crean las rutas y la aplicacion de esta vip automaticamente, pero igual se debe configurar una politica


esto se usa la opcion de `policy&object>DNAT & Virtual IPs`


![[Captura de pantalla_20260203_112312.png]]

esto es en el forti del site A, por lo que en la imagen se puede ver que en external IP addres esta usando la IP publica del site A y la ip  privada es la del site A

por lo que la policy se puede ver algo como esto

![[Captura de pantalla_20260203_112735.png]]