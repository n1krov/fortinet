---
Tema: "[[VPN SSL]]"
---


![[Pasted image 20260213110514.png]]


en el fortigate servidor
en `system > certificates > + import certificate`
subir el certificado y el key file

tambien la password

y un nombre


importar ahroa la ca que lo firmo

`system > certificates > + import certificate CA`

generalmente es tipo file

supongamos que tenemos nuestro certificado **cert** y la CA **CA_CERT**

creamos un usuario de tipo local (usuario vpn)
creamos un usuario pki, esto se hace por CLI

`config user peer`
	edit (nombre usuario pki, por conveniencia poner nombre igual al certificado)
		get
		set ca CA_CERT
	end

ahi aparecera la opcion de pki en SYSTEM

![[Captura de pantalla_20260213_111406.png]]

ahora se programara la vpn

para eso se necesita el portal pero para este ejemplo se puede usar el que viene por defecto el full-access

![[Captura de pantalla_20260213_111613.png]]luego en vpn > ssl-vpn settings programas la configuracion de vpn poniendo el puerto port1 ya que es cloud

el puerto que en este caso elige 10443, importas el certificado.
y en la ultima opcino colocar el portal mapping solo el usuario creado, qeu tiene el portal de full-access recomendable

![[Captura de pantalla_20260213_111723.png]]

hay que configurar mas opciones que no estan en la GUI para que el usuario que va a ser de cliente use el usuario configurado aqui y el cliente pki

`config vpn ssl settings`
	conifg authtentication-rule
	edit 1 
		set client-cert enable
		set user-peer (usuario pki)
		set users (usuario vpn)
		set portal full-access
	end
end

queda la policy que va a permitir el trafico a los usuarios que estan detras del fortigate qeu ace de cliente
