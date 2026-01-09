---
Tema: "[[network]]"
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

# VLANs en Fortigate

![[Pasted image 20260109114714.png]]

una VLAn nos permite subdividr una o mas ***INTERFACES fisicas*** en diferentes ***INTERFACES logicas*** 
- esto se puede llevar a cabo gracias a la insercion de las VLAN tags en los frames de ethernet

### Trama ethernet

![[Pasted image 20260109120136.png]]

en fortigate se representa en la trama de la siguiente manera

![[Pasted image 20260109120223.png]]

como se puede ver fortigate tiene reservado 4 bytes reservado para el manejo de las tags de las vlans.
los equipos de capa 2 (Swithces) tienen la capacidad de agragar o remover estos tags
los equpos de capa 3 (fortigate) tienen la capacidad de sobreescribir estos tags, y esto lo hacen para poder enrutar todo el trafico correctamente

