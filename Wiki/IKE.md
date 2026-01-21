---
Tema: "[[wiki]]"
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


IKE quiere decir internet key exchange lo que hace es negociar las llaves privadas que van a estar utilizando los dos fortigates o dos equipos 
> tener en cuenta que ipsec es un estandar y no es propietario de ninguna marca


IKE define 2 fases 
- fase 1
- fase 2

posee 2 versiones
IKEv1 (legacy, mayor adopcion)
IKEv2 (nuevo, operacion simple)

ike se encarga de establecer SA (secure asociations) 

## **Resumen: Internet Key Exchange (IKE)**

IKE es el protocolo encargado de establecer un túnel seguro mediante la negociación de llaves privadas, autenticación y cifrado.

---

### **1. Datos Técnicos Básicos**

- **Puertos:** Utiliza **UDP 500** por defecto.
    
- **NAT Traversal:** Cambia a **UDP 4500** cuando el tráfico debe cruzar un NAT.
    

### **2. Versiones de IKE**

- **IKEv1:** Es la versión **legacy** (antigua), pero actualmente tiene la **mayor adopción** en la industria.
    
- **IKEv2:** Versión más reciente, diseñada para una **operación más simple** y eficiente.
    

### **3. Negociación y Fases (SA)**

IKE permite que ambas partes configuren sus **Security Associations (SA)**, que son la base de las funciones de seguridad en IPsec. El proceso se divide en dos fases críticas:

- **Fase 1 (IKE SA):**
    
    - **Objetivo:** Establecer un canal seguro y autenticado entre los dos dispositivos (ej. entre dos FortiGate).
        
    - **Resultado:** Se genera la **IKE SA**.
        
- **Fase 2 (IPsec SA):**
    
    - **Objetivo:** Negociar los parámetros para proteger el tráfico de datos real que pasará por el túnel.
        
    - **Resultado:** Se genera la **IPsec SA**, que se usa para cifrar y descifrar los datos enviados y recibidos.
        

##### **Punto Clave para Configuración**

> Los administradores de red deciden qué algoritmos de cifrado y autenticación se permiten durante este intercambio para garantizar la integridad y privacidad.




## Topologias de RED


![[Pasted image 20260120113019.png]]

### **1. Simple (Punto a Punto)**

Es la forma más básica de conexión.

- **Estructura:** Conecta directamente una oficina sucursal (**Branch office**) con la oficina central (**Headquarters**).
- **Uso:** Ideal para empresas pequeñas con una sola sucursal que necesita acceder a los recursos del servidor central.

### **2. Hub-and-Spoke (Estrella)**

Es la topología más común en empresas en crecimiento.

- **Estructura:** El **Headquarters** actúa como el "Hub" (centro) y todas las sucursales son los "Spokes" (radios).
- **Funcionamiento:** Si una sucursal quiere hablar con otra, el tráfico normalmente debe pasar primero por la oficina central.
- **Ventaja:** Centraliza el control y es más fácil de configurar que conectar todos entre sí.

### **3. Full Mesh and Partial Mesh (Malla)**

Es la configuración más robusta y compleja.

- **Full Mesh (Malla Completa):** **Todos** los sitios tienen un túnel directo con todos los demás. Si se cae un enlace, hay múltiples rutas alternativas.
- **Partial Mesh (Malla Parcial):** Solo algunos sitios (los más críticos) están conectados entre sí directamente, mientras que otros siguen usando el modelo de oficina central.
- **Uso:** Se usa cuando se necesita baja latencia entre sucursales (ej. telefonía IP entre oficinas) o alta disponibilidad crítica.