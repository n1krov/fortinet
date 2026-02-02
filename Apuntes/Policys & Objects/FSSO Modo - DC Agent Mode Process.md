esta es la mas usada y la mas escalable en empresas donde hay mas de un controlador de dominio
tambien este modo, el collector agent controla los estados de las workstation, controla si estan prendidas, inactivas etc

![[Captura de pantalla_20260130_102551.png]]
1. el usuario se autentica contra el DC de windows
2. el Agente DC ve el evento de autenticacion y redirige al collector agent
3. el collector agent recive el evento del DC agente y redirige al fortigate
4. El fortigate conoce el usuario por su ip, entonces el usuario no necesita autenticarse constantemente

> nota que este modo trabaja con DC AGENT y COLLECTOR AGENT generalmente en el mismo servidor


el collector agent trabaja en el 8002 por UDP
y el forti trabaja por el 8000 por TCP
el collector agent envia al forti
- nombre de usuario
- nombre de host
- Direccion IP
- grupos de usuario
