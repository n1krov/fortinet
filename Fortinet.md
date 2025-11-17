
# 🛡️ Fortinet y FortiGate: Definición y Rol en Ciberseguridad

## 1. ¿Qué es Fortinet?

**Fortinet** es una corporación multinacional estadounidense conocida por desarrollar y comercializar software, dispositivos y servicios de **ciberseguridad**, como _firewalls_, antivirus, prevención de intrusiones y seguridad de puntos finales (endpoints).

### Concepto Clave: Fortinet Security Fabric

Fortinet promueve un ecosistema de seguridad integrado conocido como **Fortinet Security Fabric**. Este _framework_ tiene como objetivo proporcionar una seguridad amplia, integrada y automatizada en toda la infraestructura digital (red, nube y endpoints).

> [!info] Enfoque
> 
> Fortinet se enfoca en la consolidación de la seguridad, buscando reemplazar múltiples productos de seguridad dispares con una plataforma única y unificada.

---

## 2. 🧱 FortiGate: El Pilar Fundamental

**FortiGate** es la línea insignia de _appliances_ (dispositivos) de **firewall de próxima generación (NGFW)** de Fortinet. Es el componente central del Security Fabric y la herramienta más utilizada en laboratorios y entornos reales de seguridad.

### 2.1. Características Clave del FortiGate (NGFW)

Un FortiGate va mucho más allá de un firewall tradicional (_stateful firewall_). Combina múltiples funcionalidades de seguridad en una sola plataforma:

|**Característica**|**Descripción**|**Beneficio en Seguridad**|
|---|---|---|
|**Stateful Firewall**|Inspección del tráfico basada en el estado de la conexión.|Control de tráfico fundamental (capas 3 y 4).|
|**VPN (Virtual Private Network)**|Soporte para VPNs IPsec y SSL.|Conexión segura de sitios remotos (Site-to-Site) y usuarios (Remote Access).|
|**IPS (Intrusion Prevention System)**|Detección y bloqueo de tráfico malicioso basado en firmas y heurística.|Defensa proactiva contra exploits y ataques conocidos.|
|**Control de Aplicaciones**|Identificación y gestión de tráfico basado en la aplicación, no solo en puertos.|Permite bloquear o limitar aplicaciones específicas (ej: Netflix, torrents).|
|**Web Filtering**|Bloqueo de acceso a sitios web peligrosos o inapropiados por categoría.|Previene malware y cumplimiento de políticas de uso.|
|**Antivirus/Anti-Malware**|Escaneo de archivos y contenido para detener virus y otro _malware_.|Protección de contenido en tiempo real.|

### 2.2. Rol en Hacking y Ciberseguridad

En un contexto de **hacking ético y testing**, el FortiGate sirve como el **punto de defensa** primario que se debe intentar evadir, eludir o explotar a través de configuraciones débiles (misconfigurations) o vulnerabilidades en su sistema operativo (**FortiOS**).

En laboratorios GNS3, el FortiGate se utiliza para:

1. **Simular un entorno empresarial** (proporcionando NAT, DHCP, etc.).
    
2. **Configurar políticas de acceso** y evaluar su eficacia.
    
3. **Realizar pruebas de penetración** contra los servicios protegidos por el _firewall_.
    

> [!example] Comando Típico (FortiOS CLI)
> 
> Para ver el estado de las interfaces en la CLI de un FortiGate:
> 
> Bash
> 
> ```
> get system interface physical
> ```

