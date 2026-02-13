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

creamos un usuario de tipo local
creamos un usuario pki, esto se hace por CLI

`config user peer`
	edit (nombre, por conveniencia poner nombre igual al certificado)
		get
		set ca (CA qeu firmo el certificado)