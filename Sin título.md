
# Pools de direcciones IP

esto se usa cuando al momento de salir a internet y se efectue el [[NAT]]  y tenes un rango de ip publicas es conveniente utilizar un POOL de direcciones IP

para crearlo se debe ir hacia `policy&objects>IP pools>create new`

![[Captura de pantalla_20260203_093136.png]]

tiene 4 tipos 
overload
one-to-one
fixed port range
port block allocation


luego en el firewall policy solo resta en la parte de NAT configurar en el modo > `Use dynamic IP pool` y agregamos el pool


con 
```sh
get system session list
``` 

vemos la lista de ssesiones de nuestra red administrables
