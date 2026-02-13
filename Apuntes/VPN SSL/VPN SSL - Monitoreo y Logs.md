---
Tema: "[[VPN SSL]]"
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

en dashboard > network > SSL-VPN

![[Captura de pantalla_20260213_104655.png]]
eso esta interesante ya que se puede ver a detalle los usuarios que teineen acceso cuandtas conexiones tiene establecidas y su actividad

tambien en `Log &report > events > user events` se puede ver aqui tambine los eventos de autenticacin de usuarios

tambien en `Log &report > events > VPN events` etsan los logs de los tuneles vpns

tambien en `VPN > ssl-Vpn settings` podemos ver los timers

![[Captura de pantalla_20260213_105102.png]]

> en la imagen se puede ver uqe se puede desloguear automaticamente luego de que pasen los 3000 segundos de idle


para acceso a otros timers se hace por [[CLI]]

conifg vpn ssl settings
	get | grep time
	
