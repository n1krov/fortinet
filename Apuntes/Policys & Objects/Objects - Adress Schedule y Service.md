---
Tema: "[[policys&objects]]"
---
# 📘 FortiGate - Objetos de Políticas (Policy Objects)

---

## 🎯 Introducción

Los **objetos de políticas** (Policy Objects) en FortiGate son componentes reutilizables que se utilizan para **simplificar y estandarizar** la configuración de políticas de firewall. En lugar de escribir direcciones IP, puertos o rangos de tiempo directamente en cada política, creás objetos que podés referenciar en múltiples políticas. 

Estos objetos te permiten:
- **Centralizar la gestión** de direcciones, servicios y horarios
- **Facilitar cambios** (modificás el objeto una vez, afecta todas las políticas que lo usan)
- **Mejorar la legibilidad** de las políticas con nombres descriptivos
- **Reducir errores** al reutilizar configuraciones probadas
- **Agrupar elementos** relacionados para simplificar políticas complejas

Los principales tipos de objetos son: 
- **Address**: Direcciones IP, redes, rangos, FQDN
- **Schedules**: Ventanas de tiempo para habilitar/deshabilitar políticas
- **Services**: Protocolos y puertos personalizados
- **Internet Service Database**: Servicios de Internet predefinidos por Fortinet

---

## ✅ Requisitos Previos

Antes de trabajar con objetos de políticas, asegurate de tener:

- [ ] Acceso administrativo al FortiGate (GUI o CLI)
- [ ] Identificación clara de qué elementos necesitás configurar
- [ ] Conocimiento de las direcciones IP, redes y servicios de tu entorno
- [ ] Comprensión de los horarios/ventanas de tiempo necesarios
- [ ] Plan de nomenclatura consistente para tus objetos

> [!info] Mejores Prácticas de Nomenclatura
> Usá nombres descriptivos y consistentes:
> - Incluí el tipo:  `SRV-`, `NET-`, `HOST-`, `GRP-`
> - Sé específico:  `SRV-AD-Domain-Controller` mejor que `server1`
> - Usá convenciones: minúsculas/mayúsculas consistentes

---

## 🌐 Address Objects (Objetos de Dirección)

Los **Address Objects** definen direcciones IP, redes, rangos o nombres de dominio que podés usar en tus políticas de firewall.

### ¿Dónde Configurarlos?

**Ruta en GUI:**
```
Policy & Objects > Addresses
```

![[Pasted image 20251230085150.png]]

---

### Tipos de Address Objects

FortiGate soporta varios tipos de objetos de dirección:

| Tipo | Descripción | Ejemplo de Uso |
|------|-------------|----------------|
| **Subnet** | Red o host individual en notación CIDR | `192.168.1.0/24` o `10.0.0.50/32` |
| **IP Range** | Rango de direcciones IP consecutivas | `192.168.1.100` a `192.168.1.200` |
| **FQDN** | Nombre de dominio totalmente calificado | `www.ejemplo.com` |
| **Geography** | País o región geográfica | `Argentina`, `United States` |
| **Address Group** | Grupo de múltiples addresses | Combina varios objetos en uno |

---

### Creación de Address Object

#### Por GUI

**Paso 1:** Navegar a objetos de dirección
```
Policy & Objects > Addresses > + Create New
```

**Paso 2:** Completar los campos

| Campo | Descripción | Ejemplo |
|-------|-------------|---------|
| **Name** | Nombre descriptivo del objeto | `Active Directory` |
| **Type** | Tipo de dirección | `Subnet` |
| **Subnet/IP Range** | Dirección o red | `192.168.1.10/32` |
| **Interface** | Interfaz asociada (opcional) | `any` o `port4` |
| **Comments** | Descripción adicional | `Servidor AD principal` |

---

#### Ejemplo Práctico: Servidor Active Directory

Supongamos que querés crear un objeto para el servidor Windows que aloja Active Directory:

**Configuración:**
- **Name:** `Active Directory`
- **Type:** `Subnet`
- **Subnet/IP Range:** `192.168.1.10/32`
- **Interface:** `any`
- **Comments:** `Controlador de dominio principal`

