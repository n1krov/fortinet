---
Tema: "[[HTTP Header Security]]"
---


Esta cabecera es crucial para la seguridad moderna. Le permite al sitio controlar qué características y APIs del navegador pueden usar la página actual y los _iframes_ que esta contiene. Si la dejas vacía (o deshabilitas todo), evitas que contenido inyectado o malicioso acceda a la cámara, micrófono, geolocalización, etc.

### **Valor Recomendado (Restrictivo)**

Para maximizar la seguridad si tu sitio no necesita características del navegador, se recomienda deshabilitar todas ellas con un valor vacío:

- **Valor:** Un par de paréntesis vacíos para las directivas que quieras bloquear, por ejemplo, **`geolocation=(), camera=(), microphone=()`**.
    
- **Nota:** Si tu sitio **necesita** la cámara (por ejemplo, para escanear QR), tendrías que usar `camera=(self)`. Asumiremos la configuración más restrictiva para empezar.