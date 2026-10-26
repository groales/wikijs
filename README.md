# wikijs

Pasos clave antes de ejecutar docker compose up -d:

Como estás forzando al contenedor a escribir con tu usuario de sistema (1000:1000), debes crear manualmente la estructura de carpetas en tu servidor antes de levantar los servicios para que hereden los permisos correctos. Corre estos comandos en tu terminal:

mkdir -p db config data
chmod 700 db