**¿Qué hace?**
- Crea un objeto reutilizable llamado "Active Directory"
- Representa la IP del servidor Windows (`192.168.1.10`)
- Podés usar este objeto en múltiples políticas en lugar de escribir la IP cada vez

---

#### Por CLI

```bash
config firewall address
    edit "Active Directory"
        set subnet 192.168.1.10 255.255.255.255
        set comment "Controlador de dominio principal"
    next
end
```

**Verificar el objeto creado:**

```bash
show firewall address "Active Directory"
```

---

### Address Groups (Grupos de Direcciones)

Los **grupos de direcciones** permiten combinar múltiples objetos de dirección en uno solo, simplificando las políticas.

#### Ejemplo: Grupo de Servidores Internos

```bash
# Crear direcciones individuales
config firewall address
    edit "SRV-Web"
        set subnet 192.168.1.20 255.255.255.255
    next
    edit "SRV-Database"
        set subnet 192.168.1.30 255.255.255.255
    next
    edit "SRV-File"
        set subnet 192.168.1.40 255.255.255.255
    next
end

# Crear grupo que los contenga
config firewall addrgrp
    edit "GRP-Internal-Servers"
        set member "SRV-Web" "SRV-Database" "SRV-File"
        set comment "Todos los servidores internos"
    next
end
```

**Uso en políticas:**
Ahora podés referenciar `GRP-Internal-Servers` en lugar de listar cada servidor individualmente.

---

### FQDN (Fully Qualified Domain Name)

Los objetos FQDN son útiles para servicios externos donde la IP puede cambiar:

```bash
config firewall address
    edit "Microsoft-Updates"
        set type fqdn
        set fqdn "*. update.microsoft.com"
        set comment "Servidores de Windows Update"
    next
end
```

**¿Cuándo usar FQDN? **
- Servicios cloud donde las IPs cambian frecuentemente
- Múltiples servidores bajo un mismo dominio
- CDNs y servicios distribuidos geográficamente

> [!warning] Resolución DNS
> Los objetos FQDN requieren que el FortiGate pueda resolver DNS correctamente. Verificá la configuración de DNS antes de usar FQDN objects.

---

## ⏰ Schedule Objects (Objetos de Horario)

Los **Schedule Objects** definen **ventanas de tiempo** durante las cuales una política está activa o inactiva.

### ¿Dónde Configurarlos?

**Ruta en GUI:**
```
Policy & Objects > Schedules
```

---

### Tipos de Schedules

| Tipo | Descripción | Ejemplo de Uso |
|------|-------------|----------------|
| **One-time** | Evento único con fecha/hora de inicio y fin | Mantenimiento programado específico |
| **Recurring** | Se repite según patrón definido | Horario de oficina, días hábiles |

---

### ¿Qué Hacen los Schedules?

> [!info] Habilitación Temporal
> Los schedules **habilitan la política solo durante el intervalo de tiempo especificado**.  Fuera de ese horario, la política está deshabilitada automáticamente.

**Casos de uso comunes:**
- **Horario laboral:** Permitir acceso a Internet solo de 9:00 a 18:00
- **Días hábiles:** Deshabilitar VPN los fines de semana
- **Restricciones temporales:** Bloquear redes sociales durante horario de trabajo
- **Ventanas de mantenimiento:** Permitir acceso administrativo solo en horarios específicos

---

### Creación de Schedule Recurrente

#### Ejemplo:  Horario de Oficina

**Por GUI:**

1. **Policy & Objects** → **Schedules** → **+ Create New** → **Recurring**
2. Completar: 
   - **Name:** `Business-Hours`
   - **Days:** Lunes a Viernes
   - **Start Time:** `09:00`
   - **Stop Time:** `18:00`
3. **OK** para guardar

**Por CLI:**

```bash
config firewall schedule recurring
    edit "Business-Hours"
        set start 09:00
        set end 18:00
        set day monday tuesday wednesday thursday friday
    next
end
```

---

### Creación de Schedule One-Time

#### Ejemplo: Ventana de Mantenimiento

**Por CLI:**

```bash
config firewall schedule onetime
    edit "Maintenance-2026-02-15"
        set start 02:00 2026/02/15
        set end 06:00 2026/02/15
    next
end
```

