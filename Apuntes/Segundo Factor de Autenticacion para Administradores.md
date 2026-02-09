---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

cuando se compra un equipo te deja tener hasta 2 fortiToken pero si necesitas mas hay q pagar.

para eso puedes [[Crear Usuario]] admin 


en la CLI, si tenes las opciones de correo habilitadas puedes ver si tienes con `config system email-server`

`config system admin`

`edit usuario`
	`show`
> aqui lo que nos interesa es `email-to` qeu es asignarle un correo a este usuario.
> `two-factor` que es el que tenemos que habilitar

	set email-to email
	set two-factor disable|fortitoken|fortitoken-cloud|email|sms

