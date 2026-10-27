# Registro de errores y soluciones

Registro de incidencias encontradas durante la configuración, cómo se diagnosticaron y cómo se resolvieron.

## 2026-10-27 — Bot de Telegram borrado tras la primera configuración

**Síntoma:** se creó un bot inicial (`@Dochoadi_bot`) vía BotFather, se configuró en OpenClaw (`openclaw channels add --channel telegram --token <token>`) y quedó `connected`. Poco después, el bot fue borrado desde Telegram (BotFather), dejando la configuración de OpenClaw apuntando a un token inválido.

**Diagnóstico:**
```
openclaw channels status --probe
# → error:Unauthorized | Telegram ingress worker exited with code 1, stopped, disconnected
```

**Solución:**
1. Eliminar la cuenta de canal inválida (no se puede simplemente sobrescribir sin confirmación interactiva):
   ```
   openclaw channels remove --channel telegram --account default --delete
   ```
2. Crear un bot nuevo en BotFather (`/newbot`) → `@Kai_assistance_dochoadi_bot`.
3. Reconfigurar con el nuevo token:
   ```
   openclaw channels add --channel telegram --token <nuevo_token>
   ```

**Lección:** no borrar el bot desde BotFather mientras OpenClaw lo tiene configurado; si hace falta, borrar primero la cuenta del canal en OpenClaw.

---

## 2026-10-27 — Error "Unauthorized" tras configurar el token nuevo

**Síntoma:** después de agregar el token del bot nuevo (`@Kai_assistance_dochoadi_bot`), `openclaw channels status --probe` seguía mostrando:
```
error:Unauthorized | Telegram ingress worker exited with code 1, stopped, disconnected, transport:11m ago
```

**Diagnóstico:**
1. Se verificó el token directamente contra la API de Telegram para descartar que fuera inválido:
   ```
   curl -s "https://api.telegram.org/bot<token>/getMe"
   # → {"ok":true, "result": {... "username":"Kai_assistance_dochoadi_bot" ...}}
   ```
   El token era válido — el bot existe y responde. El problema no era el token.
2. El dato `transport:11m ago` sugería que el worker de ingestión de Telegram seguía corriendo con el proceso/token anterior en memoria (caché), sin recoger la nueva configuración.

**Solución:** reiniciar el gateway para forzar a que el worker de Telegram recargue la configuración actual:
```
openclaw gateway restart
```
Tras el reinicio, `openclaw channels status --probe` mostró `running, connected, ... works`.

**Lección:** después de cambiar el token de un canal que ya estuvo activo, reiniciar el gateway en vez de asumir que el cambio se recoge en caliente.

---

## 2026-10-27 — Pairing/emparejamiento requerido antes de usar el bot

**Síntoma:** al escribirle al bot por primera vez, Telegram respondió con un código de emparejamiento y la instrucción de aprobarlo desde el servidor:
```
Ask the bot owner to approve with:
openclaw pairing approve telegram VCBGM9ZH
```

**Diagnóstico:** `openclaw doctor` ya había advertido previamente: "No command owner is configured" — sin un operador aprobado, el bot no procesa mensajes de usuarios desconocidos.

**Solución:**
```
openclaw pairing approve telegram VCBGM9ZH
```
Esto registró automáticamente `commands.ownerAllowFrom` con el ID de Telegram del estudiante como dueño/operador del bot.

**Lección:** el primer mensaje a un canal nuevo no siempre llega directo al agente — puede requerir aprobación de "dueño" antes de empezar a conversar normalmente.

---

## 2026-10-27 — OAuth del MCP de Zapier: el callback local no es alcanzable desde el navegador del usuario

**Síntoma:** tanto `mcporter auth zapier` como `openclaw mcp login zapier` generan una URL de autorización de Zapier cuyo `redirect_uri` apunta a `http://127.0.0.1:<puerto>/...` — un servidor que levanta el propio proceso **en el VPS**. El estudiante se conecta desde su Mac/navegador, no desde el VPS, así que ese `127.0.0.1` no apunta a ningún sitio alcanzable para él.

