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


### Modos de configuracion de VPN IPsec

![[Pasted image 20260120112240.png]]

ESP -> encapsulation security payload
##### **1. Modo Túnel (El más utilizado)**

Es el modo predeterminado para las VPN de **sitio a sitio** (Lan-to-Lan) y acceso remoto.

- **¿Qué hace?:** Cifra el paquete IP **completo** (encabezado original + datos).
- **Encapsulación:** Como el encabezado original está cifrado y es ilegible para los routers de Internet, IPsec añade un **Nuevo Encabezado IP**.
- **Seguridad:** Es el más seguro porque **oculta las direcciones IP internas** (origen y destino originales) de la red.
- **Uso:** Ideal para conectar dos oficinas a través de Internet, ya que permite que dispositivos con IPs privadas se comuniquen de forma invisible para el mundo exterior.
##### **2. Modo Transporte (Específico y limitado)**

Se utiliza principalmente para comunicaciones **extremo a extremo** (Host-to-Host) entre dos dispositivos específicos.

- **¿Qué hace?:** Cifra únicamente la **carga útil** (los datos reales como TCP/UDP), pero deja el **Encabezado IP original intacto**.    
- **Visibilidad:** Cualquier atacante puede ver quién está enviando los datos y a quién, ya que las direcciones IP no se ocultan.    
- **Ventaja:** Tiene menos "sobrecarga" (overhead) porque no añade un segundo encabezado IP, lo que lo hace ligeramente más rápido.    
- **Limitación:** No puede atravesar NAT fácilmente, lo que lo hace casi inútil para la mayoría de las conexiones domésticas o móviles hacia una oficina.

##### **3. ¿Por qué el Modo Transporte se "combina"?**

Su función principal hoy en día es servir de "capa de cifrado" para otros protocolos de tunelización que ya se encargan de ocultar la red:

1. **L2TP/IPsec:** El protocolo L2TP crea el túnel (agrupa los datos), e IPsec en **Modo Transporte** se encarga de cifrarlos.    
2. **GRE sobre IPsec:** GRE crea un túnel que puede transportar tráfico multicast (como protocolos de enrutamiento OSPF/EIGRP), e IPsec en **Modo Transporte** cifra ese túnel GRE.

### **Resumen rápido para tu examen o apunte:**

| **Característica** | **Modo Túnel**               | **Modo Transporte**                 |
| ------------------ | ---------------------------- | ----------------------------------- |
| **Encabezado IP**  | Se crea uno **Nuevo**        | Se mantiene el **Original**         |
| **Cifrado**        | Paquete completo             | Solo datos (Payload)                |
| **Uso común**      | VPN Sitio a Sitio / Internet | Host a Host / Combinado (GRE, L2TP) |
| **Oculta IPs**     | Sí (Privacidad total)        | No (Visibles en tránsito)           |
