# Tracking — Configurando tu Agente de IA Personal con OpenClaw

Última actualización: 2026-10-27

## Actividad 1 — Acceso al VPS y configuración básica de OpenClaw

| Tarea | Estado | Notas |
|---|---|---|
| Conexión SSH al VPS | ✅ Completado | Confirmado por el estudiante (no consola web) |
| Confirmar OS y recursos | ✅ Completado | Ubuntu 22.04.5 LTS, host `bc-vps-457` |
| Instalar OpenClaw | ✅ Completado | OpenClaw `2026.10.7`, gateway activo en puerto `18789` |
| Configurar proveedor LiteLLM | ✅ Completado | `baseUrl: https://llm.4geeks.ai/v1`, modelo `madrid-spain/openrouter/deepseek/deepseek-v4-flash` |
| Ingresar API Key válida | ✅ Completado | Perfil `litellm:default` configurado (valor no expuesto) |
| Saltar configuración de Skills | ✅ Completado | Según guía |
| Habilitar hooks | ✅ Completado | 5/5 hooks en estado "ready" |
| Saltar Channel Workflows | ✅ Completado | Según guía |
| Abrir Try Local Chat y probar | ✅ Completado | Prueba vía CLI: el modelo respondió `"OK FUNCIONANDO"` |

## Actividad 2 — Personalización del Asistente (IDENTITY.md)

| Tarea | Estado | Notas |
|---|---|---|
| Abrir interfaz de chat local | ✅ Completado | — |
| Pedir a OpenClaw configurar Name | ✅ Completado | `Name: Kai` en `IDENTITY.md` |
| Pedir a OpenClaw configurar Emoji | ✅ Completado | `Emoji: 🤖` en `IDENTITY.md` |
| Pedir a OpenClaw configurar Greeting | ✅ Completado | Guardado como directiva en `USER.md` (esta versión de OpenClaw no usa un campo `Greeting` en `IDENTITY.md`): "¡Hola, soy Kai, ¿en qué puedo ayudarte Sr. Ochoa!" |
| Verificar personalización aplicada | ✅ Completado | Verificado leyendo `IDENTITY.md` y `USER.md` directamente |

## Actividad 3 — Respaldo en GitHub

| Tarea | Estado | Notas |
|---|---|---|
| Crear SSH key en el servidor | ✅ Completado | `~/.ssh/id_ed25519` (ed25519, sin passphrase) |
| Añadir SSH key a GitHub | ✅ Completado | Confirmado por el estudiante |
| Repositorio de respaldo en GitHub | ✅ Completado (decisión documentada) | No se creó `openclaw-dochoadi` como repo nuevo. Se usó el repo ya asignado por el classroom de 4Geeks (`dochoadi-Configurando-tu-Agente-de-IA-Personal-con-OpenClaw`, público), para no duplicar entregables ni romper la asociación de GitHub Classroom. El nombre `openclaw-<usuario>` de la guía es solo un ejemplo ilustrativo |
| Conectar workspace (`~/.openclaw/workspace`) a GitHub | ✅ Completado | Copiado a `.openclaw/workspace/` dentro de este mismo repo (vía `rsync`, excluyendo `.git`); verificación de seguridad hecha antes de copiar (sin `openclaw.json`, `.env`, tokens ni credenciales) |
| Verificar subida (captura de pantalla) | ✅ Completado | Push confirmado (`8f8c447..11ec34a`); captura tomada por el estudiante en `.openclaw/workspace` del repo |

## Actividad 4 — Verificación de brechas

| Acción | Estado | Notas |
|---|---|---|
| Revisión de checklist completo (imágenes 4-5) | ✅ Completado hoy | Ver tabla de brechas abajo |
| Repo de documentación creado | ✅ Completado | `4GeeksAcademy/dochoadi-Configurando-tu-Agente-de-IA-Personal-con-OpenClaw` |
| README.md / CONTEXT.md | ✅ Completado | Creados en este commit |

### Verificación final contra los 12 criterios de evaluación

