---
Tema: "[[CLI]]"
---

Quiero que actúes como un **asistente especializado en crear y embellecer manuales técnicos de ciberseguridad** dentro de **Obsidian**.  
Tu tarea será transformar un **texto que te proporcione** (o un **tema que te indique**) en un **manual claro, práctico y bien estructurado**, siguiendo las reglas de formato y estilo que detallo a continuación.
### Reglas de formato (Markdown + Obsidian)
Usá todas las herramientas que provee **Obsidian Markdown** para lograr un manual visualmente atractivo y funcional:
- **Encabezados jerárquicos** (`#`, `##`, `###`) para dividir el contenido por secciones.
- **Listas ordenadas** (pasos numerados) y **listas con viñetas** (resúmenes o notas).
- **Negritas** para comandos, rutas o términos clave, y _cursivas_ para énfasis.  
- **Bloques de código** para comandos, scripts o configuraciones:
- **Tablas** para comparar herramientas, comandos o parámetros.
- **Callouts** (`> [!info]`, `> [!tip]`, `> [!warning]`, `> [!example]`, `> [!note]`) para destacar puntos importantes.
- **Diagramas Mermaid** para flujos, procesos, redes o ataques.
- **Separadores** (`---`) para estructurar secciones grandes.
- **Enlaces internos** `[[ ]]` a otros apuntes de Obsidian si corresponde (por ejemplo, herramientas, conceptos, exploits).

### ✍️ Reglas de estilo
- El manual debe ser **directo, conciso y fácil de entender**, sin lenguaje rebuscado.
- Explicá **qué hace cada paso y por qué** (no solo qué ejecutar).
- Iniciá con una **breve introducción** al tema o procedimiento.
- Usá **títulos descriptivos** para que sea rápido de navegar.
- Agregá ejemplos reales y posibles errores comunes con soluciones.
- Si corresponde, incluí una **sección de resumen o checklist final**.

- La estructura general del manual debe fluir así:
    1. Introducción
    2. Requisitos previos
    3. Procedimiento paso a paso
    4. Ejemplo práctico
    5. Errores comunes / Solución de problemas
    6. Conclusión o comprobación final
### 🎯 Objetivo final
Transformar el texto o tema que te indique en un **manual técnico de ciberseguridad**:
- Bien formateado.
- Didáctico.
- Visualmente limpio y profesional.
- 100 % compatible con mi sistema de apuntes en **Obsidian**.

📘 Cuando te pase un texto o tema, generá el manual siguiendo estas reglas y estilo.

---

## Lista de Comandos Utiles

raiz -> `FORTI-01 # _`

`?` ->  para ver cosas sugerida s y entender en que contexto estoy
`abort`

| Comando                            | **Descripción Traducida**                       |
| ---------------------------------- | ----------------------------------------------- |
| `config` -> pisiciona en contextos | Configurar objeto                               |
| `get`                              | Obtener información dinámica y del sistema      |
| `show`                             | Mostrar configuración                           |
| `diagnose`                         | Herramienta de diagnóstico                      |
| `execute`                          | Ejecutar comandos estáticos                     |
| `alias`                            | Ejecutar comandos de alias                      |
| `exit`                             | Salir de la interfaz de línea de comandos (CLI) |

#### Opcion `config`
| **Concepto**            | **Explicación en Español**                                   |
| ----------------------- | ------------------------------------------------------------ |
| **alertemail**          | Configuración de correos electrónicos de alerta.             |
| **antivirus**           | Configuración del AntiVirus.                                 |
| **application**         | Configuración del control de aplicaciones.                   |
| **authentication**      | Autenticación.                                               |
| **dlp**                 | Configuración de DLP (Prevención de pérdida de datos).       |
| **dnsfilter**           | Configuración de filtro DNS.                                 |
| **dpdk**                | Configuración del asistente FortiOS DPDK.                    |
| **emailfilter**         | Configuración de AntiSpam.                                   |
| **endpoint-control**    | Configuración del control de terminales (endpoints).         |
| **extender-controller** | Configuración del controlador FortiExtender.                 |
| **file-filter**         | Filtro de archivos.                                          |
| **firewall**            | Configuración del Firewall.                                  |
| **ftp-proxy**           | Configuración de proxy FTP.                                  |
| **icap**                | Configuración del cliente ICAP.                              |
| **ips**                 | Configuración de IPS (Sistema de prevención de intrusiones). |
| **log**                 | Configuración de registros (logs).                           |
| **report**              | Configuración de reportes.                                   |
| **router**              | Configuración del Router.                                    |
| **sctp-filter**         | Configuración de filtro SCTP.                                |
| **ssh-filter**          | Configuración de filtro SSH.                                 |
| **switch-controller**   | Configuración externa de FortiSwitch.                        |
| **system**              | Configuración de operación del sistema.                      |
| **user**                | Configuración de autenticación.                              |
| **videofilter**         | Filtro de video.                                             |
| **voip**                | Configuración de VoIP (Voz sobre IP).                        |
| **vpn**                 | Configuración de VPN (Red Privada Virtual).                  |
| **waf**                 | Configuración del Firewall de Aplicaciones Web (WAF).        |
| **wanopt**              | Configuración de optimización de WAN.                        |
| **web-proxy**           | Configuración de proxy web.                                  |
| **webfilter**           | Configuración de filtro web.                                 |
| **wireless-controller** | Configuración del punto de acceso inalámbrico.               |

`config system alias ?` 
	- set
	- unset
	- get
	- show
	- next
	- abort
	- end




#### Opcion `get`

get system interface


#### Opcion `show`
show system interface

#### Opcion `diagnose`


#### Opcion `execute`

`exec ping-options`

`exec ping-options view-settings`

`exec ping-options source 1.1.1.1`

`exec ping-options reset`


#### Opcion `alias`
`alias <comando>`