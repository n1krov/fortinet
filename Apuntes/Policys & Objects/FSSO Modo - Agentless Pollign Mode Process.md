

![[Captura de pantalla_20260130_113314.png]]

1. el forti efectua la extraccion al controlador de dominio DC yu recolecta eventos de login
2. el usuario se autentica con el DC.
	1. fortigate descubre el evento del login en la siguiente extraccion
3. el usuario no necesita autenticarse
	1. Fortigate ya sabe de quién es el tráfico que está recibiendo


----

comenzamos configurando el forti esta vez con otro conector llamado

poll active directory server


en Security Fabric>external conectors


aqui un ejemplo

![[Pasted image 20260202163237.png]]

cabe aclarar tambien que en users/goups permite filtrar por usuarios o grupos
util para ver quien tiene acceso