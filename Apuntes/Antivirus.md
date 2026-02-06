---
Tema: "[[apuntes]]"
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

Aquí tienes el desglose técnico de las últimas capturas para completar tu sección de **Antivirus en Fortinet**. Estas imágenes cubren desde las técnicas de escaneo hasta la integración con Sandbox y las bases de datos de firmas.

---

## **1. Técnicas de Escaneo de Antivirus**

El FortiGate utiliza un enfoque de tres capas para detectar amenazas, procesándolas en un orden específico para optimizar recursos.

- **Antivirus Scan (Firma):** Es la primera línea de defensa. Detecta y elimina malware conocido en tiempo real basándose en una base de datos de firmas. Ayuda a preservar la reputación de tu IP pública al evitar que el tráfico malicioso salga de tu red.
    
- **Grayware Scan:** Utiliza firmas específicas para detectar "programas no deseados" (como adware o spyware) que, aunque no siempre son virus, afectan el rendimiento y la privacidad. Se aplican las mismas acciones que el antivirus general.
    
- **Machine Learning (AI) Scan:** Es una capa **opcional** que debe activarse por CLI. Utiliza modelos entrenados por FortiGuard Labs para detectar ejecutables de Windows (PEs) maliciosos basándose en su estructura, lo que permite mitigar ataques de "día cero" (zero-day) antes de que exista una firma oficial.
    
    - **Comando CLI:** `config antivirus settings` -> `set machine-learning-detection {enable | monitor | disable}`.
        

---

## **2. FortiSandbox: Detección de Día Cero**

Cuando un archivo no coincide con ninguna firma conocida, el FortiGate puede enviarlo a un entorno aislado para analizar su comportamiento.

- **Funcionamiento:** Los archivos sospechosos se ejecutan en máquinas virtuales (VMs) aisladas. El Sandbox examina qué efectos tiene el software en el sistema para confirmar con alta certeza si es malware nuevo.
    
- **Tipos de Integración:**
    
    - **FortiSandbox Cloud:** Requiere una cuenta de FortiCloud y una licencia específica integrada en el FortiGate.
        
    - **FortiSandbox Appliance:** Una unidad física en tu red local.
        
- **Sincronización:** El FortiGate puede recibir una base de datos de firmas personalizada desde el Sandbox para complementar la base de datos global de FortiGuard.
    

---

## **3. Bases de Datos de Firmas (Antivirus Database)**

No todos los modelos de FortiGate tienen la misma capacidad de almacenamiento, por lo que existen diferentes niveles de bases de datos.

- **Extended (Por defecto):** Incluye los virus comunes y amenazas recientes más activas. Está disponible en todos los modelos de FortiGate.
    
- **Extreme:** Incluye la base de datos _Extended_ más virus "dormidos" o menos frecuentes. Solo está disponible en modelos seleccionados con mayor capacidad de hardware.
    
    - **Comando CLI para cambiar:** `config antivirus settings` -> `set use-extreme-db {enable | disable}`.
        

---

## **4. Flujo de Paquetes según Modo de Inspección**

El momento en que el motor de AV actúa depende de si la política está en modo Proxy o Flow.

|**Característica**|**Proxy Inspection Mode**|**Flow-Based Inspection Mode**|
|---|---|---|
|**Mecánica**|El FortiGate retiene todos los paquetes del archivo hasta que el archivo está completo en el búfer.|El FortiGate transmite los paquetes al cliente simultáneamente mientras los copia a un búfer interno.|
|**Escaneo**|El motor de AV empieza a escanear solo después de que el archivo completo ha sido almacenado.|El motor de AV empieza a escanear cuando el archivo completo está en el búfer, pero el cliente ya recibió gran parte de los datos.|
|**Seguridad**|Máxima. Si hay virus, el cliente nunca recibe ni un solo byte del archivo.|Alta. Si hay virus al final del archivo, el FortiGate "resetea" la conexión, pero el cliente ya recibió parte del contenido.|

### **1. Modo de Inspección Proxy (Proxy-based)**

En este modo, el FortiGate actúa como un "intermediario real". No permite que el cliente y el servidor hablen directamente.

