---
Tema: "[[apuntes]]"
---
**Memory Conserve Mode** (Modo de Conservación de Memoria). Es un mecanismo de defensa vital del FortiOS que se activa cuando el FortiGate se está quedando sin RAM, evitando que el equipo se bloquee por completo.

Aquí te explico cómo funcionan estos "semáforos" de seguridad:



## ¿Qué es el Conserve Mode?

Es un estado de protección automática que adopta el FortiGate para no volverse inestable cuando la memoria está muy llena. Dependiendo de qué tan crítica sea la situación, el equipo tomará medidas más o menos drásticas.

## Los Tres Umbrales (Thresholds)

El sistema maneja tres niveles basados en el porcentaje de RAM total utilizada:

|**Nivel (Threshold)**|**Definición**|**Valor por Defecto (% de RAM)**|
|---|---|---|
|**Green (Verde)**|Es el punto de salida. El FortiGate considera que ya está a salvo y **sale del modo de conservación**.|**82%**|
|**Red (Rojo)**|Es el punto de entrada. Aquí el FortiGate **entra en modo de conservación** y empieza a limitar funciones (como no permitir cambios de configuración).|**88%**|
|**Extreme (Extremo)**|Es el nivel de pánico. El FortiGate **descarta todas las sesiones nuevas** para proteger el tráfico que ya está pasando.|**95%**|


## ¿Cómo se configura?

Si necesitas ajustar estos valores (por ejemplo, si tu equipo entra en modo rojo muy seguido y quieres darle un poco más de margen), se hace desde la CLI con estos comandos que aparecen en tu imagen:

```bash
config system global
    set memory-use-threshold-red <porcentaje>
    set memory-use-threshold-extreme <porcentaje>
    set memory-use-threshold-green <porcentaje>
end
```

### ¿Por qué es importante para tu FSSO?

Si el FortiGate entra en **Conserve Mode (Red)**, podrías notar que los usuarios de FSSO dejan de autenticarse correctamente o que el firewall deja de recibir actualizaciones del Collector Agent porque está priorizando mantener el tráfico básico vivo sobre las funciones de inspección profunda.

**Dato clave:** Si ves que tu equipo llega frecuentemente al umbral **Rojo (88%)**, es una señal de que necesitas optimizar tus políticas (quitar perfiles de seguridad innecesarios) o actualizar a un modelo de hardware con más memoria.

¿Has notado lentitud en el firewall o algún mensaje de advertencia sobre la memoria últimamente?