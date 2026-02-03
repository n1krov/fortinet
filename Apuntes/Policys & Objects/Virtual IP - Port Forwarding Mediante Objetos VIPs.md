---
Tema: "[[policys&objects]]"
---

## DST [[NAT]] (VIP)

esta imagen muestra el flujo de lo que pasa cuando nos queremos conectar a un servidor.

![[Captura de pantalla_20260203_083346.png]]

en este caso somos la maquina que se quiere conectar si vemos la cabecera ip, la ip src, el puerto src que esuno  random. la ip destino y el puerto destino

cuando sale hacia internet sale con la ip publica. y cuando pasa por el forti esa traza, el forti aplica el destination nat (dst nat) y hace el forwarding modificando la ip destino, antes tenia la publica ahora, tiene la ip del servidor en la red

esto tambien es conocido como VIP en fortigate



---

### Programar una VIP (Virtual IP)


en Policy&objects>Virtual IPs > create new virtual IP

![[Captura de pantalla_20260203_090602 1.png]]


> si tienes sdwan no te va a dejar poner el objeto sdwan como interface. debes poner una interfaz que sea integrante de una sdwan


