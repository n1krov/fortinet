
Esta cabecera controla cuánta información sobre la página actual (la URL de origen) se debe enviar al servidor de destino cuando el usuario hace clic en un enlace a otro sitio (navegación _cross-origin_).

Tu reporte indica que falta. Ya tienes una regla para esta cabecera en tu política principal (`origin-when-cross-origin`), pero vamos a asegurarnos de que el valor en tu política de prueba sea el más seguro y recomendado.

### **Valor Recomendado**

El valor más seguro y recomendado actualmente es **`strict-origin-when-cross-origin`**.

- **¿Qué hace?** Envía la URL completa solo cuando se navega dentro del mismo sitio (mismo origen). Cuando se navega a un sitio diferente (origen cruzado), solo se envía el **origen** (ej. `https://tudominio.com/`), no la ruta completa ni la _query string_. Esto protege la privacidad del usuario y evita fugas de información interna.