---
Tema: "[[apuntes]]"
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

### No ssl inspection

### **1. El problema: No SSL Inspection**

- **Oculto por cifrado:** Sin inspección, los virus y amenazas pueden pasar a través de las defensas de la red porque están "disfrazados" por el cifrado SSL/TLS.

- **Vulnerabilidad:** A menos que se habilite la inspección completa (Full SSL Inspection), el firewall no puede "ver" dentro del paquete cifrado para buscar malware.    

### **2. ¿Qué es SSL Certificate Inspection?**

Este es un modo de inspección más ligero que no descifra el contenido del tráfico, sino que analiza la "identidad" de la conexión.

- **Uso de SNI:** FortiGate utiliza el **Server Name Indication (SNI)** para identificar el nombre del servidor (hostname) al comienzo del saludo SSL (handshake).    
- **Alternativas:** Si no hay SNI disponible, el equipo revisa los campos del certificado, como el **Subject** y el **Subject Alternative Name (SAN)** para saber a qué sitio se intenta acceder.

![[Captura de pantalla_20260205_104424.png]]

