---
Tema: "[[Wiki/wiki|wiki]]"
---

## 📜 Apunte de Herramientas de Ciberseguridad 🛡️

# PuTTY: El Cliente Esencial de SSH/Telnet

> [!info] ¡Bienvenido a tu apunte de FORTINET!
> 
> Aunque PuTTY no es una herramienta de Fortinet, es un cliente fundamental que cualquier administrador o profesional de ciberseguridad (incluyendo aquellos que gestionan equipos Fortinet como FortiGate, FortiAnalyzer, etc.) utiliza diariamente para acceder y configurar sus dispositivos de forma segura. Es el puente de consola/terminal más común.

---

## 1. Introducción Breve y Fácil de Entender 💻

### ¿Qué es PuTTY?

**PuTTY** es un **cliente libre y de código abierto** para los protocolos de red SSH (Secure Shell), Telnet, rlogin, y conexión de _serial port_ (puerto serial). Su función principal es permitir la conexión a un servidor remoto, máquina o dispositivo de red a través de una ventana de terminal. Es el cliente de facto para la mayoría de los usuarios de Windows que necesitan acceder a sistemas basados en Unix/Linux o a consolas de administración de dispositivos de red.

### ¿Para qué Sirve?

- **Administración Remota Segura (SSH):** Permite acceder de forma cifrada a la línea de comandos (CLI) de servidores (Linux/Unix), _routers_, _switches_ y _firewalls_ (como FortiGate).
    
- **Transferencia de Archivos (SCP/SFTP):** Aunque PuTTY es el cliente de terminal, su paquete incluye herramientas como **`pscp`** y **`psftp`** para transferir archivos de forma segura.
    
- **Conexión a Consola (Serial):** Es vital para la configuración inicial o la recuperación de dispositivos de red cuando no hay acceso a la red (conectando un cable de consola directo al puerto serial del equipo).
    

### Contextos de Uso 🌐

1. **Administración de _Firewalls_ Fortinet:** Acceder a la CLI de un FortiGate para configuraciones avanzadas, _troubleshooting_ o ejecutar comandos específicos (`show`, `config system`, `diagnose`).
    
2. **Hacking Ético/Pentesting:** Para conectarse a _targets_ o _jump boxes_ (máquinas intermediarias) una vez que se han obtenido credenciales SSH o Telnet.
    
3. **Administración de Servidores:** Conexión habitual a servidores Linux remotos para tareas de mantenimiento, despliegue de _software_ o verificación de logs.
    

---

## 2. Guía Práctica Paso a Paso (Modo Manual de Uso) ⚙️

### 2.1. Sintaxis Básica (Interfaz Gráfica)

PuTTY se usa principalmente a través de su **interfaz gráfica (GUI)**. El flujo de trabajo básico es:

1. **Abrir PuTTY.**
    
2. En el campo **"Host Name (or IP address)"**, ingresar la dirección IP o el nombre de _host_ del dispositivo remoto (ej. `192.168.1.99`).
    
3. Seleccionar el **"Connection type"** (generalmente **SSH** o **Serial** para consolas de FortiGate).
    
4. (Opcional) Guardar la sesión en **"Saved Sessions"**.
    
5. Hacer clic en **"Open"**.
    

### 2.2. Parámetros y Opciones Comunes

Aunque la mayoría de los parámetros se configuran en la GUI, estos son los ajustes clave que siempre se deben verificar:

|**Categoría**|**Opción Clave**|**Uso/Propósito**|
|---|---|---|
|**Session**|**Connection Type**|**SSH** (por defecto para seguridad), Telnet, Rlogin, o **Serial** (para cables de consola).|
|**Connection**|**Port**|El puerto de conexión. **22** para SSH, **23** para Telnet.|
|**Connection**|**Seconds between keepalives**|Previene desconexiones por inactividad. Es útil para sesiones largas en equipos de red.|
|**Window**|**Lines of scrollback**|Aumentar para revisar historial de comandos (ej. 10000).|
|**Connection > SSH**|**Auth > Private key file for authentication**|Para usar autenticación sin contraseña (clave privada RSA/DSA).|

> [!tip] Optimización de Interfaz
> 
> Ve a Window > Appearance y cambia la fuente y el tamaño para mejorar la legibilidad. En Window > Colours, puedes configurar un esquema de colores más agradable (ej. negro sobre verde/blanco).

### 2.3. Casos de Uso Típicos

|**Escenario**|**Protocolo**|**Configuración Clave**|
|---|---|---|
|**Acceso a un FortiGate remoto**|SSH|IP del FortiGate, Puerto 22.|
|**Acceso a un servidor antiguo**|Telnet|IP del servidor, Puerto 23 (¡Desaconsejado por inseguro!).|
|**Configuración inicial o _recovery_ de un FortiGate (Consola)**|Serial|**Serial Line:** COM1 (o el puerto correcto), **Speed:** 9600, **Data bits:** 8, **Stop bits:** 1, **Parity:** None, **Flow control:** None.|

---

## 3. Ejemplos Prácticos 🚀

PuTTY es una herramienta GUI, por lo que los comandos se ejecutan _dentro_ de la ventana de terminal que abre. No obstante, aquí se muestra el uso de sus herramientas auxiliares que se ejecutan desde la línea de comandos (CLI) de Windows.