- **Buffering Completo:** El FortiGate retiene todos los paquetes del archivo en su memoria (búfer) hasta que el archivo se ha descargado por completo desde el servidor.
    
- **Escaneo Post-Almacenamiento:** El motor de Antivirus comienza el escaneo **solo después** de que el archivo entero está en el búfer.
    
- **Seguridad Máxima:** Si se detecta un virus, el FortiGate bloquea la entrega y envía un mensaje de reemplazo. El usuario **nunca recibe ni un solo byte** del archivo infectado.
    
- **Impacto:** Es más lento y genera mayor latencia, ya que el usuario debe esperar a que el firewall termine de procesar todo el archivo.

![[Captura de pantalla_20260206_104622.png]]


### **2. Modo de Inspección de Flujo (Flow-based)**

Este modo está diseñado para la velocidad. El FortiGate analiza el tráfico "al vuelo" mientras los datos pasan a través de él.

- **Transmisión Simultánea:** A diferencia del proxy, el FortiGate **transmite los paquetes al cliente al mismo tiempo** que los va copiando en su búfer interno para analizarlos.
    
- **Escaneo en Paralelo:** El motor de AV comienza a escanear una vez que tiene el archivo completo en el búfer, pero para ese momento, el cliente ya ha recibido la mayor parte de los datos.
    
- **Acción de Bloqueo:** Si el archivo es malicioso, el FortiGate no puede enviar un mensaje de reemplazo (porque la conexión ya está casi terminada). Lo que hace es **resetear o "matar" la conexión TCP** (TCP Reset) para que el último paquete no llegue y el archivo quede corrupto e inutilizable en la PC del usuario.
    
- **Impacto:** Es mucho más rápido y eficiente, ideal para entornos donde el rendimiento de la red es crítico.

![[Captura de pantalla_20260206_104705.png]]



---

## Crear antivirus

ir a  `security profiles > antivirus > create`

### **1. Configuración del Escaneo (Scan Settings)**

En la parte superior de la ventana **"New AntiVirus Profile"**, definís el comportamiento básico:

- **AntiVirus scan:** Asegurate de que el switch esté en **On** (púrpura).
    
- **Action:** Tenés dos opciones:
    
    - **Block:** Es la recomendada. El FortiGate corta la conexión si detecta un virus.
        
    - **Monitor:** Deja pasar el archivo pero genera un log. Solo usalo si estás probando si algo da falso positivo.
        
- **Feature set:** Aquí elegís el modo de inspección que venimos hablando: **Flow-based** (más rápido) o **Proxy-based** (más seguro).
    

### **2. Protocolos Inspeccionados (Inspected Protocols)**

Esta lista determina en qué túneles va a "meter la nariz" el FortiGate. Debés activar los que uses en tu red:

- **HTTP:** Fundamental para navegación web.
    
- **SMTP / POP3 / IMAP:** Para escanear correos electrónicos y sus adjuntos.
     
- **FTP / CIFS:** Para transferencias de archivos y carpetas compartidas.

### **3. Opciones de Protección APT (Advanced Persistent Threats)**

Esto sirve para archivos que intentan engañar al antivirus común:

- **Treat Windows executables in email attachments as viruses:** Si lo activás, cualquier `.exe` que llegue por mail se bloquea automáticamente, sea virus o no (política de seguridad estricta).
    
- **Include mobile malware protection:** Activa firmas específicas para virus de Android e iOS.
    
- **Quarantine:** Si lo habilitás, el FortiGate guarda una copia del archivo infectado en su memoria/disco para que después lo puedas analizar.
    

### **4. Virus Outbreak Prevention**

Esta sección usa la base de datos de **FortiGuard** para amenazas que están circulando "ahora mismo" en el mundo (en las últimas horas):

- **Use FortiGuard outbreak prevention database:** Es un chequeo extra en la nube para malware muy reciente que quizás todavía no bajó a tu base de datos local.
    

### **Paso Final: Aplicar a la Política**

Recordá que crear este perfil no sirve de nada si no lo "pegás" en la política de firewall.

1. Vas a **Policy & Objects > Firewall Policy**.
    
2. Editás la política que permite la salida a internet.
    
3. En la sección de **Security Profiles**, activás el switch de **AntiVirus** y seleccionás este perfil que creamos.