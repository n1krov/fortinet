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

### Full SSL Inspection on Inbount Traffic

![[Captura de pantalla_20260205_110935.png]]


Esta última captura introduce el concepto de **Full SSL Inspection para Tráfico Entrante (Inbound Traffic)**, que se utiliza específicamente para proteger servidores que tú administras (como un servidor web interno publicado a Internet).

A diferencia del tráfico saliente (donde proteges a tus usuarios), la inspección entrante protege tus recursos internos de ataques que vienen desde Internet ocultos en tráfico HTTPS.

### **¿Cómo funciona el proceso?**

Cuando un usuario externo (Alice) intenta conectarse a tu servidor protegido:

- **División de la conexión:** FortiGate actúa como un proxy transparente, terminando la conexión SSL del cliente y estableciendo una nueva hacia el servidor real.
    
- **Uso del Certificado Real:** Para que esto funcione, debes instalar en el FortiGate el **certificado del servidor, su clave privada y la cadena de certificados**.
    
- **Suplantación Legítima:** El FortiGate presenta el certificado firmado al usuario en nombre del servidor. Como el FortiGate posee la clave, puede descifrar el tráfico, inspeccionarlo y volverlo a cifrar antes de enviarlo al servidor interno.
    

### **Configuración en la GUI (Security Profiles > SSL/SSH Inspection)**

- **Enable SSL inspection of:** Se debe seleccionar la opción **Protecting SSL Server**.
    
- **Server Certificate:** Aquí seleccionas el certificado específico del servidor que has importado previamente (ej. `Cert_Webserver`).

| **Modo**                       | **Dirección** | **Objetivo Principal**                                    | **Requisito Clave**                                                 |
| ------------------------------ | ------------- | --------------------------------------------------------- | ------------------------------------------------------------------- |
| **Certificate Inspection**     | Saliente      | Filtrado web básico por nombre de dominio.                | Ninguno (muy fácil de implementar).                                 |
| **Full Inspection (Outbound)** | Saliente      | Protección total contra malware para usuarios internos.   | Instalar la **CA del FortiGate** en todos los PCs.                  |
| **Full Inspection (Inbound)**  | Entrante      | Protección de servidores propios contra ataques externos. | Importar **Certificado y Clave Privada** del servidor al FortiGate. |


La configuracion esta en el `FIREWALL POLICY` si editas una poiltica en  `security Profiles`

![[Captura de pantalla_20260205_111519.png]]


DONDE SE PROGRAMA ESTOS PERFILES??

ir a `security Profiles>SSL/SSH inspection`  
hay perfiles por defecto, pero puedes crear uno y tienes la configuracion importante en la sig imagen 

![[Captura de pantalla_20260205_111901.png]]

> nota qeu enable ssl inspection of > multiple clients connections to multiple servers es justo para *proteger a los usuarios de nuestra red* este no puede ver el payload

si queremos el caso de Full SSL Inspection on Inbount Traffic, tenemos que habilittar el ***protecting SSL Server***

**1. SSL Inspection Options (El "Qué" proteger)**

- **Enable SSL inspection of:** Define el escenario de uso.
    - **Multiple Clients Connecting to Multiple Servers:** Es el modo para **tráfico saliente** (tus usuarios navegando a Internet).
    - **Protecting SSL Server:** Es el modo para **tráfico entrante** (usuarios externos accediendo a un servidor web que tú administras).
- **Inspection Method:**
    - **SSL Certificate Inspection:** Solo lee la cabecera del paquete (SNI) para saber a qué dominio va el usuario. No descifra el contenido.
    - **Full SSL Inspection:** Descifra todo el tráfico para buscar malware, virus o fugas de datos antes de volver a cifrarlo.

**2. Configuración de Certificados (El "Cómo" descifrar)**

- **CA Certificate:** Aquí seleccionas el certificado que el FortiGate usará para firmar los certificados "suplentes" en tiempo real (por defecto `Fortinet_CA_SSL`).
- **Untrusted SSL certificates:** Define **qué hacer si el sitio web al que va el usuario tiene un certificado inválido o expirado**. Lo ideal es dejarlo en **Block** para mantener la seguridad.
- **Server certificate SNI check:** Verifica que el nombre del servidor en el certificado coincida con el SNI de la conexión. **Ayuda a prevenir ataques de suplantación.**

**3. Protocol Port Mapping (El "Dónde" buscar)**

Esta sección le dice al FortiGate en qué puertos debe intentar descifrar tráfico.

- **HTTPS (443):** Navegación web segura.
- **SMTPS (465), POP3S (995), IMAPS (993):** Protocolos de correo electrónico cifrados. Si activas estos, el Antivirus del FortiGate podrá escanear los archivos adjuntos de los correos que envían o reciben tus usuarios.