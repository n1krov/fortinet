---
Tema: "[[IPsec]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---


a la hora de crear tuneles IPsec hay dos tipos:
- en la parte de VPN hay 
	- IPsec Tunnel
	- IPsec Aggregate

generalmente seleccionamos ipsec tunnel

![[Captura de pantalla_20260121_085932.png]]

> La idea es seleccionar `custom` ya que los demas tiene muchisimas configuraciones por detras y hechas por fortinet

#### Nomenclatura de VPN

para las dos fases es recomendable poner
- v.SITE_destino_
- v2.SITE_destino_

## Configuración Custom de VPN IPsec en FortiGate

Cuando seleccionas la opción **Custom**, el FortiGate te permite definir manualmente cada parámetro de las dos fases de IKE.

### 1. Configuración de Red y Gateway
Esta sección define **hacia dónde** va el túnel y cómo se comporta a nivel de red.

![[Captura de pantalla_20260121_090437.png]]

- **Remote Gateway:** Define la IP pública del otro extremo (el "Peer").
	- podemos seleccionar **estatica o dinamica si tiene dns**
	- hay un modo dialup user, en este caso el forti seria unservidor vpn y otros equipos se conectan a nosotros

- **Local gateway** sirve por si  tenemos mas de una ip por intefaz y queremos seleccionar cual ip utilizar

- **Modo config** solo cuando trabajamos con *dialup user*

- **NAT Traversal:** Fundamental si el FortiGate está detrás de un router haciendo NAT. Como vimos en la primera imagen, esto activará el puerto **UDP 4500**.

- **Dead Peer Detection (DPD):** Es el mecanismo para verificar si el otro extremo sigue "vivo". Si no responde a los mensajes de control, el FortiGate baja el túnel para intentar reconstruirlo o liberar recursos.

### 2. Autenticación e IKE

- **Method:** Generalmente se usa **Pre-shared Key (PSK)**, una contraseña secreta compartida entre ambos administradores.
    
- **IKE Version:** Puedes elegir entre **1 (Legacy)** o **2 (New)**.
    
- **IKE Mode (Fase 1):** * **Main Mode:** Más seguro, oculta las identidades de los dispositivos.
    
    - **Aggressive Mode:** Más rápido pero menos seguro (transmite el ID en texto plano). Se usa a veces cuando uno de los extremos tiene IP dinámica.
        

### 3. Phase 1 Proposal 

Aquí defines el "idioma" de seguridad para la **IKE SA**. Ambos extremos deben tener exactamente los mismos valores para que el túnel levante.

![[Captura de pantalla_20260121_090544.png]]

- **Encryption & Authentication:** Las parejas de algoritmos (ej. AES256 con SHA256).
    
- **Diffie-Hellman (DH) Groups:** Determinan la fuerza de la clave generada para el intercambio. A números más altos (ej. 14, 19, 21), mayor seguridad matemática.
    
- **Key Lifetime:** Tiempo que dura la Fase 1 antes de renegociar las llaves.
    

### 4. Phase 2 Selectors

Esta es la parte donde defines **qué tráfico** tiene permiso para entrar al túnel.

- **Local Address / Remote Address:** Aquí especificas las subredes internas que se van a comunicar.
    
    - _Ejemplo:_ Si tu oficina usa `192.168.1.0/24` y la sucursal `10.0.1.0/24`, esos son tus selectores.
        
- **Outcome:** Si los selectores no coinciden en ambos lados, el túnel puede subir (Fase 1 OK), pero el tráfico no pasará (Fase 2 fallida).
    

|**Parámetro**|**Fase 1 (IKE SA)**|**Fase 2 (IPsec SA)**|
|---|---|---|
|**Objetivo**|Establecer canal de control seguro|Cifrar los datos del usuario|
|**Identificación**|IP pública / ID local|Subredes internas (Selectores)|
|**Cifrado**|Propuesta de Fase 1|Propuesta de Fase 2|
|**Duración**|Larga (ej. 86400s)|Corta (ej. 43200s)|
