---
Tema: "[[CLI]]"
---
# 📘 FortiGate - Sniffer de Paquetes para Debug

---

## 🎯 Introducción

El **sniffer de paquetes** integrado en FortiGate es una herramienta de diagnóstico fundamental para troubleshooting de conectividad y análisis de tráfico de red. A diferencia de herramientas externas como Wireshark o tcpdump, el sniffer de FortiGate te permite capturar y analizar tráfico **directamente desde el firewall**, sin necesidad de equipos adicionales.

Esta herramienta es invaluable para: 
- **Diagnosticar problemas de conectividad** entre redes
- **Verificar si el tráfico está llegando** al FortiGate
- **Analizar flujos de paquetes** en tiempo real
- **Validar configuraciones de políticas** de firewall
- **Troubleshootar VPNs**, DNS, y otros servicios

> [!warning] Impacto en Performance
> El sniffer consume recursos del CPU.  Usalo con filtros específicos y en ventanas cortas de tiempo en ambientes de producción.

---

## ✅ Requisitos Previos

Antes de usar el sniffer, asegurate de tener:

- [ ] Acceso SSH o consola al FortiGate
- [ ] Privilegios administrativos
- [ ] Conocimiento básico de protocolos de red (TCP, UDP, ICMP)
- [ ] Identificación de la interfaz donde querés capturar tráfico
- [ ] Comprensión básica de filtros BPF (Berkeley Packet Filter)

> [!info] Sintaxis de Filtros
> FortiGate usa sintaxis similar a **tcpdump** (filtros BPF) para especificar qué tráfico capturar.

---

## 🛠️ Sintaxis Básica del Sniffer

### Estructura del Comando

```bash
diagnose sniffer packet <interfaz> <filtros> <nivel_verbosidad> <cantidad_paquetes>
```

**Parámetros:**

| Parámetro | Descripción | Obligatorio |
|-----------|-------------|-------------|
| `<interfaz>` | Interfaz donde capturar (ej: `port1`, `wan1`, `any`) | ✅ Sí |
| `<filtros>` | Filtros BPF para el tráfico (entre comillas) | ❌ No (por defecto:  todo) |
| `<nivel_verbosidad>` | Nivel de detalle de la captura (1-6) | ❌ No (por defecto:  3) |
| `<cantidad_paquetes>` | Número de paquetes a capturar | ❌ No (por defecto: ilimitado) |

---

## 📝 Procedimiento Paso a Paso

### 1️⃣ Sniffer Básico por Protocolo

Para capturar tráfico ICMP en una interfaz específica:

```bash
diagnose sniffer packet port4 icmp
```

**¿Qué hace?**
- Captura **solo tráfico ICMP** (pings) en la interfaz `port4`
- Usa nivel de verbosidad por defecto (3)
- Captura paquetes continuamente hasta que lo detengas con `Ctrl+C`

**Cuándo usarlo:**
- Verificar si los pings están llegando o saliendo por esa interfaz
- Troubleshootar problemas de conectividad básica

---

### 2️⃣ Filtros Específicos con Host Destino

Para capturar tráfico ICMP hacia un host específico:

```bash
diagnose sniffer packet port4 "icmp && dst host 8.8.8.8"
```

**¿Qué hace?**
- Captura **solo paquetes ICMP** que van hacia `8.8.8.8`
- El operador `&&` combina múltiples condiciones
- Los filtros complejos **deben ir entre comillas**

**Desglose del filtro:**
- `icmp` → Solo protocolo ICMP
- `&&` → Operador lógico AND (ambas condiciones deben cumplirse)
- `dst host 8.8.8.8` → Destino debe ser esta IP

> [!tip] Dirección del Tráfico
> - `src host <ip>` → Tráfico **desde** esa IP
> - `dst host <ip>` → Tráfico **hacia** esa IP
> - Sin especificar → En cualquier dirección

---

### 3️⃣ Filtrado por Puerto y Protocolo

Para capturar consultas DNS (UDP puerto 53):

```bash
diagnose sniffer packet port4 "udp && port 53 && dst host 8.8.8.8"
```

