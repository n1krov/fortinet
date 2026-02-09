---
Tema: "[[apuntes]]"
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

primero configurar el fortigate para el correo electronico

>[!important] si es gmail, en seguridad de la cuenta habilitar accesoa  aplicaciones poco seguras


hacer en cli
`config system email-server`

### **Parámetros de `config system email-server`**

- **`type: custom`**: Indica que estás definiendo manualmente los parámetros del servidor de correo.
- **`server: notification.fortinet.net`**: Este es el host encargado de enviar los correos. Es un servicio gratuito que ofrece Fortinet para alertas básicas.
- **`port: 465`**: El puerto utilizado para la conexión. El puerto 465 es el estándar para **SMTPS** (SMTP sobre SSL/TLS).
- **`security: smtps`**: Establece que la comunicación entre el FortiGate y el servidor de correos debe estar cifrada para proteger la información de las alertas.
- **`authenticate: disable`**: En este caso, la autenticación está desactivada. Esto es común cuando se usan servidores internos de confianza o el servicio de notificación de Fortinet en ciertos escenarios.


luego

`set server <server_smtp>`

`set authenticate enable`

`set username <correo>`
`set password <pass>`

>[!warning]
>Esto necesita un equipo con capacidad ssl


luego **Configurar las alertas**

`config alertmail set`

si tiras un `get`
![[Captura de pantalla_20260209_093850.png]]

### **1. Destinatarios y Frecuencia**

- **`mailto1 / mailto2 / mailto3`**: Son los campos donde debés ingresar las direcciones de correo que recibirán las alertas.
- **`email-interval`**: Está configurado en **5 minutos**. Esto sirve para agrupar alertas; si ocurren 10 virus en un minuto, el equipo esperará 5 minutos para enviarte un solo correo con el resumen, evitando saturar tu bandeja de entrada.
- **`filter-mode: category`**: Indica que vas a elegir qué te avisa basándote en categorías de logs (como Antivirus, IPS, etc.).
### **2. Selección de Logs (El "Qué" te notifica)**

En tu captura, casi todos los sensores están en `disable`. Para que te lleguen alertas de lo que estuvimos configurando, deberías pasar a `enable` los siguientes:

- **`antivirus-logs`**: Te avisa cada vez que el motor AV detecta y bloquea malware.
- **`IPS-logs`**: Te notifica sobre intentos de intrusión o ataques de exploits bloqueados.
- **`webfilter-logs`**: Útil si querés saber cuándo alguien intenta entrar a sitios prohibidos.
- **`HA-logs`**: Crucial si tenés dos equipos; te avisa si el primario falla y el secundario toma el control.

### **3. Advertencias de Licencia**

- **`FDS-license-expiring-warning`**: Si lo habilitás, el equipo te enviará un recordatorio antes de que venzan tus servicios de FortiGuard.
- **`FDS-license-expiring-days: 15`**: Es el tiempo de antelación. Te avisará 15 días antes del vencimiento.


---

luego **Programando las alertas**

`config alertmail set`

`set email-interval` cada cuanto mandar correo
`set username correo`

`set mailto1 correo`

luego haces enable a los eventos que uno quiere ir informando
`set evento enable`
`end`

si quieres testear las conexiones
`diagnose log alertmail test`

