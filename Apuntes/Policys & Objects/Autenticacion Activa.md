---
Tema: "[[policys&objects]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

esto sirve para poder crear politicas que permitan restringir la navegacion de internet o a ciertos sitios. y cuando un usuario quiera acceer a algo restringido deba iniciar sesion

ir a User & Autentication > Create New
1. poner local user
2. establecemos usuario y password
3. no vamos a tildar el two factor autentication
4. ventana para añadir a un grupo ese usuario a crear

luego editamos o establecemos una politica de firewall donde en source debemos agregar el usuario creado en su defecto. lo cual significa que la navegacion se permite solo por este usuario u otro objeto agregado si existe en la policy

![[Captura de pantalla_20260130_090418.png]]


#### Agregar el controlador de dominio al forti para los usuarios

este es en el caso que cuentes conun servidor de dominio de usuarios. y desees agregarlos

ir a 
user & autentications > LDAP servers > create new

