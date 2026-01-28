---
Tema: "[[network]]"
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

este es un caso practico que describe como hacer tuneles [[IPsec]] y [[SD-WAN]]

como siempre se usa 

![[topologia.png]]

casos como este que se quieran hacer uso de una zona sd-wan con **tuneles** como elementos
tener en cuenta que los tuneles no deben tener referencia. excepto la fase 2

paso 1: Creamos los tuneles. en este caso son 4. como vimos en [[Crear Tunel]]
- isp1 a
- isp1 b
- isp2 a
- isp2 b

una vez creado los tuneles, seguimos con el sigiente paso


paso 2: crear las SD-WAN
creamos la sd-wan llamada `IPsec` este tendra como interfaces los dos tuneles de cada forti.

en la zona A tendriamos que tener 2 sd_wan
- la sd-wan de internet
- sd-wan ipsec

paso 3 rutas estaticas
ponemos a donde queremos ir, en este caso la maquina `workstation` que tiene la red 10.0.2.0/24

y aqqui la novedad de forti 7: en la interfaz le podemos agragar la zona ipsec que armamos

![[Captura de pantalla_20260127_084723.png]]

ahora con esto creado sabe por donde navegar. pero faltaria crear las politicas

paso 4: crear las Politicas de Firewall

en este caso lo importante es el 
incomming -> lan (puerto 4) que es donde esta conectado el dispositivo
outgoing -> el sdwan `ipsec`

no va NAT, ya que no va por internet directamente

---

luego de la configuracion queda por darle uso a sd-wan haciendo que balancee de tal forma que vaya por el ipsec con la **menor latencia** posible entre esos tuneles definidos.

para eso vamos a sd-wan > Performance SLAs