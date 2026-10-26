# wikijs

## Requisitos Previos

- Docker Engine instalado
- Docker Compose instalado
- Red Docker externa `proxy` creada
- Contraseña segura generada para `POSTGRES_PASSWORD`

## Archivos de este Repositorio

Este repositorio contiene:

- `compose.yaml` - Stack base de Wikijs y PostgreSQL
- `.env.example` - Plantilla de variables de entorno
- `README.md` - Esta documentación

---

# Despliegue con Docker Compose

### 1. Clonar el repositorio

```bash
git clone https://github.com/groales/wikijs.git
cd wikijs
```

### 2. Preparar directorios

Como estás forzando al contenedor a escribir con tu usuario de sistema (1000:1000), debes crear manualmente la estructura de carpetas en tu servidor antes de levantar los servicios para que hereden los permisos correctos. Corre estos comandos en tu terminal:

```bash
mkdir -p db config data
chmod 700 db
```

### 3. Preparar `.env`

Genera una contraseña segura:

```bash
openssl rand -base64 32
```

Copia el archivo de ejemplo:

```bash
cp .env.example .env
```

Contenido esperado:

```env
POSTGRES_PASSWORD='tu_password_generado'
```


```bash
docker network create proxy
```

### 4. Desplegar

```bash
docker compose up -d
```
### 5. Verificar arranque

```bash
docker compose ps
docker compose logs 
```
