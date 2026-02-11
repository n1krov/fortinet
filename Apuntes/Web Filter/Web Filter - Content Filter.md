---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---


esto entran en esta categoria, tanto content filter como web filter

![[Captura de pantalla_20260211_091602.png]]


en Security Profiles > web fileter > static URL filter esta la opcion

![[Captura de pantalla_20260211_091730.png]]

es colocar un patron y aquellas que coinciden se podran exemptuar o blquear

puwede ser un windlcard o regex,
es necesario que el modo de inspeccion debe estar en deep inspection ya que fortigate tiene que entrar y ver el contenido


### Opciones de Clasificación (Rating Options)

![[Captura de pantalla_20260211_093526.png]]

- **Allow websites when a rating error occurs:**
    
    - **¿Qué hace?:** Si el FortiGate intenta consultar a los servidores de FortiGuard para saber si una página es "Juegos" o "Noticias" y la consulta falla (por ejemplo, por un microcorte de internet), esta opción permite que el usuario entre a la página de todos modos.
        
    - **Recomendación:** Se suele activar para no interrumpir la navegación por problemas externos, pero si tu prioridad es la seguridad máxima, desactivarlo bloqueará cualquier sitio que no pueda ser verificado.
        
- **Rate URLs by domain and IP Address:**
    
    - **¿Qué hace?:** Cuando está activo, el FortiGate no solo revisa el nombre del dominio (ej: `google.com`), sino también la dirección IP del servidor donde está alojado.
        
    - **Utilidad:** Ayuda a bloquear sitios maliciosos que usan IPs conocidas por actividades sospechosas, incluso si cambian de nombre de dominio constantemente.