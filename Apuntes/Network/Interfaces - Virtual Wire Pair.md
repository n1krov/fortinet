---
Tema: "[[network]]"
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

Siguiendo con la parte de  [[Modos de Operacion]]
existe una configuracion adicional que se puede usar en ambos modos tanto para transparente o NAT y esta es la Virtual wire pair que esta dentro de `network>Interfaces`

![[Pasted image 20260113104521.png]]

con el virtual wire pair, lo que se hace es crear un circuito en donde seleccionamos 2 interfaces y el trafico que entre por alguna de ellas SOlamente puede ir hacia la otra a la que estaba asociada.
esto pierde conectividad total con el resto de las interfaces

esto basicamente crea un circuito logico entre ellas dos y las aisla totalmetne de las demas

- ventaja: nos reduce el broadcast {no tengo ni idea que significa  esto}

![[Pasted image 20260113104554.png]]

> nota que hasta antes de configurar teniamos el puerto 2 y 3 en el mismo dominio de broadcast. ahora en esa imagen se ve separado

si creamos el virtual wire pair tambien al lado en [[policys&objects]] se crea una opcion llamada "firewall virtual wire pair policy"

y la configuracion es parecida, solo cambia una pequeña parte

![[Pasted image 20260113104927.png]]

