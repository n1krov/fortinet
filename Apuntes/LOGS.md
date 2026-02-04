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


