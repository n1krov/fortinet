---
Tema: "[[VPN SSL]]"
---

Quiero que actúes como un asistente especializado en mejorar y embellecer mis apuntes de **hacking y ciberseguridad - FORTINET** en Obsidian.

### Reglas de formato:
- Usa **Markdown** y todas las herramientas nativas de Obsidian:  
  - Encabezados jerárquicos (#, ##, ###…)  
  - Negritas, cursivas, tachado  
  - Listas ordenadas y no ordenadas  
  - Tablas para comparaciones  
  - Callouts (`> [!info]`, `> [!tip]`, `> [!warning]`, `> [!example]`, etc.)  
  - Diagramas con **Mermaid** (especialmente diagramas de redes, flujos y ataques)  
  - Bloques de código y comandos de terminal (bash, python, etc.)  
  - Separadores `---` para estructurar  

### Reglas de estilo:
- Embellecé y organizá mis notas para que sean **claras, fáciles de leer y visualmente atractivas**.  
- Si algo está enredado o difícil de entender, simplificalo y hacelo **más didáctico**.  
- Agregá **ejemplos prácticos** (comandos reales, simulaciones, casos de uso).  
- Respetá los **enlaces e imágenes** que yo incluya. No borres ni inventes enlaces/imágenes nuevas.  
- Podés usar **diagramas de red (Mermaid), tablas comparativas y listas de pasos** para explicar ataques, defensas y herramientas.  
- El resultado final debe ser un apunte **técnico, claro y útil para estudiar hacking**.  

Cuando te pase un texto, transformalo siguiendo estas reglas.

Aqui te va el texto:

---

en VPN > SSL-VPN Portals

al momento de configurar existen 3 modos

## Modos de Split Tunneling en FortiGate

El Split Tunneling determina qué tráfico del usuario viaja por el túnel cifrado hacia la oficina y qué tráfico sale directamente por el internet del usuario (casa/café).


![[Captura de pantalla_20260213_091443.png]]

### 1. Disabled (Túnel Completo)

- **Comportamiento:** Todo el tráfico del cliente, sin excepción, se dirige a través del túnel SSL VPN.

vas a tener que crear politica tanto para la red interna, como para la salida a internet

la politica para salida a internet debe ser algo como esto

![[Captura de pantalla_20260213_095538.png]]

ahi estan las politicas de vpn ssl client, nota los destinos. tiene a dos subredes.

![[Captura de pantalla_20260213_095623.png]]

- **Uso:** Máxima seguridad. El FortiGate inspecciona hasta lo que el usuario busca en Google o ve en YouTube.
    
- **Desventaja:** Consume mucho ancho de banda y CPU del firewall innecesariamente.
    

### 2. Enabled Based on Policy Destination (Inclusivo)

- **Comportamiento:** Solo el tráfico cuyo destino coincida con las redes definidas en las **Políticas de Firewall** de la VPN se enviará por el túnel.
    
- **Lógica:** "Si hay una regla que me permite ir al Servidor A, voy por el túnel. Si quiero ir a Netflix, voy por mi internet local".
    
- **Configuración:** Depende totalmente de lo que pongas como _Destination_ en la política de IPv4/IPv6 de la VPN.
    

### 3. Enabled for Trusted Destinations (Exclusivo + Override)

Como bien mencionaste, este modo es el más granular y utiliza el **Override** para marcar excepciones.

- **Comportamiento:** Todo el tráfico se dirige al túnel **excepto** aquel que coincida con destinos explícitamente marcados como de confianza (Trusted).
    
- **El rol del Routing Address Override:** Es aquí donde especificas esos hosts o redes locales (como la IP `192.168.1.4` que se ve en tu captura) que quieres **excluir** del túnel.
    
- **Diferencia clave:** Mientras que el segundo modo te dice qué _incluir_ en la VPN, este modo asume que todo va por la VPN y te pide especificar qué _excluir_. Sirve para forzar que el tráfico a internet sea corporativo pero permitiendo que el usuario acceda a su propia impresora local o dispositivos domésticos sin desconectar la VPN.
    

---

### Resumen para tu apunte de Obsidian:

> [!IMPORTANT] **Routing Address Override** Funciona como una "Lista de Excepciones". En el modo _Enabled for Trusted Destinations_, el tráfico a los destinos que pongas en esta lista **no** entrará al túnel SSL, permitiendo la comunicación con recursos locales del usuario mientras el resto de su navegación sigue protegida por la empresa.