**¿Qué hace?**
- Captura solo tráfico **UDP**
- Específicamente en el **puerto 53** (DNS)
- Solo hacia el servidor DNS `8.8.8.8`

**Aplicaciones prácticas:**
- Verificar si las consultas DNS salen correctamente
- Troubleshootar resolución de nombres
- Confirmar que el tráfico usa el servidor DNS correcto

**Otros ejemplos de filtrado por puerto:**

```bash
# HTTPS hacia un servidor web
diagnose sniffer packet wan1 "tcp && port 443 && dst host 1.1.1.1"

# SSH desde una IP específica
diagnose sniffer packet port2 "tcp && port 22 && src host 192.168.1.100"

# Todo el tráfico desde/hacia una IP
diagnose sniffer packet any "host 10.0.0.50"
```

---

### 4️⃣ Niveles de Verbosidad

El **nivel de verbosidad** controla cuánta información se muestra de cada paquete capturado:

```bash
diagnose sniffer packet port4 "udp && port 53 && dst host 8.8.8.8" 4
```

**Niveles disponibles:**

| Nivel | Información Mostrada | Uso Recomendado |
|-------|---------------------|-----------------|
| **1** | Solo cantidad de paquetes capturados | Verificación rápida de presencia de tráfico |
| **2** | Headers de capa 2 (MAC addresses) | Troubleshooting de switching/ARP |
| **3** | Headers de capa 3 (IP) - **Default** | Uso general, balance entre detalle y legibilidad |
| **4** | Headers de capa 3 y 4 (IP + TCP/UDP) | Análisis de puertos y conexiones TCP |
| **5** | Headers + primeros 32 bytes de payload | Análisis de contenido de aplicación |
| **6** | Paquete completo en hexadecimal | Análisis profundo de protocolo |

> [!tip] Nivel Recomendado
> El **nivel 4** es ideal para la mayoría de troubleshooting, mostrando IPs, puertos, flags TCP, y tamaño de paquetes sin abrumar con información innecesaria.

**Ejemplo de salida en nivel 4:**

```
2026-01-13 10:45:23.456789 port4 in 192.168.1.100.12345 -> 8.8.8.8. 53:  udp 42
2026-01-13 10:45:23.478234 port4 out 8.8.8.8.53 -> 192.168.1.100.12345: udp 128
```

---

### 5️⃣ Limitar Cantidad de Paquetes

Para capturar un número específico de paquetes y detenerse automáticamente:

```bash
diagnose sniffer packet any "icmp" 4 10
```

**¿Qué hace?**
- Captura tráfico ICMP en **todas las interfaces** (`any`)
- Muestra con nivel de verbosidad **4**
- Se detiene automáticamente después de **10 paquetes**

**Ventajas:**
- Evita saturar la pantalla con capturas infinitas
- Útil para capturas rápidas y controladas
- Mejor para ambientes de producción

> [!warning] Captura en Todas las Interfaces
> Usar `any` como interfaz puede generar mucha información.  Limitá siempre la cantidad de paquetes cuando uses esta opción.

---

## 💡 Ejemplos Prácticos por Escenario

### Ejemplo 1: Troubleshooting de Ping

**Problema:** Un usuario no puede hacer ping a internet desde la LAN. 

```bash
# Paso 1: Verificar si el ping llega a la interfaz LAN
diagnose sniffer packet port1 "icmp && dst host 8.8.8.8" 4 20

# Paso 2: Verificar si sale por la interfaz WAN
diagnose sniffer packet wan1 "icmp && dst host 8.8.8.8" 4 20

# Paso 3: Verificar si vuelve la respuesta
diagnose sniffer packet wan1 "icmp && src host 8.8.8.8" 4 20
```

**Interpretación:**
- Si ves el ping en `port1` pero no en `wan1` → Problema de política o routing
- Si ves salir por `wan1` pero no vuelve → Problema de NAT o routing externo
- Si vuelve por `wan1` pero no llega a `port1` → Problema de política de retorno

---

### Ejemplo 2: Análisis de Tráfico DNS

**Problema:** Las consultas DNS parecen no resolverse.

```bash
# Capturar consultas DNS salientes y respuestas
diagnose sniffer packet any "udp && port 53" 4 50
```

