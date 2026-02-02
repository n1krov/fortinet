---
Tema: "[[CLI]]"
---
### Ver el estado de la memoria

```sh
diagnose hardware sysinfo conserve
```


### Ver informacion de alguna interfaz
```sh
diagnose hardware deviceinfo nic <port_x>
```

```sh
diagnose sys top
```

### Matar un proceso

```sh
diagnose sys kill 11 <PID>
```


### Caso de que los tuneles fallan

por si fallan los tuneles o algo de IPsec, 

```
diagnose debug disable
```

```sh
diagnose vpn ike log filter clear
```

```sh
diagnose vpn ike log filter name <nombre_vpn>
```


```sh
diagnose vpn ike log filter list
```



```sh
diagnose vpn ike log filter list
```


una vez seteado el filtro se puede 
```sh
diagnose debug application ike -1
```

-1 poara que nos de toda la info posible

luego
```sh
diagnose debug enable
```

para detenerlo
diagnose debug disable




#############################
IPSEC
#############################

diagnose debug disable
diagnose vpn ike log filter clear
diagnose vpn ike log filter name <phase1-name>
diagnose debug application ike -1
diagnose debug enable

diagnose debug reset
diagnose debug disable

#############################
Traffic Flow
#############################

estp es em el caso de que exista un error de configuracion que sea imperceptible a la vista y no nos demos cuanta que es lo que falla exactamente

diagnose debug disable 
diagnose debug flow trace stop 
diagnose debug flow filter clear 
diagnose debug reset 

diagnose debug flow filter saddr 10.0.1.10
diagnose debug flow filter daddr 8.8.8.8
diagnose debug flow show function-name enable
diagnose debug console timestamp enable
diagnose debug flow trace start 999
diagnose debug enable