**Intentos:**
1. Túnel SSH (`ssh -L <puerto>:127.0.0.1:<puerto> root@VPS`) — en teoría válido, pero el proceso de `mcporter auth` tiene un timeout de 300s y expiró dos veces mientras se coordinaba el túnel (incluyendo un intento donde el túnel no llegó a confirmarse y dio `ERR_CONNECTION_REFUSED` / `channel N: open failed: connect failed: Connection refused`).
2. Al reintentar sin esperar demasiado entre pasos, `mcporter auth zapier` sí completó la autorización (quedó cacheada) — confirmado porque una ejecución posterior terminó instantáneamente sin pedir nueva autorización, y en Zapier (pestaña Clients) el cliente `OpenClaw` pasó de `Pending` a `Connected`.
3. Para la autenticación propia de **OpenClaw** (`openclaw mcp login zapier`, independiente de la de `mcporter`), se usó un método más simple que no requiere túnel: el propio comando imprime en el log `Open this URL to authorize... If the callback cannot reach this terminal, run openclaw mcp login zapier --code <code>`. El estudiante autorizó en su navegador, el navegador intentó redirigir a `127.0.0.1:8989/oauth/callback?code=...` (dando error de conexión, visible pero sin problema), y se copió el valor de `code=` de la barra de direcciones para pasarlo manualmente:
   ```
   openclaw mcp login zapier --code <código_copiado_de_la_url>
   ```

**Solución final:** usar el flag `--code <código>` en vez de depender de que el callback local sea alcanzable — evita por completo la necesidad de túnel SSH.

**Lección:** cuando un flujo OAuth de un MCP corre en un servidor remoto (VPS) y el navegador está en otra máquina, buscar primero si la herramienta ofrece un modo de "pegar el código manualmente" antes de montar un túnel SSH — es más simple y más rápido.

---

## 2026-10-27 — MCP de Zapier conectado pero falla con "SSE error: Non-200 status code (405)"

**Síntoma:** tras autenticar correctamente (`openclaw mcp login zapier` exitoso), `openclaw mcp probe zapier` fallaba con:
```
SSE error: Non-200 status code (405); server unavailable
```

**Diagnóstico:** el servidor se había registrado sin especificar el transporte HTTP, y OpenClaw intentaba conectarse por defecto usando SSE (Server-Sent Events), que el endpoint de Zapier no acepta (responde 405 Method Not Allowed a ese tipo de conexión).

**Solución:** forzar el transporte correcto (`streamable-http`) al guardar la configuración del servidor:
```
openclaw mcp set zapier '{"url":"https://mcp.zapier.com/api/v1/connect","auth":"oauth","transport":"streamable-http"}'
```
Tras esto, `openclaw mcp probe zapier` mostró `zapier: 17 tools, Codex approval auto`.

**Lección:** si un MCP HTTP falla con error 405 en el handshake, revisar/forzar el tipo de transporte (`streamable-http` vs `sse`) antes de asumir que es un problema de autenticación o de red.

---

## 2026-10-27 — Captura de pantalla expuso el email de un tercero real en un repositorio público

**Síntoma:** durante una prueba inicial del flujo, se le pidió a Kai enviar un email real a una dirección de un tercero (no el estudiante) con el asunto "soy el bot de Dani es una prueba". La captura de esa conversación se subió al repositorio **sin redactar el email del destinatario**, en el commit `fe7263a`. El repositorio (`dochoadi-Configurando-tu-Agente-de-IA-Personal-con-OpenClaw`) es **público**.

**Diagnóstico:** en la revisión final de brechas se identificó que esto viola el criterio de evaluación "no se exponen credenciales ni datos personales en los materiales entregados" — el email de un tercero real, sin su consentimiento, es un dato personal, y además se le envió un correo real sin motivo legítimo más allá de la prueba.

**Solución aplicada:**
1. Se reemplazó la captura por una versión con el email del destinatario tachado/redactado visualmente.
2. **Limitación conocida y aceptada:** la versión original (sin redactar) permanece en el historial de git de ese commit anterior, ya pusheado a GitHub antes de detectarse el problema. Reescribir el historial (`git filter-repo` + `push --force`) habría sido la solución completa, pero el estudiante decidió conscientemente no hacerlo y dejar el historial como está, asumiendo que el dato estuvo expuesto brevemente en un repositorio público académico.

**Lección:** no usar direcciones de correo de terceros reales en pruebas de integraciones de email, ni siquiera como "prueba" — usar siempre una dirección propia, una de pruebas controlada (ej. un alias propio), o un servicio de email descartable. Revisar las capturas de pantalla por datos de terceros **antes** de subirlas, no después.
