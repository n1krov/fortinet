---
Tema: "[[IPsec]]"
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

probablemente quiras cambiar el nombre de los tuneles pero eso unicamente es posible por [[CLI]]

`show | grep -f nombre_tunel`

copiamos la salida y la pegamos en un bloc de notas para editarlo
borramos lo que tenga como `<---` y configuraciones que no sean del nombre del tunel poruqe por ahi el grep te trae alguna config con el nombre que tiene incluido el patron de la busqueda. fijarse eso.


ahi reemplazar el nombre en el bloc de notas. luego eliminar las politicas. en ese ejemplo del curso. el borro todo lo que tenia asociado con ese nombre y tuvo que borrar el tunel, las poliiticas y las firewall policys

luego entrar a la CLI y pegar lo de tu bloc de notas y listo


es una forma artesanal de cambiar el nombre pero no queda otra opcion

otra forma es tomar un backup y editar en las configuraciones en bruto los nombres, esto depende de la complejidad de politicas y reglas que tengas