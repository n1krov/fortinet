---
Tema: "[[network]]"
---

Embellecé y organizá mis apuntes de hacking en Obsidian usando Markdown (encabezados, listas, callouts, tablas, mermaid, bloques de código).  
Simplificá lo confuso, agregá ejemplos de comandos/técnicas.  
Respetá OBLIGATORIAMENTE enlaces e imágenes.  
Objetivo: notas claras, técnicas y atractivas.  

Aqui va el texto:

---


NAT  es una tecnología en routers y firewalls que traduce direcciones IP privadas de una red local a una única dirección IP pública para que múltiples dispositivos compartan una conexión a internet, resolviendo la escasez de direcciones IPv4 y añadiendo una capa de seguridad al ocultar las IPs internas. Funciona como un intermediario, reemplazando la IP privada de un dispositivo por la IP pública del router al salir a internet y viceversa al recibir datos, y es fundamental para el funcionamiento de la mayoría de redes domésticas. 

Cómo funciona

1. **Envío**: Un dispositivo (ej. tu móvil) con IP privada envía una solicitud a internet.
2. **Traducción (Salida)**: El router intercepta el paquete y reemplaza la IP privada de origen por su propia IP pública, anotando el cambio en una tabla.
3. **Recepción**: La respuesta de internet llega al router (su IP pública).
4. **Traducción (Entrada)**: El router busca en su tabla y traduce la IP pública de vuelta a la IP privada del dispositivo original, reenviando el paquete. 

Tipos comunes de NAT

- **NAT Estática**: Mapea una IP privada a una IP pública fija, útil para servidores.
- **NAT Dinámica**: Asigna una IP pública de un grupo disponible a cada dispositivo, más costoso.
- **NAT de Sobrecarga (PAT/NAPT)**: El más común en hogares; mapea muchas IPs privadas a una única IP pública usando diferentes puertos, siendo muy eficiente. 

Beneficios

- **Ahorro de IPs**: Permite que miles de dispositivos usen una sola IP pública.
- **Seguridad**: Oculta las direcciones IP internas, dificultando ataques directos desde internet. 

Desventajas

- **Rendimiento**: Puede añadir latencia (lag).
- **Compatibilidad**: Puede causar problemas con ciertos juegos o aplicaciones que requieren conexiones directas