# Modelo de Datos del CRM en Notion

## Principio de gobernanza de datos

Notion opera como single source of truth operativo para el estado de negocio. Toda información que el sistema local necesita conocer del negocio reside en Notion: clientes activos y su tier, voz de cada cliente codificada en system_prompt, métricas registradas por cliente, y fase del pipeline de cada prospecto.

Los datos técnicos asociados a operaciones (fotografías, embeddings, modelos LoRA, logs) residen en disco del Mac local. Notion no almacena estos datos directamente; almacena referencias a su ubicación y estado de procesamiento.

La separación entre Notion (estado de negocio) y filesystem local (datos técnicos pesados) constituye un contrato de datos explícito que el sistema respeta en todas sus operaciones.

## Estructura de bases de datos

El CRM de CMAI en Notion se organiza en bases de datos relacionadas. Las bases principales son: CRM de prospectos, Clientes Activos, Auditorías GMB, Reportes Mensuales (también denominados Entregas Mensuales), Tareas, Finanzas, Plantillas, y un Dashboard como vista agregada.

Adicionalmente existen páginas no-base que actúan como repositorios de configuración: Plantillas (contenido reutilizable), Claves API (secretos centralizados con acceso restringido), y Prompt CEO (contexto de inicio para sesiones de chat con LLMs).

## Base CRM de prospectos

La base CRM de prospectos gestiona el pipeline desde identificación de un taller hasta su firma como cliente. Todo taller nuevo entra en esta base.

Los campos definidos son los siguientes.

El campo `taller_nombre` (Title) almacena el nombre comercial del taller.

El campo `gmb_place_id` (Rich text) almacena el identificador único de Google Places. Es necesario para auditorías automáticas.

El campo `gmb_url` (URL) almacena el enlace directo a la ficha de Google Maps.

El campo `telefono` (Phone) almacena el teléfono principal del taller.

El campo `whatsapp` (Phone) almacena el WhatsApp de contacto, preferentemente del dueño.

El campo `email` (Email) almacena el email de contacto.

El campo `direccion` (Rich text) almacena la dirección postal incluyendo barrio.

El campo `barrio` (Select) categoriza al taller por barrio de operación. Los valores típicos para Barcelona incluyen Gracia, Eixample, Sants, Sant Andreu, Nou Barris, Horta-Guinardó, Les Corts, Sarrià-Sant Gervasi, Poblenou. Es un filtro clave para segmentación.

El campo `rating_gmb` (Number) almacena el rating actual de la ficha en escala de cero a cinco.

El campo `num_resenas` (Number) almacena la cantidad total de reseñas.

El campo `estado_pipeline` (Select) categoriza la fase del prospecto en el pipeline. Los valores definidos son: Identificado, Contactado, Interesado, Auditoría enviada, Llamada programada, Propuesta, Firmado, Rechazado, No responde.

El campo `nota_potencial` (Number) almacena un rating interno del prospecto en escala de uno a diez.

El campo `fecha_primer_contacto` (Date) registra el primer WhatsApp o llamada al prospecto.

El campo `fecha_ultima_accion` (Date) registra la última interacción y se usa como filtro para detectar prospectos estancados.

El campo `razon_rechazo` (Rich text) registra el motivo cuando el estado del pipeline es Rechazado, alimentando aprendizaje para futuros prospectos.

El campo `notas` (Rich text) registra notas libres sobre el prospecto.

## Triggers de IA sobre la base CRM de prospectos

Un cron semanal detecta prospectos en estado Contactado cuya `fecha_ultima_accion` es superior a siete días, y emite alerta al operador.

Al cambiar el estado a Firmado, un webhook activa el endpoint FastAPI `/onboarding/init?cliente_id=...` que crea automáticamente la ficha correspondiente en la base Clientes Activos.

## Base Clientes Activos

La base Clientes Activos almacena los clientes firmados con contrato vigente. Es la base sobre la que el stack local opera para producción de contenido.

El campo `cliente_id` (Title) opera como clave primaria del sistema completo. El formato es slug del taller seguido de número correlativo (patrón ejemplificable: `tallerperez_001`).

El campo `taller_nombre` (Rich text) almacena el nombre comercial.

El campo `gmb_location_id` (Rich text) es crítico para los workflows de lectura de reseñas y publicación. Se obtiene mediante la API de Google My Business durante el onboarding y es inmutable tras su asignación.

