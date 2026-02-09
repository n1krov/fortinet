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


es un perfil de seguridad que permite que los usuarios no puedan descargar ciertos archivos

ir a `security profiles > file filter > create new`

esta politica funciona en ambos [[Modos de Inspeccion - FLOW-Based Y PROXY-Based]]


![[Pasted image 20260209090441.png]]


## **Configuración de File Filter Profile**

El filtrado de archivos permite un control granular basado en protocolos y tipos de archivos, operando de manera independiente al Antivirus.

### **1. Configuración del Perfil (Panel Izquierdo)**

- **Name**: En este caso, el perfil se llama `Archivos bloqueados`.
    
- **Scan archive contents**: Al estar activado, el FortiGate "mirará" dentro de archivos comprimidos (como .zip o .rar) para ver qué hay adentro.
    
- **Feature set**:
    
    - **Flow-based**: Análisis rápido paquete por paquete.
        
    - **Proxy-based** (Seleccionado): Reconstruye el archivo completo en memoria para un análisis mucho más exhaustivo antes de permitir su paso. Es el modo recomendado para filtrado de archivos.
        

### **2. Creación de Reglas (Panel Derecho - "Create New")**

Cuando presionas el botón **+ Create New**, defines qué buscar exactamente:

- **Protocols**: Seleccionas en qué canales de comunicación quieres filtrar. En la imagen se ven protocolos de transferencia de archivos y correo como **CIFS** (carpetas compartidas), **FTP**, **HTTP**, **IMAP**, **POP3** y **SMTP**.
    
    - _Nota_: La "P" roja junto a MAPI y SSH indica que esos protocolos solo se pueden filtrar completamente en **Modo Proxy**.
        
- **Traffic**: Define la dirección del tráfico: **Incoming** (descargas), **Outgoing** (subidas) o **Both** (ambas).
    
- **Match Files**:
    
    - **Password-protected only**: Si se marca, la regla solo afectará a archivos que estén cifrados con contraseña (los cuales el firewall no puede inspeccionar por dentro).
        
    - **File types**: Aquí seleccionas las extensiones o tipos de archivos específicos (ej. `.exe`, `.dll`, `.pdf`, `.docx`).
        
- **Action**:
    
    - **Monitor**: Deja pasar el archivo pero guarda un registro en los logs.
        
    - **Block**: Corta la transferencia del archivo inmediatamente.
        


---

## **Integración de Security Profiles en la Política de Firewall**

Cuando editás una política (como se ve en la imagen), debés bajar hasta la sección de **Security Profiles** para elegir qué motores de inspección actuarán sobre ese tráfico específico.


![[Captura de pantalla_20260209_091908.png]]

### **1. Selección de Perfiles (El "Qué" aplicar)**

En la imagen vemos que se han activado selectivamente los perfiles que estuvimos analizando:

- **IPS**: Se seleccionó el sensor llamado `Botnet`. Este motor buscará firmas de ataques y bloqueará cualquier intento de comunicación con servidores de comando y control conocidos.
    
- **File Filter**: Se aplicó el perfil `Archivos bloqueados`. Este se encargará de filtrar extensiones o tipos de archivos específicos (como .exe o .docx) según lo que definiste en su configuración.
    
- **SSL Inspection**: Es **crítico**. Se seleccionó `deep-inspection`. Sin esto, el IPS y el File Filter no podrían ver nada si el tráfico viaja por HTTPS (puerto 443).
    

### **2. Inspection Mode (El "Cómo" analizar)**

- En la parte superior de la sección se ve que la política está en **Flow-based**.
    
- **Dato importante**: Aunque la política sea _Flow_, el FortiGate es capaz de procesar el `File Filter` si este requiere modo proxy, pero lo ideal para mantener la coherencia y velocidad del motor IPS es que coincidan los modos siempre que sea posible.
    

---

## **Logging Options: El registro de actividad**

Al final de la política (abajo a la izquierda), definís qué se guarda en los logs:

- **Log Allowed Traffic**: Está configurado en **All Sessions**.
    
- **¿Por qué esto es importante?**: Al elegir `All Sessions`, el FortiGate guardará registro de absolutamente cada conexión, permitiéndote ver en el monitor de **Forward Traffic** exactamente qué archivos bloqueó el `File Filter` o qué firmas detectó el `IPS`.
    
