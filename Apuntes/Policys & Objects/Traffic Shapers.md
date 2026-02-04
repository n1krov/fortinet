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


### crear traffic shaper (shared o compartido)

en el menu traffic shapers > aqui se crea el **perfil** aqui se establece el trafico garantizado, el maximoe etc para despues aplicarle politicas

![[Captura de pantalla_20260204_083903.png]]

### Creacion de Traffic shaping POLICY


![[Captura de pantalla_20260204_084607.png]]

ahi explico la imagen segun su item

| **Ítem** | **Campo**                      | **Descripción Técnica**                                                                                                                                               |
| -------- | ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1**    | **If Traffic Matches**         | **Sección de Clasificación:** Define las condiciones que debe cumplir el paquete para que se le aplique el modelado de tráfico.                                       |
| **2**    | **Then**                       | **Sección de Acción:** Define qué límites de ancho de banda y en qué interfaz se aplicarán las restricciones una vez que el tráfico coincide.                         |
| **3**    | **Source**                     | **Origen del tráfico:** Direcciones IP, rangos o subredes de donde proviene la conexión (ej. tu red local LAN).                                                       |
| **4**    | **Destination**                | **Destino del tráfico:** Direcciones IP o rangos hacia donde se dirige el tráfico (ej. servidores externos o `all` para internet).                                    |
| **5**    | **Service**                    | **Protocolo/Puerto:** Filtra por servicios específicos como TCP/443 (HTTPS), UDP/53 (DNS), etc.                                                                       |
| **6**    | **Application / URL Category** | **Capa 7 (Aplicación):** Permite ser específico y limitar aplicaciones concretas (ej. Facebook, Spotify) o categorías (ej. Streaming), independientemente del puerto. |
| **7**    | **Outgoing Interface**         | **Interfaz de Salida:** El punto donde se aplica el "shaping". El tráfico se limita justo antes de salir del FortiGate hacia el destino (ej. SD-WAN).                 |
| **8**    | **Apply Shaper**               | **Activador de Modelado:** Habilita el uso de perfiles de limitación de ancho de banda definidos previamente.                                                         |
| **9**    | **Shared Shaper**              | **Limitador Compartido:** Aplica el perfil seleccionado al **total** de la suma de todo el tráfico que haga match con esta política.                                  |
| **10**   | **Reverse Shaper**             | **Shaper Inverso:** Aplica el límite de ancho de banda al tráfico de **bajada** (download). Si no se activa, solo se limita la subida.                                |
| **11**   | **Per-IP Shaper**              | **Limitador por IP:** Si se activa, el límite definido se aplica de forma individual a cada dirección IP de origen, evitando que un solo usuario sature el canal.     |
| **12**   | **Assign shaping class ID**    | **Clasificación de Prioridad:** Etiqueta el tráfico con un ID para que el FortiGate sepa qué tráfico es prioritario sobre otro en caso de congestión severa.          |


para aplicar a la descarga usas el reverse shaper aplicando el perfil de **traffic shaper**
para la subida el **shared shaper**

