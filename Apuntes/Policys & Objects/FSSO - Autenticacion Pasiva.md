---
Tema: "[[policys&objects]]"
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

Autenticacion pasiva con forti FSSO

existen tambien 3 tipos de FSSO q forti soporta

### DC Agent Mode Process

esta es la mas usada y la mas escalable

![[Captura de pantalla_20260130_102551.png]]
1. el usuario se autentica contra el DC de windows
2. el Agente DC ve el evento de autenticacion y redirige al collector agent
3. el collector agent recive el evento del DC agente y redirige al fortigate
4. El fortigate conoce el usuario por su ip, entonces el usuario no necesita autenticarse constantemente

> nota que este modo trabaja con DC AGENT y COLLECTOR AGENT generalmente en el mismo servidor


el collector agent trabaja en el 8002 por UDP
y el forti trabaja por el 8000 por TCP
el collector agent envia al forti
- nombre de usuario
- nombre de host
- Direccion IP
- grupos de usuario


### Collector Agent-Based Polling Mode Process

Como se muestra en la imagen, 

![[Captura de pantalla_20260130_103235.png]]
en este modo:
1. el usuario se autentica con el DC (domain controller)
2. el colector ahce la extraccion en el DC para recolectar eventos de login
3. el colector luego redireccion a los logins al fortigate
4. el usuario no necesita autenitcarse

> nota que el dc agent, no esta instalado, esto es sin dc agent, en el servidor esta instalado solo el agente de collector

el collector agent trabaja en el 445 por TCP
y el forti trabaja por el 8000 por TCP
el collector agent envia al forti
- nombre de usuario
- nombre de host
- Direccion IP
- grupos de usuario


### Agentless Pollign Mode Process
![[Captura de pantalla_20260130_113314.png]]