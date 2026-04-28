# Índice del Knowledge Base CMAI para RAG

## Propósito de este corpus

Este corpus de documentos constituye la base de conocimiento estable de Conversational Motors AI (CMAI) optimizada para indexación en una base vectorial y consumo por modelos de lenguaje vía RAG (Retrieval Augmented Generation).

El corpus contiene exclusivamente conocimiento atemporal: definiciones, estructuras, procesos, reglas y conceptos. No contiene estados dinámicos del negocio (clientes activos, MRR actual, fase actual del roadmap, tareas pendientes) que se inyectan externamente en tiempo de ejecución desde el sistema operativo (Notion CRM, dashboard, base de datos de estado).

## Separación de capas de conocimiento

El sistema CMAI opera bajo una arquitectura de tres capas de conocimiento.

La capa estable (este corpus) define qué cosas existen y cómo funcionan. Sus documentos son válidos sin referencia a un momento específico en el tiempo.

La capa dinámica (Notion CRM y bases de datos operativas) define el estado actual del negocio: qué clientes están activos, qué prospectos están en cada fase, qué tareas están pendientes, qué reportes se han generado.

La capa táctica (sesiones de chat, planes semanales, decisiones del operador) combina las dos anteriores para producir acciones concretas en momentos específicos.

Este corpus es la capa estable. Los documentos están diseñados para ser indexados una vez y consultados por embeddings semánticos sin necesidad de regeneración por cambios en el estado del negocio.

## Documentos del corpus

El corpus se compone de doce documentos independientes y autocontenidos.

### Documento 01: Definición de Conversational Motors AI

Define la identidad de la empresa, su propuesta de valor, los siete pilares de filosofía corporativa, la visión estratégica, el modelo de negocio SaaS vertical, los cinco principios técnicos fundacionales, y el posicionamiento comercial.

### Documento 02: Tesis SEO Conversacional y Reputación Visual

Define el SEO Conversacional como metodología, define el sistema de Reputación Visual con sus cinco fases, explica los mecanismos por los que Google premia este contenido, describe los cuatro moats competitivos de CMAI, define el KPI canario que valida la tesis, y enumera los antipatrones rechazados.

### Documento 03: Catálogo de Tiers de Servicio

Define el principio de cascada del catálogo, especifica los Tiers Basic, PRO, Premium y Enterprise con su stack técnico, entrega contractual y perfil de cliente. Define las reglas de transición entre tiers, el pricing por cohorte, y los antipatrones del catálogo.

### Documento 04: Metodología Tier PRO — Pipeline de Reputación Visual

Describe el pipeline operativo del Tier PRO desde captura fotográfica del técnico hasta entrega del Pack al dueño. Especifica los nueve pre-requisitos de un cliente Tier PRO, las once etapas del pipeline, los SLAs operativos, los cinco KPIs específicos, y las fases del onboarding ritualizado de treinta días.

### Documento 05: Arquitectura del Stack IT Local

Describe la filosofía local-first y agnóstica del stack, las doce capas en orden de dependencia (sistema base, gestores, runtime, modelos, fine-tuning, vectorial, API, contenedores, orquestación, captura, calidad externa), el router LLM como materialización del agnosticismo, el presupuesto de RAM en hardware de referencia, la estructura de directorios del proyecto, y las políticas de servicios externos.

### Documento 06: Modelo de Datos del CRM en Notion

Define el principio de gobernanza con Notion como single source of truth operativo, especifica las bases de datos (CRM de prospectos, Clientes Activos, Reportes Mensuales, Tareas, Finanzas, Plantillas) con todos sus campos y tipos, define los triggers de IA sobre cada base, describe el wrapper de integración Python, las reglas de integridad de datos, y los campos críticos para alta de cliente Tier PRO.

### Documento 07: Workflows de Automatización

Define los cinco workflows operativos implementados en n8n: Workflow 1 (Alerta de reseñas), Workflow 2 (Posts GMB semanales), Workflow 3 (Reporte mensual), Workflow 4 (Tier PRO Receptor), Workflow 5 (Tier PRO Publicación). Especifica nodos, condiciones de adopción, y reglas operativas comunes. Define la estrategia futura de agentes con tool calling.

