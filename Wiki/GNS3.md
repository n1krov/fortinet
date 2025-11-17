---
Tema: "[[Wiki/wiki|wiki]]"
---

## 🌐 GNS3: Graphical Network Simulator-3

> [!summary] Definición Breve
> 
> GNS3 (Graphical Network Simulator-3) es un software de virtualización de redes que permite a los usuarios diseñar, construir, configurar y probar redes complejas en un entorno virtual libre de riesgo. Utiliza la virtualización real de dispositivos, permitiendo la integración de sistemas operativos de hardware de red genuino (como Cisco IOS, FortiOS, Juniper JunOS, etc.) en máquinas virtuales (VMs) o containers.

---

### 1. Historia y Concepto Central 💡

GNS3 fue creado inicialmente en 2005 por Jeremy Grossmann. Nació como una herramienta para emular _routers_ Cisco utilizando el _software_ **Dynamips**. Con el tiempo, ha evolucionado para convertirse en una plataforma de simulación híbrida capaz de interactuar con casi cualquier dispositivo virtualizable.

- **Emulación vs. Simulación:**
    
    - **Emulación:** GNS3 puede _emular_ hardware específico (como los _routers_ antiguos de Cisco 3700 series) usando Dynamips, lo que implica recrear el funcionamiento interno del _hardware_ para ejecutar su _software_ real.
        
    - **Virtualización:** El enfoque moderno de GNS3 es la _virtualización_, permitiendo integrar imágenes de sistemas operativos (OS) reales de _vendors_ como Cisco (mediante **VIRL/CML**), Fortinet, Palo Alto, o servidores Linux/Windows como **Máquinas Virtuales (VM)** dentro de la topología.
        

---

### 2. Arquitectura y Componentes Clave 🏗️

GNS3 opera con una arquitectura distribuida que permite ejecutar simulaciones tanto localmente como en servidores remotos.

#### Componentes Principales

| **Componente**                   | **Descripción**                                                                                                                | **Función Principal**                                               |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| **GNS3 GUI (Cliente)**           | La interfaz gráfica que el usuario utiliza para diseñar la topología, arrastrar dispositivos y configurar enlaces.             | Diseño, visualización y gestión de la simulación.                   |
| **GNS3 Server (Backend)**        | El motor de procesamiento que gestiona la ejecución de los dispositivos virtuales (VMs, _containers_, o emulaciones Dynamips). | Ejecución del _software_ de red y manejo de los enlaces.            |
| **Dynamips**                     | Herramienta de emulación de _routers_ Cisco antiguos.                                                                          | Emular el _hardware_ para ejecutar imágenes de Cisco IOS.           |
| **Integración con Hipervisores** | Conexión con _software_ de virtualización como **VMware Workstation**, **Oracle VirtualBox** o **Docker**.                     | Alojar los dispositivos virtuales (ej. FortiGate VM, Linux Server). |

---

### 3. Aplicaciones y Contextos de Uso 🛠️

GNS3 es una herramienta indispensable para profesionales y estudiantes en el campo de las redes y la ciberseguridad.

#### 3.1. Redes y Certificaciones

- **Práctica de Configuración:** Permite practicar configuraciones de protocolos avanzados como BGP, OSPF, EIGRP, MPLS sin necesidad de _hardware_ físico.
    
- **Preparación para Certificaciones:** Es crucial para estudiar para certificaciones de _vendors_ como **CCNA**, **CCNP**, **JNCIE** o certificaciones de _firewalls_ (ej. **NSE de Fortinet**).
    

#### 3.2. Ciberseguridad y Hacking Ético

- **Laboratorios de Seguridad:** Permite construir un entorno de **Blue Team/Red Team** con servidores, _firewalls_ (como **FortiGate VM** o **Palo Alto VM**), y máquinas de ataque (ej. **Kali Linux**).
    
- **Análisis de _Malware_:** Se puede crear una red aislada (_sandbox_) para detonar y analizar _malware_ de forma segura, observando su comportamiento.
    
- **Prueba de Políticas de Acceso:** Permite simular y probar reglas de _firewall_ o políticas de acceso antes de implementarlas en una red de producción.
    

---

### 4. Ventajas y Desventajas (Pros y Contras) ✅❌

| **Ventaja (Pro)**                                                                                             | **Desventaja (Con)**                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Realismo Alto**                                                                                             | **Demanda de Recursos**                                                                                                                      |
| Ejecuta sistemas operativos de red reales (IOS, FortiOS, JunOS) en lugar de solo simular su comportamiento.   | Requiere una gran cantidad de memoria RAM y CPU, especialmente al ejecutar múltiples VMs reales.                                             |
| **Costo-Efectividad**                                                                                         | **Curva de Aprendizaje**                                                                                                                     |
| Permite crear laboratorios complejos sin la inversión de miles de dólares en _hardware_ físico.               | La configuración inicial, especialmente la integración con hipervisores y la importación de imágenes, puede ser compleja para principiantes. |
| **Portabilidad**                                                                                              | **Legalidad de Imágenes**                                                                                                                    |
| Un laboratorio completo puede ser guardado en un solo archivo de proyecto y movido entre diferentes máquinas. | El usuario es responsable de obtener legalmente las imágenes de los sistemas operativos (ej. Cisco IOS) que utiliza. GNS3 no las incluye.    |

---

### 5. Taxonomía de Dispositivos Virtuales 🌲

GNS3 es una plataforma abierta que soporta diversas tecnologías de virtualización.

> [!quote] Nota Histórica
> GNS3 es a menudo comparado con Packet Tracer (herramienta de simulación de Cisco). La principal diferencia radica en que GNS3 utiliza sistemas operativos de red reales para la mayoría de sus nodos, lo que ofrece un comportamiento mucho más fiel a la realidad.


### Instalacion - Debian-based distributions

GNS3 is not available through apt; you will have to use `pipx`.

Refresh apt:

```
sudo apt update
```

Install python and the required emulation & gui packages:

```
sudo apt install python3 python3-pip pipx python3-pyqt5 python3-pyqt5.qtwebsockets python3-pyqt5.qtsvg qemu-kvm qemu-utils libvirt-clients libvirt-daemon-system virtinst dynamips software-properties-common ca-certificates curl gnupg2 
```

Use pipx to install gns3:

```
pipx install gns3-serverpipx install gns3-gui
```

To launch the GUI, we will need to prepare the virtual environment. Inject the GNS server and QT elements:

```
pipx inject gns3-gui gns3-server PyQt5
```

Finally, launch with `gns3`.