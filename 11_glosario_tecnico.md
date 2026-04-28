# Glosario Técnico CMAI

## Estructura del glosario

El glosario técnico de CMAI define los términos especializados utilizados en la operación del negocio. Las definiciones cubren cuatro dominios: tecnología de IA y modelos, infraestructura técnica, métricas de SaaS, y conceptos específicos del producto.

Cada término se define de forma autocontenida, sin requerir contexto adicional para su comprensión.

## Dominio: Google y SEO

### GMB (Google My Business)

Producto de Google que permite a un negocio gestionar su ficha en Google Maps y en los resultados de búsqueda local. Es el producto principal sobre el que opera CMAI. La ficha GMB incluye: nombre del negocio, dirección, teléfono, horarios, fotografías, reseñas, posts, y categoría.

### Google Business Profile API

Interfaz de programación oficial de Google que permite leer y escribir sobre fichas GMB de forma automatizada. CMAI utiliza esta API para lectura de métricas (Insights), publicación de posts, y gestión de reseñas.

### SEO (Search Engine Optimization)

Conjunto de técnicas para mejorar la visibilidad de un sitio web o ficha en los resultados orgánicos de un motor de búsqueda. En el contexto de CMAI, el SEO se aplica sobre la ficha GMB, no sobre un sitio web.

### SEO Local

Subdisciplina del SEO orientada a búsquedas con intención geográfica explícita o implícita (consultas tipo "taller cerca de mí" o "cerrajero Barcelona"). El SEO local opera principalmente sobre fichas GMB y directorios geolocalizados.

### SEO Conversacional

En el contexto de CMAI, alineación del contenido del taller (reseñas, respuestas, publicaciones) con el lenguaje natural utilizado por usuarios en consultas de búsqueda generativa. Las consultas conversacionales contienen entidades específicas (marca, modelo, síntoma, barrio) en lenguaje natural, en contraste con keywords densas.

### Google SGE (Search Generative Experience)

Funcionalidad de Google que compone resúmenes generados por IA en la página de resultados de búsqueda. SGE extrae pasajes con entidades de fuentes diversas (reseñas, fichas, contenido web) para componer la respuesta. Las reseñas con entidades técnicas son citables por SGE; las reseñas genéricas no.

### SERP (Search Engine Results Page)

Página de resultados de un motor de búsqueda. La SERP contiene resultados orgánicos, anuncios, fragmentos enriquecidos, y resúmenes generativos cuando aplican.

## Dominio: Modelos de IA y frameworks

### LLM (Large Language Model)

Modelo de inteligencia artificial entrenado sobre grandes corpus de texto, capaz de generar texto y razonar sobre lenguaje natural. CMAI utiliza LLMs locales y, en casos específicos, LLMs vía API externa.

### Modelo MoE (Mixture of Experts)

Arquitectura de modelo de IA donde solo un subconjunto de parámetros se activa por token procesado. Un modelo MoE de treinta mil millones de parámetros totales con tres mil millones activos por token presenta velocidad de inferencia equivalente a un modelo denso de tres mil millones, con calidad de generación cercana a un modelo denso de setenta mil millones.

### Modelo denso

Arquitectura de modelo de IA donde todos los parámetros se activan en cada inferencia. Los modelos densos presentan latencia y consumo de memoria proporcional a su tamaño total.

### Modelo multimodal

Modelo de IA capaz de procesar más de una modalidad de entrada (típicamente texto e imágenes). En CMAI se utiliza un modelo multimodal en el rango de siete mil millones de parámetros para extracción de atributos técnicos de fotografías de taller.

### Embedding

Representación numérica densa (típicamente vector de varios centenares o miles de dimensiones) que codifica el significado semántico de un fragmento de texto. Los embeddings permiten búsqueda por similitud semántica en lugar de coincidencia léxica exacta.

### RAG (Retrieval Augmented Generation)

Técnica que combina recuperación de contexto desde una base vectorial con generación de texto por LLM. El flujo típico es: el query del usuario se transforma en embedding, se buscan documentos similares en la base vectorial, los documentos recuperados se inyectan como contexto en el prompt del LLM, el LLM genera respuesta condicionada por ese contexto.

### LoRA (Low-Rank Adaptation)

Técnica de fine-tuning ligero para LLMs. En lugar de actualizar todos los parámetros del modelo, LoRA entrena adaptadores pequeños de bajo rango que se aplican sobre las capas existentes. CMAI utiliza LoRA para personalizar la voz del modelo por cliente en el Tier Premium.

### Fine-tuning

Proceso de continuar el entrenamiento de un modelo base sobre un dataset específico para adaptar su comportamiento a un dominio o tarea concreta.

### System Prompt

Instrucciones de configuración que se envían al LLM al inicio de cada generación, definiendo rol, tono, restricciones y contexto operativo. En CMAI cada cliente tiene su system_prompt propio que codifica la voz del taller.

### Tool Calling (Function Calling)

Capacidad de un LLM para invocar funciones externas estructuradas durante una conversación. El modelo decide cuándo invocar una función, qué parámetros pasar, e interpreta la respuesta. CMAI utilizará tool calling en la fase de agentes operativos.

### V-JEPA (Video Joint-Embedding Predictive Architecture)

Arquitectura de IA para análisis de vídeo desarrollada por Meta. No predice tokens; predice representaciones abstractas (embeddings) en el espacio latente. Su ventaja es la construcción de world models robustos. Su limitación es que no genera texto ni imágenes. CMAI evalúa V-JEPA como base potencial del Tier Enterprise para certificación de procesos de reparación.

## Dominio: Infraestructura

### Ollama

