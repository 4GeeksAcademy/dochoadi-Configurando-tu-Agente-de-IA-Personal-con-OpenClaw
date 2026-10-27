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
