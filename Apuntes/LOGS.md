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

fortinet los equipos de gama media alta ya te dan generalmente un disco prara que vaya guardando los logs pero sies gama baja no tiene.

ademas fortinet recomienda el uso de fortianalyzer.

### Configuracion por [[CLI]]

`config log disk setting`

![[Captura de pantalla_20260204_110218.png]]

- **`status : enable`**: El registro de logs en el disco duro local del dispositivo está activado.
- **`maximum-log-age : 7`**: Los logs se conservarán en el disco por un máximo de **7 días**. Los más antiguos se eliminarán automáticamente.
- **`log-quota : 0`**: Define el espacio máximo (en MB) dedicado a logs. Al estar en **0**, el sistema utiliza el valor por defecto basado en la capacidad total del disco.
- **`max-log-file-size : 20`**: El tamaño máximo de cada archivo de log antes de cerrarse y rotar es de **20 MB**.
- **`roll-schedule : daily`** y **`roll-time : 00:00`**: Los archivos de log se "rotan" (se cierran y se crea uno nuevo) **diariamente a medianoche**.
- **`diskfull : overwrite`**: Comportamiento crítico. Cuando el disco asignado a logs se llena, el FortiGate **sobrescribe los registros más antiguos** con los nuevos para no interrumpir el servicio.
- **`full-warning-threshold` (75, 90, 95)**: Son los porcentajes de ocupación del disco que dispararán alertas (warnings) para avisar al administrador antes de que se llegue al límite.

>[!note] **Dato útil:** En entornos de producción con mucho tráfico, se recomienda enviar estos logs a un **FortiAnalyzer** o un servidor Syslog externo para no desgastar el almacenamiento local (especialmente si es memoria Flash).


para cambiar alguno por ej

`set maximum-log-age 3`


en Log & report > Log settings estan estas configuraciones

Aquí tienes el desglose de ambas capturas para que las añadas a tus apuntes. La primera es la configuración del **disco local** por línea de comandos y la segunda es la configuración de **almacenamiento remoto** desde la interfaz gráfica.

### **Configuración de Log Remoto y GUI (Web)**

Esta sección gestiona el envío de logs a servidores externos y preferencias de visualización.

- **Remote Logging and Archiving**:
    - **Send logs to FortiAnalyzer/FortiManager**: Está en `Disabled`. Esto significa que el equipo no está enviando datos a una unidad de gestión centralizada de Fortinet.
    - **Send logs to syslog**: Desactivado. No se están enviando logs a servidores de terceros (como un SIEM o servidor Linux).
- **Log Settings**:
    - **Event Logging**: Configurado en `All`, registrando todos los eventos del sistema (login, cambios de configuración, túneles VPN, etc.).
    - **Local Traffic Log**: Configurado como `Customize`. Permite elegir específicamente qué tráfico de la red registrar (Permitido, Denegado, etc.).
- **GUI Preferences**:    
    - **Resolve Hostnames/Unknown Applications**: Activados. El FortiGate intentará mostrar nombres de dominio y aplicaciones conocidos en lugar de solo direcciones IP y puertos en los reportes.

##### **Diferencia Clave para tus notas:**

> Mientras que el **disco local** (Captura 1) es útil para revisiones rápidas, el **almacenamiento remoto** (Captura 2) es esencial para el cumplimiento legal y el análisis histórico a largo plazo, ya que el disco local tiene capacidad limitada y termina sobrescribiendo datos.



 **Comandos Esenciales de Logging en FortiGate**

**1. FortiAnalyzer (Logs Remotos)**

Para configurar o verificar el envío de datos al recolector central de Fortinet.

- `config log fortianalyzer setting`: Entra al menú de configuración para definir la IP del FortiAnalyzer y el estado del envío.
    
- `set status enable`: Activa el envío de logs.
    
- `set server <IP_del_Analyzer>`: Define el destino.
    
- `get`: Dentro del menú, muestra la configuración actual (IP, estado de conexión, serial del Analyzer).
    

 **2. Syslog (Servidores de Terceros)**

Si utilizas un servidor Linux o un SIEM externo.

- `config log syslogd setting`: Accede a la configuración del demonio de syslog.
    
