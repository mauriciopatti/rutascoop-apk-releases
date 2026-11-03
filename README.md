# Rutas Coop — APKs publicados

Distribución de la app de cortes y reanudaciones de la **Cooperativa Alto
Verde**. Acá solo viven los APK y el archivo de versiones: el código está en
otro repo.

Este repo es **público** porque la app lee `version.json` por su URL raw, y una
URL raw de un repo privado necesitaría un token dentro del APK.

## `version.json`

Es lo que la app consulta al abrir para saber si hay algo nuevo.

| campo | qué es |
|---|---|
| `latest_version_code` | build number de la última publicada (el número después del `+`) |
| `latest_version_name` | cómo se muestra: `"1.4.1"` |
| `min_version_code` | piso obligatorio: por debajo, la app no deja trabajar |
| `apk_url` | de dónde se baja el APK del último Release |
| `notas` | qué cambió, se le muestra al operario |

## Publicar una versión

1. Subir el build number en `pubspec.yaml`. **Sin esto la app no se entera.**
2. Compilar el release firmado y verificar que el SHA-256 del certificado
   empiece con `c59d314d`. Con otra firma, los teléfonos rechazan la
   instalación.
3. Crear el Release acá con el APK como asset.
4. Editar `version.json`: `latest_version_code`, `latest_version_name`,
   `apk_url` y `notas`.

El `version.json` lo sirve un CDN: tarda unos minutos en propagarse.

## `min_version_code`

Tocarlo solo cuando una versión vieja **ya no puede trabajar** (cambió el
contrato de la API). Es la única pantalla que atrapa al operario y no se puede
cerrar.

La app recorta `min_version_code` al `latest` si quedara por encima, para que
un error de carga no deje a toda la cuadrilla sin poder entrar.
