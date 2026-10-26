# Wiki.js

[![Wiki.js](https://img.shields.io/badge/Wiki.js-Docs-0A0A0A?logo=wiki.js)](https://docs.requarks.io/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker)](https://www.docker.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Despliegue de **Wiki.js** con Docker Compose y PostgreSQL, configurado para producción con red externa `proxy`.

## Tabla de Contenidos

- [Requisitos Previos](#requisitos-previos)
- [Archivos del Repositorio](#archivos-del-repositorio)
- [Variables de Entorno](#variables-de-entorno)
- [Despliegue con Docker Compose](#despliegue-con-docker-compose)
- [Verificación del Despliegue](#verificación-del-despliegue)
- [Acceso a Wiki.js](#acceso-a-wikijs)
- [Solución de Problemas](#solución-de-problemas)
- [Actualizaciones](#actualizaciones)
- [Licencia](#licencia)

---

## Requisitos Previos

Antes de empezar, asegúrate de tener lo siguiente:

| Requisito | Detalle |
|-----------|---------|
| Docker Engine | Instalado y en ejecución |
| Docker Compose | v2+ (comando `docker compose`) |
| Red Docker externa `proxy` | Creada previamente para integración con reverse proxy |
| Contraseña segura | Para `POSTGRES_PASSWORD` (se genera en el paso 3) |

## Archivos del Repositorio

| Archivo | Descripción |
|---------|-------------|
| `compose.yaml` | Stack base de Wiki.js y PostgreSQL |
| `.env.example` | Plantilla de variables de entorno |
| `README.md` | Esta documentación |

## Variables de Entorno

Copia `.env.example` a `.env` y configura los valores:

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `POSTGRES_PASSWORD` | Contraseña del usuario `wikijs_user` de PostgreSQL | `TuContrasenaSeguraAqui` |

## Despliegue con Docker Compose

### 1. Clonar el repositorio

```bash
git clone https://github.com/groales/wikijs.git
cd wikijs
```

### 2. Preparar directorios

Los volúmenes montan carpetas locales en los contenedores. Crea la estructura y ajusta los permisos para que coincidan con el usuario del contenedor (`1000:1000`):

```bash
mkdir -p db config data
chmod 700 db
```

### 3. Preparar `.env`

Genera una contraseña segura y configura el archivo de entorno:

```bash
openssl rand -base64 32
cp .env.example .env
```

Edita `.env` con tu contraseña generada:

```env
POSTGRES_PASSWORD='tu_password_generado'
```

### 4. Crear la red externa `proxy`

Wiki.js se conecta a la red `proxy` (external) para exponerse a través de un reverse proxy:

```bash
docker network create proxy
```

### 5. Desplegar los servicios

```bash
docker compose up -d
```

## Verificación del Despliegue

Comprueba que ambos contenedores están en ejecución:

```bash
docker compose ps
docker compose logs
```

Salida esperada:

```
NAME                IMAGE                             STATUS
wikijs              lscr.io/linuxserver/wikijs:latest   Up (healthy)
wikijs-db           postgres:18-alpine                  Up (healthy)
```

## Acceso a Wiki.js

Una vez desplegados los servicios:

- Wiki.js estará disponible en `http://localhost:3000` (puerto mapeado del contenedor)
- Configura tu reverse proxy en la red `proxy` para el dominio deseado

En el primer acceso, sigue el asistente de configuración para crear la cuenta de administrador y finalizar la instalación de Wiki.js.

## Solución de Problemas

| Problema | Causa probable | Solución |
|----------|----------------|----------|
| `wikijs-db` no arranca | Permisos incorrectos en `./db` | Ejecuta `chmod 700 db` y reinicia |
| No se puede conectar a la red `proxy` | Red no creada | Ejecuta `docker network create proxy` |
| Error de autenticación PostgreSQL | Contraseña incorrecta en `.env` | Sincroniza `POSTGRES_PASSWORD` entre ambos servicios y reinicia |
| Wiki.js no responde | Contenedor no sano | Revisa `docker compose logs wikijs` |

## Actualizaciones

Para actualizar las imágenes a la última versión:

```bash
docker compose pull
docker compose up -d
```

Los datos persisten en los volúmenes `./db`, `./config` y `./data`.

## Licencia

Este proyecto está bajo la Licencia MIT. Consulta el archivo `LICENSE` para más información.

---

Hecho con ❤️ por [groales](https://github.com/groales)
