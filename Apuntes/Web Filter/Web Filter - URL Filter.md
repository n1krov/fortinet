---
Tema: "[[apuntes]]"
---

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
    

---

### **Resumen para tu apunte: Jerarquía de Inspección Web**

En un perfil de Web Filter, el FortiGate procesa las reglas en este orden:

1. **URL Filter (Estático)**: Si la URL está en tu lista manual, se aplica la acción y se detiene el proceso.
    
2. **FortiGuard Categories**: Si no hay regla manual, consulta la categoría en la nube.
    

