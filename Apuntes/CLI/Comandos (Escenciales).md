---
Tema: "[[CLI]]"
---
# 📘 FortiGate CLI - Lista de Comandos Útiles

---

## 🎯 Introducción

Este manual cubre los **comandos esenciales de la CLI de FortiGate** que necesitás dominar para administrar eficientemente tu firewall. La CLI de FortiOS tiene una estructura jerárquica y contextual que puede parecer compleja al principio, pero una vez que entiendas su lógica, te permitirá realizar configuraciones avanzadas de forma rápida y precisa.

Este documento sirve como **referencia rápida** para: 
- Navegar por los diferentes contextos de configuración
- Ejecutar comandos de diagnóstico y operación
- Filtrar y buscar información específica
- Gestionar alias y comandos personalizados

---

## ✅ Requisitos Previos

Antes de trabajar con estos comandos, asegurate de tener:

- [ ] Acceso SSH o consola al FortiGate
- [ ] Credenciales administrativas válidas
- [ ] Comprensión básica de la estructura jerárquica de FortiOS
- [ ] Familiaridad con el prompt de comandos de FortiGate

> [!info] Prompt del Sistema
> El prompt raíz en FortiGate se ve así:  `FORTI-01 # _`
> Indica que estás en el nivel superior de la CLI con privilegios administrativos.

---

## 🧭 Navegación y Comandos de Ayuda

### Comando de Ayuda Contextual

```bash
? 
```

**¿Qué hace?**
- Muestra los comandos y opciones disponibles en el **contexto actual**
- Te ayuda a entender en qué nivel de configuración estás
- Lista los parámetros válidos para el comando que estás escribiendo

**Ejemplo de uso:**

```bash
FORTI-01 # config ? 
# Muestra todos los contextos de configuración disponibles

FORTI-01 # config system interface
FORTI-01 (interface) # edit port1
FORTI-01 (port1) # set ? 
# Muestra todos los parámetros configurables para la interfaz
```

> [!tip] Autocompletado
> Usá la tecla **TAB** para autocompletar comandos y nombres de objetos.  Presioná **TAB dos veces** para ver todas las opciones disponibles.

---

### Comandos de Navegación

```bash
abort    # Cancela cambios y sale del contexto actual
next     # Guarda y pasa al siguiente objeto en el mismo nivel
end      # Guarda cambios y sale al nivel raíz
```

**Diferencias clave:**

| Comando | Guarda Cambios | Acción |
|---------|----------------|--------|
| `abort` | ❌ No | Descarta cambios y sale del contexto |
| `next` | ✅ Sí | Guarda y continúa en el mismo nivel de configuración |
| `end` | ✅ Sí | Guarda y vuelve al nivel raíz |

---

## 🔍 Filtrado con Grep

FortiOS soporta el uso de `grep` para filtrar la salida de comandos:

```bash
get system interface | grep -f "port1"
```

**Opciones de grep:**

| Opción | Descripción |
|--------|-------------|
| `-f` | Muestra resultados con contexto adicional (líneas antes y después) |
| `-i` | Búsqueda insensible a mayúsculas/minúsculas (case-insensitive) |

**Ejemplos prácticos:**

```bash
# Buscar configuración específica de port1 con contexto
get system interface | grep -f "port1"

# Buscar cualquier referencia a "wan" sin importar mayúsculas
show | grep -i wan

# Buscar políticas con tráfico específico
diagnose firewall iprope list 100 | grep -f "192.168.1.100"
```

> [!tip] Combinando Filtros
> Podés encadenar múltiples greps:  `show | grep -i firewall | grep -i policy`

---

## 📋 Comandos Principales de FortiOS

### Tabla de Referencia Rápida

| Comando | Descripción | Uso Principal |
|---------|-------------|---------------|
| `config` | Posiciona en contextos de configuración | Modificar configuración del sistema |
| `get` | Obtiene información dinámica y del sistema | Ver estado actual y estadísticas |
| `show` | Muestra configuración guardada | Revisar configuración actual |
| `diagnose` | Herramientas de diagnóstico avanzadas | Troubleshooting y análisis profundo |
| `execute` | Ejecuta comandos operacionales | Operaciones puntuales (ping, backup, etc.) |
| `alias` | Ejecuta comandos de alias personalizados | Atajos para comandos frecuentes |
| `exit` | Sale de la CLI | Cerrar sesión |

