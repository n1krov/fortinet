---
Tema: "[[apuntes]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

todo lo que vimos antes es para operar el perfil de web filter en modo flow based

pero ahora veremos que hay en modo proxy based



#### Category Usage Quota

security profiles > web filter

![[Pasted image 20260211094037.png]]

cuando trabajamos con cuotas hay que trabajar tambien con [[Application Control]]
ya que tambien cae por aca el trafico

basta con que este en monitor

si quieres tener un monitor a la vista y agregarlo debes ir a dashboard > fortiguard quota


#### Search engines

![[Captura de pantalla_20260211_094857.png]]

esta el safe  search para que no muestren en el navegador contenido inapropiado. muy util en instituciones

la restriccion de acceso a youtube a contenidos que son restrictivos o yt considera medio inapropiados

#### Proxy options

![[Captura de pantalla_20260211_095543.png]]

aca esta la opcion de restringir las cuentas de google a dominios especificos
ej: si pusiste el dominio empresa.com no te va a dejar loguearte si  no tenes upesto en tu correo `usuario@empresa.com`