### Documento 08: Modelo Comercial y Funnel de Ventas

Define la estructura del funnel comercial con sus seis etapas y tasas de conversión esperadas. Especifica los criterios de filtrado de prospectos (eliminatorios y puntuadores), la estructura de los tres mensajes del funnel, los criterios rojos para no firmar, el pricing por cohorte, el canal de referidos, los materiales comerciales por nivel temporal, los antipatrones comerciales, y las métricas semanales del funnel.

### Documento 09: Estrategia de Salida y Venta de la Empresa

Define el horizonte temporal y la valoración objetivo del exit. Describe la composición del activo vendido (cartera de contratos, propiedad intelectual, playbooks operativos, infraestructura). Define los cuatro tipos de comprador con sus perfiles. Describe el cronograma del proceso de venta en cinco fases. Especifica la estructura del data room. Define los factores de justificación de múltiplo alto. Diferencia las Rutas A y B.

### Documento 10: KPIs y Métricas Operativas

Define el sistema de métricas en cuatro niveles. Especifica las métricas SaaS estándar (MRR, ARR, ARPU, churn, CAC, LTV, NRR, NPS), los siete KPIs del dashboard semanal, los cinco KPIs específicos del Tier PRO, el KPI canario de validación de tesis SEO, las métricas semanales del funnel, las métricas financieras mensuales, las métricas de auditoría técnica, las métricas de cohorte para due diligence, y los rituales de revisión.

### Documento 11: Glosario Técnico

Define los términos especializados de cuatro dominios: Google y SEO (GMB, SERP, SGE), Modelos de IA y frameworks (LLM, MoE, multimodal, embedding, RAG, LoRA, fine-tuning, system prompt, tool calling, V-JEPA), Infraestructura (Ollama, MLX, ChromaDB, FastAPI, Docker, n8n, MCP, Open WebUI, uv, Homebrew), y Métricas SaaS estándar. Incluye también conceptos específicos del producto CMAI.

### Documento 12: Reglas de Operación y Antipatrones

Define las diez reglas no negociables del negocio. Especifica los antipatrones operativos generales, antipatrones técnicos, antipatrones comerciales, antipatrones de pricing, antipatrones de catálogo, y antipatrones de la tesis SEO. Define también las reglas operativas del asistente de IA que apoya al operador.

## Recomendaciones de uso para sistema RAG

### Estrategia de chunking

Los documentos están redactados con párrafos autocontenidos (cada párrafo se entiende sin contexto adicional), facilitando estrategias de chunking por párrafo, por subsección, o por documento completo según el caso de uso.

### Términos clave repetidos

Los términos clave (cliente_id, tier, system_prompt, Tier PRO, Reputación Visual, SEO Conversacional, moat, churn) aparecen explícitamente y de forma repetida en los documentos, mejorando la calidad de retrieval por similitud semántica.

### Independencia entre documentos

Cada documento puede consultarse de forma independiente sin requerir contexto de otros documentos del corpus. Las referencias cruzadas se realizan mediante términos canónicos definidos en el glosario, no mediante referencias a secciones específicas.

### Datos no incluidos en este corpus

El corpus no incluye: estado actual del negocio (clientes específicos, MRR actual, etapa actual del roadmap), datos personales de prospectos o clientes, credenciales o secretos operativos, contenido específico de un cliente individual (system_prompts particulares, fotografías), historial de conversaciones operativas.

Esta información debe inyectarse externamente en tiempo de ejecución desde las fuentes operativas correspondientes (Notion CRM, base de datos de estado, sistema de archivos de cliente).

## Política de actualización del corpus

El corpus se actualiza cuando se modifica conocimiento estable de la empresa: nueva metodología, nuevo tier de servicio, modificación de la estructura de datos del CRM, redefinición de KPIs, cambio de stack técnico, ajuste de reglas operativas.

El corpus no se actualiza por cambios en el estado del negocio (nuevo cliente firmado, cambio de fase, nueva tarea, métrica del mes). Estos cambios viven en la capa dinámica y no requieren reindexación del knowledge base.
