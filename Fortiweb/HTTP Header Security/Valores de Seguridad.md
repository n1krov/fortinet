---
Tema: "[[HTTP Header Security]]"
---

### 1. **DENY**

- **Efecto:** Le dice al navegador que **nunca** debe cargar la página dentro de un `iframe` o similar, sin importar quién sea el sitio que intenta enmarcarla.
    
    - _Ejemplo:_ Si pones `X-Frame-Options: DENY`, ni siquiera una página de tu propio sitio (`tudominio.com`) podrá cargar otra página de tu sitio en un `iframe`.
        
- **Seguridad:** Es la opción **más estricta** y segura.
    
- **Riesgo de Funcionalidad:** Puede romper la funcionalidad legítima si tu propio sitio usa _iframes_ para cargar partes internas (aunque esto es menos común en diseños modernos).
    

### 2. **SAMEORIGIN**

- **Efecto:** Permite que la página sea cargada en un marco **solo si** el sitio que intenta enmarcarla pertenece al **mismo origen** (mismo dominio, protocolo y puerto).
    
    - _Ejemplo:_ `tudominio.com/pagina1.html` puede cargar `tudominio.com/pagina2.html` en un `iframe`. Pero `otrodminio.com` no podrá.
        
- **Seguridad:** Ofrece un **excelente equilibrio** entre seguridad y funcionalidad, ya que previene el Clickjacking externo, pero permite el uso de _iframes_ para fines internos.
    
- **Recomendación:** Este es el **Valor Recomendado** en la mayoría de los casos.
    

### 3. **ALLOW-FROM** `uri`

- **Efecto:** Permite que la página sea cargada en un marco **solo si** el sitio que intenta enmarcarla coincide exactamente con la URL (origen) que especificas en `uri`.
    
    - _Ejemplo:_ `X-Frame-Options: ALLOW-FROM https://tudominio-socio.com` permitiría solo al dominio socio incrustar tu página.
        
- **Problema:** Este valor ha sido **reemplazado** por la directiva `frame-ancestors` dentro de la cabecera más moderna y flexible **`Content-Security-Policy (CSP)`**.
    
- **Compatibilidad:** No es compatible con todos los navegadores modernos (especialmente Chrome y Safari lo dejaron de soportar), por lo que **no se recomienda** usarlo. Si necesitas una excepción, usa CSP.