---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

en `security profiles > dns filter`

![[Captura de pantalla_20260211_111703.png]]
es muy similar a loq ue nos encontramos en [[Web Filter]]


### 1. FortiGuard Category Based Filter

Es el corazón del filtro. Permite bloquear o monitorear el acceso a sitios basándose en su categoría (como las que ves en la lista: _Adult Content, Gambling, Dating_, etc.).
- **Action (Monitor):** En tu captura, varias categorías están en modo "Monitor". Esto significa que el FortiGate **deja pasar el tráfico**, pero registra la actividad en los logs para que sepas que alguien entró ahí.
- **Redirect to Block Portal:** Si cambias la acción a esta opción, cuando un usuario intente entrar a un sitio de esa categoría, el FortiGate interceptará la consulta DNS y lo mandará a una página de advertencia.

### 2. Opciones Superiores

- **Redirect botnet C&C requests to Block Portal:** Esta es una de las opciones más importantes de seguridad. Si una computadora de tu red está infectada y trata de contactar al servidor del atacante (Command & Control), el FortiGate bloquea esa resolución DNS automáticamente.
- **Enforce 'Safe Search'**: Fuerza a que buscadores como Google, Bing o YouTube solo muestren resultados aptos para todo público, filtrando contenido explícito desde la consulta DNS.

### 3. Static Domain Filter

Funciona como una "lista negra" o "lista blanca" manual.
- **Domain Filter:** Aquí puedes escribir dominios específicos (ej: `facebook.com`) para bloquearlos o permitirlos sin importar a qué categoría de FortiGuard pertenezcan.

![[Captura de pantalla_20260211_114744.png]]
### Filtro de Dominios Específicos

Has agregado manualmente dos entradas para bloquear el acceso a una plataforma educativa:

- **Dominios:** `udemy.com` y `www.udemy.com`.
    
- **Type (Simple):** Significa que el FortiGate busca la coincidencia exacta de ese texto.
    
- **Action (Redirect to Block Portal):** En lugar de resolver la IP real, el FortiGate enviará al usuario a una página de bloqueo.
    
- **Status (Enable):** Ambos filtros están activos ahora mismo.
    

### Opciones de Redirección y Errores

En la parte inferior ves configuraciones que definen qué pasa cuando algo falla:

- **Redirect Portal IP:** Está usando la IP por defecto de FortiGuard (`208.91.112.55`). Esta es la dirección a la que se envía el tráfico cuando un sitio es bloqueado por el filtro DNS.
    
- **Allow DNS requests when a rating error occurs:** Está **desactivado**. Esto significa que, si por alguna razón el servicio de clasificación de FortiGuard no responde, el FortiGate **bloqueará** la consulta DNS por seguridad en lugar de permitirla.



DNS translation es cuando matchee ese dominio con su IP destino la reemplazas por otra ip.

![[Captura de pantalla_20260211_115306.png]]


por ultimo las options

![[Pasted image 20260211115728.png]]


cuando no pueda hacer la conexional fortiguard puedes bloquearlo o dejar pasar el trafico
y habilitar la opcion de log

---
la aplicacion tambien se hace de la misma forma, en una politica definida. ya que esto es un perfil de seguridad

esto, al igual que [[Web Filter]], debe usar los servidores de fortiGuard.

