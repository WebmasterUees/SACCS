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
2. En Dokploy, crear servicio WordPress (Application o Compose segun estrategia).
3. Definir variables sensibles en Dokploy (no en Git):
   - DB_HOST
   - DB_NAME
   - DB_USER
   - DB_PASSWORD
   - WP_HOME
   - WP_SITEURL
4. Durante build/deploy, instalar WordPress core + plugins desde fuente/version definida.
5. Mantener contenido de medios en volumen persistente y backups.

## Si quieres incluir plugin o theme custom

Mueve tu codigo custom a:

- `app/public/wp-content/plugins/<mi-plugin-custom>/`
- `app/public/wp-content/themes/<mi-theme-custom>/`

Esas carpetas si se pueden versionar en Git.

## Nota importante

El ZIP original traia WordPress completo con plugins y uploads.
Este repo fue recortado intencionalmente para mejores practicas de control de versiones.
Si quieres un modo "snapshot completo" (incluyendo plugins/themes), puedo prepararte una segunda variante.
