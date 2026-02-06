---
Tema: "[[policys&objects]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

es una configuracion LOCAL en cada POLICY

flow based es el mas ligero y el qeu consume menos recursos, analiza el trafico en medida que atraviesa la policy sin actuar de proxy. esto puede gnerear falso positivo

el modo proxy es todo lo contrario.


#### Flow Based

![[IPS_engine-flowmode.png]]

explicacion modo flow based

Este diagrama detalla el flujo de procesamiento de un paquete dentro del **IPS Engine** cuando el FortiGate opera en **Flow-based inspection mode**. A diferencia del modo proxy, el modo flujo analiza los datos "al vuelo" sin detener el paquete completo, lo que reduce la latencia.

##### **1. Decodificadores de Capas Iniciales (L2 a L4)**

Antes de analizar el contenido, el motor debe entender la estructura del paquete:

- **L2 (Data Link) Decoder:** Elimina los protocolos de enlace (como VLANs o MPLS) para revelar el tráfico IP.
    
- **L3 (Network) Decoder:** Identifica si el paquete es IPv4 o IPv6 y procesa protocolos de red.
    
- **L4 (Transport) Decoder:** Gestiona la sesión de transporte, identificando si es TCP, UDP o SCTP.
    

##### **2. L7 (Application) Decoder y Procesamiento SSL**

Una vez identificada la aplicación, el motor decide si necesita descifrar el tráfico:

- **¿Es SSL?:** Si el tráfico está cifrado, el motor verifica si existe una **exención** (Exempt). Si está exento, el tráfico pasa directamente al escaneo sin ser descifrado.
    
- **SSL Decoder:** Si no hay exención, el decodificador SSL descifra el paquete para exponer el contenido de la aplicación. Aquí es donde intervienen los procesadores de hardware (CP8/CP9) para acelerar el proceso.
    
- **SSL Mirroring:** Si está configurado, una copia del tráfico descifrado puede enviarse a otro puerto para análisis externo antes de seguir su curso.
    

##### **3. Decodificación de Contenido y Aplicación**

Con el contenido ya visible (sea porque no era SSL o porque ya fue descifrado):

- **Transport Decoder:** Identifica protocolos específicos como HTTP, FTP, IMAP o BitTorrent para aplicar **Web Filtering** básico.
    
- **Content Decoder:** Descomprime y analiza archivos o datos (GZIP, PDF, JS, HTML). Estos datos se envían a un búfer para que el **Flow AV** (Antivirus en modo flujo) los analice en busca de malware.
    

##### **4. Single-Pass Rule Matching (El motor central)**

Esta es la fase donde se toman las decisiones de seguridad en un solo paso coordinado:

- El motor contrasta el contenido contra **firmas de IPS**, sensores de **Application Control**, listas de **Botnets** y filtros de **URL locales** simultáneamente.
    

##### **5. Acción Final y Re-encapsulado**

- **Action:** Se determina si el paquete se bloquea, se permite o solo se monitorea.
    
- **SSL Encoding:** Si el paquete fue descifrado originalmente, el motor lo vuelve a cifrar antes de que abandone el firewall para mantener la integridad de la conexión.
    


> El modo **Flow-based** es extremadamente eficiente porque utiliza un diseño de **"paso único" (Single-Pass)**. Esto significa que una vez que el paquete es descifrado y decodificado, todos los perfiles de seguridad (IPS, AV, App Control) lo analizan al mismo tiempo, evitando múltiples procesos de escaneo que ralentizarían la red.



#### **Arquitectura de Inspección: Modo Proxy (Proxy-based)**

A diferencia del modo flujo, el **Modo Proxy** actúa como un intermediario completo. El FortiGate termina la sesión del cliente, almacena el contenido en un búfer para examinarlo íntegramente y luego establece una nueva sesión con el destino.


![[proxy-based.png]]

### **Flujo del Paquete en Modo Proxy**

1. **Entrada al Motor IPS:** El paquete llega y pasa por el motor IPS inicial, donde se aplican de forma simultánea (Single-Pass) el control de aplicaciones, IPS y filtrado de botnets.
    
2. **Decisión de Descifrado SSL:**
    
    - Si el tráfico es SSL y se requiere inspección, el motor utiliza los procesadores de hardware (CP8/CP9) para **descifrar la sesión**.
        
    - Si no es SSL o no requiere inspección, pasa directamente a los servicios de seguridad del proxy.
        
3. **Búfer del Proxy y Servicios UTM:** Aquí es donde el modo proxy marca la diferencia. Al reconstruir el archivo o la página completa en memoria, puede aplicar filtros más profundos:
    
    - **VoIP Inspection / DLP / Antispam / Web Filtering / Antivirus / ICAP**.
        
    - Este análisis es más exhaustivo que el de modo flujo porque el proxy "entiende" el protocolo completo antes de reenviarlo.
        
4. **Re-encapsulado y Salida:** Una vez que el contenido es declarado limpio:
    
    - Si fue descifrado, el motor vuelve a **cifrar el paquete**.
        
    - El paquete sale del FortiGate hacia su destino final.
        

---

### **Resumen Comparativo para tu Apunte**

|**Característica**|**Modo Flujo (Flow)**|**Modo Proxy (Proxy)**|
|---|---|---|
|**Latencia**|Muy baja (análisis al vuelo)|Mayor (debe reconstruir archivos)|
|**Exhaustividad**|Alta, pero puede omitir ciertos detalles|Máxima (analiza el contenido completo)|
|**Uso de Recursos**|Menor uso de memoria RAM|Mayor uso de memoria para el buffering|
