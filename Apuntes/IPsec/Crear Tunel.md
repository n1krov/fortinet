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

> tener encuenta muy importante que esto debe ahcerse en ambos lados si tenemos un fortigate en cada Site... como en nuestro caso


![[topologia.png]]

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

![[Captura de pantalla_20260121_093700.png]]


> Importante cambiarle el nombre por la nomenclatura a "**v2.**"
> en lo posible tambien trabajar con objetos

La Fase 2 es donde se define **qué tráfico específico** proteger y cómo se cifrarán esos datos finales.

### 1. Selectores de Tráfico (Imagen 5 y 6)

- **Local/Remote Address:** Define las subredes que tienen permitido cruzar el túnel. En tu ejemplo (Imagen 6), el tráfico fluye de la red `10.0.1.0/24` a la `10.0.2.0/24`.    
- **Protocol & Ports:** Al estar marcados como **All**, el túnel permite cualquier tipo de tráfico (TCP, UDP, ICMP) entre esas redes.

### 2. Propuesta de Fase 2 (Phase 2 Proposal - Imagen 6)

Es el conjunto de algoritmos para los datos del usuario.

- **Encryption & Authentication:** En tu captura se usa **DES** y **MD5**. _Nota: Estos son algoritmos antiguos; en producción se suele preferir AES y SHA._
    
- **Perfect Forward Secrecy (PFS):** Si se activa, obliga a generar claves nuevas e independientes de la Fase 1 para cada renegociación, aumentando la seguridad.
    
- **Key Lifetime:** Determina cada cuánto tiempo expira la **IPsec SA**. En tu captura está configurado en **43,200 segundos** (12 horas).
    

### **3. Funciones de Control (Imagen 6)**

- **Enable Replay Detection:** Protege contra ataques de "replay", donde un atacante captura paquetes válidos y los reenvía para intentar engañar al receptor.
    
- **Auto-negotiate:** Si se activa, el FortiGate intentará levantar el túnel automáticamente en cuanto haya tráfico interesado, sin esperar a que el otro lado inicie.
    


### Diferencia de Tiempos (Lifetime)

> La **Fase 1 (IKE SA)** suele durar mucho más (ej. 86,400s / 24h) que la **Fase 2 (IPsec SA)** (ej. 43,200s / 12h). Esto asegura que las llaves que protegen tus datos cambien con mayor frecuencia que el canal de control.


| **Parámetro**      | **Fase 1 (IKE SA)**                | **Fase 2 (IPsec SA)**          |
| ------------------ | ---------------------------------- | ------------------------------ |
| **Objetivo**       | Establecer canal de control seguro | Cifrar los datos del usuario   |
| **Identificación** | IP pública / ID local              | Subredes internas (Selectores) |
| **Cifrado**        | Propuesta de Fase 1                | Propuesta de Fase 2            |
| **Duración**       | Larga (ej. 86400s)                 | Corta (ej. 43200s)             |



luego de crear el tunel no alcaza, eso es porque las vpn son de tipo **enrutadas** lo que quiere decir es que loq ue hace es crear la interfaz
> necesitamos utilizar [[Rutas Estáticas]] hacia esa interfaz

### Creación de Ruta Estatica para conectar el Tunel

![[Captura de pantalla_20260122_092339.png]]


tip muy importante de buena practica: a la hora de crear una static route para el tunel es recomendable crear una ruta estatica similar nada mas que se sube el Administrative Distance a un numero alto por si tenemos otra vpn y queremos que el blackhole no se meta en el medio y la interfaz debe ser **Blackhole**
esto es para que si se cae el tunel le pegue directo al blackhole y no consuma recursos y genere trafico inncecesario

![[Captura de pantalla_20260122_092931.png]]

en la imagen se puede ver al configuracion de las opciones

POR ULTIMO FALTA UNA [[Policy Routes]] que permita el trafico


en resumen para crear un tunel, debes 
- crear tunel
- ruta estatica para el tunel
- firewall policy ida y vuelta o lo que necesites

si tenes mas de una vpn o vpn de backup. lo ideal es trabajar con zonas y ahi agregas las 2 ips. 
siempre es recomendable trabajar con objetos