**Qué buscar:**
- Consultas saliendo (queries) → Puerto origen alto, destino 53
- Respuestas entrando (responses) → Puerto origen 53, destino alto
- Si solo ves consultas pero no respuestas → Problema de conectividad con DNS

---

### Ejemplo 3: Verificar Tráfico HTTPS

**Problema:** Un sitio web específico no carga desde la red interna.

```bash
# Capturar intento de conexión HTTPS
diagnose sniffer packet any "tcp && port 443 && host 1.1.1.1" 4 100
```

**Qué buscar:**
- `SYN` → Cliente intenta conectar
- `SYN-ACK` → Servidor responde
- `ACK` → Handshake completo (conexión establecida)
- Si ves `SYN` pero no `SYN-ACK` → Problema de conectividad o bloqueo

---

### Ejemplo 4: Monitoreo de Tráfico desde IP Específica

**Problema:** Sospechas de actividad anómala desde un host interno.

```bash
# Capturar TODO el tráfico de una IP específica
diagnose sniffer packet any "host 192.168.1.150" 4 200
```

**Utilidad:**
- Ver todos los destinos contactados
- Identificar puertos y protocolos usados
- Detectar patrones de escaneo o exfiltración

---

### Ejemplo 5: Troubleshooting de VPN

**Problema:** Túnel VPN no establece correctamente.

```bash
# Capturar tráfico IKE (UDP 500) y ESP
diagnose sniffer packet wan1 "udp && port 500" 4 50

# Capturar tráfico ESP (protocolo 50)
diagnose sniffer packet wan1 "proto 50" 4 50
```

**Qué buscar:**
- IKE phase 1 negociation (puerto 500)
- ESP packets (tráfico encriptado del túnel)
- Si falta alguno → Identificar dónde falla la negociación

---

## 🔍 Operadores y Filtros Avanzados

### Operadores Lógicos

| Operador | Sintaxis | Ejemplo |
|----------|----------|---------|
| AND | `&&` o `and` | `tcp && port 80` |
| OR | `\|\|` o `or` | `port 80 \|\| port 443` |
| NOT | `!` o `not` | `!icmp` (todo excepto ICMP) |

### Filtros por Protocolo

```bash
# TCP
diagnose sniffer packet any "tcp"

# UDP  
diagnose sniffer packet any "udp"

# ICMP
diagnose sniffer packet any "icmp"

# Protocolo por número (ej: ESP = 50)
diagnose sniffer packet any "proto 50"
```

### Filtros por Puerto

```bash
# Puerto específico (cualquier dirección)
diagnose sniffer packet any "port 443"

# Puerto de origen
diagnose sniffer packet any "src port 1024"

# Puerto de destino
diagnose sniffer packet any "dst port 22"

# Rango de puertos
diagnose sniffer packet any "portrange 1000-2000"
```

### Filtros por Red

```bash
# Red completa (CIDR)
diagnose sniffer packet any "net 192.168.1.0/24"

# Red de origen
diagnose sniffer packet any "src net 10.0.0.0/8"

# Red de destino
diagnose sniffer packet any "dst net 172.16.0.0/16"
```

### Filtros por Flags TCP

```bash
# Solo paquetes SYN (inicio de conexión)
diagnose sniffer packet any "tcp[tcpflags] & tcp-syn != 0"

# Solo paquetes RST (reset de conexión)
diagnose sniffer packet any "tcp[tcpflags] & tcp-rst != 0"

# Solo paquetes FIN (cierre de conexión)
diagnose sniffer packet any "tcp[tcpflags] & tcp-fin != 0"
```

---

## 🎨 Combinaciones Útiles de Filtros

```bash
# HTTP y HTTPS desde una subred específica
diagnose sniffer packet any "src net 192.168.1.0/24 && (port 80 || port 443)" 4 100

# Todo excepto ICMP
diagnose sniffer packet any "!icmp" 4 50

# Tráfico entre dos hosts específicos
diagnose sniffer packet any "(host 192.168.1.10 && host 10.0.0.20)" 4 100

# SMTP, POP3 e IMAP (correo electrónico)
diagnose sniffer packet any "port 25 || port 110 || port 143" 4 50

# DNS sobre TCP (transferencias de zona)
diagnose sniffer packet any "tcp && port 53" 4 30
```

