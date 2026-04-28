# Arquitectura del Stack IT Local CMAI

## Filosofía del stack

El stack IT de CMAI se construye sobre cinco principios técnicos no negociables.

El principio Local-first establece que los datos de clientes residen en el hardware del operador y no salen a servicios externos sin aprobación explícita por operación.

El principio Agnóstico establece la existencia de una capa de abstracción entre la lógica de negocio y los modelos concretos. La sustitución de un modelo por otro debe poder realizarse modificando configuración sin modificar lógica de negocio.

El principio Modular establece que cada componente del stack es reemplazable sin romper los demás componentes.

El principio Documentado establece que cada script del sistema cuenta con un README mínimo que incluye descripción funcional, instrucciones de ejecución, y dependencias.

El principio Reproducible establece que el stack completo debe poder levantarse desde cero por un colaborador externo en menos de dos horas siguiendo la documentación de setup.

## Capas del stack en orden de dependencia

El stack se organiza en doce capas con dependencia estricta. Una capa no puede instalarse sin que las capas inferiores estén operativas.

### Capa cero: Sistema base

La capa cero es el sistema operativo. La plataforma de referencia es macOS reciente con Apple Silicon. Las herramientas básicas incluyen Xcode Command Line Tools y un terminal moderno.

### Capa uno: Gestor de paquetes del sistema

La capa uno es Homebrew como gestor de paquetes de macOS. Sirve para instalar dependencias del sistema como git, Python, Docker Compose y Ollama.

### Capa dos: Entorno Python moderno

La capa dos es uv como gestor de dependencias y entornos virtuales Python. uv reemplaza la combinación clásica de pip, venv y poetry, ofreciendo resolución de dependencias significativamente más rápida. Cada proyecto opera en su propio entorno virtual gestionado por uv.

### Capa tres: Runtime de modelos de IA

La capa tres es Ollama como gestor de modelos locales. Ollama abstrae los runtimes subyacentes (MLX en Apple Silicon, llama.cpp en otras plataformas) bajo una interfaz uniforme. Ollama opera como daemon arrancado al inicio de sesión del sistema.

### Capa cuatro: Modelos descargados

La capa cuatro consiste en los modelos de IA descargados localmente y disponibles para inferencia.

El modelo principal de generación de texto es un modelo de tipo Mixture of Experts en el rango de treinta mil millones de parámetros totales con tres mil millones activos por token. Este tipo de modelo combina velocidad de inferencia equivalente a un modelo denso pequeño con calidad de generación cercana a un modelo denso grande.

El modelo de respaldo de generación de texto es un modelo denso en el rango de setenta mil millones de parámetros, utilizado en escenarios de fallback cuando el modelo principal produce resultados insatisfactorios.

El modelo multimodal de visión es un modelo en el rango de siete mil millones de parámetros capaz de procesar imágenes y producir descripciones estructuradas en JSON. Es el modelo crítico para el pipeline del Tier PRO.

El modelo de embeddings es un modelo ligero (en torno a quinientos megabytes) optimizado para producir vectores densos a partir de texto. Es el modelo que alimenta la base vectorial ChromaDB.

### Capa cinco: Framework de fine-tuning

La capa cinco es MLX, framework nativo de Apple para Apple Silicon. MLX se utiliza para entrenamiento de adaptadores LoRA personalizados por cliente en el contexto del Tier Premium. MLX presenta eficiencia superior a PyTorch en hardware M-series por aprovechamiento de unified memory.

### Capa seis: Memoria vectorial

La capa seis es ChromaDB en modo persistente local. La base vectorial se organiza con una colección por cliente y por dominio. Los nombres de colecciones siguen el patrón `cliente_<id>_resenas`, `cliente_<id>_faqs`, `cliente_<id>_servicios`. La función de embedding se implementa como wrapper sobre el modelo de embeddings de Ollama.

### Capa siete: Framework de API

La capa siete es FastAPI servido por uvicorn. Los endpoints principales incluyen: generación de post GMB con RAG, respuesta a reseñas con tono del taller, recepción de webhook Telegram para Tier PRO, pipeline completo de generación de reseña visual, y endpoint de health check que verifica la salud de Ollama, ChromaDB y la API de Notion.

### Capa ocho: Contenedores

La capa ocho es Docker Desktop. Los contenedores se utilizan para aislar servicios que se benefician de aislamiento (n8n, Open WebUI, opcionalmente ChromaDB). Un único docker-compose.yml en `~/conversational-motors/infra/` orquesta toda la infraestructura no-Python.

### Capa nueve: Orquestación de workflows

La capa nueve es n8n self-hosted en Docker. n8n implementa los cinco workflows operativos del sistema mediante composición visual de nodos. La configuración de n8n incluye timezone Europe/Madrid y persistencia en volumen local.

### Capa diez: Captura de datos del taller

La capa diez es la API de Telegram Bot. Un único bot atiende a todos los clientes; la diferenciación se realiza por telegram_bot_chat_id registrado en Notion. La librería Python utilizada es python-telegram-bot en modo asíncrono.

### Capa once: Calidad y fallback externo