El campo `gmb_url` (URL) almacena el enlace a la ficha.

El campo `tier` (Select) categoriza el plan contratado. Los valores son Basic, PRO, Premium.

El campo `fecha_alta` (Date) registra el día de inicio del contrato.

El campo `fecha_proxima_renovacion` (Date) registra la fecha objetivo de renovación, alimentando alertas con treinta días de antelación.

El campo `mrr` (Number) almacena el importe mensual contratado en euros. Este campo es de modificación manual exclusiva del operador.

El campo `estado` (Select) categoriza la situación del contrato. Los valores son Activo, Pausado, En churn, Baja.

El campo `dueno_nombre` (Rich text) registra el nombre del contacto principal.

El campo `dueno_telefono` (Phone) registra el WhatsApp directo del dueño.

El campo `telegram_bot_chat_id` (Number) registra el identificador de chat de Telegram del dueño, requerido para entrega de Packs en Tier PRO.

El campo `tecnicos_telegram_ids` (Rich text) registra como JSON array los identificadores de chat autorizados a enviar fotografías.

El campo `system_prompt` (Rich text largo) almacena la voz codificada del taller. Se redacta en sesión con el dueño durante el onboarding y presenta longitud típica entre doscientas y quinientas palabras.

El campo `lora_entrenada` (Checkbox) indica si el cliente cuenta con LoRA fine-tuned activa. Es verdadero solo para clientes Premium con LoRA en producción.

El campo `lora_version` (Rich text) almacena la versión o fecha de la LoRA activa, siguiendo patrón ejemplificable como `v3_20261015`.

El campo `fotos_semana` (Number) almacena el contador semanal de fotografías recibidas. Es el KPI canario de riesgo de churn para clientes Tier PRO.

El campo `resenas_mes` (Number) almacena el conteo de reseñas recibidas en el mes en curso.

El campo `nps_ultimo` (Number) almacena el último NPS reportado en escala de cero a diez. Se actualiza trimestralmente.

El campo `notas_operativas` (Rich text) registra observaciones operativas relevantes para el trato con el cliente.

## Triggers de IA sobre la base Clientes Activos

Al iniciar generación de un post o respuesta a reseña, el sistema lee el `system_prompt` del cliente.

Al recibir una fotografía Tier PRO, el sistema incrementa el contador `fotos_semana`.

Un cron semanal evalúa clientes Tier PRO con `fotos_semana` inferior a quince y dispara alerta para contacto del embajador interno del taller.

Un cron mensual el día primero genera registros en la base Reportes Mensuales para cada cliente activo.

## Base Reportes Mensuales

La base Reportes Mensuales almacena las métricas mensuales agregadas por cliente. Es el insumo del workflow de reportes mensuales que opera como arma anti-churn.

El campo `reporte_id` (Title) almacena identificador único en formato `<cliente_id>_YYYY-MM`.

El campo `cliente` (Relation) enlaza a la base Clientes Activos.

El campo `mes` (Date) almacena el primer día del mes reportado.

El campo `llamadas_totales` (Number) almacena el total de llamadas obtenidas desde GMB Insights API.

El campo `llamadas_vs_mes_anterior_pct` (Formula) calcula la variación porcentual respecto al mes anterior.

El campo `busquedas_directas` (Number) almacena las búsquedas por nombre del taller.

El campo `busquedas_descubrimiento` (Number) almacena las búsquedas por categoría o servicio.

El campo `rutas_pedidas` (Number) almacena las solicitudes de "Cómo llegar" desde la ficha.

El campo `resenas_nuevas` (Number) almacena las reseñas recibidas en el mes.

El campo `resenas_respondidas_pct` (Number) almacena el porcentaje de reseñas respondidas.

El campo `ranking_palabras_clave` (Rich text) almacena un JSON con las diez palabras clave principales y su posición actual.

El campo `analisis_generado` (Rich text) almacena el párrafo redactado por el modelo de lenguaje resumiendo la evolución del mes.

El campo `pdf_url` (URL) almacena el enlace al PDF generado en `clientes/<id>/informes/`.

El campo `enviado` (Checkbox) marca confirmación de envío por email.

El campo `fecha_envio` (Date) almacena el timestamp del envío.

## Base Tareas

La base Tareas implementa la lista de tareas operativa del operador, vinculada a las fases del roadmap.

