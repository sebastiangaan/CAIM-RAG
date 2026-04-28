# 00 · Contexto Actual

> **Yo edito solo este archivo.** Cada noche al cerrar la jornada, en 30 segundos:
> 1. Cambio la **fecha de hoy** y el **día con Mac**.
> 2. Añado una línea nueva a la **bitácora** (1-2 frases del avance del día).
> 3. Si algo nuevo ha quedado **operativo**, lo añado a esa lista.
>
> Con eso basta. El LLM compara este documento con el roadmap completo (`CMAI_Consciencia_Permanente_v3.1.md` sección 16) y me dice qué toca hoy.

---

## Hoy

- **Fecha:** 26 abril 2026 (domingo)
- **Día con Mac:** 4

---

## Bitácora (append-only · 1-2 líneas por día)

- **Día 4 · 26 abr** · Open WebUI configurado con Knowledge Base CMAI. Notion auditado vía Claude+MCP. Campo Barrio añadido al CRM. Documentación CMAI consolidada a v3.1.
- **Día 3 · 25 abr** · Qwen3:30b-a3b descargado. Open WebUI instalado en localhost:3000.
- **Día 2 · 24 abr** · Llama 3.3 70B descargada. Revisión estructura Notion existente.
- **Día 1 · 23 abr** · Mac recibido. Ollama instalado. Primer test de modelos locales.

---

## Lo que está operativo

**Hardware**
- MacBook Pro M5 Max 128GB · macOS actualizado · FileVault activado

**Modelos en Ollama**
- qwen3:30b-a3b (principal)
- llama3.3:70b (backup)

**Interfaces**
- Open WebUI en `localhost:3000` con workspace CMAI
- Knowledge Base "CMAI Core" con CMAI_Consciencia_Permanente_v3.1.md y este archivo
- System prompt configurado con rol CTO+COO

**Notion (operado vía Claude+MCP)**
- 6 bases: CRM Talleres · Clientes Activos · Auditorías GMB · Entregas mensuales · Finanzas · Tareas
- 3 páginas: Plantillas · Claves API · Prompt CEO
- CRM con campo Barrio añadido (10 zonas Barcelona)

**APIs y cuentas**
- Claude Pro con MCP activo (Notion / Gmail / Drive)
- Anthropic API key
- Google Cloud Console + GMB API activada
- Telegram Bot Token generado
- Gemini API (residual)

**Comercial**
- Propuesta Basic 1 página PDF (Canva)
- 50 talleres pre-identificados en lista (sin meter en CRM aún)
- Top 10 con gmb_place_id

---

## Identidad (fija · no cambia)

- **Empresa:** Conversational Motors AI (CMAI)
- **Fundador:** Sebastian, 25 años, Barcelona
- **Negocio:** posicionamiento GMB para talleres con IA local
- **Planes:** €250-300 Basic / €600-700 PRO / €1.200-1.500 Premium
- **Horario:** turno nocturno 23:30-7:30 · productivo 14:00-22:00 · sueño 8:00-14:00
- **Objetivo 3 meses:** 3 clientes firmados · alta autónomo · €2.000 MRR
- **Objetivo 27 años:** 100 clientes · €1M patrimonio · venta empresa o ruta Seed

---

## Instrucciones para el LLM al iniciar sesión

Cuando empiece una conversación nueva:

1. **Lee la fecha de hoy** y el día con Mac de la sección "Hoy".
2. **Lee la última línea de la bitácora** para saber qué hice ayer.
3. **Lee qué está operativo** en este documento.
4. **Compara con el roadmap** de `CMAI_Consciencia_Permanente_v3.1.md` sección 16 (Fases 0-7) y sección 23 (checklist maestro).
5. **Identifica el gap:** lo que el roadmap dice que debería estar hecho a estas alturas vs lo que realmente está operativo.
6. **Dime qué toca hoy** priorizando:
   - Primero, lo comercial si hay clientes en juego (sección 14, Plan de Asalto).
   - Segundo, lo técnico crítico (lo que bloquea la siguiente fase).
   - Nunca optimizaciones prematuras ni tareas de fases futuras si hay pendientes ahora.
7. **Termina siempre con `Next action:`** — una sola tarea ejecutable hoy.

Si Sebastian pide algo distinto, responde a eso. Si pregunta "¿qué toca?" o "¿por dónde sigo?" o similar, ejecuta los 7 pasos.

---

**Última edición:** 26 abril 2026 · 21:00