Aplicación que gestiona la descarga y ejecución de modelos de IA locales. Ollama abstrae los runtimes subyacentes (MLX, llama.cpp) bajo una interfaz uniforme y opera como daemon en macOS.

### MLX

Framework de Apple para entrenamiento e inferencia de modelos de IA en Apple Silicon. MLX aprovecha la unified memory de los chips serie M y presenta eficiencia superior a PyTorch en hardware Apple. CMAI utiliza MLX para entrenamiento de adaptadores LoRA.

### ChromaDB

Base de datos vectorial open source que opera en modo persistente local. ChromaDB almacena embeddings y permite búsqueda por similitud. CMAI organiza ChromaDB con una colección por cliente y por dominio.

### FastAPI

Framework Python para construcción de APIs HTTP. FastAPI autogenera documentación OpenAPI, soporta operación asíncrona, y presenta rendimiento competitivo. Es el framework principal del servicio CMAI.

### Docker

Plataforma de contenedores que aísla aplicaciones y sus dependencias. CMAI utiliza Docker Desktop sobre macOS para ejecutar n8n, Open WebUI, y otros servicios sin contaminar el sistema host.

### n8n

Plataforma de automatización visual no-code basada en composición de nodos. Cada workflow es una secuencia de nodos donde cada nodo ejecuta una operación específica. CMAI opera n8n self-hosted sobre Docker.

### MCP (Model Context Protocol)

Protocolo abierto desarrollado por Anthropic que define cómo un LLM se conecta a herramientas externas (Notion, Gmail, Drive, otros) en tiempo real. El operador de CMAI utiliza MCP vía Claude para operar Notion CRM. En el futuro, MCP podrá integrarse con LLMs locales cuando el soporte madure.

### Open WebUI

Interfaz web self-hosted que se conecta a Ollama para ofrecer una experiencia tipo ChatGPT pero ejecutándose localmente. Open WebUI soporta memoria persistente nativa, RAG con documentos subidos como Knowledge Base, y soporte progresivo de MCP. Es la interfaz personal del operador mientras se construye la FastAPI propia.

### uv

Gestor moderno de paquetes y entornos virtuales para Python. Sustituye la combinación de pip, venv y poetry, ofreciendo resolución de dependencias significativamente más rápida.

### Homebrew

Gestor de paquetes para macOS que permite instalar herramientas de línea de comandos. Es la capa base sobre la que se instala el resto del stack.

## Dominio: Métricas SaaS

### MRR (Monthly Recurring Revenue)

Suma de los importes mensuales contratados por todos los clientes activos en un mes determinado.

### ARR (Annual Recurring Revenue)

MRR multiplicado por doce. Métrica de referencia para cálculos de valoración mediante múltiplo SaaS.

### ARPU (Average Revenue Per User)

MRR dividido por número de clientes activos. Mide el ticket medio del negocio.

### Churn

Porcentaje de clientes que cancelan suscripción en un periodo determinado, calculado sobre el total de clientes al inicio del periodo.

### CAC (Customer Acquisition Cost)

Coste total de adquisición de un cliente nuevo, incluyendo gastos de marketing, ventas, y tiempo del operador imputado.

### LTV (Lifetime Value)

Valor económico esperado de un cliente durante su vida útil. Estimado como ARPU multiplicado por margen, dividido por churn.

### NPS (Net Promoter Score)

Métrica de satisfacción de clientes. Se calcula como porcentaje de promotores (puntuación nueve o diez sobre diez en probabilidad de recomendación) menos porcentaje de detractores (puntuación cero a seis).

### NRR (Net Revenue Retention)

MRR del mes actual de una cohorte dividido por el MRR del mes inicial de esa cohorte. Un NRR superior al cien por ciento indica que el revenue de expansion supera al revenue perdido por churn.

## Dominio: Conceptos del producto CMAI

### Cliente Tier Basic

Taller con suscripción al servicio de optimización GMB con publicaciones programadas, respuesta a reseñas, y reporte mensual.

### Cliente Tier PRO

Taller con suscripción ampliada que incluye el sistema de Reputación Visual (procesamiento de fotografías antes/después).

### Cliente Tier Premium

Taller con suscripción premium que incluye LoRA personalizada, agente conversacional, y dashboard en tiempo real.

### Cliente Tier Enterprise

Cliente especializado (aseguradora, flota, segunda mano premium) bajo el producto de certificación de procesos con Pasaporte Digital de Servicio. Disponible solo bajo Ruta 2.

### Reputación Visual

Sistema operativo donde cada trabajo del taller produce activos digitales SEO trazables a través de fotografías antes/después procesadas por modelo multimodal.

### Pack

Objeto JSON producido por el pipeline del Tier PRO que contiene: imágenes branded, variantes de reseña generadas, mensaje WhatsApp sugerido, y metadatos de la intervención.

### Pasaporte Digital de Servicio

Certificado digital firmado criptográficamente que valida intervenciones de taller con trazabilidad técnica. Producto del Tier Enterprise condicional a Ruta 2.

### Embajador interno

Persona del taller (típicamente un técnico o el dueño) responsable de mantener la cadencia de captura fotográfica del Tier PRO. Su identificación es pre-requisito del onboarding ritualizado.

### Cliente_id

Identificador único de cliente en el sistema CMAI. Sigue formato slug del taller seguido de número correlativo. Es la clave primaria que enlaza Notion, filesystem local, y bases vectoriales ChromaDB.

### Workflow

Automatización concreta implementada en n8n. CMAI opera cinco workflows numerados por orden de implementación.

### Onboarding ritualizado

Proceso estructurado de incorporación de un cliente Tier PRO con duración de treinta días. Incluye visita física, formación de técnicos, identificación del embajador interno, e iteración del system_prompt.
