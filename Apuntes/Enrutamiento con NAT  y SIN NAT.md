---
Tema: "[[apuntes]]"
---

Entender la distinción entre el enrutamiento que incorpora

**NAT (Traducción de Direcciones de Red)** y el que no lo hace es fundamental para comprender cómo se estructuran las redes modernas, especialmente en un mundo donde las direcciones IPv4 son un recurso limitado. El enrutamiento básico se encarga simplemente de decidir qué camino debe seguir un paquete de datos para llegar a su destino, basándose en tablas de rutas. Sin embargo, cuando añadimos NAT a esta ecuación, introducimos una capa de transformación que altera las identidades de los dispositivos involucrados en la comunicación. 

A continuación, se detallan las diferencias estructurales y funcionales entre ambos métodos:

1. Enrutamiento con NAT (Network Address Translation)

El enrutamiento con NAT es el estándar en entornos domésticos y empresariales donde se necesita conectar múltiples dispositivos internos a la red pública (Internet) utilizando una sola dirección IP pública. 

- **Traducción de Direcciones:** El router "traduce" las direcciones IP privadas (como 192.168.1.x) de los dispositivos locales a una dirección IP pública única asignada por el proveedor de servicios (ISP).
- **Visibilidad y Seguridad:** Los dispositivos internos permanecen ocultos para el mundo exterior. Un atacante en Internet solo ve la IP pública del router y no puede iniciar una conexión directa con una computadora interna a menos que haya una regla previa (como el redireccionamiento de puertos).
- **Conservación de IPs:** Es la herramienta principal para combatir el agotamiento de direcciones IPv4, ya que permite que miles de hosts compartan una sola dirección global.
- **Impacto en el Rendimiento:** NAT introduce una ligera carga de procesamiento adicional, ya que el router debe reescribir los encabezados de cada paquete y mantener una tabla de traducción en tiempo real. 

2. Enrutamiento sin NAT (Enrutamiento "Puro")

En este modo, el dispositivo actúa como un enrutador convencional. Se utiliza principalmente en el núcleo de las redes de los proveedores de servicios o entre subredes internas de una organización de gran escala. 

- **Transparencia de Extremo a Extremo:** No hay cambios en las direcciones IP de origen o destino. El paquete mantiene su identidad original desde que sale del emisor hasta que llega al receptor.
- **Conectividad Bidireccional:** A diferencia de NAT, donde el tráfico suele ser unidireccional (de adentro hacia afuera), en el enrutamiento puro los clientes de diferentes redes pueden comunicarse libremente entre sí en ambas direcciones, siempre que las tablas de rutas lo permitan.
- **Requerimientos de IP:** Cada dispositivo que desee salir a Internet debe poseer una dirección IP pública única y válida a nivel mundial. Si se usa internamente, cada subred debe estar perfectamente definida en las tablas de rutas.
- **Rendimiento Óptimo:** Al no tener que modificar los paquetes, el retardo es mínimo, lo que resulta ideal para protocolos sensibles al tiempo como la telefonía IP (VoIP) o servicios de alta velocidad. 

Comparativa de Casos de Uso


| Caracteristica        | Con Nat                                                           | Sin NAT (Puro)                                                             |
| --------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Uso común**         | Hogares, oficinas, redes Wi-Fi públicas.                          | Core de ISPs, redes de datos backbone, interconexión de VLANs.             |
| **Identidad IP**      | Se ocultan las IPs privadas detrás de una IP pública.             | Las IPs son visibles y consistentes en todo el trayecto.                   |
| **Seguridad inicial** | Proporciona una barrera natural contra conexiones no solicitadas. | Requiere firewalls explícitos para proteger cada host expuesto.            |
| **Complejidad**       | Mayor carga de CPU en el router por la traducción.                | Mayor simplicidad de procesamiento, pero requiere gestión estricta de IPs. |

En resumen, mientras que el enrutamiento con NAT es una solución de "parche" y seguridad que permite la coexistencia de millones de dispositivos bajo pocas IPs públicas, el enrutamiento sin NAT representa el modelo ideal de Internet (extremo a extremo), donde cada nodo es alcanzable de forma directa, algo que se busca estandarizar con la adopción total de **IPv6**, donde el uso de NAT se vuelve innecesario debido a la abundancia de direcciones disponibles.