El campo `tarea` (Title) almacena descripción corta.

El campo `fase_roadmap` (Select) categoriza por fase del roadmap.

El campo `prioridad` (Select) categoriza por prioridad. Los valores son: P0, P1, P2, Backlog.

El campo `estado` (Select) categoriza por estado de progreso. Los valores son: Pendiente, En curso, Completada, Bloqueada.

El campo `fecha_limite` (Date) almacena deadline si aplica.

El campo `cliente_asociado` (Relation) enlaza a un cliente específico cuando la tarea aplica a un cliente concreto.

El campo `notas` (Rich text) registra contexto adicional.

## Base Finanzas

La base Finanzas almacena el P&L mensual, runway y ahorro hacia objetivos financieros.

El campo `mes` (Title) almacena identificador en formato `YYYY-MM`.

El campo `ingresos_clientes` (Number) almacena el total facturado del mes.

El campo `ingresos_otros` (Number) almacena ingresos no recurrentes.

El campo `gastos_infra` (Number) almacena gastos de APIs, dominios y suscripciones.

El campo `gastos_legales` (Number) almacena gastos de RETA, gestor, modelos fiscales.

El campo `gastos_marketing` (Number) almacena gastos de herramientas de marketing.

El campo `gastos_personales_necesarios` (Number) almacena gastos personales no atribuibles al negocio.

El campo `beneficio_neto` (Formula) calcula ingresos menos gastos.

El campo `ahorro_acumulado` (Number) almacena el ahorro acumulado, actualizado manualmente el día primero de cada mes.

El campo `runway_meses` (Formula) calcula meses de runway según ahorro y gastos mensuales.

El campo `notas` (Rich text) registra decisiones financieras del mes.

## Base Plantillas

La base Plantillas almacena contenido reutilizable que se duplica por cliente o por situación. Las subpáginas principales incluyen: plantilla de System Prompt para taller, plantilla de seguimiento de prospecto con cuatro bloques (Auditoría, Propuesta, Llamada, Firma), plantilla de onboarding Basic con seis pasos, plantilla de onboarding PRO con doce pasos incluyendo visita física.

## Wrapper de integración Notion

El módulo `integrations/notion_client.py` expone un conjunto de funciones que abstraen las operaciones de Notion para el resto del sistema.

La función `get_cliente(cliente_id)` retorna el diccionario completo de datos del cliente.

La función `list_clientes_activos(tier=None)` retorna la lista de clientes activos, con filtro opcional por tier.

La función `update_fotos_semana(cliente_id, delta=1)` incrementa el contador semanal de fotografías.

La función `create_reporte_mensual(cliente_id, data)` crea un nuevo registro en la base Reportes Mensuales.

La función `get_prospectos_por_estado(estado)` retorna la lista de prospectos filtrada por estado del pipeline.

La función `list_tareas_p0()` retorna la lista de tareas con prioridad P0.

## Reglas de integridad de datos

La regla uno establece que no se eliminan registros (rows) de las bases CRM ni Clientes Activos. Las bajas se gestionan mediante cambio de estado, no mediante borrado.

La regla dos establece que los cambios del campo `tier` requieren entrada en `notas_operativas` con motivo y fecha del cambio.

La regla tres establece que el campo `mrr` se modifica exclusivamente por el operador humano, nunca por procesos automáticos.

La regla cuatro establece que el campo `gmb_location_id` es inmutable tras su asignación inicial. Si una ficha cambia de location_id, se considera cliente nuevo y se crea registro independiente.

La regla cinco establece backup mensual de Notion mediante exportación oficial a Markdown, persistido en almacenamiento secundario.

## Campos críticos en alta de cliente Tier PRO

El alta de un cliente Tier PRO requiere que diez campos estén rellenados como condición previa a cualquier operación del sistema. La ausencia de cualquiera de ellos bloquea operaciones para ese cliente bajo principio fail fast.

Los campos críticos son: `cliente_id` generado, `gmb_location_id` obtenido vía Google API, `system_prompt` redactado con el dueño, `telegram_bot_chat_id` del dueño, `tecnicos_telegram_ids` con al menos un técnico, `tier` igual a PRO, `mrr` con importe acordado, `fecha_alta` registrada, carpeta local creada en filesystem, y archivo `logo.png` del taller en directorio `assets`.
