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

supongamos que el site a y el site b, tienen la misma ip de subred por ej, 192.168.1.0/24

se crean los tuneles para ambos lados, pero lo que cambia es la fase 2

la fase 2 se debe configurar tanto la ip local como la remota IPs ficticias que no existan en la red local ni en la red remota

por ej 
172.21.0.0/24 para A
172.22.0.0/24 para B

![[Captura de pantalla_20260203_103534.png]]

ahora tambien lo importante es definir las **rutas estaticas**

recordar que las rutas estaticas es para donde va a ir, en este caso por ejemplo si estamos en el site B. la ruta estatica indica a donde queremos ir y por que interfaz

![[Captura de pantalla_20260203_103719.png]]

> nota que la interface es el tunel que se creó


continuamos con la firewall policy para ambos sitios a y b

este es un ejemplo desde el site A saliendo a internet hacia el B, incluyendo la configuracion del [[IP Pools]]

![[Captura de pantalla_20260203_104127.png]]

nota que se utiliza nat para esta policy
lo importante tambien es que aca el outgoing es el tunel que se configuro anteriormente

la configuracion del nat es mediante el uso de ip pools ya que aqui usamos el modo fixed port range mapeando las ips ficticias con las subnets existentes uno a uno. Esto es porque el el tunel no tiene IP

esta configuracino se hace lo mismo para el site A.

---

volviendo al site B: COMO SE CONFIGURA LA LLEGADA DE INTERNET

aqui utilizaremos el concepto de [[Virtual IP - Port Forwarding Mediante Objetos VIPs]]

y aqui se entiende el por que se hace esto de la red ficticia.

cuando un paquete del site A llega al site B. llega con la ip enmascarada. 172.21.0.0/24, ES MAS, el site b NO tienen por que saber que tienen la misma subred. o puede ser que no saben que desde el site A se esta enmascarando y viene con la 172...

por lo que esta es la razon. ademas que se enmascara para poder trabajar con las VIPs.

>[!note] Las ips ficticias actuan como un puente entre ambos sites con la misma subnet

aqui la politica del trafico que viene del SIte A

![[Captura de pantalla_20260203_105513.png]]
> no lleva NAT aqui
> nota que aqui la configuracion critica esta enel Destination. que es una VIP llamada IPSEC

La configuracion se ve de la siguiente manera
