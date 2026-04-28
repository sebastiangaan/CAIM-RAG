

El LLM compara este documento con el roadmap completo (`CMAI_Consciencia_Permanente_v3.1.md` sección 16) y me dice qué toca hoy.


---

## Bitácora (append-only · 1-2 líneas por día)

- **Día 5 · 27 abr** · Construcción y validación del corpus RAG de CMAI: 13 documentos atemporales indexados en Open WebUI con nomic-embed-text, resolviendo problemas de configuración hasta conseguir retrieval funcional 
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

Historico de mas reciente a mas antiguo.


## Día 27 abril 2026 · Sesión RAG + Open WebUI

### Lo que se hizo

**Generación del corpus RAG CMAI:**
- Se generaron 13 documentos markdown optimizados para embeddings semánticos.
- Conocimiento estable y atemporal extraído del CMAI_Consciencia_Permanente_v3.1.md.
- Documentos cubren: definición empresa, tesis SEO, catálogo tiers, metodología PRO, 
  arquitectura stack, modelo CRM, workflows, funnel comercial, exit strategy, 
  KPIs, glosario técnico y reglas de operación.
- Validados contra referencias temporales y estados dinámicos (grep limpio).

**Configuración Open WebUI Knowledge Base:**
- Engine de embeddings: Ollama → nomic-embed-text.
- API Ollama: http://host.docker.internal:11434.
- Chunk size: 1000 tokens / Overlap: 200 / Lote de incrustación: 32.
- Knowledge Base: "CMAI Core RAG" con los 13 documentos indexados.
- Workspace: "CMAI" con System Prompt configurado.
- Búsqueda web: desactivada.
- Code Interpreter: desactivado.

**Problemas encontrados y resueltos:**
1. Embeddings mezclados (Sentence Transformers + nomic) → KB eliminada y reindexada limpia.
2. Precios en palabras ("seiscientos euros") no matcheaban queries numéricas → 
   añadidos precios en formato €600-700 + chunk resumen al inicio de cada sección crítica.
3. Chunk size pequeño separaba headers de contenido → subido a 1000/200.
4. Lote de incrustación=1 causaba timeouts silenciosos → cambiado a 32.
5. Modelo respondía en inglés con datos inventados (Dallas, Ford, dólares) → 
   era Qwen3 sin workspace activo. Resuelto usando # para adjuntar KB manualmente.

**Tests de validación pasados:**
- ✅ 4 moats competitivos: recuperados correctamente.
- ✅ Stack técnico PRO vs Premium: recuperado correctamente.
- ✅ Regla de precio no negociable: recuperada correctamente.

**Lección técnica clave:**
Cualquier concepto con número exacto ("cuatro moats", "tres tiers") necesita un 
párrafo resumen compacto al inicio de su sección. Si el detalle está en chunks 
separados, el retriever puede perder alguno.

### Estado del stack al cierre del día

- ✅ Open WebUI operativo con RAG funcional.
- ✅ Knowledge Base CMAI Core RAG indexada y validada.
- ✅ nomic-embed-text como modelo de embeddings local.
- ⚠️ Hay que usar # manualmente en cada chat para adjuntar la KB 
  (bug de vinculación automática del workspace en esta versión de Open WebUI).
- ⚠️ Fuentes citadas no visibles en la interfaz (cosmético, no afecta retrieval).
- ✅ Qwen3:30b-a3b funciona igual que Llama 3.3 con la KB correctamente.



