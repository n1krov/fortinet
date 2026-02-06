---
Tema: "[[policys&objects]]"
---

Quiero que actúes como un asistente especializado en mejorar y embellecer mis apuntes de **hacking y ciberseguridad - FORTINET** en Obsidian.

### Reglas de formato:
- Usa **Markdown** y todas las herramientas nativas de Obsidian:  
  - Encabezados jerárquicos (#, ##, ###…)  
  - Negritas, cursivas, tachado  
  - Listas ordenadas y no ordenadas  
  - Tablas para comparaciones  
  - Callouts (`> [!info]`, `> [!tip]`, `> [!warning]`, `> [!example]`, etc.)  
  - Diagramas con **Mermaid** (especialmente diagramas de redes, flujos y ataques)  
  - Bloques de código y comandos de terminal (bash, python, etc.)  
  - Separadores `---` para estructurar  

### Reglas de estilo:
- Embellecé y organizá mis notas para que sean **claras, fáciles de leer y visualmente atractivas**.  
- Si algo está enredado o difícil de entender, simplificalo y hacelo **más didáctico**.  
- Agregá **ejemplos prácticos** (comandos reales, simulaciones, casos de uso).  
- Respetá los **enlaces e imágenes** que yo incluya. No borres ni inventes enlaces/imágenes nuevas.  
- Podés usar **diagramas de red (Mermaid), tablas comparativas y listas de pasos** para explicar ataques, defensas y herramientas.  
- El resultado final debe ser un apunte **técnico, claro y útil para estudiar hacking**.  

Cuando te pase un texto, transformalo siguiendo estas reglas.

Aqui te va el texto:

---


## **¿Qué es Application Control y cómo funciona?**

El control de aplicaciones permite al FortiGate identificar y tomar acciones (permitir, bloquear o monitorear) sobre el tráfico generado por aplicaciones específicas, incluso si estas intentan ocultarse o usar puertos no estándar.

### **1. Capacidades Principales**
- **Detección de Tráfico:** Identifica aplicaciones populares como Facebook, Skype, Gmail o LogMeIn.
- **Soporte Extenso:** Clasifica miles de aplicaciones en categorías, incluyendo aplicaciones P2P (como BitTorrent) y herramientas de proxy o evasión.
- **Escaneo de Protocolos Seguros:** Para analizar aplicaciones que viajan cifradas (como la mayoría hoy en día), es **obligatorio** tener habilitado un perfil de inspección SSL/SSH en la política de firewall.

### **2. Mecanismo de Funcionamiento**
- **Motor IPS:** El control de aplicaciones no trabaja solo; utiliza el **motor IPS** para realizar el análisis de paquetes.
- **Escaneo en Modo Flujo (Flow-based):** A diferencia de otros servicios, el control de aplicaciones siempre utiliza el escaneo basado en flujo para mantener el rendimiento y la baja latencia (no utiliza modo proxy).
- **Patrones de Aplicación:** Compara el tráfico en tiempo real contra una base de datos de "firmas" o patrones conocidos:
    - Solo genera reportes de los paquetes que coinciden con un patrón que el administrador ha habilitado.        
    - Es capaz de detectar aplicaciones incluso si los usuarios intentan saltarse las restricciones usando un **proxy externo**.

> El **Application Control** es una pieza clave del motor de "paso único" (Single-Pass) del FortiGate. Mientras el tráfico fluye, el motor IPS lo compara simultáneamente con firmas de aplicaciones para decidir su destino, garantizando visibilidad total sobre lo que realmente hacen los usuarios en la red.
