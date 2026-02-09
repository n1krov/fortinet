---
Tema: "[[apuntes]]"
---
siguiendo con las opciones de web filter aqui empieza la parte de URL filter

![[Captura de pantalla_20260209_114124.png]]

antes presentar como trabaja el url filter

![[Captura de pantalla_20260209_114952.png]]

cuando viene una URL antes de mostrar la pagina pasa por 3 importantes filtros. el primero es por el `static URL filter`, si esta en allowed/monitor pasa a ver los `filtros de categorias de fortiguard` y luego si esta permitido procede a los `filtros avanzados`.
## **Static URL Filter: Control Manual y Granular**

Esta capa de seguridad permite definir reglas "en blanco y negro" para dominios o URLs exactas, independientemente de la categoría a la que pertenezcan.

### **1. Opciones de Bloqueo Estático**

- **Block invalid URLs**: Bloquea solicitudes que no cumplen con el formato estándar de una URL (RFC), evitando ataques que intentan confundir al motor de inspección.
- **URL Filter**: Es el interruptor principal para activar tu lista negra o blanca personalizada de dominios.
- **Block malicious URLs discovered by FortiSandbox**: Esta es una integración de inteligencia. Si tu **FortiSandbox** analiza un archivo y descubre que intenta comunicarse con una URL nueva y peligrosa, el FortiGate bloqueará esa URL automáticamente aquí.
- **Content Filter**: Permite bloquear páginas basándose en **palabras clave** que aparezcan dentro del texto del sitio web (ej. bloquear sitios que contengan la palabra "apuestas" aunque la URL sea desconocida).

### **2. Funcionamiento del URL Filter (Lista Manual)**

Cuando activas el **URL Filter**, puedes añadir entradas con tres tipos de coincidencia:

1. **Simple**: Coincidencia exacta del dominio (ej. `www.facebook.com`).
2. **Wildcard**: Uso de comodines para cubrir subdominios (ej. `*.google.com`).
3. **Regular Expression (Regex)**: Para patrones complejos de URLs.
### **3. Acciones Disponibles**

Para cada entrada manual, puedes elegir:

- **Exempt**: El sitio salta todas las demás inspecciones (AV, IPS) para maximizar la velocidad (usar solo en sitios de extrema confianza).
- **Allow**: Permite el acceso pero sigue inspeccionando el contenido.
- **Block**: El usuario ve la página de reemplazo de Fortinet.
- **Monitor**: Registra el acceso en los logs sin interrumpir al usuario.


### **Resumen para tu apunte: Jerarquía de Inspección Web**

En un perfil de Web Filter, el FortiGate procesa las reglas en este orden:

1. **URL Filter (Estático)**: Si la URL está en tu lista manual, se aplica la acción y se detiene el proceso.
    
2. **FortiGuard Categories**: Si no hay regla manual, consulta la categoría en la nube.
    



![[Captura de pantalla_20260209_114510.png]]

### **Static URL Filter: Control Manual Directo**

El filtrado estático te permite crear una "lista blanca" o "lista negra" personalizada. Es la primera capa que revisa el FortiGate antes de consultar las categorías en la nube.

#### **1. Opciones de Configuración Principal**

- **Block invalid URLs:** Bloquea peticiones HTTP mal formadas que no cumplen los estándares, lo que ayuda a prevenir intentos de evasión del firewall.
    
- **URL Filter:** El interruptor que activa tu lista personalizada de dominios.
    
- **Block malicious URLs discovered by Fortisandbox:** Bloquea automáticamente URLs que el Sandbox ha identificado como peligrosas tras analizar archivos maliciosos.
    

#### **2. Tipos de Coincidencia (Type)**

Cuando creas una nueva entrada (como se ve con `*.udemy.com`), defines cómo el equipo debe leer la dirección:

- **Simple:** Coincidencia exacta del texto ingresado.
    
- **Wildcard:** Usa comodines (como el `*`) para incluir subdominios o variaciones.
    
- **Regular Expression (Regex):** Para patrones de búsqueda complejos y avanzados.
    

#### **3. Acciones Disponibles (Action)**

- **Exempt (Eximir):** Es la más potente. El tráfico **salta todas las demás inspecciones** (Antivirus, IPS, Web Filter). Se usa para sitios de absoluta confianza donde la inspección causa errores (ej. sitios de bancos o actualizaciones de Windows).
- **Block:** El sitio se corta inmediatamente y se muestra la página de reemplazo.
- **Allow:** Permite el acceso al sitio, pero el tráfico **sigue siendo escaneado** por el Antivirus y el IPS.    
- **Monitor:** Solo registra la visita en los logs sin aplicar ninguna restricción.

### **Resumen para tu apunte: La Regla de Oro**

> **Prioridad:** El **Static URL Filter** siempre tiene prioridad sobre el **Category Filter**. Si bloqueas "Educación" en las categorías pero pones `udemy.*` como **Exempt** aquí, el usuario podrá entrar a Udemy sin problemas.



