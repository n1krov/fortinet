---
Tema: "[[apuntes]]"
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

## **IPS: Anomalías vs. Exploits**

El sistema de prevención de intrusiones no solo busca virus, sino que analiza comportamientos y patrones de tráfico para identificar ataques activos.

### **1. Anomalías (Anomaly)**

Se refieren a comportamientos del tráfico que se desvían de lo normal o esperado.

- **Naturaleza:** Pueden ser ataques de **día cero** (desconocidos) o ataques de denegación de servicio (**DoS**).
    
- **Método de Detección:** Se basan en el **análisis de comportamiento**. El IPS no busca una "firma" específica, sino que observa si el tráfico es inusual.
    
- **Herramientas de detección:**
    
    - **Firmas IPS basadas en tasa (Rate-based):** Detectan si hay demasiadas conexiones en muy poco tiempo.
        
    - **Políticas DoS:** Reglas específicas para frenar inundaciones de tráfico.
        
    - **Inspección de restricciones de protocolo:** Verifica que el tráfico cumpla estrictamente con las reglas del protocolo (ej. que un paquete HTTP no tenga campos deformados).
        
- **Ejemplo típico:** Una tasa de tráfico anormalmente alta, como un ataque de inundación (DoS/flood).
    

### **2. Exploits**

Son ataques dirigidos a aprovechar una vulnerabilidad específica y conocida en un software o sistema.

- **Naturaleza:** Es un ataque **conocido y confirmado**.
    
- **Método de Detección:** Se detectan cuando el tráfico o un archivo coincide exactamente con un **patrón de firma** predefinido.
    
- **Herramientas de detección:** Utiliza bases de datos actualizadas de:
    
    - Firmas de **IPS**.
        
    - Firmas de **WAF** (Web Application Firewall).
        
    - Firmas de **Antivirus**.
        
- **Ejemplo típico:** Un intento de aprovechar una vulnerabilidad conocida en una aplicación (ej. una inyección SQL o un desbordamiento de búfer documentado).
    

|**Característica**|**Anomalía (Behavioral)**|**Exploit (Signature)**|
|---|---|---|
|**Enfoque**|¿Cómo se comporta el tráfico?|¿A qué se parece el tráfico?|
|**Objetivo**|Ataques nuevos o masivos (DoS)|Vulnerabilidades específicas conocidas|
|**Mecanismo**|Análisis estadístico y de protocolo|Comparación con base de datos de firmas|


>[!note] Recordá que el motor IPS es el "corazón" que también hace funcionar al **Application Control**. Mientras que el Application Control usa las firmas para identificar la app, el IPS las usa para identificar si esa app está intentando explotar una vulnerabilidad.



## **Funcionamiento del IPS en Fortinet**

El IPS es un sistema de seguridad que monitorea el tráfico de red de forma constante para identificar y bloquear amenazas. Se caracteriza por trabajar principalmente en **modo de inspección basado en flujo (flow-based)** para garantizar la velocidad.

### **1. Capacidades de Detección y Bloqueo**

El motor IPS está diseñado para detectar dos tipos principales de eventos maliciosos:

- **Exploits conocidos:** Ataques específicos que coinciden con patrones guardados en una base de datos de firmas.
    
- **Anomalías de protocolo:** Errores en la estructura de los paquetes de red o comportamientos que se desvían de los estándares establecidos.
    

### **2. Componentes del IPS**

Para realizar su trabajo, el sistema utiliza tres elementos clave:

- **Bases de datos de firmas IPS:** Repositorio actualizado con los patrones de ataques conocidos.
    
- **Decodificadores de protocolo:** Analizan el tráfico para entender qué protocolo se está usando y asegurar que cumpla con sus reglas.
    
- **Motor IPS (El núcleo):** Es el procesador central que coordina otros servicios de seguridad cuando trabajan en modo "flow-based".
    

### **3. Servicios que dependen del motor IPS**

Cuando el FortiGate está configurado en modo flujo, el motor IPS se encarga de ejecutar los siguientes perfiles de seguridad:

- **Application Control:** Identificación de aplicaciones.
    
- **Antivirus (flow based):** Escaneo de malware al vuelo.
    
- **Web filter (flow based):** Filtrado de sitios web.
    
- **Email filter (flow based):** Filtrado de correos electrónicos.
    
- **DLP (Data Leak Prevention):** Prevención de fuga de datos (específicamente cuando se usa en modo sniffer de un solo brazo).

---


en security profiles > create new

Para crear y configurar un **IPS Sensor** (Sistema de Prevención de Intrusiones) en FortiGate, debés definir reglas específicas que identifiquen y bloqueen tanto exploits conocidos como comportamientos anómalos.

### **1. Configuración General del Sensor**

Al iniciar un **New IPS Sensor**, definís el comportamiento base:

- **Block malicious URLs:** Al activar este switch, el motor IPS utiliza su base de datos para bloquear automáticamente el acceso a dominios conocidos por alojar malware o ser parte de redes de comando y control (C2).
    

### **2. IPS Signatures and Filters (El núcleo del sensor)**

Esta sección es donde seleccionás qué ataques específicos querés frenar.

- **Exploits:** Mediante el botón `Create New`, podés agregar firmas individuales que detectan ataques confirmados (como inyecciones SQL o desbordamientos de búfer) comparando el tráfico contra patrones conocidos.
    
- **Filtros:** En lugar de elegir una por una, podés crear filtros basados en:
    
    - **Severidad:** Crítica, alta, media, etc.
        
    - **Target:** Servidor o cliente.
        
    - **OS:** Windows, Linux, etc.
        
- **Action:** Podés definir si el IPS debe simplemente monitorear el evento o bloquearlo activamente.
    

### **3. Botnet C&C (Comando y Control)**

Esta función es vital para detectar si algún equipo de tu red interna ya ha sido comprometido.

- **Scan Outgoing Connections to Botnet Sites:** Analiza las conexiones que salen de tu red hacia internet.
    
- **Disable / Block / Monitor:** * **Block:** Corta la comunicación si un dispositivo interno intenta hablar con un servidor de una botnet.
    
    - **Monitor:** Permite la conexión pero genera un log de alerta para que sepas qué equipo está infectado.
        
