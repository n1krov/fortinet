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
diagnose vpn
```

