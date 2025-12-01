---
Tema: "[[HTTP Header Security]]"
---
## 🛡️ ¿Qué es `Permissions-Policy`?

Esta cabecera controla a qué **características del navegador** (APIs) tiene permiso de acceder tu página web y cualquier `iframe` que cargue. Esto incluye la cámara, el micrófono, la geolocalización, la pantalla completa, etc.

- **Propósito:** Prevenir que _scripts_ de terceros (o código malicioso) utilicen funciones sensibles sin permiso.
    

## 📝 Configuración Sencilla y 

Esta cabecera es crucial para la seguridad moderna. Le permite al sitio controlar qué características y APIs del navegador pueden usar la página actual y los _iframes_ que esta contiene. Si la dejas vacía (o deshabilitas todo), evitas que contenido inyectado o malicioso acceda a la cámara, micrófono, geolocalización, etc.

￼### **Valor Recomendado (Restrictivo)**

Para maximizar la seguridad si tu sitio no necesita características del navegador, se recomienda deshabilitar todas ellas con un valor vacío:

- **Valor:** Un par de paréntesis vacíos para las directivas que quieras bloquear, por ejemplo, **`geolocation=(), camera=(), microphone=()`**.
    
- **Nota:** Si tu sitio **necesita** la cámara (por ejemplo, para escanear QR), tendrías que usar `camera=(self)`. Asumiremos la configuración más restrictiva para empezar.Segura

La manera más sencilla y segura de configurar esto es **deshabilitar todas las características** que tu sitio **no** necesita. Si tu sitio solo muestra información y no necesita la cámara ni la geolocalización, puedes bloquearlas por defecto.

### **Valor Recomendado (Muy Restrictivo)**

Para la mayoría de los sitios informativos o de administración, se recomienda bloquear las funciones sensibles:

- **Header Value:** 

```txt
geolocation=(), camera=(), microphone=(), fullscreen=(self)`
```

| **Directiva**       | **Significado**                                                        |
| ------------------- | ---------------------------------------------------------------------- |
| `geolocation=()`    | Bloquea el acceso a la ubicación geográfica.                           |
| `camera=()`         | Bloquea el acceso a la cámara.                                         |
| `microphone=()`     | Bloquea el acceso al micrófono.                                        |
| `fullscreen=(self)` | Permite que solo tu propio sitio (`self`) active la pantalla completa. |

---

### **Instrucciones para FortiWeb**

1. Asegúrate de que **URL Filter** esté en **Off**.
    
2. Copia y pega el valor recomendado en el campo Header Value:
    
    geolocation=(), camera=(), microphone=(), fullscreen=(self)
