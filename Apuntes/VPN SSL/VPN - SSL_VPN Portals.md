---
Tema: "[[VPN SSL]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

en `VPN > ssl-vpn Portals > create new`

![[Pasted image 20260212093437.png]]


Esta captura de pantalla muestra la creación de un nuevo **SSL-VPN Portal** en la interfaz de FortiOS, configurado específicamente para el **Web Mode** (Modo Web). Este modo es ideal para usuarios que necesitan acceso rápido a recursos internos sin instalar software adicional, ya que funciona enteramente a través de un navegador web.

### 1. Modos de Conexión

- **Tunnel Mode (Desactivado):** En esta configuración, no se permite el uso de FortiClient para crear un adaptador virtual en la PC del usuario.
- **Web Mode (Activado):** Es el corazón de este portal. Permite a los usuarios acceder a una página HTTPS en el FortiGate para autenticarse y usar aplicaciones publicadas.

### 2. Personalización del Portal

- **Portal Message:** Has configurado el saludo "VPN SSL WEB" que los usuarios verán al entrar.
- **Theme (Dark Matter):** Define la apariencia visual (colores y estilo) de la interfaz web para el usuario.
- **Show Session Information / Login History:** Estas opciones habilitadas permiten que el usuario vea detalles de su conexión actual y sus inicios de sesión previos por seguridad.

### 3. Funcionalidad del Usuario

- **Show Connection Launcher:** Habilita la herramienta **Quick Connection**, que permite al usuario escribir manualmente una dirección (como una IP interna o URL) para conectar vía RDP, SSH o HTTP sin que esté predefinida.
- **RDP/VNC clipboard:** Permite copiar y pegar texto entre la PC del usuario y los servidores remotos a los que acceda a través del portal.
- **Predefined Bookmarks:** Esta sección (actualmente vacía) es donde puedes crear "Favoritos". Por ejemplo, podrías añadir un botón directo que diga "Servidor de Archivos" para que el usuario no tenga que recordar la dirección IP interna.


Cuando un usuario se conecte a este portal:
1. Ingresa a la URL del FortiGate y se autentica.
2. Accede a los recursos a través del navegador.
3. **Dato Clave:** Su dirección IP de origen es reemplazada por la **IP interna del FortiGate** al interactuar con los servidores de la oficina.

Este tipo de portal es muy útil para contratistas o empleados en cibercafés donde no se pueden instalar aplicaciones, pero recuerda que solo soporta protocolos específicos como HTTP/HTTPS, RDP, SSH y SMB/CIFS.