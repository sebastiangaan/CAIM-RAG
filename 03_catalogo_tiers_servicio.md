# Catálogo de Tiers de Servicio CMAI

## Resumen de precios por tier

Los precios de los tiers de CMAI son: Tier Basic €250-300/mes, Tier PRO €600-700/mes, Tier Premium €1.200-1.500/mes, Tier Enterprise €3.000-5.000/mes. Todos los precios siguen una regla de cohorte: precio reducido para los primeros clientes de validación, precio pleno a partir del umbral de cohorte de cada tier. El precio no se negocia ni se reduce por presión del cliente.

## Comparativa de stack técnico por tier

El stack técnico de cada tier es acumulativo: cada tier incluye todo el stack del tier anterior más componentes adicionales. El Tier Basic usa modelo de lenguaje local, n8n como orquestador y API de Google Business Profile. El Tier PRO añade sobre el Basic un modelo multimodal local para visión (Qwen2.5-VL), base vectorial ChromaDB por cliente y bot Telegram para captura de fotografías. El Tier Premium añade sobre el PRO una LoRA fine-tuned por cliente, FastAPI multi-tenant y dashboard React con métricas en tiempo real. El Tier Enterprise añade sobre el Premium modelos V-JEPA para análisis de vídeo y firma criptográfica para certificados digitales.

## Principio del catálogo

El catálogo de productos CMAI se compone de tres tiers comerciales escalonados (Basic, PRO, Premium) y un cuarto tier condicional (Enterprise) que se activa únicamente bajo una ruta estratégica concreta de escalado con financiación externa.

El principio rector del catálogo establece que los tiers no se lanzan simultáneamente. Cada tier se lanza únicamente cuando el tier anterior ha sido validado con clientes pagando y procesos estables. Saltar este orden implica romper unit economics y generar deuda operativa.

Cada tier presenta tres dimensiones definitorias: stack técnico que lo soporta, entrega contractual al cliente, y perfil de cliente al que se dirige.

## Tier Basic

El Tier Basic se posiciona en el rango de precios entre doscientos cincuenta y trescientos euros mensuales (€250-300/mes), con escalonamiento por cohorte (precio menor para los cuatro primeros clientes en validación, precio pleno a partir del quinto cliente).

El stack técnico que soporta el Tier Basic se compone de un modelo de lenguaje local para generación de texto, orquestador n8n para automatizaciones programadas, y la API de Google Business Profile para lectura de métricas y publicación de contenido.

La entrega contractual del Tier Basic incluye cinco componentes: optimización completa de la ficha de Google My Business, dos publicaciones semanales generadas automáticamente con el system_prompt del taller, respuesta a la totalidad de reseñas recibidas, reporte mensual con métricas de GMB Insights, y proceso de solicitud de reseñas a clientes del taller.

El perfil de cliente objetivo del Tier Basic son talleres pequeños con uno o dos mecánicos que no requieren modificar su proceso operativo. El Tier Basic actúa como entrada al embudo de conversión y como punto de validación inicial del producto.

## Tier PRO

El Tier PRO se posiciona en el rango de precios entre seiscientos y setecientos euros mensuales (€600-700/mes), con escalonamiento por cohorte (precio menor para los tres primeros clientes, precio pleno a partir del cuarto cliente).

El Tier PRO constituye el diferencial principal del catálogo CMAI. Es el tier donde se materializa la metodología Reputación Visual y donde se concentra el margen operativo del negocio.

El stack técnico que soporta el Tier PRO se compone del stack del Tier Basic, ampliado con un modelo multimodal local para extracción de atributos visuales, base vectorial ChromaDB para gestión de embeddings por cliente, y bot Telegram para captura de fotografías por parte de técnicos.

La entrega contractual del Tier PRO incluye todos los componentes del Tier Basic más cuatro componentes adicionales: el Sistema de Reputación Visual que procesa fotografías antes/después, la generación de Pack diario de reseñas técnicas con SEO conversacional, las reseñas con entidades técnicas reales extraídas de cada intervención, y el proceso de onboarding ritualizado de treinta días con visita física obligatoria al taller.

El perfil de cliente objetivo del Tier PRO son talleres con tres o más mecánicos cuyo dueño está dispuesto a integrar el flujo fotográfico en la operación diaria. El Tier PRO requiere modificación del proceso del taller; un cliente que no acepta esta modificación no es candidato válido para PRO.

## Tier Premium

