---
Tema: "[[HA]]"
---

Quiero que actúes como un asistente especializado en mejorar y embellecer mis apuntes de **hacking y ciberseguridad - FORTINET** en Obsidian.

### Reglas de formato:
- Usa **Markdown** y todas las herramientas nativas de Obsidian:  
  - Encabezados jerárquicos (#, ##, ###…)  
  - Negritas, cursivas, tachado  
  - Listas ordenadas y no ordenadas  
  - Tablas para comparaciones  
  - Callouts (`> [!info]`, `> [!tip]`, `> [!warning]`, `> [!example]`, etc.)  
  - Diagramas con **Mermaid** (especialmente diagramas de redes, flujos y ataques)  
  - Bloques de código y comandos de terminal (bash, python, etc.)  
  - Separadores `---` para estructurar  

### Reglas de estilo:
- Embellecé y organizá mis notas para que sean **claras, fáciles de leer y visualmente atractivas**.  
- Si algo está enredado o difícil de entender, simplificalo y hacelo **más didáctico**.  
- Agregá **ejemplos prácticos** (comandos reales, simulaciones, casos de uso).  
- Respetá los **enlaces e imágenes** que yo incluya. No borres ni inventes enlaces/imágenes nuevas.  
- Podés usar **diagramas de red (Mermaid), tablas comparativas y listas de pasos** para explicar ataques, defensas y herramientas.  
- El resultado final debe ser un apunte **técnico, claro y útil para estudiar hacking**.  

Cuando te pase un texto, transformalo siguiendo estas reglas.

Aqui te va el texto:

---

para construir un cluster de fortigates en modo activo-pasivo


contaremos con la sig topologia de red

![[Pasted image 20260115085636.png]]

## GUI

`system > HA` y el modo aparece como 
- standalone: sin alta disponibilidad
- active-pasive
- active-active

> seleccionar activo-pasivo nomas

![[Pasted image 20260115084931.png]]

el modo activo pasivo tiene las siguientes opciones
- device priority.

para configuraciones de cluster

- el nombre del grupo
- password
> si estos valores no coinicden en otros forti probablemente no funcione

- session pickup -> sincroniza las sesiones.
- monitor interfaces > si se desconecta el cable desde el cliente y forti lo detecta, esta opcion se encargara de reemplazar por un nuevo activo
- heartbeat interfaces (si tuvieramos un cluster de 4 necesitariamos un switch de por mediopara que las interfaces de [[Heartbeat]] puedan comunicarse entre si)

existe por ultmo la opcion para reservar la interfaz de administracion, eso hay que habilitarlo 

![[Pasted image 20260115093459.png]]

asegurarse de poner la interfaz correspondiente previamente configurada y la Subnet

y lo mismo para los dispositivos que quieras conectar