**¿Qué hace?**
- Crea una ventana de tiempo única
- Activa solo el 15 de febrero de 2026 entre las 02:00 y 06:00
- Útil para mantenimientos programados o eventos específicos

---

### Uso de Schedules en Políticas

Una vez creado el schedule, lo usás en la configuración de la política:

```bash
config firewall policy
    edit 10
        set name "Internet-Office-Hours"
        set srcintf "port4"
        set dstintf "port2"
        set srcaddr "LAN-Network"
        set dstaddr "all"
        set schedule "Business-Hours"  # ← Aquí se aplica el schedule
        set service "HTTP" "HTTPS"
        set action accept
        set nat enable
    next
end
```

**Resultado:** Los usuarios solo pueden navegar de lunes a viernes, de 9:00 a 18:00.

---

## 🔌 Service Objects (Objetos de Servicio)

Los **Service Objects** definen protocolos y puertos específicos que el tráfico puede usar.

### ¿Dónde Configurarlos?

**Ruta en GUI:**
```
Policy & Objects > Services
```

![[Pasted image 20251230085606.png]]

---

### Servicios Predefinidos vs.  Personalizados

FortiGate incluye **cientos de servicios predefinidos**: 
- HTTP (TCP 80)
- HTTPS (TCP 443)
- SSH (TCP 22)
- DNS (UDP 53)
- FTP (TCP 21)
- Y muchos más... 

Pero podés **crear servicios personalizados** para aplicaciones específicas.

---

### Creación de Service Personalizado

#### Ejemplo: Aplicación Empresarial

Supongamos que tenés una aplicación interna que usa TCP puerto 8888:

**Por GUI:**

1. **Policy & Objects** → **Services** → **+ Create New**
2. Completar:
   - **Name:** `APP-Custom-Portal`
   - **Protocol Type:** `TCP/UDP/SCTP`
   - **Protocol:** `TCP`
   - **Destination Port:** `8888`
3. **OK** para guardar

**Por CLI:**

```bash
config firewall service custom
    edit "APP-Custom-Portal"
        set tcp-portrange 8888
        set comment "Portal interno de gestión"
    next
end
```

---

### Service Groups (Grupos de Servicios)

Similar a los address groups, podés agrupar servicios:

```bash
# Crear grupo de servicios web
config firewall service group
    edit "GRP-Web-Services"
        set member "HTTP" "HTTPS" "HTTP-ALT"
        set comment "Todos los servicios web estándar"
    next
end
```

---

### Especificación de Tráfico Permitido

> [!tip] Control Granular
> Los objetos de servicio te permiten especificar **exactamente qué tipo de tráfico está permitido**.  En lugar de usar `ALL`, definí solo los servicios necesarios.

**Ejemplo restrictivo:**

```bash
config firewall policy
    edit 20
        set name "Allow-Web-Only"
        set srcintf "port4"
        set dstintf "port2"
        set srcaddr "Workstations"
        set dstaddr "all"
        set service "HTTP" "HTTPS" "DNS"  # Solo estos servicios
        set action accept
        set nat enable
    next
end
```

**Resultado:** Los usuarios solo pueden navegar web (HTTP/HTTPS) y resolver DNS.  Otros protocolos como FTP, SSH, Telnet están bloqueados.

---

## 🌍 Internet Service Database

El **Internet Service Database** es una base de datos mantenida por Fortinet que contiene definiciones de servicios populares de Internet identificados por dirección IP, dominio, o región.

### ¿Qué es? 

Es una colección de **servicios y aplicaciones de Internet categorizados** que FortiGate actualiza regularmente a través de FortiGuard: 

- **Redes sociales:** Facebook, Twitter, LinkedIn
- **Streaming:** Netflix, YouTube, Spotify
- **Cloud:** AWS, Azure, Google Cloud
- **Comunicación:** Zoom, Microsoft Teams, Slack
- **Y miles más.. .**

---

### ¿Dónde Aplicarlo?

En la configuración de políticas, se aplica en los campos **Source** o **Destination**. 

! [[Pasted image 20251230092605.png]]

---

### ¿Cuándo Usar Internet Service Database?

> [!info] Políticas más Restrictivas
> Usá Internet Service Database cuando querés ser **más específico** sobre qué servicios de Internet están permitidos, sin tener que mantener listas de IPs manualmente.