| # | Criterio | Estado | Nota |
|---|---|---|---|
| 1 | OpenClaw instalado y accesible en el VPS | ✅ | Verificado, versión `2026.10.7` |
| 2 | LiteLLM configurado y conectado a modelo funcional | ✅ | Probado con mensaje real |
| 3 | Chat local devuelve respuesta válida | ✅ | `"OK FUNCIONANDO"` |
| 4 | Repo `openclaw-<tu_usuario>` existe y es accesible | ⚠️ Riesgo asumido | Se usó el repo ya asignado por el classroom de 4Geeks en vez de crear uno nuevo con ese nombre literal, para no duplicar entregables ni romper la asociación de GitHub Classroom. **Riesgo aceptado conscientemente por el estudiante** |
| 5 | Workspace subido a GitHub | ✅ | `.openclaw/workspace/` en el repo, commit `11ec34a` |
| 6 | `IDENTITY.md` con Name, Emoji y Greeting | ⚠️ Riesgo asumido | Name y Emoji están en `IDENTITY.md`. El Greeting quedó guardado en `USER.md`, no en `IDENTITY.md` — limitación de esta versión de OpenClaw (su plantilla no tiene campo `Greeting`). **Riesgo aceptado conscientemente por el estudiante** |
| 7 | Personalización vía conversación, no edición manual | ✅ | Confirmado — Name, Emoji y Greeting se configuraron hablando con OpenClaw |
| 8 | SSH configurado para git push | ✅ | Clave `~/.ssh/id_ed25519` generada y probada contra GitHub |
| 9 | Historial de commits muestra subida del workspace | ✅ | Commit `11ec34a` |
| 10 | Sin archivos sensibles subidos | ✅ | Verificado archivo por archivo y en todo el historial de git (sin `openclaw.json`, `.env`, tokens ni credenciales) |
| 11 | Conexión por SSH (no consola web) | ✅ | Confirmado por el estudiante |
| 12 | Pasos en el orden correcto de la guía | ✅ | Seguido en orden: Actividad 1 → 2 → 3 → 4 |

**Decisión final:** 10/12 criterios cumplidos sin reservas; 2/12 cumplidos con una desviación documentada y un riesgo de interpretación de rúbrica que el estudiante decidió asumir conscientemente en vez de forzar una solución artificial (crear un repo redundante, o editar `IDENTITY.md` a mano para simular un campo que OpenClaw no soporta).

## Checklist de seguridad final (antes de cualquier push)
- [x] Sin `openclaw.json` / `.env` en el workspace
- [x] Sin API keys o tokens en texto plano
- [x] `.gitignore` presente y verificado
- [x] Historial completo de git escaneado, sin secretos

---

# Conecta tu Agente: Telegram, Google Docs y Calendario

Última actualización: 2026-10-27

## Paso 1 — Conexión con Telegram

| Tarea | Estado | Notas |
|---|---|---|
| Crear bot con BotFather y obtener token | ✅ Completado | Bot final: `@Kai_assistance_dochoadi_bot` (un bot anterior se creó y se borró accidentalmente — ver `ERRORS.md`) |
| Configurar OpenClaw para usar Telegram | ✅ Completado | `openclaw channels add --channel telegram --token <token>` |
| Aprobar emparejamiento (command owner) | ✅ Completado | `openclaw pairing approve telegram VCBGM9ZH` — registró al estudiante como `commands.ownerAllowFrom` |
| Validar la conexión con mensaje de prueba | ✅ Completado | Ver `openclaw-connection/01-telegram-conectado.png` — Kai respondió con el saludo personalizado y confirmó estar funcionando |

## Paso 2 — Configuración del MCP de Zapier

| Tarea | Estado | Notas |
|---|---|---|
| Crear cuenta gratuita en Zapier | ✅ Completado | Servidor "4Geeks MCP Server" con apps Google Docs, Google Calendar, Gmail |
| Generar URL de endpoint MCP | ✅ Completado | `https://mcp.zapier.com/api/v1/connect` |
| Agregar el MCP al agente en OpenClaw | ✅ Completado | `openclaw mcp add zapier --url ... --auth oauth`, autenticado con `mcporter` + `openclaw mcp login --code`; hubo que forzar `transport: streamable-http` (ver `ERRORS.md`) |
| Conectar y verificar Google Docs | ✅ Completado | Verificado: el agente lista 19 acciones y creó un documento real (`pruebabot`) — ver Paso 3 |
| Conectar y verificar Google Calendar | ✅ Completado (listado) | El agente lista 22 acciones de Calendar; creación real de evento pendiente de probar en Paso 3 |

## Paso 3 — Flujo completo de punta a punta

