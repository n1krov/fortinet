---
Tema: "[[network]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá obligatoriamente enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---

![[Pasted image 20251230102005.png]]

como se puede ver en la imagen hay dos forti que estan conectados entre si, esto simula una red empresarial de diferentes areas donde cada area gestiona su propio firewall.

en este caso ambos cuentan con salida a internet pero el winows server A no puede ver al Windows B poruqe necesitan definirse las rutas estaticas


aca un ejemplo de configuracion de ruta estatica desde el site B

![[Pasted image 20251230102727.png]]

en este caso en el site A esta el **Forti A** que es `172.21.0.1` y el **Forti B** `172.21.0.2`

el Forti A es el que conoce al equipo **Windows Server** `10.0.1.10`
el Forti B es el que conoce al equipo **Windows 7** `10.0.2.10`

lo que hay que hacer es que desde el site A se pueda llegar a la red del site B 
y esto se lo puede hacer con el forti mediante rutas estaticas

se configura primero en network>static routes la redes para ambos forti a y b 

luego se crean las politicas, son dos para cada forti
en B:
- desde_A
- hacia_A
en A:
- desde_B