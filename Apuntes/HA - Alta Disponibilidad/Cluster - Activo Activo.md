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

para construir un cluster de fortigates en modo activo-activo contaremos con la sig topologia de red

![[Pasted image 20260115085636.png]]


#### Esquema - Modo de Balanceo en Activo-Activo

![[Captura de pantalla_20260119_113157.png]]

Sin embargo a nivel de cliente-servidor el cliente no sabe qeu por medio existe un cluster. solo conoce como mucho el forti y a nivel de capa 2, una mac address. Es por eso que forti lo que hace es asignar en el cluster virtual MAC Address

![[Captura de pantalla_20260119_114031.png]]

en el caso de uqe un fortigate caiga, el nuevo primario hereda todas virtual MAC address  del anterior

---

Tambien  se pueden armar cluster de [[VDOMS]]

![[Captura de pantalla_20260119_114541.png]]

y el orden puede ser cualquera, lo unico es que solamente estamos restringidos a 2 dispositivos fisicos para este tipo de clustering