El Tier Premium se posiciona en el rango de precios entre mil doscientos y mil quinientos euros mensuales (€1.200-1.500/mes), con escalonamiento por cohorte (precio menor para los dos primeros clientes, precio pleno a partir del tercero).

El stack técnico que soporta el Tier Premium se compone del stack del Tier PRO, ampliado con LoRA fine-tuned por cliente entrenada sobre el corpus propietario del taller, FastAPI multi-tenant para servir endpoints específicos por cliente, y dashboard React para visualización de métricas en tiempo real.

La entrega contractual del Tier Premium incluye todos los componentes del Tier PRO más cinco componentes adicionales: LoRA personalizada entrenada sobre la voz específica del taller, agente conversacional disponible veinticuatro horas, dashboard en tiempo real con métricas operativas, análisis de competencia automatizado, y API privada accesible al cliente para integraciones propias.

El perfil de cliente objetivo del Tier Premium son talleres premium o cadenas con cinco o más centros que requieren personalización profunda y posicionamiento diferenciado. Los clientes Premium son los activos más valiosos en escenario de exit por su predictibilidad de revenue y bajo churn.

## Tier Enterprise

El Tier Enterprise es un tier condicional que se activa exclusivamente bajo la Ruta 2 de escalado (financiación seed con escalado a equipo de tres a cinco personas y desarrollo de capacidades de visión por computador para vídeo).

El Tier Enterprise se posiciona en el rango de precios entre tres mil y cinco mil euros mensuales (€3.000-5.000/mes).

El stack técnico que soporta el Tier Enterprise se compone del stack del Tier Premium ampliado con modelos de tipo V-JEPA (Video Joint-Embedding Predictive Architecture) para análisis de vídeo de procesos de reparación, y mecanismos de firma criptográfica para emisión de certificados digitales.

La entrega contractual del Tier Enterprise se centra en dos productos: certificación automatizada de procesos de reparación mediante análisis de vídeo, y emisión del Pasaporte Digital de Servicio (documento firmado criptográficamente que valida intervenciones de taller con trazabilidad técnica).

El perfil de cliente objetivo del Tier Enterprise son aseguradoras (reducción de fraude en siniestros), gestores de flotas corporativas (auditoría de mantenimiento), operadores de vehículos de segunda mano premium (validación de historial), y talleres diferenciadores que requieren credenciales digitales para sus clientes finales.

## Reglas de transición entre tiers

Un cliente puede ascender de Basic a PRO o de PRO a Premium mediante upsell explícito. La transición requiere modificación del campo tier en la base de datos Clientes Activos, registro de la transición en el campo notas operativas con motivo y fecha, y cumplimiento de los pre-requisitos del tier destino.

Un cliente no puede operar simultáneamente en dos tiers. El tier actual define el conjunto completo de servicios entregables y la tarifa mensual aplicable.

Un cliente puede descender de tier (downgrade) únicamente bajo solicitud explícita del cliente. El downgrade activa proceso de revisión por riesgo de churn.

## Pricing por cohorte

El pricing de cada tier sigue una regla de cohorte que diferencia precios entre clientes tempranos (validación) y clientes posteriores (precio pleno). Los clientes tempranos aceptan precio reducido a cambio de proporcionar testimonios y casos de estudio que CMAI utiliza para acelerar conversión de cohortes posteriores.

La regla de pricing es no negociable: el precio no se reduce por regateo del cliente. Si un cliente considera el precio elevado, la respuesta operativa es explicar el valor entregado o aceptar la pérdida del prospecto. Reducir precio por presión negociadora erosiona unit economics y comunica debilidad estructural del producto.

## Antipatrones del catálogo

CMAI rechaza explícitamente la operación bajo cinco antipatrones de catálogo.

El primer antipatrón es lanzar múltiples tiers simultáneamente. El catálogo se construye en cascada con validación previa de cada nivel.

El segundo antipatrón es ofrecer descuentos sobre tarifa establecida para cerrar venta. La tarifa se mantiene; el descuento únicamente se acepta de forma estructurada como precio de cohorte temprana.

El tercer antipatrón es ampliar entregables fuera del scope del tier para retener cliente en riesgo de churn. La retención se opera sobre calidad de entrega del scope contractual, no sobre adición de servicios no compensados.

El cuarto antipatrón es operar el Tier PRO sin onboarding ritualizado completo. El onboarding ritualizado es condición necesaria del tier, no opcional.

El quinto antipatrón es ofrecer Tier Enterprise antes de que la Ruta 2 esté formalmente activada con capital y equipo. El Tier Enterprise no figura en el catálogo de oferta estándar.