La capa once es la API externa de Anthropic Claude. Esta capa se utiliza exclusivamente para tres casos de uso: redacción de propuestas comerciales formales que requieren calidad superior, análisis estratégicos complejos como benchmarks de competidores o redacción de Confidential Information Memorandum, y fallback de calidad cuando el modelo local produce respuestas insatisfactorias en tres intentos consecutivos sobre tarea crítica.

## Router de LLM y agnosticismo de modelos

El principio de agnosticismo se materializa en un módulo router de LLM (`core/llm_router.py`) que abstrae las llamadas a modelos detrás de una enumeración de tipos de tarea.

Los tipos de tarea definidos son cuatro: HIGH_VOLUME_TEXT (volumen alto, calidad media, modelo local), QUALITY_CRITICAL (volumen bajo, calidad alta, modelo externo Claude), VISION (procesamiento de imágenes, modelo multimodal local), y EMBEDDING (vectorización de texto, modelo de embeddings local).

Cada tipo de tarea tiene asignado un proveedor (ollama o anthropic) y un nombre de modelo concreto. La lógica de negocio invoca al router con un tipo de tarea, no con un modelo concreto. La sustitución de un modelo por otro requiere modificar únicamente la tabla de configuración del router.

## Presupuesto de RAM en hardware de referencia

El hardware de referencia es un equipo con Apple Silicon serie M con 128 gigabytes de memoria unificada. El presupuesto de RAM en operación normal se distribuye aproximadamente como sigue: dieciocho gigabytes para el modelo MoE principal, ocho gigabytes para el modelo multimodal, cuatro gigabytes para ChromaDB y embeddings, tres gigabytes para Open WebUI bajo Docker, tres gigabytes para n8n bajo Docker, tres gigabytes para FastAPI y Python, y aproximadamente doce gigabytes para macOS y aplicaciones del sistema.

El total operativo en condiciones normales se sitúa en torno a cincuenta y un gigabytes, dejando un margen libre cercano a setenta y siete gigabytes. Este margen permite cargar el modelo de respaldo de setenta mil millones de parámetros (cuarenta y dos gigabytes adicionales) durante operaciones de fallback sin degradación del sistema.

## Estructura de directorios del proyecto

El proyecto raíz se ubica en `~/conversational-motors/`. La estructura de subdirectorios sigue el patrón:

El directorio `api/` contiene el código FastAPI del servicio principal.

El directorio `core/` contiene la lógica de negocio independiente de modelos, incluyendo el router LLM y el orquestador multimodal.

El directorio `integrations/` contiene los wrappers de integración con servicios externos (Notion, Telegram, Google Business).

El directorio `rag/` contiene la base ChromaDB persistente bajo el subdirectorio `chroma_db/`.

El directorio `clientes/` contiene un subdirectorio por cliente con la estructura interna `<cliente_id>/fotos/`, `<cliente_id>/data_raw/`, `<cliente_id>/assets/`, `<cliente_id>/informes/`.

El directorio `n8n/` contiene los workflows exportados como JSON bajo el subdirectorio `workflows/`.

El directorio `lora/` contiene los scripts de entrenamiento y los adaptadores LoRA por cliente del Tier Premium.

El directorio `infra/` contiene el docker-compose.yml y la configuración de infraestructura.

El directorio `docs/` contiene la documentación interna del proyecto.

El directorio `logs/` contiene los logs operativos rotados por fecha.

El directorio `scripts/` contiene scripts utilitarios de mantenimiento.

## División de roles entre herramientas

La operación diaria se reparte entre tres herramientas según el tipo de tarea.

Open WebUI conectado al modelo principal local opera como asistente personal del operador. Se utiliza para preguntas estratégicas, generación de posts GMB, redacción de respuestas a reseñas, y consulta sobre conceptos técnicos.

Claude (cliente web o desktop) con MCP conectado a Notion, Gmail y Drive opera como herramienta de operación de servicios externos. Se utiliza para crear o modificar registros en Notion CRM, leer correos electrónicos específicos, y analizar documentos en Drive.

n8n con el modelo principal local opera como motor de automatizaciones programadas. Se utiliza para auditorías recurrentes, generación masiva de contenido programada, y respuestas automáticas a triggers temporales.

## Servicios externos y costes operativos

Los servicios externos utilizados por CMAI se limitan a aquellos sin alternativa local viable. La política operativa establece que cualquier servicio externo debe poder ser sustituido por componente local cuando la madurez técnica lo permita.

Los servicios externos en categoría de uso permanente son: la API de Google Business Profile (única vía oficial para operar fichas GMB), la API de Notion (CRM operativo), y la API de Telegram Bot (canal de captura de fotografías).

Los servicios externos en categoría de uso transitorio son la API de Anthropic Claude (uso decreciente conforme aumenta calidad del modelo local), y la API de Gemini (uso residual hasta sustitución total por modelo local).

Los servicios externos rechazados explícitamente por política son n8n cloud (los datos saldrían del entorno controlado), APIs de OpenAI con coste por volumen, almacenamiento en Drive de datos sensibles de clientes, y aplicaciones beta de bajo nivel de madurez.
