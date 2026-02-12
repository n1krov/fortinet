---
Tema: "[[apuntes]]"
---
# Fortinet: SSL VPN Deep Dive

Las **VPN (Virtual Private Networks)** extienden una red privada a través de una red pública, permitiendo conectar de forma segura dispositivos y oficinas remotas.

---

## 1. Conceptos Fundamentales de VPN

Para que una VPN sea considerada segura, debe cumplir con tres pilares:

- **Tamper proof (Integridad):** Los atacantes no pueden modificar los mensajes o archivos.
    
- **Encrypted (Confidencialidad):** Usuarios no autorizados no pueden "escuchar" o interceptar el tráfico.
    
- **Authenticated (Autenticación):** Solo usuarios conocidos y autorizados pueden acceder a la red.
    

---

## 2. SSL VPN vs. IPsec VPN

A diferencia de IPsec (un estándar de la industria), la **SSL VPN** es específica del fabricante (Vendor Specific).

|**Característica**|**SSL VPN**|**IPsec VPN**|
|---|---|---|
|**Capa OSI**|Capa de aplicación (SSL/TLS)|Capa de red (IP/ESP)|
|**Instalación**|No requiere (en Web Mode)|Requiere instalación (FortiClient)|
|**Configuración**|Simple (Client-to-FortiGate)|Flexible (Mesh/Star topologies)|
|**Rendimiento**|Depende del navegador/cliente|**Más rápido** (Cifrado por hardware en FortiOS)|
|**Uso Ideal**|Usuarios móviles, cafés, librerías|Tráfico Office-to-Office, Data Centers|

---

## 3. Modos de Despliegue SSL VPN 🚀

El FortiGate permite dos modos de operación que pueden habilitarse por separado o simultáneamente en los **SSL VPN Portals**.

### A. Web Mode (Clientless)

Ideal para accesos rápidos sin software adicional.


![[Captura de pantalla_20260212_092038.png]]

- **Requisito:** Solo un navegador web.
- **Funcionamiento:** El usuario se conecta a un portal HTTPS, se autentica y accede vía **Quick Connection** o **Bookmarks**.
- **Protocolos Soportados:** Limitado a FTP, HTTP/HTTPS, RDP, SMB/CIFS, SSH, Telnet, VNC y Ping.
- **IP Masking:** La IP de origen del usuario es reemplazada por la IP interna del FortiGate.


### B. Tunnel Mode

Ofrece una experiencia de red completa.

- **Requisito:** Requiere **FortiClient** instalado en el host.
    
- **Funcionamiento:** Se crea un adaptador virtual en la PC del cliente que recibe una IP de la red interna.
    
- **Ventaja:** Soporta cualquier protocolo de red, no está limitado al navegador.
    

---

## 4. Configuración vía CLI 💻

Para habilitar o deshabilitar estos modos rápidamente en un portal específico:

```
config vpn ssl web portal
    edit <portal-name>
        set tunnel-mode [enable|disable] #
        set web-mode [enable|disable]    #
    next
end
```

---

## 5. ⚠️ Consideraciones de Salud del Sistema

Al implementar servicios de alta demanda como VPN o filtrado de contenido, es vital monitorear la memoria para evitar el **Conserve Mode**.

> [!CAUTION] **Umbrales de Memoria (Defaults)**
> 
> - **82% (Green):** El FortiGate sale del modo de conservación.
>     
> - **88% (Red):** El FortiGate entra en modo de conservación y deja de aceptar cambios de config.
>     
> - **95% (Extreme):** Se descartan todas las sesiones nuevas.
>     

---

## 6. 🔗 Requisitos de FSSO (Integración AD)

Si vas a autenticar usuarios mediante su sesión de Windows (Active Directory), recuerda los requisitos técnicos para que el **Collector Agent** funcione:

1. **Resolución DNS:** El servidor DNS local debe poder resolver los nombres de las estaciones de trabajo (los eventos de login de Microsoft no traen la IP).
    
2. **Polling de Estaciones:** Para saber si el usuario sigue conectado, el agente debe poder comunicarse con los hosts.
    
3. **Puertos Abiertos:** **TCP 139 y 445** deben estar abiertos entre el agente/FortiGate y los hosts.
    


adicionalmente puedes habilitar mas funciones en  System > feature visibility > ssl-vpn personal bookmarks | ssl-vpn realms