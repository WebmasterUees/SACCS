# Catalogo WordPress - Git Repository (Sanitized)

Este repositorio esta preparado para subir solo lo necesario a Git,
evitar secretos y mantener un flujo limpio para Dokploy.

## Que contiene este repo

- `conf/`: configuraciones de Nginx/PHP/MySQL del proyecto.
- `app/public/wp-content/mu-plugins/`: codigo MU plugins (si existe codigo custom).
- `app/public/wp-config.example.php`: plantilla segura de configuracion.
- `docs/plugins-manifest.txt`: lista de plugins detectados en el ZIP original.
- `docs/themes-manifest.txt`: lista de themes detectados en el ZIP original.

## Que NO se sube por defecto

- `wp-config.php` real (tiene secretos).
- `wp-content/uploads/` (archivos pesados de medios).
- Core completo de WordPress (`wp-admin`, `wp-includes`, etc).
- Plugins/themes instalados desde WordPress.org (se listan en `docs/`).
- SQL dumps y logs.

## Flujo recomendado (GitHub + Dokploy)

1. Subir este repo a GitHub.
2. En Dokploy, crear servicio WordPress usando Docker Compose.
3. Definir variables sensibles en Dokploy (no en Git):
   - MYSQL_DATABASE
   - MYSQL_USER
   - MYSQL_PASSWORD
   - MYSQL_ROOT_PASSWORD
4. En Dokploy Compose -> Environment, copiar las variables de `.env.example` con valores reales.
5. En Dokploy Compose -> Domains, asignar el dominio a `wordpress` en puerto 80 y habilitar HTTPS.
6. Mantener contenido de medios en volumen persistente y backups.

## Paso a paso rapido para aprender la interfaz de Dokploy

1. Git Sources -> conecta GitHub.
2. Projects -> Create Project -> SACCS.
3. Crea environment `development`.
4. Dentro de development: Create Service -> Docker Compose.
5. En General:
   - Repository: este repo
   - Branch: dev
   - Compose file: docker-compose.yml
6. En Environment: agrega las variables del archivo `.env.example` con passwords reales.
7. Deploy y revisa Deployments + Logs.
8. En Domains:
   - Host: tu subdominio dev
   - Service: wordpress
   - Container port: 80
   - HTTPS: ON (letsencrypt)
9. Cuando validas en dev, merge de dev -> main.
10. Repite el mismo servicio en environment `production` apuntando a branch main.

## Si quieres incluir plugin o theme custom

Mueve tu codigo custom a:

- `app/public/wp-content/plugins/<mi-plugin-custom>/`
- `app/public/wp-content/themes/<mi-theme-custom>/`

Esas carpetas si se pueden versionar en Git.

## Nota importante

El ZIP original traia WordPress completo con plugins y uploads.
Este repo fue recortado intencionalmente para mejores practicas de control de versiones.
Si quieres un modo "snapshot completo" (incluyendo plugins/themes), puedo prepararte una segunda variante.
