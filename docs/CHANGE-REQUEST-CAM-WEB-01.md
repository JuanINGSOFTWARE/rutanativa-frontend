# Solicitud de mantenimiento CAM-WEB-01

- Clasificacion: preventiva; no se atribuye a una incidencia de produccion observada.
- Motivacion: detectar una URL de API vacia, mal formada o con protocolo no HTTP antes de iniciar las solicitudes del cliente.
- Observacion del codigo: `src/js/api/http.js` lee `import.meta.env.VITE_API_URL` directamente y concatena la ruta, sin validar la configuracion.
- Hallazgo adicional confirmado durante la linea base visual: el temporizador de `RegistroPage.js` podia redirigir a login despues de que el usuario ya habia navegado fuera de registro.
- Alcance aprobado: `src/js/api/url.js`, `tests/url.test.js`, `src/js/api/http.js`, `src/js/pages/RegistroPage.js` y `package.json` para agregar un script de pruebas sin eliminar los existentes.
- Contrato: aceptar URLs absolutas HTTP/HTTPS validas; rechazar configuracion vacia, valor mal formado y protocolos distintos de HTTP/HTTPS.
- Criterios: `npm test` y `npm run build` terminan correctamente; las llamadas existentes mantienen su comportamiento con la URL local valida; el redirect de registro no debe sacar de una sesion iniciada despues de navegar inmediatamente a login.
- Riesgo: bloquear el inicio si falta la configuracion requerida; el `.env.example` documenta el valor esperado.
- Estado: solicitado y registrado; la correccion del redirect esta implementada y verificada en navegador; la validacion de URL aun no esta implementada.
- Ambiente local previsto: `VITE_API_URL=http://127.0.0.1:5000/api/v1`.
