---
Tema: "[[CLI]]"
---
# 📘 FortiGate CLI - Gestión de Interfaces

---

## 🎯 Introducción

La **CLI (Command Line Interface)** de FortiGate es una herramienta fundamental para la configuración y administración del firewall. Este manual se centra en la **gestión de interfaces de red**, uno de los componentes más críticos de cualquier FortiGate, ya que define cómo el dispositivo se comunica con las redes internas y externas. 

Dominar estos comandos te permitirá: 
- Visualizar y modificar configuraciones de puertos
- Habilitar servicios de administración
- Configurar parámetros de red (IP, DHCP, etc.)

---

## ✅ Requisitos Previos

Antes de comenzar, asegurate de tener: 

- [ ] Acceso SSH o consola al FortiGate
- [ ] Credenciales con privilegios administrativos
- [ ] Conocimiento básico de la estructura de red de tu entorno
- [ ] Identificación de las interfaces físicas disponibles

> [!warning] Advertencia
> Modificar configuraciones de interfaces puede interrumpir la conectividad.  Realizá cambios en ventanas de mantenimiento o tené acceso alternativo al dispositivo. 

---

## 🛠️ Comandos Básicos de Gestión de Interfaces

### 1️⃣ Listar Interfaces Disponibles

Para ver todas las interfaces físicas y virtuales configuradas en el FortiGate:

```bash
get system interface
```

**¿Qué hace?**
- Muestra un listado de todas las interfaces del sistema
- Incluye información como:  nombre, IP, estado (up/down), tipo de interfaz
- Útil para identificar los puertos antes de modificarlos

**Ejemplo de salida:**

```
name: port1      ip:  192.168.1.1/24      status: up
name: port2      ip: 10.0.0.1/24         status: up
name: wan1       ip:  DHCP                status: up
```

---

### 2️⃣ Editar Configuración de una Interfaz

Para acceder al modo de configuración de una interfaz específica: 

```bash
config system interface
    edit port1
```

**¿Qué hace?**
- Entra al contexto de configuración de interfaces (`config system interface`)
- Selecciona la interfaz específica que querés modificar (`edit port1`)
- Desde aquí podés cambiar IP, habilitar servicios, configurar VLAN, etc. 

> [!info] Contexto de Configuración
> En FortiOS, `config` entra a un contexto de configuración, y `edit` selecciona un objeto específico dentro de ese contexto.

---

### 3️⃣ Mostrar Configuración Actual de la Interfaz

Dentro del contexto de edición de una interfaz: 

```bash
show
```

**¿Qué hace?**
- Muestra **toda la configuración** de la interfaz seleccionada
- Incluye: IP, máscara, servicios habilitados, alias, VLAN ID, etc. 
- Es una verificación previa antes de hacer cambios

**Ejemplo de salida:**

```
config system interface
    edit "port1"
        set vdom "root"
        set ip 192.168.1.1 255.255.255.0
        set allowaccess ping https ssh
        set type physical
        set alias "LAN"
        set status up
    next
end
```

---

### 4️⃣ Agregar Servicios de Acceso a la Interfaz

Para habilitar servicios administrativos en una interfaz (por ejemplo, HTTP):

```bash
append allowaccess http
```

**¿Qué hace? **
- **Agrega** el servicio `http` a la lista de servicios permitidos en esa interfaz
- `append` añade sin sobrescribir los servicios existentes
- Permite acceder a la GUI web del FortiGate desde esa interfaz

**Servicios comunes:**

| Servicio | Descripción |
|----------|-------------|
| `ping` | Permite responder a pings ICMP |
| `http` | Acceso a la GUI web (puerto 80) |
| `https` | Acceso a la GUI web segura (puerto 443) |
| `ssh` | Acceso por SSH (puerto 22) |
| `telnet` | Acceso por Telnet (puerto 23, no recomendado) |
| `fgfm` | FortiManager communication |
| `snmp` | Monitoreo SNMP |

> [!tip] Buena Práctica
> Usá siempre `https` y `ssh` en lugar de `http` y `telnet` para evitar credenciales en texto plano.

**Alternativas:**

```bash
set allowaccess ping https ssh    # Sobrescribe la lista completa
append allowaccess http            # Agrega a la lista existente
unset allowaccess http             # Elimina un servicio específico
```

---

### 5️⃣ Guardar y Salir de la Configuración

Para **aplicar y guardar** los cambios realizados:

```bash
end
```

**¿Qué hace?**
- Sale del contexto de configuración actual
- **Guarda automáticamente** todos los cambios realizados
- Los cambios entran en vigencia inmediatamente

> [!warning] Importante
> A diferencia de otros dispositivos, FortiGate **NO requiere** un comando `save` o `write memory`. El comando `end` aplica y guarda automáticamente.

---

## 💡 Ejemplo Práctico Completo

**Escenario:** Querés habilitar acceso HTTP y SSH en el puerto `port2` para administración local.

```bash
# Paso 1: Ver interfaces disponibles
get system interface

# Paso 2: Entrar a configuración de port2
config system interface
    edit port2
    
    # Paso 3: Verificar configuración actual
    show
    
    # Paso 4: Agregar servicios de administración
    append allowaccess http
    append allowaccess ssh
    
    # Paso 5: Verificar cambios (opcional)
    show
    
    # Paso 6: Guardar y salir
    end
```

**Resultado:** Ahora podés acceder a la GUI web y SSH desde la red conectada a `port2`.

---

## ⚠️ Errores Comunes y Soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| No puedo acceder a la GUI después de cambios | `allowaccess` no incluye `http` o `https` | Usá consola/SSH para agregar `append allowaccess https` |
| Cambios no se aplican | No ejecutaste `end` | Siempre finalizá con `end` para guardar |
| "Command fail" al editar interfaz | Nombre de interfaz incorrecto | Verificá el nombre exacto con `get system interface` |
| Perdí acceso después de cambiar IP | IP nueva no es alcanzable desde tu red | Usá consola local o restablecé a configuración de fábrica |

> [! example] Recuperación de Acceso Perdido
> Si perdés acceso administrativo por configuración incorrecta de `allowaccess`:
> 1. Conectate por **consola serial**
> 2. Ejecutá:  `config system interface` → `edit <interfaz>` → `set allowaccess https ssh` → `end`

---

## 🔍 Verificación Final

Después de configurar una interfaz, verificá que todo funcione correctamente:

```bash
# Ver estado de interfaces
get system interface physical

# Verificar servicios habilitados en una interfaz específica
get system interface | grep -A 20 port1

# Probar conectividad (desde el FortiGate)
execute ping <ip-destino>

# Ver sesiones activas en una interfaz
diagnose sys session list
```


---

## 🎓 Conclusión

La gestión de interfaces desde CLI es una habilidad fundamental en FortiGate. Estos comandos básicos te permiten: 

✅ Visualizar y auditar configuraciones existentes  
✅ Habilitar servicios de administración de forma granular  
✅ Recuperar acceso en casos de configuración incorrecta  
✅ Automatizar configuraciones mediante scripts

> [!note] Próximos Pasos
> Explorá configuraciones avanzadas como:  asignación de IP estática (`set ip`), configuración de DHCP server, creación de VLANs, y configuración de zonas de seguridad. 

---

**Etiquetas:** #fortinet #fortigate #cli #interfaces #networking #firewall