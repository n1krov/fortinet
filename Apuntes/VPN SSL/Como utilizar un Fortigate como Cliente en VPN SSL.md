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

> copiar el subject del certificado correspondiente y pegarlo en la ventana de pki como aparece en la siguiente imagen...

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

queda la policy que va a permitir el trafico a los equipos que estan detras del fortigate qeu ace de cliente

![[Captura de pantalla_20260213_112343.png]]
- se deja el NAT
- en el incoming esta el tunel
- outgoing ocupa el puerto 1 de lan que lleva a los recursos remotos
- como source toma el usuario vpn
- el destino como se trata de un split tunneling hay que especificar la subred para enrutar el trafico 

![[Captura de pantalla_20260213_112528.png]]

---

## Lado cliente

se importa los certificados de la misma forma que el anterior

aqui hay que crear una **interfaz ssl** en `network > interfaces > create new interface`

![[Captura de pantalla_20260213_112801.png]]

nota que el type es el ssl vpn tunnel
la interface es la wan1 que sale a internet

configuramos el usuario pki por cli
`config user peer`
	edit (nombre usuario pki, por conveniencia poner nombre igual al certificado)
		get
		set ca CA_CERT
	end

> copiar el subject del certificado correspondiente y pegarlo en la ventana de pki

aqui en la seccion `vpn > ssl-vpn clients` creamos el cliente

![[Captura de pantalla_20260213_113753.png]]
nota que
la interface es la vpn
la ip es la ip del servidor hacia donde nos vamos a conectar
el puerto definido en el forti del servidor
el nombre de usuario (usuario vpn)
el certificado y el usuario pki programado previamente


por ultimo la firewall policy

![[Captura de pantalla_20260213_113954.png]]

nota que el incoming es la red internay el outgoing es el tunel



---
# TEORIA FortiGate como Cliente SSL VPN (Modo Túnel)

Esta arquitectura permite que un dispositivo FortiGate actúe como un cliente para conectarse a otro FortiGate que funciona como servidor. Es una alternativa robusta a IPsec en ciertos escenarios de red.

## 1. ¿Cómo funciona esta arquitectura? ⚙️

A diferencia del túnel tradicional para usuarios (FortiClient), aquí el túnel se establece **entre dos dispositivos de hardware**:

1. **Iniciación:** El FortiGate cliente inicia la conexión hacia el FortiGate servidor usando una interfaz de tipo **SSL VPN Tunnel**.
    
2. **Autenticación:** El cliente utiliza una combinación de **PSK (Clave Pre-compartida)** y un **Certificado de Cliente PKI** para validarse ante el servidor.
    
3. **Establecimiento del Túnel:** Se crea una interfaz virtual (`ssliclient_port`) en el cliente. El servidor le asigna una dirección IP virtual de un pool reservado y el cliente añade dinámicamente rutas hacia las subredes remotas.
    
4. **Acceso:** Los dispositivos detrás del FortiGate cliente pueden acceder a los recursos detrás del servidor a través de este túnel cifrado (SSL/TLS).
    

## 2. Requisitos de Autenticación y Certificados 🔐

Para que el túnel sea exitoso, la confianza debe ser mutua y explícita:

- **Certificado CA:** El FortiGate servidor requiere un certificado CA válido.
    
- **Certificado de Cliente PKI:** El FortiGate cliente debe presentar un certificado PKI para autenticarse.
    
- **Cuenta de Usuario Local:** El cliente se autentica en el servidor usando una cuenta de usuario local configurada en el servidor (PSK).
    

## 3. Ventajas y Desventajas ⚖️

|**Ventajas**|**Desventajas**|
|---|---|
|**Traspasa bloqueos:** Evita problemas si el tráfico ESP o los puertos UDP 500/4500 (IPsec) están bloqueados por proveedores intermedios.|**Gestión de certificados:** Requiere la instalación y mantenimiento de certificados CA y PKI en ambos extremos.|
|**Sin fragmentación:** Útil para evitar problemas de paquetes IKE grandes que se fragmentan y fallan si el peer no soporta fragmentación IKE.|**Específico de Vendor:** Al ser SSL VPN, es una solución propietaria de Fortinet.|
|**Flexibilidad:** Soporta topologías de tipo Hub-and-Spoke.||

---

## 4. El Esquema de Conexión 🌐

Según el diagrama proporcionado:

- **Lado Cliente (Branch Office/Home):** El tráfico sale por el puerto WAN (ej. Port4) a través del túnel SSL.
    
- **SSL VPN Tunnel:** El canal cifrado protege los datos mientras viajan por internet.
    
- **Lado Servidor (HQ/Company):** Recibe la conexión en su puerto WAN (ej. Port1) y dirige el tráfico hacia los recursos remotos internos (ej. Port2).
    

> [!IMPORTANT] **Nota sobre el tráfico**
> 
> Cualquier aplicación basada en IP ejecutada en las máquinas de los usuarios detrás del FortiGate cliente puede enviar tráfico a través de este túnel de forma transparente.

---

## 5. Recordatorio de Salud del Sistema 🩺

Al mantener túneles activos entre sitios, vigila el consumo de recursos en ambos FortiGates:

- **88% (Red Threshold):** El equipo entra en modo conservación de memoria; podrías tener problemas para levantar nuevos túneles o realizar cambios de configuración.
- **95% (Extreme Threshold):** Se descartan sesiones nuevas.
