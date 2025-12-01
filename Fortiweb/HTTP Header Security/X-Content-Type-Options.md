---
Tema: "[[HTTP Header Security]]"
---

Esta cabecera previene un ataque llamado **MIME Sniffing**, donde el navegador intenta adivinar el tipo de archivo, lo que podría permitir que un atacante suba un archivo de texto inofensivo que luego se ejecute como un script malicioso.

### **Regla a Configurar**

1. En tu política de prueba "New Secure Header Rule" de FortiWeb, haz clic en **"Create New"** (o "Edit" si ya tienes una regla).
    
2. Para esta nueva regla, selecciona:
    
    - **Secure Header Type:** `X-Content-Type-Options`
        
    - **Header Value:** `NOSNIFF` (Este es el único valor válido y recomendado. Asegúrate de escribirlo tal cual).