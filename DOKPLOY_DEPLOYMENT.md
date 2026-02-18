# Guía de Despliegue en Dokploy para KoboToolbox

Esta guía explica cómo desplegar KoboToolbox en Dokploy utilizando los archivos configurados (`docker-compose.dokploy.yml` y `.env.dokploy`).

## Requisitos Previos

1. Tener Dokploy instalado en un servidor Linux.
2. Tener acceso al repositorio de Git donde se encuentra este código.

## Paso 1: Configurar la Red en Dokploy

Dokploy utiliza redes externas para permitir que diferentes aplicaciones se comuniquen.

1. Ve al panel de Dokploy.
2. Ve a **Settings / Networks**.
3. Asegúrate de que exista una red llamada `dokploy-network`. Si no existe, créala.

## Paso 2: Crear el Proyecto y el Stack

Aunque el despliegue se divide en servicios (frontend, backend, etc.), la forma más robusta de gestionarlo en Dokploy es mediante un **Stack**.

1. Crea un nuevo **Project** en Dokploy (ej: `KoboToolbox`).
2. Dentro del proyecto, añade un nuevo **Stack**.
3. En la configuración del Stack:
   - Selecciona **GitHub** (o tu proveedor de Git).
   - Elige el repositorio y la rama.
   - En **Compose Path**, escribe: `docker-compose.dokploy.yml`.

## Paso 3: Configurar Variables de Entorno

Antes de desplegar, debes configurar las variables de entorno:

1. Ve a la pestaña **Environment** del Stack.
2. Copia el contenido de `.env.dokploy` y pégalo allí.
3. **IMPORTANTE**: Cambia los valores de ejemplo:
   - `PUBLIC_DOMAIN_NAME`: Tu dominio (ej: `kobo.midominio.com`).
   - `DJANGO_SECRET_KEY`: Una clave larga y aleatoria.
   - `ENKETO_API_KEY`: Otra clave aleatoria.
   - `POSTGRES_PASSWORD`, `KOBO_MONGO_PASSWORD`, `REDIS_PASSWORD`: Contraseñas seguras.

## Paso 4: Desplegar

1. En la pestaña **Deploy**, haz clic en **Deploy**.
2. Dokploy clonará el repositorio, montará los scripts automáticamente y levantará todos los servicios.

## Consejos Adicionales

### Modo Mantenimiento
Si necesitas poner el sitio en mantenimiento:
1. En el Stack de Dokploy, puedes añadir la variable de entorno `COMPOSE_PROFILES=maintenance`.
2. O simplemente levanta el servicio `maintenance` manualmente si Dokploy lo permite a través de su CLI.

### Persistencia de Datos
He configurado **Named Volumes** (`kobo-db-data`, etc.) para que tus bases de datos persistan incluso si eliminas el stack y lo vuelves a crear.

### Scripts
Los scripts internos de Kobo se montan desde las carpetas del repositorio (`./postgres`, `./mongo`, etc.). No necesitas hacer nada extra, Dokploy se encarga de que estén disponibles para los contenedores.
