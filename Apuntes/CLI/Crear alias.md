---
Tema: "[[CLI]]"
---
# 📘 FortiGate CLI - Creación de Alias

---

## 🎯 Introducción

Los **alias** en FortiGate son atajos personalizados que te permiten ejecutar comandos complejos o frecuentes con un simple nombre. Esta funcionalidad es extremadamente útil para: 

- **Optimizar el tiempo** al ejecutar comandos largos o repetitivos
- **Estandarizar operaciones** comunes en tu equipo
- **Reducir errores** al evitar escribir comandos complejos manualmente
- **Crear herramientas de diagnóstico** personalizadas

En lugar de escribir `diagnose sys session filter src 192.168.1.10` cada vez, podrías crear un alias llamado `checksessions` que ejecute ese comando automáticamente.

---

## ✅ Requisitos Previos

Antes de crear alias, asegurate de tener:

- [ ] Acceso SSH o consola al FortiGate
- [ ] Privilegios administrativos
- [ ] Conocimiento del comando que querés convertir en alias
- [ ] Un nombre descriptivo y fácil de recordar para tu alias

> [!info] Persistencia de Alias
> Los alias se guardan en la configuración del FortiGate y persisten después de reinicios, a diferencia de alias temporales en sistemas Linux/Unix.

---

## 🛠️ Procedimiento Paso a Paso

### 1️⃣ Entrar al Contexto de Configuración de Alias

```bash
config system alias
```

**¿Qué hace?**
- Entra al contexto de configuración donde se gestionan los alias
- Desde aquí podés crear, editar o eliminar alias existentes

---

### 2️⃣ Crear o Editar un Alias

```bash
edit nombre
```

**¿Qué hace?**
- Si `nombre` no existe, lo crea
- Si `nombre` ya existe, entra en modo edición
- El nombre del alias debe ser único y descriptivo

**Reglas para nombres de alias:**

| ✅ Válido | ❌ Inválido | Motivo |
|----------|-------------|---------|
| `check-sessions` | `check sessions` | No usar espacios |
| `port1info` | `show-port1` | Evitar comandos nativos |
| `backup_config` | `123alias` | No empezar con números |

> [!tip] Convenciones de Nombres
> Usá nombres descriptivos que indiquen claramente qué hace el alias:
> - `check-` para comandos de consulta
> - `show-` para visualización
> - `clear-` para operaciones de limpieza
> - `diag-` para diagnósticos

---

### 3️⃣ Definir el Comando del Alias

```bash
set command "comando"
```

**¿Qué hace?**
- Establece el comando que se ejecutará cuando llames al alias
- El comando debe ir entre **comillas dobles**
- Podés usar cualquier comando válido de FortiOS

**Ejemplo básico:**

```bash
FORTI-01 (nombre) # set command "show system interface port1"
```

**Comandos comunes para alias:**

```bash
# Mostrar información de interfaz específica
set command "get system interface port1"

# Ver sesiones activas filtradas
set command "diagnose sys session list | grep -f 192.168.1.100"

# Estado de recursos del sistema
set command "get system performance status"

# Verificar estado de VPN
set command "get vpn ipsec tunnel summary"

# Ver políticas de firewall activas
set command "diagnose firewall iprope list 100"

# Backup rápido de configuración
set command "execute backup config ftp backup.conf 192.168.1.10 admin password123"
```

> [!warning] Limitaciones
> Los alias **NO pueden**:
> - Ejecutar múltiples comandos secuencialmente
> - Incluir comandos de configuración (`config`, `edit`, `set`)
> - Usar variables o parámetros dinámicos
> - Ejecutar otros alias

---

### 4️⃣ Guardar y Salir del Contexto

Después de definir el comando, tenés tres opciones:

#### Opción A: Guardar y Volver a Raíz

```bash
end
```

**¿Qué hace?**
- Guarda el alias creado o modificado
- Sale completamente del contexto de configuración
- Vuelve al prompt raíz `FORTI-01 # `

#### Opción B:  Guardar y Continuar en el Contexto

```bash
next
```

**¿Qué hace?**
- Guarda el alias actual
- Permanece en el contexto `config system alias`
- Te permite crear o editar otro alias inmediatamente

**Útil cuando creás múltiples alias:**

```bash
config system alias
    edit check-port1
        set command "get system interface port1"
    next
    
    edit check-port2
        set command "get system interface port2"
    next
    
    edit check-sessions
        set command "diagnose sys session stat"
    end
```

#### Opción C: Cancelar Sin Guardar

```bash
abort
```

**¿Qué hace?**
- **Descarta todos los cambios** realizados en ese alias
- Sale del contexto de configuración
- Útil si cometiste un error y querés empezar de nuevo

---

## 💡 Ejemplos Prácticos

### Ejemplo 1: Alias Simple para Ver Interfaz

```bash
config system alias
    edit port1-info
        set command "get system interface port1"
    end
```

**Uso:**

```bash
alias port1-info
```

**Salida esperada:**
```
== [onboard]
name: port1
status: up
ip: 192.168.1.1 255.255.255.0
... 
```

---

### Ejemplo 2: Alias para Diagnóstico de Sesiones

```bash
config system alias
    edit session-stats
        set command "diagnose sys session stat"
    end
```

**Uso:**

```bash
alias session-stats
```

**Utilidad:**
- Ver rápidamente estadísticas de sesiones activas
- Monitorear uso de recursos de tabla de sesiones
- Útil para troubleshooting de performance

---

### Ejemplo 3: Alias para Verificar Estado de HA

```bash
config system alias
    edit ha-status
        set command "get system ha status"
    end
```

**Uso:**

```bash
alias ha-status
```

**Utilidad:**
- Verificar rápidamente el estado de alta disponibilidad
- Confirmar sincronización entre nodos
- Identificar el nodo primario y secundario

