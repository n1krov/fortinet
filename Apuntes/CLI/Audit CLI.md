---
Tema: "[[CLI]]"
---
esta es una nueva funcionalidad de forti 7

si hacemos 

```sh
config system global
```

```sh
get | grep cli
```

encontraremos el `cli-audit-log`, que si lo habilitamos lo que vamos a encontrar son los comandos que se ejecutaron en la CLI


para hacerlo simplemente hacer
```sh
set cli-audit-log activate
```

 `end`


esto es como tener guardado el history


para ver la actividad de comandos por CLI lo hacemos desde la GUI en la parte de 

`log&report > events> system events`