**Casos de uso:**

1. **Bloquear redes sociales específicas**
   ```bash
   # Permitir todo excepto redes sociales
   config firewall policy
       edit 30
           set name "Block-Social-Media"
           set srcintf "port4"
           set dstintf "port2"
           set srcaddr "all"
           set internet-service-src enable
           set internet-service-src-name "Facebook" "Twitter" "Instagram"
           set action deny
       next
   end
   ```

2. **Permitir solo servicios corporativos**
   ```bash
   # Solo Microsoft 365 y Google Workspace
   config firewall policy
       edit 31
           set name "Corporate-Cloud-Only"
           set srcintf "port4"
           set dstintf "port2"
           set srcaddr "Corporate-Users"
           set internet-service enable
           set internet-service-name "Microsoft-Office365" "Google-Apps"
           set action accept
       next
   end
   ```

3. **Optimizar ancho de banda bloqueando streaming**
   ```bash
   # Bloquear servicios de video streaming
   config firewall policy
       edit 32
           set name "Block-Streaming"
           set srcintf "port4"
           set dstintf "port2"
           set internet-service-name "Netflix" "YouTube" "Twitch"
           set action deny
       next
   end
   ```

---

### Ventajas del Internet Service Database

✅ **Actualización automática** - Fortinet mantiene las IPs actualizadas  
✅ **Cobertura completa** - Incluye CDNs y servidores distribuidos globalmente  
✅ **Categorización** - Servicios organizados por categoría  
✅ **Sin mantenimiento manual** - No necesitás actualizar listas de IPs  
✅ **Mayor precisión** - Identifica servicios mejor que reglas basadas en puertos

---

### Ver Internet Services Disponibles

**Por CLI:**

```bash
# Listar todas las categorías
diagnose internet-service list

# Buscar servicio específico
diagnose internet-service name | grep -i microsoft

# Ver detalles de un servicio
diagnose internet-service name Microsoft-Office365
```

---

## 💡 Ejemplo Práctico Integrado

### Escenario Completo

**Requisitos:**
- Permitir a los empleados acceder a Internet durante horario laboral
- Solo a servicios corporativos (Microsoft 365, Google Workspace)
- Bloquear redes sociales
- Permitir acceso completo al servidor Active Directory

---

### Paso 1: Crear Address Objects

```bash
# Red de empleados
config firewall address
    edit "NET-Employees-LAN"
        set subnet 192.168.10.0 255.255.255.0
        set comment "Red de empleados"
    next
    
    # Servidor AD
    edit "SRV-Active-Directory"
        set subnet 192.168.1.10 255.255.255.255
        set comment "Controlador de dominio"
    next
end
```

---

### Paso 2: Crear Schedule

```bash
config firewall schedule recurring
    edit "Office-Hours-Extended"
        set start 08:00
        set end 19:00
        set day monday tuesday wednesday thursday friday
    next
end
```

---

### Paso 3: Crear Service (si es necesario)

En este caso usaremos servicios predefinidos, pero si hubiera servicios personalizados:

```bash
config firewall service custom
    edit "Custom-App-8080"
        set tcp-portrange 8080
    next
end
```

---

### Paso 4: Crear Políticas

```bash
# Política 1: Acceso a servicios corporativos durante horario laboral
config firewall policy
    edit 100
        set name "Corporate-Internet-Access"
        set srcintf "port4"
        set dstintf "wan1"
        set srcaddr "NET-Employees-LAN"
        set internet-service enable
        set internet-service-name "Microsoft-Office365" "Google-Apps"
        set schedule "Office-Hours-Extended"
        set action accept
        set nat enable
        set logtraffic all
    next
    
    # Política 2: Bloquear redes sociales explícitamente
    edit 101
        set name "Block-Social-Media"
        set srcintf "port4"
        set dstintf "wan1"
        set srcaddr "NET-Employees-LAN"
        set internet-service enable
        set internet-service-name "Facebook" "Twitter" "Instagram" "TikTok"
        set action deny
        set logtraffic all
    next
    
    # Política 3: Acceso al servidor AD (siempre disponible)
    edit 102
        set name "Access-AD-Server"
        set srcintf "port4"
        set dstintf "port4"
        set srcaddr "NET-Employees-LAN"
        set dstaddr "SRV-Active-Directory"
        set schedule "always"
        set service "ALL"
        set action accept
    next
end
```