- `set status enable`: Habilita el servicio.
    
- `set server <IP_Syslog>`: Especifica la dirección del servidor remoto.
    
- `set mode {udp | legacy-reliable}`: Define el protocolo de transporte (UDP es el estándar).
    

 **3. Event Filter (Filtro de Eventos)**

Sirve para decidir **qué tipo** de eventos del sistema se guardan y cuáles se ignoran para no saturar el disco.

- `config log eventfilter`: Entra al menú de filtros.
    
- `show`: Muestra qué categorías de eventos están activadas (ej. `system`, `vpn`, `user`, `router`).
    
- `set <categoría> {enable | disable}`: Permite activar o desactivar eventos específicos. Por ejemplo, si no usas VPN, puedes desactivar esos logs para ahorrar espacio.
    

 **4. Config Log Settings (Configuración General)**

Controla el comportamiento global de los logs en el equipo.

- `config log setting`: Menú principal de ajustes.
    
- `show`: Muestra configuraciones como el `log-location` (disco, memoria o remoto) y si el registro de tráfico local está activo.
    

**Privacidad y Diagnóstico**

**5. User-Anonymize (Anonimización de Datos)**

Se utiliza cuando, por normativas de privacidad (como GDPR), **no se quiere registrar información sensible** del usuario.

- **Comando:** `set user-anonymize enable` (dentro de `config log setting`).
    
- **¿Para qué sirve?:** Reemplaza el nombre de usuario o datos identificativos en los logs por un valor genérico o hash, permitiendo ver el tráfico pero protegiendo la identidad del individuo.
    

**6. Diagnose Sys Logdisk Usage (Estado del Disco)**

Este es un comando de diagnóstico, no de configuración.

- **Comando:** `diagnose sys logdisk usage`
    
- **¿Para qué sirve?:** 1. Muestra el **espacio total** del disco duro dedicado a logs.
    2. Indica cuánto espacio hay **disponible**.
    3. Desglosa cuánto están ocupando los distintos tipos de logs (Traffic, Event, UTM).
    4. Es vital para saber si el umbral de advertencia (threshold) de tus capturas anteriores está cerca de activarse.
`diagnose log test`


----

log & report > forward traffic

La sección de **Forward Traffic** es el centro de monitoreo principal del FortiGate. Aquí ves en tiempo real (o histórico) todo el tráfico que atraviesa el firewall desde una interfaz de origen hacia una de destino.

**1. Información de Flujo (Columnas Principales)**
- **Source / Destination:** Quién se comunica con quién. Puedes ver tanto direcciones IP como nombres de usuario (ej. `test user`) y países de destino mediante banderas.
- **Application Name:** Identifica qué aplicación está usando el usuario (ej. `YouTube_Video.Play`, `Dropbox_File.Download`, `BitTorrent`).
- **Result:** Indica si el tráfico fue permitido (check verde) o denegado (círculo rojo).
- **Policy ID:** Muestra exactamente qué regla de firewall permitió o bloqueó ese tráfico. Es vital para troubleshooting.

**2. Análisis de Seguridad (Panel Derecho)**
Cuando seleccionas un log (como el que tienes resaltado en amarillo), el panel de la derecha te da el "peritaje" detallado:
- **Security Action:** Explica por qué se bloqueó algo. En tu imagen vemos un `Deny: UTM Blocked`.
- **Application Control:** Muestra la categoría y el nivel de riesgo de la aplicación detectada.
- **Sent/Received Bytes:** El volumen de datos transferidos en esa sesión específica.

**3. Tipos de Bloqueos Visibles**
En tu pantalla se aprecian tres tipos de acciones comunes:
- **UTM Blocked:** El tráfico fue denegado por un perfil de seguridad (como Web Filter o Application Control).
- **Policy Violation:** El tráfico no cumplió con los requisitos de la política de firewall (regla de denegación explícita).
- **Resultados Aceptados:** Tráfico legítimo que fluye normalmente consumiendo ancho de banda.

> **Forward Traffic** registra el tráfico "de paso" por el equipo. Es la herramienta principal para verificar si una política de firewall o un perfil de seguridad (UTM) está actuando correctamente sobre los usuarios.
