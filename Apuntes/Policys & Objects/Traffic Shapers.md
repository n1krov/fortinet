---
Tema: "[[policys&objects]]"
---

Esta imagen explica el concepto de **Traffic Shapers** (Modeladores de Tráfico) en FortiGate. Su función principal es controlar el consumo de ancho de banda para asegurar que las aplicaciones críticas tengan prioridad y que ningún usuario o proceso sature la conexión.

## **Traffic Shapers en FortiGate**

El **Traffic Shaping** permite configurar el **Rate Limiting** (limitación de tasa) tanto para el tráfico de subida como de bajada. Se definen dos valores clave:

- **Guaranteed Bandwidth:** El ancho de banda mínimo que el FortiGate reserva para ese tráfico.
    
- **Maximum Bandwidth:** El límite máximo que ese tráfico no puede superar, incluso si hay recursos sobrantes.
    

Existen dos formas principales de aplicar estas reglas, como se ve en tu imagen:

### **1. Shared Traffic Shaper (Compartido)**

- **Funcionamiento:** El ancho de banda definido se reparte entre **todos** los usuarios o dispositivos que activen la política.
    
- **Ejemplo:** Si asignas 100 Mbps a la categoría "Streaming", y hay 10 personas viendo videos, los 100 Mbps se dividen entre todos ellos.
    

### **2. Per-IP Traffic Shaper (Por IP)**

- **Funcionamiento:** El límite de ancho de banda se aplica de forma **individual** a cada dirección IP.
    
- **Ejemplo:** Si pones un máximo de 10 Mbps por IP, cada usuario podrá consumir hasta 10 Mbps individualmente, sin importar cuántos otros usuarios estén conectados.
    

### **¿Dónde se configura?**

Como indica la barra azul en tu captura, esto se gestiona en:

> **Policies & Objects > Traffic Shaping**

**Dato útil para tus laboratorios:**

Normalmente, usarás un **Shared Shaper** para servicios globales (como actualizaciones de Windows) y un **Per-IP Shaper** para evitar que un solo usuario acapare todo el ancho de banda con descargas pesadas.



---

como se aplica

en 

> **Policies & Objects > Traffic Shaping**

estan las pestañas | traffic shapers | traffic shaping policies | traffic shaping profiles |


crear traffic shaper