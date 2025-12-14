# Guía de instalación de n8n en Render  

Esta guía te ayuda a desplegar n8n en la plataforma Render utilizando un repositorio mínimo en GitHub. Sigue estos pasos para tener tu instancia de n8n operativa.  

## 1. Crear el repositorio con los archivos básicos  

1. Crea un nuevo repositorio en GitHub (por ejemplo `n8n-install-guide`) sin inicializar con README ni .gitignore.  
2. Añade un archivo `Dockerfile` en la raíz con el siguiente contenido:  

```
FROM docker.n8n.io/n8nio/n8n:latest
```  

   Este Dockerfile utiliza la imagen oficial de n8n más reciente.  
3. Añade un archivo `render.yaml` en la raíz para que Render conozca cómo desplegar el servicio. Usa este contenido:  

```
services:
  - type: web
    name: n8n-demo
    env: docker
    plan: free
    autoDeploy: true
    healthCheckPath: /
    envVars:
      - key: N8N_HOST
        value: n8n-demo.onrender.com
```

   Ajusta el valor de `name` y `value` de `N8N_HOST` a tu preferencia; Render generará el subdominio en función de este nombre.  
4. Confirma los cambios con un mensaje de commit descriptivo.  

## 2. Desplegar en Render  

1. Accede a tu cuenta en [Render](https://render.com/) y elige **New > Web Service** para crear un nuevo servicio.  
2. Conecta tu cuenta de GitHub y selecciona el repositorio creado. Render detectará el `Dockerfile` automáticamente.  
3. Selecciona el plan gratuito (Free). Mantén activada la opción **Auto Deploy** para que Render vuelva a desplegar cuando haya nuevos commits.  
4. Inicia el despliegue. La construcción de la imagen y el despliegue pueden tardar unos 2–3 minutos.  

## 3. Ajustar variables de entorno  

Una vez que el servicio esté activo, Render asignará una URL pública (por ejemplo `https://n8n-demo.onrender.com`). Debes ajustar las variables de entorno para que n8n genere URLs correctas en los webhooks:  

1. Copia la URL pública del servicio.  
2. En la sección **Environment** de Render, edita o añade las variables:  
   - `N8N_HOST`: establece el dominio sin protocolo, por ejemplo `n8n-demo.onrender.com`.  
   - `WEBHOOK_URL`: establece la URL completa con protocolo, por ejemplo `https://n8n-demo.onrender.com/`.  
3. Guarda los cambios y reinicia el servicio para que las nuevas variables tomen efecto.  

Con estos pasos tendrás tu instancia de n8n desplegada en Render lista para procesar flujos y webhooks. 
