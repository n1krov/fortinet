---
Tema: "[[HA]]"
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

HA comandos por [[CLI]]


`diagnose sys ha status`

con esto se puede ver quienes son los equipos conectados al cluster. quien es el primario y secundarios, hostnames, prioridades del serial number y prioridad de usuario

tambien muestra `primary_ip` que es la ip asociasda da la interfaz de [[Heartbeat]], pero esta es un manejo interno del kernel a la hora de configurar el cluster segun el serial number. el que tenga el serial number mas alto tiene la direccion ip mas baja


`diagnose sys ha checksum cluster`
los fortigate a la hora de saber si estan sincronizados, hacen un checksum entre ellos en el archivo de configuracion. 

tambien podemos ejecutar o cambiar de usuario haciendo 
`execute ha manage <num_root_primary> admin`