---

## 🔧 Comando `config` - Contextos de Configuración

El comando `config` te permite entrar en diferentes áreas de configuración del FortiGate. 

### Sintaxis Básica

```bash
config <contexto> <sub-contexto>
    edit <objeto>
        # Realizar cambios
    end
```

### Contextos de Configuración Disponibles

#### **Seguridad y Protección**

| Contexto | Descripción |
|----------|-------------|
| `antivirus` | Configuración del motor AntiVirus |
| `application` | Control de aplicaciones (App Control) |
| `ips` | Sistema de prevención de intrusiones |
| `dlp` | Prevención de pérdida de datos (Data Loss Prevention) |
| `webfilter` | Filtrado de contenido web |
| `dnsfilter` | Filtrado y protección DNS |
| `emailfilter` | AntiSpam y filtrado de correo |
| `file-filter` | Filtrado de archivos por tipo |
| `videofilter` | Control de streaming de video |
| `waf` | Firewall de aplicaciones web (Web Application Firewall) |

#### **Conectividad y Red**

| Contexto | Descripción |
|----------|-------------|
| `firewall` | Políticas y objetos de firewall |
| `router` | Configuración de enrutamiento |
| `vpn` | Configuración de VPN (IPsec, SSL-VPN) |
| `system` | Operación general del sistema |
| `wireless-controller` | Gestión de puntos de acceso WiFi |
| `switch-controller` | Control de FortiSwitch |
| `extender-controller` | Gestión de FortiExtender |

#### **Autenticación y Usuarios**

| Contexto | Descripción |
|----------|-------------|
| `user` | Usuarios, grupos y autenticación |
| `authentication` | Esquemas de autenticación |
| `endpoint-control` | Control de dispositivos finales (NAC) |

#### **Monitoreo y Reportes**

| Contexto | Descripción |
|----------|-------------|
| `log` | Configuración de logs y eventos |
| `report` | Generación de reportes |
| `alertemail` | Alertas por correo electrónico |

#### **Servicios Adicionales**

| Contexto | Descripción |
|----------|-------------|
| `web-proxy` | Proxy web explícito |
| `ftp-proxy` | Proxy FTP |
| `icap` | Cliente ICAP para análisis externo |
| `voip` | Políticas de voz sobre IP |
| `wanopt` | Optimización de WAN |
| `sctp-filter` | Filtrado de protocolo SCTP |
| `ssh-filter` | Inspección de tráfico SSH |
| `dpdk` | Configuración DPDK (aceleración de paquetes) |

---

### Subcomandos Dentro de `config`

Una vez dentro de un contexto de configuración: 

```bash
config system alias
```

**Comandos disponibles:**

| Comando | Descripción |
|---------|-------------|
| `edit <nombre>` | Crea o edita un objeto específico |
| `delete <nombre>` | Elimina un objeto |
| `purge` | Elimina todos los objetos del contexto |
| `get` | Muestra valores actuales |
| `show` | Muestra configuración guardada |
| `end` | Guarda y sale del contexto |

---

### Dentro de un Objeto Editado

```bash
config system admin
    edit <usuario>
```

**Comandos disponibles:**

| Comando | Descripción |
|---------|-------------|
| `set <parámetro> <valor>` | Establece un valor |
| `unset <parámetro>` | Elimina un parámetro (vuelve al default) |
| `get` | Muestra configuración actual |
| `show` | Muestra configuración completa del objeto |
| `next` | Guarda y continúa al siguiente objeto |
| `abort` | Cancela cambios y sale |
| `end` | Guarda cambios y sale al nivel raíz |

---

### Ejemplo Práctico:  Configurar 2FA para un Usuario

```bash
# Entrar al contexto de administradores
config system admin
    # Editar usuario específico
    edit admin
        # Ver configuración actual
        show
        
        # Habilitar autenticación de dos factores
        set two-factor fortitoken
        
        # Guardar y salir
    end
```

> [!warning] Cuidado con `purge`
> El comando `purge` elimina **TODOS** los objetos del contexto actual sin confirmación. Usalo con extrema precaución.

---

## 📊 Comando `get` - Información Dinámica

El comando `get` muestra información **en tiempo real** del estado del sistema. 

```bash
get system interface
```

