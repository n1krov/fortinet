---
Tema: "[[policys&objects]]"
---

Quiero que actúes como un asistente especializado en crear, mejorar y embellecer mis apuntes de **conceptos y wiki** en Obsidian.

### Reglas de formato:

- Usa **Markdown** y todas las herramientas nativas de Obsidian:
    - Encabezados jerárquicos (#, ##, ###…) para organizar los temas.
    - **Negritas** y _cursivas_ para resaltar ideas clave.
    - Listas ordenadas y no ordenadas para definiciones, características o pasos.
    - Tablas para comparar conceptos, pros/cons o clasificaciones.
    - Callouts (`> [!info]`, `> [!quote]`, `> [!summary]`, etc.) para destacar definiciones, notas históricas o ejemplos.
    - Diagramas con **Mermaid** (mapas mentales, jerarquías, taxonomías).
    - Separadores `---` para dividir secciones claramente.

### Reglas de estilo:

- Transformá los textos en apuntes de **estilo enciclopédico**: claros, neutros, precisos y fáciles de consultar.
- Si el texto es largo, dividilo en secciones con títulos descriptivos.
- Iniciá siempre con una **definición breve y clara** del concepto.
- Agregá ejemplos de uso, contexto histórico y aplicaciones cuando sea relevante.
- Usá tablas, diagramas y listas para facilitar la comprensión y consulta rápida.
- Si se mencionan términos relacionados, podés destacarlos como **links internos de Obsidian** (`[[Concepto Relacionado]]`).
- Evitá redundancias: simplificá y resumí sin perder precisión.

Cuando te pase un texto de un concepto, transformalo en una **nota wiki estructurada, clara y útil** para consulta rápida y aprendizaje.

---

# Fortinet single sign on ogent configuration



![[Pasted image 20260131160058.png]]

## 1. Configuración de Escucha (Listening Ports)

Estos son los "oídos" del agente. Define por qué puertos se comunicará con otros dispositivos:

- **FortiGate (8000):** Es el puerto estándar donde el FortiGate se conecta al agente para pedir la lista de usuarios.
    
- **DC Agent (8002):** Si usas el modo "DC Agent" (un pequeño software instalado directamente en tus Controladores de Dominio), este puerto recibe los eventos de logon desde ellos.
    
- **Enable SSL:** Si lo marcas, la comunicación entre el FortiGate y este agente estará cifrada (recomendado para seguridad, pero debe coincidir en ambos lados).
    

## 2. Registro de Eventos (Logging)

- **Log level:** Determina qué tanta información se guarda. "Warning" solo guarda errores importantes. Si tienes problemas de conexión, cámbialo a "Information" o "Debug" temporalmente.
    
- **Log file size limit:** Evita que el archivo de log llene el disco duro del servidor. **10MB** es el estándar, pero si estás debugeando mucho, podrías subirlo.
    

## 3. Autenticación (Authentication)

- **Require authenticated connection from FortiGate:** Es una capa de seguridad vital. El "Password" que ves ahí (los puntitos) debe ser el mismo que configures en el FortiGate al dar de alta este servidor FSSO. Si no coinciden, el firewall no podrá leer la lista de usuarios.
    

## 4. Temporizadores (Timers)

Estos controlan qué tan "fresca" es la información:

- **Workstation verify interval (5 min):** El agente hace un "ping" o chequeo remoto a las PCs para ver si el usuario sigue ahí. Si la PC no responde, asume que el usuario se fue.
    
- **Dead entry timeout (480 min):** Es el tiempo máximo que una sesión permanece activa antes de ser borrada si no hay actividad detectada (8 horas en este caso).
    
- **IP address change verify (60 sec):** Revisa si la IP de un usuario cambió (común en redes WiFi).
    

---

## 5. Tareas Comunes (Botones de la derecha)

Son los accesos rápidos para administrar el sistema:

- **Show Monitored DCs:** Te muestra a qué servidores de Windows (Domain Controllers) está conectado el agente.
    
- **Show Logon Users:** Es el más útil; abre una lista en tiempo real de qué usuarios ha detectado el sistema y sus IPs.
    
- **Select Domains To Monitor:** Aquí eliges qué dominios o unidades organizativas (OU) de tu Active Directory quieres que el FortiGate "vea".
    
- **Set Group Filters:** Sirve para filtrar y enviar al FortiGate solo los grupos que te interesan (por ejemplo, "Usuarios_VPN" o "Ventas"), evitando saturar la memoria del firewall con grupos innecesarios.
    

---

### Un detalle importante:

Support NTLM authentication"**. Esto permite que, si el método pasivo (FSSO) falla, el FortiGate pueda pedirle al navegador del usuario que se autentique mediante un desafío/respuesta de Windows.



## Seleccionar grupos del dominio que