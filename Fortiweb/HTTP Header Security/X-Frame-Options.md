---
Tema: "[[HTTP Header Security]]"
aliases:
---
La cabecera **`X-Frame-Options`** es una medida de seguridad que le dice al navegador si una página web tiene permiso para ser cargada dentro de elementos de enmarcado como `<iframe>`, `<frame>` u `<object>` en otro sitio.

Su propósito principal es mitigar el **Clickjacking** (Secuestro de Clics), donde un atacante carga tu sitio en un marco invisible y engaña al usuario para que haga clic en un botón de tu sitio creyendo que está haciendo clic en algo del sitio del atacante.