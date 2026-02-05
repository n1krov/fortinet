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



#### Full SSL Inspection

![[Captura de pantalla_20260205_110039.png]]

Esta última imagen explica el concepto de **Full SSL Inspection** (también conocido como _Deep SSL Inspection_), que es el nivel más alto de seguridad para el tráfico cifrado.


 **Full SSL Inspection en Tráfico Saliente**

A diferencia de la inspección de certificados básica, el modo **Full Inspection** permite al FortiGate "abrir" el tráfico cifrado para inspeccionar su contenido en busca de virus o filtraciones de datos.

**¿Cómo funciona el proceso? (Pasos 1-5)**

1. **Interceptación:** El FortiGate actúa como un intermediario (Man-in-the-Middle) interceptando la solicitud HTTPS del usuario hacia internet.
    
2. **Validación:** El firewall se conecta al servidor web real (ej. `www.ex.ca`) y recibe su certificado original.
    
3. **Suplantación Segura:** El FortiGate genera un nuevo certificado "falso" que tiene el mismo nombre que el sitio original, pero firmado por la **CA (Autoridad de Certificación) del propio FortiGate**.
    
4. **Entrega al Cliente:** El FortiGate envía este certificado generado al navegador del usuario. El navegador es "engañado" pensando que se ha conectado directamente al servidor web, siempre y cuando confíe en la CA del FortiGate.
    
5. **Doble Túnel SSL:** Se establecen dos conexiones cifradas independientes: una entre el navegador y el FortiGate, y otra entre el FortiGate y el servidor web.
    

**Ventajas y Requisitos Críticos**

- **Visibilidad Total:** Al descifrar el tráfico en el medio (paso 5), el FortiGate puede aplicar Antivirus, IPS y control de aplicaciones dentro del contenido que antes estaba oculto.
    
- **Requisito de Confianza:** Para que esto funcione sin alertas de seguridad constantes en las computadoras, es **obligatorio instalar el certificado CA del FortiGate** en el almacén de certificados de todos los dispositivos de la red.
> paraa q fucnione se debe descargar el certificado e instalar en los navegadores correspondientes

- **Protección contra Malware:** Es el único método capaz de detectar virus que viajan dentro de sesiones HTTPS.