| Tarea | Estado | Notas |
|---|---|---|
| Enviar mensaje por Telegram pidiendo crear un documento | ✅ Completado | "Crea un documento en Google docs q se llame pruebabot" — ver `openclaw-connection/03-documento-creado-google-docs.png` |
| Verificar que el agente pide info adicional si falta | ✅ Completado | Prueba adicional con mensaje ambiguo ("Crea un documento en Google docs", sin más datos) — Kai preguntó por nombre y contenido antes de crear nada; ver `openclaw-connection/05-agente-pide-info-faltante.png` |
| Verificar creación del documento en Google Docs | ✅ Completado | Documento real creado y enlazado: `pruebabot`, propietario `dochoadi@gmail.com` |
| Verificar creación del evento en el calendario | ✅ Completado | Evento "Manicura" creado el 28/10/2026 15:00-16:00 en `dochoadi@gmail.com` — ver `openclaw-connection/04-evento-creado-google-calendar.jpg` |
| Confirmar mensaje de finalización por Telegram | ✅ Completado | Kai confirmó cada acción con ✅ y resumen estructurado |
| **Flujo combinado real** (un solo mensaje → documento + evento vinculado → una confirmación) | ⚠️ Completado con riesgo asumido | Se probó con: *"Crea un fichero Excel que se llame Dashboard de empresa y reserva en el calendario para el 17 de noviembre para felicitar a Dani por cumpleaños"* → Kai creó ambas cosas en paralelo y confirmó todo en un solo mensaje (ver `openclaw-connection/06-flujo-combinado-solicitud.png` y `07-flujo-combinado-confirmacion.png`). **Dos matices frente al criterio literal de la guía:** (1) se usó **Google Sheets**, no Google Docs; (2) el evento de calendario fue para un recordatorio de cumpleaños, no literalmente "tiempo para revisar el documento". El estudiante decidió conscientemente que esta prueba es equivalente en patrón técnico (una solicitud → múltiples acciones relacionadas → una confirmación) y no repetir con los términos exactos de la guía |
## Paso 4 — Evidencias y entrega

| Tarea | Estado | Notas |
|---|---|---|
| Carpeta `openclaw-connection/` creada | ✅ Completado | Dentro de este mismo repo |
| Captura: Telegram conectado | ✅ Completado | `openclaw-connection/01-telegram-conectado.png` |
| Captura: MCP Zapier conectado | ⏳ Pendiente | Se vio en pantalla (estado "OpenClaw — Connected" en Zapier) pero no se guardó archivo; opcional, ya cubierto funcionalmente por las demás capturas |
| Captura: conversación completa en Telegram | ✅ Completado | `openclaw-connection/02-conversacion-telegram-email.png` — **email del destinatario tachado/redactado** tras detectar en la revisión final que exponía el dato personal de un tercero real sin su consentimiento (ver nota de privacidad abajo) |
| Captura: documento creado en Google Docs | ✅ Completado | `openclaw-connection/03-documento-creado-google-docs.png` |
| Captura: evento creado en el calendario | ✅ Completado | `openclaw-connection/04-evento-creado-google-calendar.jpg` |
| Captura: agente pide info faltante | ✅ Completado | `openclaw-connection/05-agente-pide-info-faltante.png` |
| Captura: flujo combinado (solicitud) | ✅ Completado | `openclaw-connection/06-flujo-combinado-solicitud.png` |
| Captura: flujo combinado (confirmación) | ✅ Completado | `openclaw-connection/07-flujo-combinado-confirmacion.png` |
| `notes.md` con decisiones/problemas | ✅ Completado | `openclaw-connection/notes.md` |

## Verificación final contra "Qué vamos a evaluar"

| # | Criterio | Estado | Nota |
|---|---|---|---|
| 1 | Bot de Telegram correctamente creado y conectado | ✅ | `@Kai_assistance_dochoadi_bot` |
| 2 | MCP de Zapier agregado y activo en la configuración del agente | ✅ | `openclaw mcp show zapier` → 17 herramientas; sin captura dedicada del panel (cubierto funcionalmente) |
| 3 | Integración con Google Docs: el agente crea un documento en respuesta a la solicitud | ✅ | `pruebabot` |
| 4 | Integración con Google Calendar: el agente crea un evento vinculado a la revisión del documento | ⚠️ Riesgo asumido | Se verificó el patrón técnico (una solicitud → documento + evento → una confirmación) con Google Sheets + evento de cumpleaños, no literalmente "Google Docs + revisión del documento". Decisión consciente del estudiante, documentada aquí en vez de forzar una repetición idéntica a la guía |
| 5 | El agente confirma la finalización de la tarea por Telegram | ✅ | En todas las pruebas |
| 6 | El agente solicita información faltante cuando el mensaje no es suficientemente específico | ✅ | Ver captura 05 |
| 7 | Las capturas documentan cada integración y la conversación completa de punta a punta | ✅ | 7 capturas + notes.md |
| 8 | No se exponen credenciales ni datos personales en los materiales entregados | ✅ (corregido) | Se detectó y corrigió en revisión final: una captura exponía el email real de un tercero (destinatario de un email de prueba) — se redactó/tachó antes de la entrega. El email propio del estudiante (`dochoadi@gmail.com`) se mantiene visible por ser evidencia esperada de su propia cuenta conectada |

**Decisión final:** 7/8 criterios cumplidos sin reservas; 1/8 (#4) cumplido con un riesgo de interpretación documentado y aceptado conscientemente por el estudiante, igual que en la actividad anterior.

## Registro de errores de esta actividad

Ver [`ERRORS.md`](./ERRORS.md).
