# Notas de configuración

- Se creó el bot de Telegram con BotFather; el primer bot se borró accidentalmente y hubo que recrearlo (ver `ERRORS.md` para el detalle y la lección).
- Para el MCP de Zapier, se usó la herramienta `mcporter` (recomendada por Zapier) para la autenticación OAuth inicial, y por separado el comando propio `openclaw mcp login zapier` para la credencial que usa OpenClaw.
- El flujo OAuth intenta redirigir a un callback en `localhost` del VPS, inalcanzable desde el navegador del estudiante; se resolvió usando el modo `--code <código>` de OpenClaw (pegar manualmente el código de la URL de error) en vez de montar un túnel SSH.
- El servidor MCP de Zapier falló inicialmente con error 405 al conectar por SSE; se resolvió forzando `transport: streamable-http` en la configuración.
- Se verificaron las 3 apps conectadas (Gmail, Google Calendar, Google Docs — 63 acciones en total) antes de probar el flujo completo.
- El flujo de punta a punta se probó dos veces: una con datos completos (creó documento y evento sin preguntar nada) y otra con un mensaje ambiguo ("crea un documento en Google Docs" sin más detalles), donde el agente correctamente pidió nombre y contenido antes de actuar.
- No se encontraron archivos sensibles en el repositorio ni en el historial de git durante la verificación final de seguridad.