---

## 🔍 Verificación de Objetos

### Ver Address Objects

```bash
# Listar todos los address objects
show firewall address

# Ver address específico
show firewall address "Active Directory"

# Ver address groups
show firewall addrgrp
```

---

### Ver Schedules

```bash
# Listar schedules recurrentes
show firewall schedule recurring

# Listar schedules one-time
show firewall schedule onetime
```

---

### Ver Services

```bash
# Listar servicios personalizados
show firewall service custom

# Ver service groups
show firewall service group
```

---

### Verificar Uso de Objetos

```bash
# Ver dónde se usa un objeto específico
diagnose sys checkused firewall. address "Active Directory"

# Ver políticas que usan un servicio
diagnose sys checkused firewall.service. custom "APP-Custom-Portal"
```

> [!warning] Objetos en Uso
> No podés eliminar un objeto que esté siendo usado por políticas activas. Primero debés eliminar las referencias en las políticas.

---

## ⚠️ Errores Comunes y Soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| No puedo eliminar un objeto | Está siendo usado en políticas | Usá `diagnose sys checkused` para encontrar referencias |
| FQDN no resuelve correctamente | Configuración DNS incorrecta | Verificá `config system dns` |
| Schedule no funciona | Zona horaria incorrecta | Verificá `config system global` → `set timezone` |
| Internet Service no bloquea | Orden de políticas incorrecto | Política de bloqueo debe estar ANTES de allow |
| Address group vacío | No agregaste miembros | Verificá `set member` en la configuración del grupo |

> [!example] Error de Orden de Políticas
> ```bash
> # ❌ Incorrecto - Allow antes que Deny
> Policy 1: Allow ALL Internet (action accept)
> Policy 2: Block Social Media (action deny)  # Nunca se evalúa
> 
> # ✅ Correcto - Deny antes que Allow
> Policy 1: Block Social Media (action deny)
> Policy 2: Allow ALL Internet (action accept)
> ```

---

## 🎓 Conclusión

Los objetos de políticas son fundamentales para una administración eficiente de FortiGate. Este manual cubrió: 

✅ **Address Objects** - Centralizar direcciones IP, redes, FQDN  
✅ **Schedules** - Controlar ventanas de tiempo de políticas  
✅ **Services** - Definir protocolos y puertos personalizados  
✅ **Internet Service Database** - Aplicar restricciones granulares por servicio

**Beneficios de usar objetos:**
- Configuración más limpia y mantenible
- Cambios centralizados afectan múltiples políticas
- Mejor documentación y comprensión de las políticas
- Reducción de errores de configuración
- Facilita auditorías de seguridad

> [!tip] Mejores Prácticas Finales
> 1. **Documentá todo** - Usá el campo `comments` en cada objeto
> 2. **Nomenclatura consistente** - Definí y seguí un estándar de nombres
> 3. **Agrupá lógicamente** - Usá grupos para simplificar políticas complejas
> 4. **Revisá regularmente** - Auditá objetos no utilizados y eliminá los obsoletos
> 5. **Versioná cambios** - Hacé backups antes de modificaciones importantes

---

## 📋 Referencia Rápida

```bash
# Address Objects
config firewall address
    edit "<nombre>"
        set subnet <ip> <mask>
        set comment "<descripción>"
    next
end

# Address Groups
config firewall addrgrp
    edit "<nombre-grupo>"
        set member "<obj1>" "<obj2>" "<obj3>"
    next
end

# Schedules (Recurring)
config firewall schedule recurring
    edit "<nombre>"
        set start HH:MM
        set end HH:MM
        set day monday tuesday wednesday thursday friday
    next
end

# Services
config firewall service custom
    edit "<nombre>"
        set tcp-portrange <puerto>
        set comment "<descripción>"
    next
end

# Verificar uso
diagnose sys checkused firewall.address "<nombre>"
```

---

**Etiquetas:** #fortinet #fortigate #policy-objects #address #schedule #services #internet-service #configuracion #seguridad