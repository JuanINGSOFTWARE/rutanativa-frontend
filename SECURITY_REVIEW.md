# Revisión de seguridad de dependencias

## Alcance

Auditoría local de la candidata `v0.2.0-rc1` y verificación posterior al mantenimiento preventivo en `maintenance/seguridad-dependencias`.

## Hallazgos iniciales

`npm audit` encontró cuatro paquetes con avisos HIGH y cero CRITICAL:

- `vite@8.0.14` (dependencia directa de desarrollo): avisos asociados a Vite y `launch-editor`, incluidos manejo de rutas en Windows, argumentos del editor y middleware de desarrollo.
- `postcss@8.5.15` (transitiva): lectura o divulgación de archivos `.map` mediante source maps.
- `nanoid@3.3.12` (transitiva): posible bucle indefinido para tamaños inválidos en ciertos generadores.
- `source-map-js@1.2.1` (transitiva): posible denegación de servicio con offsets de source maps indexados.

`npm audit --omit=dev` reportó cero vulnerabilidades en la primera candidata. Estos hallazgos correspondían a la cadena de herramientas de desarrollo y no a dependencias de ejecución incluidas en el paquete estático.

## Corrección y verificación

Se revisó `npm audit fix --dry-run` y se aplicaron actualizaciones compatibles, sin `--force` ni cambios al manifiesto:

- Vite: `8.0.14` → `8.3.5`
- PostCSS: `8.5.15` → `8.5.31`
- nanoid: `3.3.12` → `3.3.20`
- source-map-js: `1.2.1` → `1.2.2`

`npm ci` completó correctamente. `npm run check` pasó las siete pruebas y el build de producción. Tanto `npm audit` como `npm audit --omit=dev` reportaron cero vulnerabilidades en la candidata revisada.

## Evidencias

Los informes de auditoría inicial y posterior, el plan de actualización, la instalación reproducible y los controles están en `C:/AlojamientosEmpresa/evidencias/`:

- `npm_audit_rc1.json`
- `npm_audit_produccion_rc1.json`
- `audit_fix_plan.txt`
- `audit_fix_aplicado.txt`
- `npm_ci_rc2.txt`
- `check_rc2.txt`
- `npm_audit_rc2.json`
- `npm_audit_produccion_rc2.json`

La revisión se limita a las dependencias informadas por npm y no sustituye una auditoría completa del código o del entorno de despliegue.
