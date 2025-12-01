---
Tema: "[[HTTP Header Security]]"
---

La cabecera **Content-Security-Policy (CSP)** es la defensa más avanzada contra los ataques de inyección de código como **Cross-Site Scripting (XSS)**. Le dice al navegador **exactamente de dónde** está permitido cargar recursos (scripts, estilos, imágenes, fuentes, etc.).

Configurar una CSP puede ser complejo, ya que un error puede hacer que partes legítimas de tu sitio dejen de funcionar. Vamos a empezar con una **política estricta, pero básica y segura**, que suele funcionar para la mayoría de los sitios web que cargan recursos solo desde su propio dominio.

## Valor a Ingresar

Ingresa la siguiente línea exactamente como está en el campo **Header Value**:

```
default-src 'self'; frame-ancestors 'self';
```

### Explicación de las directivas:

1. **`default-src 'self'`**: Esta es la regla maestra. Significa que **todos** los recursos (scripts, imágenes, estilos, fuentes, conexiones AJAX) solo pueden ser cargados desde tu **propio dominio** (el origen del sitio). ***Esto bloquea el contenido cargado desde sitios de terceros, a menos que se especifique una excepción***.
    
2. **`frame-ancestors 'self'`**: Esta directiva le dice al navegador que solo tu propio sitio puede incrustarte en un `<iframe>`. Esto previene el Clickjacking (es la versión moderna y preferida de `X-Frame-Options: SAMEORIGIN`).

Utilizaremos la directiva `default-src 'self'` que restringe todos los tipos de recursos (scripts, imágenes, AJAX, etc.) para que solo se carguen desde el **propio origen** del sitio (`'self'`).

- **Header Value:** `default-src 'self'; frame-ancestors 'self';`

| **Directiva**            | **Significado**                                                                                                                                                                                                            |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default-src 'self'`     | Regla general: solo permite cargar recursos (scripts, imágenes, fuentes, etc.) desde tu **propio dominio**.                                                                                                                |
| `frame-ancestors 'self'` | **Reemplaza a `X-Frame-Options: SAMEORIGIN`** y le dice al navegador que solo tu propio sitio puede incrustarte en un `iframe`. (Lo incluimos aquí para asegurarnos de que la protección contra Clickjacking sea moderna). |

### **Regla a Configurar en FortiWeb**

1. En tu política de prueba "New Secure Header Rule", haz clic en **"Create New"**.
    
2. Selecciona:
    
    - **Secure Header Type:** `Content-Security-Policy`
        
    - **Header Value:** `default-src 'self'; frame-ancestors 'self';`
        
    - **URL Filter / Exception:** Desactivado (Off) / En blanco.
        
3. Guarda la regla.
    

## ¡Prueba de Sitio Crítica!

Dado que la CSP es tan estricta, es fundamental que, una vez que apliques esta política de prueba, **navegues por todo tu sitio web**.

- **¿Se ven todas las imágenes?**
    
- **¿Funcionan todos los botones y menús desplegables?**
    
- **¿Hay íconos o fuentes que no cargan?**
    

Si algo no carga, significa que ese recurso proviene de un dominio externo (como Google Analytics, un CDN, o un script de terceros) y necesitarás añadir excepciones a la CSP.
