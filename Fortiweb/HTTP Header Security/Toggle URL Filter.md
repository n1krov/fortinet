---
Tema: "[[HTTP Header Security]]"
---
## Función

Cuando el interruptor está:

### 1. Desactivado (Off - Estado Predeterminado)

- **Efecto:** La regla de cabecera de seguridad (por ejemplo, `X-Frame-Options: SAMEORIGIN`) se aplica a **TODAS las peticiones HTTP/S** que pasan por esta política de seguridad.
    
- **Uso:** Este es el modo que usaremos para las cabeceras de seguridad generales, ya que deben aplicarse a todo el sitio.
    

### 2. Activado (On)

- **Efecto:** Se abrirán campos adicionales donde puedes especificar una o más **URL de Petición** (Request URLs) o **filtros basados en Regex**. La cabecera solo se añadirá a las respuestas de las páginas que coincidan con ese filtro.
    
- **Uso (Ejemplo):**
    
    - Podrías usarlo si solo una página específica de tu sitio (`/micuenta/`) necesita una **Content-Security-Policy (CSP)** muy estricta, mientras que el resto del sitio usa una CSP más relajada.
        
    - Podrías usarlo para aplicar un `X-Frame-Options: ALLOW-FROM` solo a un recurso que sabes que se debe incrustar desde un dominio externo, mientras que el resto del sitio usa `DENY`.