---

## ⚠️ Errores Comunes y Soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| "Parse error" en el filtro | Sintaxis incorrecta del filtro BPF | Verificá que los filtros complejos estén entre comillas |
| Captura vacía (no muestra paquetes) | Filtro demasiado restrictivo o interfaz incorrecta | Usá filtros más amplios primero (`any`) y ajustá |
| Demasiados paquetes, no se puede leer | No limitaste cantidad o usaste `any` sin filtro | Agregá límite de paquetes y filtros específicos |
| No se detiene la captura | No especificaste cantidad de paquetes | Usá `Ctrl+C` para detener manualmente |
| FortiGate se pone lento durante captura | Sniffer consume recursos del CPU | Usá filtros específicos, nivel de verbosidad bajo, y captura corta |

> [!example] Corrección de Errores
> ```bash
> # ❌ Incorrecto - Falta comillas
> diagnose sniffer packet port1 tcp && port 80
> 
> # ✅ Correcto - Con comillas
> diagnose sniffer packet port1 "tcp && port 80"
> ```

---

## 🔍 Verificación e Interpretación

### Detener la Captura

```bash
# Presiona Ctrl+C para detener la captura
^C
0 packets received by filter
0 packets dropped by kernel
```

### Interpretar la Salida (Nivel 4)

```
2026-01-13 10:45:23.456789 port4 in 192.168.1.100.54321 -> 8.8.8.8.53: udp 42
```

**Desglose:**
- `2026-01-13 10:45:23.456789` → Timestamp exacto
- `port4` → Interfaz donde se capturó
- `in` → Dirección (in = entrante, out = saliente)
- `192.168.1.100.54321` → IP origen y puerto origen
- `8.8.8.8.53` → IP destino y puerto destino
- `udp` → Protocolo
- `42` → Tamaño del paquete en bytes

---

## 📌 Checklist de Troubleshooting

Cuando uses el sniffer para diagnosticar problemas, seguí este orden:

- [ ] Identificá la interfaz por donde debería pasar el tráfico
- [ ] Capturá en la interfaz de entrada (LAN)
- [ ] Capturá en la interfaz de salida (WAN)
- [ ] Verificá si hay NAT aplicado
- [ ] Buscá paquetes de retorno
- [ ] Comprobá flags TCP (SYN, ACK, RST)
- [ ] Revisá políticas de firewall que puedan estar bloqueando

---

## 🎓 Conclusión

El sniffer integrado de FortiGate es una herramienta esencial para: 

✅ **Diagnosticar conectividad** - Ver exactamente qué tráfico pasa por el firewall  
✅ **Verificar políticas** - Confirmar que el tráfico fluye como esperás  
✅ **Troubleshootar servicios** - Analizar DNS, VPN, y otros protocolos  
✅ **Detectar problemas de NAT** - Verificar si las traducciones son correctas  
✅ **Análisis de seguridad** - Identificar tráfico anómalo o ataques

Con los niveles de verbosidad, filtros específicos, y límites de captura, podés realizar diagnósticos precisos sin impactar la performance del firewall.

> [!tip] Mejores Prácticas
> 1. Siempre usá **filtros específicos** en producción
> 2. Limitá la **cantidad de paquetes** capturados
> 3. Usá **nivel 4** para la mayoría de casos
> 4. Capturá en **interfaz específica** cuando sea posible
> 5. Documentá los resultados para análisis posterior

---

## 📋 Referencia Rápida

```bash
# Sintaxis completa
diagnose sniffer packet <interfaz> "<filtros>" <nivel> <cantidad>

# Ejemplos rápidos
diagnose sniffer packet any "icmp" 4 10                    # ICMP básico
diagnose sniffer packet port1 "tcp && port 443" 4 50      # HTTPS
diagnose sniffer packet wan1 "udp && port 53" 4 30        # DNS
diagnose sniffer packet any "host 192.168.1.100" 4 100    # Todo de una IP

# Detener captura
Ctrl+C
```

---

**Etiquetas:** #fortinet #fortigate #sniffer #troubleshooting #network #debug #tcpdump #diagnostico