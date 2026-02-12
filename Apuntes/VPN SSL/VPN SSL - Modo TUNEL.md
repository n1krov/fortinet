---
Tema: "[[VPN SSL]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---


# 🌐 Fortinet SSL VPN: Tunnel Mode & FortiClient

A diferencia del Web Mode, el **Tunnel Mode** ofrece una experiencia de red completa y transparente para el usuario, actuando de forma muy similar a una conexión física a la oficina.

## 1. ¿Cómo funciona el Tunnel Mode? 🛠️

El proceso de conexión sigue una lógica de encapsulamiento y asignación de identidad de red:

1. **Conexión y Autenticación:** El usuario inicia el **FortiClient** y se conecta al gateway de VPN del FortiGate. Tras ingresar credenciales válidas, el túnel se establece.
    
2. **Creación del Adaptador Virtual:** El FortiClient instala y activa un adaptador de red virtual en el host llamado `fortissl`.
    
3. **Asignación de IP:** El FortiGate asigna una dirección IP virtual al cliente desde un pool de direcciones reservadas (ej. `10.212.134.200 - 10.212.134.210`).
    
4. **Encapsulamiento:** Todo el tráfico que sale desde el host hacia la red privada es encapsulado en **SSL/TLS**.
    
5. **Transparencia:** La IP de origen del tráfico del usuario es la IP virtual asignada por el FortiGate, permitiendo que el firewall aplique políticas como si el usuario estuviera físicamente en la LAN.
    

## 2. El Rol de FortiClient 🛡️

El FortiClient no es solo un lanzador, es el motor que permite la existencia del túnel:

- **Software Necesario:** A diferencia del modo web, el modo túnel **requiere obligatoriamente** la instalación de este cliente de VPN.
    
- **Adaptador `fortissl`:** Es el componente que engaña al sistema operativo para que crea que está conectado a una red local.
    
- **Soporte Multi-Aplicación:** Al trabajar a nivel de red, permite que **cualquier aplicación** de IP (no solo navegadores) envíe tráfico a través del túnel.
    

## 3. Ventajas y Desventajas ⚖️

|**Ventajas**|**Desventajas**|
|---|---|
|**Soporte total:** Funciona con cualquier software (SAP, ERP, bases de datos, etc.).|**Instalación:** Requiere instalar y mantener software en cada host.|
|**Seguridad:** Tráfico totalmente cifrado y autenticado.|**Permisos:** El usuario suele necesitar privilegios de administrador para instalar el adaptador virtual.|
|**Experiencia de usuario:** Una vez conectado, se siente como estar en la oficina.||

---

## 4. Configuración Rápida (CLI) 💻

Si necesitas habilitar el modo túnel y definir el rango de IPs desde la consola:

```
# Habilitar modo túnel en un portal
config vpn ssl web portal
    edit "full-access"
        set tunnel-mode enable
        set ip-pools "SSLVPN_TUNNEL_ADDR1"
    next
end

# Configuración global de IPs
config vpn ssl settings
    set tunnel-ip-pools "SSLVPN_TUNNEL_ADDR1"
    set dns-server1 192.168.1.10
end
```


---

configuracion modo tunel

![[Captura de pantalla_20260212_112838.png]]

Esta parte de la configuración es el "cerebro" del **Modo Túnel**. Aquí decides cómo se comportará la computadora del usuario una vez que se conecte mediante el **FortiClient**.

A continuación, te explico las opciones más importantes de la imagen para que las entiendas a la perfección:

### 1. Split Tunneling (El tráfico dividido)

Esta es la decisión más importante. Define si **todo** lo que haga el usuario en internet pasa por tu oficina, o solo lo necesario:

- **Disabled (Desactivado):** Todo el tráfico del usuario (Facebook, YouTube, correos internos) entra al túnel. Es más seguro porque tú controlas todo, pero consume mucho ancho de banda de tu internet.
    
- **Enabled Based on Policy Destination:** Solo el tráfico que va hacia los servidores que tú definiste en las políticas del firewall viajará por la VPN. El resto (como ver Netflix) sale por el internet de la casa del usuario.
    
- **Enabled for Trusted Destinations:** Es similar al anterior, pero tú marcas específicamente qué destinos son seguros para que no pasen por el túnel.
    

### 2. Source IP Pools

Aquí seleccionas el "estanque" de direcciones IP que el FortiGate le prestará a los usuarios remotos. Sin una IP de este rango, el usuario no puede navegar dentro de tu red.

### 3. Tunnel Mode Client Options

Son funciones de conveniencia para el usuario final en su FortiClient:

- **Allow client to save password:** Permite que el usuario guarde su contraseña para no escribirla cada vez. (Menos seguro, pero más cómodo).
    
- **Allow client to connect automatically:** En cuanto el FortiClient detecte internet, intentará levantar la VPN solo.
    
- **Allow client to keep connections alive:** Evita que la VPN se corte si el usuario deja de usar la PC por unos minutos.
    
- **DNS Split Tunneling:** Permite que las consultas de nombres internos (como `servidor.local`) vayan a tu oficina, pero las consultas públicas (como `google.com`) las resuelva el internet del usuario.
    


> [!TIP] **Recuerda:**
> 
> El modo túnel requiere **obligatoriamente** instalar el FortiClient. Si no puedes instalar software en la PC, tendrías que usar el **Web Mode** (navegador únicamente).