**¿Qué hace?**
- Muestra el **estado actual** de las interfaces (no solo la configuración)
- Incluye estadísticas dinámicas:  paquetes, bytes, errores
- Información sobre estado de conexión (up/down)

**Diferencia entre `get` y `show`:**

| Comando | Tipo de Información | Ejemplo |
|---------|---------------------|---------|
| `get` | Estado dinámico actual | IPs obtenidas por DHCP, estadísticas de tráfico |
| `show` | Configuración guardada | Configuración estática de IP, parámetros guardados |

```bash
# Ver estado actual de interfaces (dinámico)
get system interface

# Ver estadísticas de sesiones activas
get system session status

# Ver información de hardware
get system status

# Ver estado de HA
get system ha status
```

---

## 📝 Comando `show` - Configuración Guardada

El comando `show` muestra la **configuración guardada** en el sistema.

```bash
show system interface
```

**¿Qué hace?**
- Muestra la configuración tal como está guardada en el sistema
- No incluye información dinámica o estadísticas
- Útil para auditar y documentar configuraciones

**Ejemplos:**

```bash
# Ver toda la configuración del firewall
show

# Ver configuración de una sección específica
show firewall policy

# Ver configuración de un objeto específico
show firewall address

# Exportar configuración completa
show full-configuration
```

> [!tip] Backup de Configuración
> Usá `show full-configuration` para obtener un backup completo de la configuración en texto plano que podés guardar. 

---

## 🔬 Comando `diagnose` - Herramientas de Diagnóstico

El comando `diagnose` proporciona herramientas avanzadas de troubleshooting.

**Comandos diagnósticos comunes:**

```bash
# Ver sesiones activas
diagnose sys session list

# Limpiar tabla de sesiones
diagnose sys session clear

# Sniffer de paquetes
diagnose sniffer packet any "host 192.168.1.100" 4

# Debug de flujo de tráfico
diagnose debug flow filter addr 10.0.0.50
diagnose debug flow trace start 100
diagnose debug enable

# Ver tabla de routing
diagnose ip route list

# Test de conectividad de políticas
diagnose firewall iprope lookup-policy src 192.168.1.10 dst 8.8.8.8 proto 6 sport 12345 dport 443
```

> [!warning] Impacto en Performance
> Algunos comandos de diagnóstico (especialmente debug y sniffer) pueden impactar el rendimiento del FortiGate. Usarlos con precaución en producción.

---

## ⚡ Comando `execute` - Comandos Operacionales

El comando `execute` (o `exec`) ejecuta operaciones puntuales que no modifican configuración permanente.

### Opciones de Ping

```bash
# Ver opciones configuradas de ping
exec ping-options view-settings

# Configurar IP de origen para ping
exec ping-options source 1.1.1.1

# Resetear opciones de ping a valores default
exec ping-options reset

# Ejecutar ping
exec ping 8.8.8.8
```

### Comandos Execute Comunes

```bash
# Reiniciar el FortiGate
exec reboot

# Hacer backup de configuración
exec backup config ftp backup-20260113.conf 192.168.1.100 admin password

# Restaurar configuración
exec restore config ftp backup-20260113.conf 192.168.1.100 admin password

# Actualizar firmware
exec restore image ftp FGT_60E-7.4.1.img 192.168.1.100 admin password

# Test de conectividad avanzado
exec traceroute 8.8.8.8

# Verificar conectividad a FortiGuard
exec update-now

# Limpiar caché DNS
exec clear system dns-cache

# Desconectar usuario SSL-VPN
exec vpn sslvpn del-tunnel <tunnel-id>
```

---

## 🏷️ Comando `alias` - Atajos Personalizados

Los alias te permiten crear atajos para comandos frecuentemente usados.

```bash
# Ejecutar un alias existente
alias nombre_alias
```

**Ejemplo de creación de alias:**

```bash
config system alias
    edit "check-sessions"
        set command "diagnose sys session stat"
    next
end

# Ahora podés ejecutar:
alias check-sessions
```

> [!note] Gestión de Alias
> Para aprender cómo crear, modificar y gestionar alias, consultá la nota sobre creación de alias en tu vault. 

---

## 💡 Ejemplos Prácticos Integrados

### Ejemplo 1: Auditoría de Seguridad de Interfaces

```bash
# Ver todas las interfaces y sus IPs
get system interface

# Ver configuración detallada de seguridad
show system interface

# Filtrar solo interfaces con HTTP habilitado
show system interface | grep -i http

# Verificar servicios de administración permitidos
get system interface | grep -f allowaccess
```

