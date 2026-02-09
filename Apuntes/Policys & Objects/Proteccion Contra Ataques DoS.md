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

ir a `policy&objects > IPv4DoS policy > create new`


## **Configuración de IPv4 DoS Policy**

A diferencia de un sensor IPS estándar, la política DoS está diseñada para frenar ataques masivos de inundación basándose en **umbrales (thresholds)** de tráfico.

### **1. Criterios de Aplicación (Matching)**

En la parte superior de la política (segunda imagen), defines dónde se aplicará la protección:

- **Incoming Interface:** Es vital seleccionar la interfaz por donde ingresa el tráfico sospechoso (ej. `ISP 1 (port2)`).
    
- **Source / Destination Address:** Generalmente se configura como `all` para proteger todo el tráfico entrante, o se especifica si se quiere proteger un servidor VIP concreto.
    
- **Service:** El protocolo a monitorear (usualmente `ALL`).

### **2. Clasificación de Anomalías (L3 y L4)**

El FortiGate divide la inspección en dos capas del modelo OSI:

#### **Anomalías de Capa 3 (L3 Anomalies)**

Se centran en el protocolo IP y el volumen de sesiones:

- **ip_src_session / ip_dst_session:** Limita cuántas sesiones concurrentes puede tener una sola IP de origen o dirigirse a una sola IP de destino. El umbral mostrado es de **5000** sesiones.
    

#### **Anomalías de Capa 4 (L4 Anomalies)**

Detectan ataques de inundación (flooding) y escaneos de puertos:

- **tcp_syn_flood:** Protege contra el agotamiento de recursos por intentos de conexión TCP incompletos. Umbral configurado: **200**.
    
- **udp_flood / icmp_flood:** Detecta ráfagas inusuales de tráfico UDP o ICMP que intentan saturar el ancho de banda. Umbrales: **100** y **50** respectivamente.
    
- **tcp_port_scan / udp_scan:** Identifica si alguien está barriendo puertos para encontrar vulnerabilidades. Umbral bajo (**10**) para una detección rápida.
    

---

### **3. Acciones y Umbrales (Thresholds)**

Para cada anomalía, debes configurar tres parámetros clave:

- **Logging:** Si está en `On`, generará un registro en **Log & Report > Anomaly** cuando se supere el umbral.
    
- **Action:**
    
    - **Block:** Corta el tráfico que exceda el límite definido.
        
    - **Monitor:** Solo registra el evento sin bloquear, ideal para "tunear" los umbrales antes de entrar en producción.
        
- **Threshold:** Es el valor numérico de ráfaga (paquetes por segundo o sesiones) que dispara la acción.
    

|**Anomalía**|**Descripción**|**Umbral Inicial (Monitor)**|**Umbral Producción (Block)**|**Acción Sugerida**|
|---|---|---|---|---|
|**tcp_syn_flood**|Inundación de intentos de conexión TCP.|2000|**400 - 800**|Block|
|**udp_flood**|Ráfagas masivas de paquetes UDP.|1000|**500 - 1000**|Block|
|**icmp_flood**|Ataque de "Ping" masivo.|250|**100 - 250**|Block|
|**tcp_port_scan**|Escaneo de puertos TCP abiertos.|100|**30 - 50**|Block|
|**udp_scan**|Intento de descubrir servicios UDP.|100|**30 - 50**|Block|
|**ip_src_session**|Máximo de sesiones desde una sola IP.|5000|**1000 - 2000**|Block|
|**ip_dst_session**|Máximo de sesiones hacia un solo destino.|5000|**2000 - 5000**|Block|


en Log & `report > anomalies` pueden ver las anomalias