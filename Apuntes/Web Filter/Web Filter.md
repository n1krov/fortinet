---
Tema: "[[apuntes]]"
---

## **¿Cuándo se activa el Web Filtering?**

El filtrado web no actúa sobre cualquier paquete, sino que espera a que se realice una **solicitud específica de contenido**.

### **1. El Flujo de Conexión (Pasos previos)**

Antes de que el Web Filter entre en acción, ocurren varios pasos técnicos:

1. **DNS Request/Response**: El cliente pregunta por la IP del dominio (ej. `www.acme.com`).
    
2. **TCP Three-Way Handshake**: Se establece la conexión básica mediante los paquetes **SYN**, **SYN/ACK** y **ACK**.
    
3. **Establecimiento de sesión**: En este punto la conexión está abierta, pero el firewall aún no sabe qué página exacta quiere ver el usuario.
    

### **2. El disparador: HTTP GET**

El filtrado web se basa en la **solicitud (request)** del usuario.

- El motor de Web Filter se activa únicamente cuando recibe un paquete **HTTP GET**.
    
- En este paquete viaja la URL completa (ej. `www.acme.com/juegos`).
    
- Al recibir el GET, el FortiGate extrae la URL, consulta su categoría en FortiGuard y decide si permite o bloquea la carga de la página.
    

---

## **Resumen de FortiGuard Category Filter**

Para que ese paso funcione, el equipo utiliza la base de datos en la nube:

- **Clasificación**: Los sitios se dividen en múltiples categorías y subcategorías compatibles con el firmware actualizado.
    
- **Conexión en vivo**: Requiere un contrato activo con FortiGuard.
    
- **Periodo de gracia**: Si la licencia vence, tienes un margen de **dos días** antes de que el filtrado deje de funcionar.
    
- **Alternativa**: Se puede usar **FortiManager** como servidor local de categorías en lugar de consultar siempre a la nube de Fortinet.
    

> El filtrado web **no bloquea la conexión TCP inicial**, sino el intento de obtener contenido específico. Por eso, verás que la sesión TCP se establece correctamente en los logs, pero el contenido es bloqueado inmediatamente después del HTTP GET.



---
este es un servicio pago, requiere un contrato con fortinet