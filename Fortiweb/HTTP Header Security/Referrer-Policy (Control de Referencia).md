---
Tema: "[[HTTP Header Security]]"
---

Esta cabecera controla cuánta información sobre la página actual (la URL de origen) se debe enviar al servidor de destino cuando el usuario hace clic en un enlace a otro sitio (navegación _cross-origin_).

Tu reporte indica que falta. Ya tienes una regla para esta cabecera en tu política principal (`origin-when-cross-origin`), pero vamos a asegurarnos de que el valor en tu política de prueba sea el más seguro y recomendado.

| **Valor**                             | **Explicación Breve**                                                                                                                               | **Seguridad / Privacidad**                                                                                                                                        |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`no-referrer`**                     | **Nunca** envía información de la página de origen (Referer) a ningún destino.                                                                      | Máxima privacidad. Puede romper el análisis web.                                                                                                                  |
| **`no-referrer-when-downgrade`**      | No envía Referer solo cuando pasas de **HTTPS a HTTP**. Sí lo envía de HTTPS a HTTPS.                                                               | Buena opción por defecto, pero menos estricta.                                                                                                                    |
| **`same-origin`**                     | Solo envía el Referer (URL completa) si el destino está en el **mismo dominio** (origen). No lo envía si el destino es externo.                     | Bueno para mantener la privacidad frente a terceros.                                                                                                              |
| **`origin`**                          | Siempre envía solo el **origen** (ej., `https://tudominio.com/`), sin la ruta ni los parámetros de la URL, tanto a destinos internos como externos. | Privacidad moderada. Revela el dominio de origen.                                                                                                                 |
| **`strict-origin`**                   | Similar a `origin`, pero solo de **HTTPS a HTTPS**. No envía nada si va a HTTP.                                                                     | Mejor que `origin`, es más seguro.                                                                                                                                |
| **`origin-when-cross-origin`**        | Envía la URL completa solo a destinos del **mismo dominio**. Envía solo el **origen** a destinos externos.                                          | Equilibrio.                                                                                                                                                       |
| **`strict-origin-when-cross-origin`** | **⭐ RECOMENDADO**                                                                                                                                   | Envía la URL completa solo al **mismo dominio**. Envía solo el **origen** a destinos externos, **solo si es HTTPS**. No envía nada si el destino externo es HTTP. |
| **`unsafe-url`**                      | Siempre envía la **URL completa** (ruta y parámetros) sin restricciones de protocolo.                                                               | Mínima privacidad. No recomendado, es inseguro.                                                                                                                   |

### **Valor Recomendado**

El valor más seguro y recomendado actualmente es **`strict-origin-when-cross-origin`**.

- **¿Qué hace?** Envía la URL completa solo cuando se navega dentro del mismo sitio (mismo origen). Cuando se navega a un sitio diferente (origen cruzado), solo se envía el **origen** (ej. `https://tudominio.com/`), no la ruta completa ni la _query string_. Esto protege la privacidad del usuario y evita fugas de información interna.
  
  