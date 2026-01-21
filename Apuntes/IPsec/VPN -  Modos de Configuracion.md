---
Tema: "[[IPsec]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---


### Modos de configuracion de VPN IPsec

![[Pasted image 20260120112240.png]]

**ESP** -> encapsulation security payload
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

