---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---


Lo que se suele hacer es por ahi crear primero un grupo de usuarios (previamente tener los usuarios creados)

aqui un ejemplo, nota que es de tipo local (firewall)

![[Captura de pantalla_20260204_093702.png]]

el portal cautivo se programa a nivel de INTERFAZ una vez elegida la interface de donde esta conectados los clientes.

configuramos en INTERFACE > Network > security mode

![[Captura de pantalla_20260204_094204.png]]

podemos configurar por usuarios y elegirlos, excluir usurios y servicios. y efectuar redirecciones

### Portal Cautivo - Disclaimer

para habilitar el disclaimer se lo hace unicamente de la [[CLI]] y se habilita en el ***firewall policy***

`config firewall policy`
`edit "<policy>"`
`set disclaimer enable/disable`
`end`


## Cambiar mensajes y estilos

El fortinet tiene programado multiples mensajes de avisos, eso se puede encontrar ***system > REPLACEMENT MESSAGES***

ahi esta la ` [simple view] | [extend view]`