### Ejemplo 2: Troubleshooting de Conectividad

```bash
# Configurar ping desde una interfaz específica
exec ping-options source 192.168.1.1

# Hacer ping a destino
exec ping 8.8.8.8

# Ver tabla de routing
diagnose ip route list

# Verificar si existe política que permita el tráfico
diagnose firewall iprope lookup-policy src 192.168.1.10 dst 8.8.8.8 proto 1

# Ver sesiones activas relacionadas
diagnose sys session filter src 192.168.1.10
diagnose sys session list
```

### Ejemplo 3: Revisión de Configuración de Usuario

```bash
# Ver usuarios administrativos
show system admin

# Entrar a configuración de usuario
config system admin
    edit admin
        # Ver configuración actual
        show
        
        # Habilitar 2FA
        set two-factor fortitoken
        
        # Verificar cambio
        get
        
        # Guardar
    end

# Confirmar cambio guardado
show system admin | grep -A 10 "edit admin"
```

---

## ⚠️ Errores Comunes y Soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| "Command parse error" | Sintaxis incorrecta o contexto equivocado | Usá `?` para ver opciones válidas en ese contexto |
| Cambios no se guardan | No ejecutaste `end` o `next` | Siempre finalizá con `end` para guardar |
| No puedo salir del contexto | Estás en un nivel anidado profundo | Ejecutá `end` múltiples veces o `abort` para salir sin guardar |
| "Object not found" | Nombre de objeto incorrecto | Verificá el nombre exacto con `show` antes de editar |
| Grep no muestra resultados | Sintaxis incorrecta o no hay coincidencias | Probá con `-i` para búsqueda insensible a mayúsculas |

> [!example] Recuperación Rápida
> Si te perdiste en la navegación de contextos, ejecutá `end` repetidamente hasta volver al prompt raíz, o usá `abort` para cancelar cambios y salir.

---

## 🔍 Verificación y Mejores Prácticas

### Checklist de Buenas Prácticas

- [ ] Siempre ejecutá `show` antes de modificar para ver la configuración actual
- [ ] Usá `get` después de cambios para verificar el estado dinámico
- [ ] Aprovechá `grep` para filtrar información en sistemas con muchas configuraciones
- [ ] Creá alias para comandos de diagnóstico que usás frecuentemente
- [ ] Documentá los cambios realizados
- [ ] Hacé backup antes de cambios mayores con `exec backup config`

### Comandos de Verificación Post-Cambio

```bash
# Ver última configuración guardada
get system status

# Verificar cambios específicos
show | grep -f <término-buscado>

# Revisar logs de cambios administrativos
exec log filter category 2
exec log display
```

---

## 🎓 Conclusión

Este manual cubre los comandos esenciales de FortiGate que te permiten: 

✅ Navegar eficientemente por la CLI jerárquica  
✅ Configurar todos los aspectos del firewall con `config`  
✅ Monitorear estado en tiempo real con `get`  
✅ Revisar configuraciones guardadas con `show`  
✅ Diagnosticar problemas con `diagnose`  
✅ Ejecutar operaciones operacionales con `execute`  
✅ Optimizar tu flujo de trabajo con `alias`

La CLI de FortiGate es extremadamente poderosa.  Con estos comandos fundamentales, tendrás una base sólida para administrar cualquier aspecto del firewall de forma eficiente. 

> [!tip] Práctica Constante
> La mejor forma de dominar la CLI es usarla diariamente. Empezá con comandos de consulta (`get`, `show`) que son seguros, y gradualmente incorporá comandos de configuración a medida que ganes confianza.

---

## 📌 Referencia Rápida

```bash
# Ayuda contextual
? 

# Navegación
config <contexto>    # Entrar a configuración
edit <objeto>        # Editar objeto
show                 # Ver configuración
get                  # Ver estado actual
end                  # Guardar y salir
abort                # Cancelar y salir

# Filtrado
<comando> | grep -f "término"    # Con contexto
<comando> | grep -i "término"    # Case-insensitive

# Operaciones
exec ping <ip>
exec backup config
diagnose sys session list
alias <nombre>
```

---

**Etiquetas:** #fortinet #fortigate #cli #comandos #referencia #troubleshooting #configuracion