---

### Ejemplo 4: Alias con Filtrado de Información

```bash
config system alias
    edit show-wan-ip
        set command "get system interface wan1 | grep -i ip"
    end
```

**Uso:**

```bash
alias show-wan-ip
```

**Utilidad:**
- Ver solo la IP de la interfaz WAN sin información adicional
- Útil para scripts o verificaciones rápidas

---

### Ejemplo 5: Creación de Múltiples Alias

```bash
config system alias
    edit check-cpu
        set command "get system performance status | grep CPU"
    next
    
    edit check-memory
        set command "get system performance status | grep Memory"
    next
    
    edit check-sessions
        set command "diagnose sys session stat"
    next
    
    edit check-vpn
        set command "get vpn ipsec tunnel summary"
    end
```

**Resultado:** Ahora tenés cuatro alias de monitoreo listos para usar.

---

## 🔍 Gestión de Alias Existentes

### Ver Todos los Alias Configurados

```bash
show system alias
```

**Salida:**

```
config system alias
    edit "port1-info"
        set command "get system interface port1"
    next
    edit "session-stats"
        set command "diagnose sys session stat"
    next
end
```

---

### Modificar un Alias Existente

```bash
config system alias
    edit port1-info
        set command "show system interface port1"
    end
```

> [!info] Sobrescritura
> Al usar `set command` en un alias existente, el comando anterior se sobrescribe completamente. 

---

### Eliminar un Alias

```bash
config system alias
    delete port1-info
end
```

**¿Qué hace?**
- Elimina permanentemente el alias especificado
- No requiere confirmación
- El cambio es inmediato

---

### Listar Alias de Forma Rápida

```bash
show system alias | grep edit
```

**Salida:**

```
    edit "port1-info"
    edit "session-stats"
    edit "ha-status"
```

---

## ⚠️ Errores Comunes y Soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| "Command fail" al ejecutar alias | Comando dentro del alias tiene sintaxis incorrecta | Verificá el comando ejecutándolo manualmente antes de crear el alias |
| Alias no aparece después de crearlo | No ejecutaste `end` o `next` | Siempre guardá con `end` o `next` |
| "Unknown action" al usar alias | El nombre del alias coincide con un comando nativo | Cambiá el nombre del alias a algo único |
| Alias ejecuta comando diferente al esperado | Comando fue modificado por error | Verificá con `show system alias` y corregí |
| No puedo usar parámetros con el alias | Los alias no soportan parámetros | Creá múltiples alias para diferentes variaciones |

> [!example] Error Común:  Comillas Faltantes
> ```bash
> # ❌ Incorrecto
> set command get system interface port1
> 
> # ✅ Correcto
> set command "get system interface port1"
> ```

---

## 🔍 Verificación y Prueba

Después de crear un alias, verificá que funcione correctamente:

### 1. Confirmar que el Alias Existe

```bash
show system alias | grep -A 2 "edit \"nombre-alias\""
```

### 2. Ejecutar el Alias

```bash
alias nombre-alias
```

### 3. Comparar con el Comando Original

```bash
# Ejecutar alias
alias port1-info

# Ejecutar comando directamente
get system interface port1

# Ambos deben producir la misma salida
```

---

## 📌 Checklist de Creación de Alias

- [ ] El comando funciona correctamente cuando lo ejecuto manualmente
- [ ] El nombre del alias es descriptivo y único
- [ ] El comando está entre comillas dobles
- [ ] Guardé el alias con `end` o `next`
- [ ] Probé el alias después de crearlo
- [ ] Documenté el propósito del alias (si es complejo)

---

## 🎯 Casos de Uso Recomendados

### Para Administradores de Red

```bash
config system alias
    edit net-health
        set command "get system performance status"
    next
    edit routing-table
        set command "get router info routing-table all"
    next
    edit arp-table
        set command "get system arp"
    end
```

### Para Seguridad y Monitoreo

```bash
config system alias
    edit active-threats
        set command "diagnose ips session list"
    next
    edit blocked-ips
        set command "diagnose firewall iprope list 100"
    next
    edit av-stats
        set command "diagnose sys scanunit stats"
    end
```

### Para Troubleshooting

```bash
config system alias
    edit debug-traffic
        set command "diagnose debug flow trace start 100"
    next
    edit session-clear
        set command "diagnose sys session clear"
    next
    edit dns-cache-clear
        set command "execute clear system dns-cache"
    end
```

---

## 🎓 Conclusión

Los alias en FortiGate son una herramienta poderosa para: 

✅ **Aumentar eficiencia** - Reducir tiempo en tareas repetitivas  
✅ **Mejorar consistencia** - Estandarizar comandos en el equipo  
✅ **Facilitar troubleshooting** - Tener comandos de diagnóstico listos  
✅ **Reducir errores** - Evitar typos en comandos complejos  

Con esta funcionalidad, podés crear tu propio toolkit personalizado de comandos que se adapte a tus necesidades específicas de administración.

> [!tip] Mejores Prácticas
> - Creá alias para comandos que usás más de 3 veces al día
> - Documentá tus alias en un archivo externo para compartir con tu equipo
> - Usá nombres consistentes (ej: todos los alias de diagnóstico empiezan con `diag-`)
> - Revisá y actualizá tus alias periódicamente

---

## 📋 Resumen de Sintaxis

```bash
# Crear/Editar alias
config system alias
    edit <nombre>
        set command "<comando>"
    end

# Ejecutar alias
alias <nombre>

# Ver alias existentes
show system alias

# Eliminar alias
config system alias
    delete <nombre>
end
```

---

**Etiquetas:** #fortinet #fortigate #cli #alias #productividad #automatizacion #troubleshooting