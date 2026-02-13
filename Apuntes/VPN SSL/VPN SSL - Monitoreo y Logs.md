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

Timers Principales de Sesión y Autenticación

- **`idle-timeout : 3000`** Determina el tiempo (en segundos) que una sesión de administración (como tu acceso actual por CLI o GUI) puede estar inactiva antes de que el FortiGate te desconecte automáticamente por seguridad. En este caso, son **50 minutos**.
    
- **`auth-timeout : 28800`** Es el tiempo máximo de validez para una autenticación de usuario. Una vez transcurrido este tiempo (8 horas), el usuario deberá volver a ingresar sus credenciales, sin importar si ha estado activo o no. Este valor es crítico en entornos con **SSL VPN** o **FSSO**.
    

---

### 🛡️ Seguridad y Control de Login

- **`login-block-time : 60`** Si alguien intenta adivinar una contraseña y falla repetidamente, el FortiGate bloqueará nuevos intentos desde esa IP durante este periodo (60 segundos). Ayuda a mitigar ataques de fuerza bruta.
    
- **`login-timeout : 30`** Es el tiempo máximo que el sistema espera a que un usuario complete el ingreso de sus credenciales (usuario/password) desde que se abre la pantalla de inicio de sesión. Si tardas más de 30 segundos, la conexión se cierra.

 Protocolos y Tráfico Web (HTTP/DTLS)

- **`dtls-hello-timeout : 10`** Específico para **SSL VPN** cuando utiliza el protocolo **DTLS** (UDP) para mejorar el rendimiento de aplicaciones como voz o video. Es el tiempo de espera para el saludo inicial del protocolo.
    
- **`http-request-header-timeout : 20`** / **`http-request-body-timeout : 30`** Controlan cuánto tiempo espera el FortiGate para recibir los encabezados o el cuerpo de una solicitud HTTP antes de cerrar la conexión por inactividad. Previenen que conexiones lentas o incompletas saturen los recursos del equipo.

como configurarlo 

conifg vpn ssl settings
	get | grep time
	set auth-timeout (int)
