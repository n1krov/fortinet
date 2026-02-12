---
Tema: "[[VPN SSL]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

en VPN > ssl-vpn setings

![[Captura de pantalla_20260212_094125.png]]

Esta pantalla es el centro de control global para las conexiones VPN en tu FortiGate. Mientras que los **Portales** (que vimos antes) definen _qué_ ve el usuario, los **SSL-VPN Settings** definen _cómo_ se conecta el equipo a la red.

### 1. Connection Settings (Configuración de Conexión)

- **Enable SSL-VPN:** Actualmente está desactivado. Es el interruptor principal para prender el servicio.
    
- **Listen on Interface(s):** Aquí debes elegir por qué "puerta" entrarán los usuarios (usualmente tu interfaz WAN o Internet).
    
- **Listen on Port (443):** Es el puerto estándar HTTPS. Si tu administración web del FortiGate también usa el 443, tendrás un conflicto; en ese caso, se suele cambiar la VPN al puerto **10443**.
    
- **Server Certificate:** El certificado que asegura que la conexión es legítima. Por defecto viene uno de Fortinet, pero lo ideal es usar uno firmado para evitar el aviso de "sitio no seguro".
    
- **Idle Logout:** Si el usuario no tiene actividad por 300 segundos (5 minutos), el FortiGate cerrará la sesión automáticamente para liberar recursos y por seguridad.
    

### 2. Tunnel Mode Client Settings

Estas opciones se aplican solo si usas el **Tunnel Mode** (con FortiClient):

- **Address Range:** Define qué IPs internas recibirán los usuarios remotos. En tu captura, el FortiGate asignará automáticamente IPs en el rango `10.212.134.200 - 10.212.134.210`.
    
- **DNS Server:** Los usuarios usarán el mismo DNS que tenga configurado su propia computadora ("Same as client system DNS").
    

### 3. Authentication / Portal Mapping

Esta es la parte más importante para la organización:

- Aquí es donde vinculas **Quién** con **Qué**. Por ejemplo: puedes crear una regla para que el grupo de Active Directory "Contadores" sea dirigido al portal "Web Portal" que configuramos antes, mientras que el grupo "IT_Admins" sea dirigido a un portal con "Full Access".
    
