# Contexto del ejercicio

## Objetivo
Desplegar y configurar una instancia funcional de OpenClaw en un VPS propio, conectarla a un modelo de IA mediante LiteLLM, y verificar que responde correctamente en el chat local — un asistente de IA autoalojado y privado, sin depender de plataformas de terceros.

## Alcance
- No es un proyecto de código: es una tarea de infraestructura y configuración.
- El resultado principal es un sistema funcionando, documentado con evidencia (capturas, configuración no sensible, historial de commits).

## Entorno
- VPS: Ubuntu 22.04.5 LTS
- Herramienta: OpenClaw `2026.10.7`
- Proveedor de modelo: LiteLLM (`https://llm.4geeks.ai/v1`)

## Criterios de evaluación
- OpenClaw instalado y accesible en el VPS.
- Proveedor LiteLLM configurado y conectado a un modelo funcional.
- Chat local devuelve respuesta válida.
- Repositorio `openclaw-<usuario>` existe y es accesible.
- `~/.openclaw/workspace` subido exitosamente a GitHub.
- `IDENTITY.md` presente con Name, Emoji y Greeting personalizados.
- Personalización hecha conversando con OpenClaw, no editando archivos a mano.
- SSH configurado para `git push` desde el VPS.
- Historial de commits muestra la subida del workspace.
- Sin archivos sensibles (API keys, tokens, credenciales) subidos.
- Conexión al VPS por SSH (no consola web/gráfica).
- Pasos seguidos en el orden de la guía de 4Geeks.

## Verificación final de seguridad antes de entregar
El workspace no debe contener: `openclaw.json`, archivos `.env`, API keys/tokens, ni credenciales. Si se sube algo sensible por error: eliminar el repo, limpiar el workspace, recrear el repo y contactar al instructor.