### 3.1. Ejemplo 1: Conexión SSH a un FortiGate (Concepto)

Imagina que ya has abierto PuTTY y has introducido la IP. Una vez conectado, verás el _prompt_ de FortiGate.

Bash

```
# Comandos a ejecutar DENTRO de la sesión PuTTY (CLI de FortiGate)

# Muestra el estado del sistema (útil para diagnóstico)
diagnose sys session status

# Entra al modo de configuración para modificar la hora (ejemplo)
config system global
set timezone 22 # Código de zona horaria
end

# Comprueba la configuración de las interfaces
show system interface
```

> [!example] ¡Uso Práctico!
> 
> Si tienes un FortiGate en la IP 192.168.5.1, configurarías: Host Name: 192.168.5.1, Port: 22, Connection type: SSH.

### 3.2. Ejemplo 2: Transferencia Segura de un _Firmware_ con `pscp`

**`pscp`** (PuTTY Secure Copy) se utiliza para copiar archivos de forma segura. Es parte del paquete PuTTY, pero se ejecuta desde el **Símbolo del Sistema (CMD) de Windows**.

**Objetivo:** Subir un archivo de _firmware_ (`FGT_firmware.out`) al FortiGate o a un servidor remoto.

Bash

```bash
# Sintaxis: pscp [opciones] [origen] [usuario@destino:ruta]

# Copiar el archivo local 'FGT_firmware.out' al directorio /var del FortiGate (192.168.5.1)
pscp C:\Users\Admin\Desktop\FGT_firmware.out admin@192.168.5.1:/var/ftproot/
```

**Explicación:**

- `pscp`: El comando para transferencia segura.
    
- `C:\Users\...`: La **ruta del archivo** en la máquina local.
    
- `admin@192.168.5.1`: El **usuario** y la **dirección IP** del FortiGate/servidor.
    
- `:/var/ftproot/`: La **ruta de destino** en el FortiGate.
    
- _El comando pedirá la contraseña del usuario `admin` después de ejecutarse._
    

### 3.3. Ejemplo 3: Conexión _Quick Launch_ (Línea de Comandos)

Se puede iniciar PuTTY directamente desde la línea de comandos de Windows sin pasar por la GUI para una conexión rápida:

Bash

```
# Sintaxis: putty.exe [-ssh|-telnet|-rlogin] [user@]host [-P port]

# Conexión SSH rápida al servidor 10.10.10.1 como usuario 'sysadmin'
putty.exe -ssh sysadmin@10.10.10.1

# Conexión a un FortiGate con un puerto SSH no estándar (ej. 2222)
putty.exe -ssh admin@192.168.5.1 -P 2222
```

---

## 4. Tips y Buenas Prácticas 💡

### 4.1. Consejos para Optimizar su Uso

- **Guardar Sesiones:** Siempre guarda tus sesiones (ej. `FortiGate-Oficina-SSH` o `Server-Prod-Jumphost`) con la IP, el puerto y el protocolo ya configurados para ahorrar tiempo.
    
- **Archivos de Clave:** En entornos de alta seguridad, usa claves SSH en lugar de contraseñas. Puedes usar **PuTTYgen** (otra herramienta del paquete) para crear pares de claves y cargar la clave privada en la configuración de `Connection > SSH > Auth`.
    
- **Habilitar _Keepalives_:** Para evitar que la sesión se cierre por inactividad (especialmente cuando lees logs grandes o estás en una llamada), ve a `Connection` y establece **`Seconds between keepalives`** a un valor bajo (ej. 30).
    

### 4.2. Posibles Errores Comunes y Cómo Evitarlos 🛑

|**Error Común**|**Causa**|**Solución**|
|---|---|---|
|**"Network error: Connection refused"**|El servidor no tiene el servicio SSH/Telnet activo o un _firewall_ está bloqueando el puerto.|Verificar que el servicio está activo y que las reglas del _firewall_ (como la _Policy_ en FortiGate) permiten el acceso al puerto (generalmente 22).|
|**"Network error: Software caused connection abort"**|Conexión inestable o la sesión fue cerrada por el servidor remoto (ej. por _timeout_ de inactividad).|Aumentar el valor de **`Seconds between keepalives`** en la configuración de la sesión PuTTY.|
|**"Access denied"**|Credenciales de usuario/contraseña incorrectas o clave SSH no configurada.|Verificar las credenciales, asegurarse de que la tecla `CAPS LOCK` esté desactivada, o verificar la ruta al archivo de clave privada (`.ppk`).|

---

## 5. Diagrama de Flujo de Conexión Segura (Mermaid) 🧠

Este diagrama ilustra el proceso de conexión a un dispositivo de red (como un FortiGate) utilizando PuTTY.

Fragmento de código

```
graph TD
    A[Usuario Abre PuTTY] --> B{Configura Parámetros};
    B --> C[Introduce IP y Puerto (Ej: 192.168.5.1:22)];
    C --> D(Selecciona Protocolo SSH);
    D --> E[Clic en 'Open'];
    E --> F{Establecimiento de Conexión};
    F -- Exitoso --> G[Prompt de CLI - Pide Credenciales];
    G --> H[Acceso a la Consola de FortiGate];
    F -- Fallido --> I[Error: Connection Refused/Timeout];
    H --> J[Ejecución de Comandos Diagnósticos/Configuración];
```