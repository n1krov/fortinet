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


