# Workflows de Automatización CMAI

## Principio de los workflows

CMAI implementa cinco workflows operativos principales mediante n8n self-hosted en Docker. Los workflows automatizan operaciones recurrentes que forman el núcleo de la entrega contractual a clientes.

El principio de adopción de workflows establece que cada workflow se incorpora únicamente cuando existe un volumen mínimo de clientes que lo justifica. La construcción de un workflow precede en una fase a su uso productivo.

Los workflows se nombran con número correlativo en orden de prioridad de implementación: Workflow 1 (Alerta de reseñas), Workflow 2 (Posts GMB semanales), Workflow 3 (Reportes mensuales), Workflow 4 (Tier PRO Receptor), Workflow 5 (Tier PRO Publicación).

## Workflow 1: Alerta de reseña con respuesta IA

El Workflow 1 detecta nuevas reseñas en fichas GMB de clientes activos, genera una propuesta de respuesta condicionada por el system_prompt del taller, y entrega la propuesta al dueño vía Telegram para aprobación.

El flujo se descompone en seis nodos secuenciales.

El primer nodo es un trigger cron que se ejecuta diariamente a las nueve horas.

El segundo nodo consulta la base Clientes Activos en Notion para obtener la lista de clientes con estado Activo.

El tercer nodo, por cada cliente, consulta la API de Google My Business solicitando reseñas recibidas en las últimas veinticuatro horas.

El cuarto nodo aplica condicional IF para filtrar únicamente reseñas no procesadas previamente.

El quinto nodo invoca al modelo de lenguaje principal con el system_prompt del cliente y el texto de la reseña, generando una respuesta propuesta.

El sexto nodo envía la respuesta propuesta al chat Telegram del dueño con botones inline para aprobar, editar o descartar.

El impacto operativo del Workflow 1 es la reducción del tiempo dedicado por el operador a respuesta de reseñas desde un orden de cuarenta y cinco minutos diarios hasta un orden de cinco minutos de revisión.

La condición de adopción del Workflow 1 es la existencia de al menos un cliente activo con tier Basic o superior.

## Workflow 2: Posts GMB semanales

El Workflow 2 genera dos publicaciones semanales para la ficha GMB de cada cliente activo, condicionadas por el system_prompt del taller, y las entrega al dueño para aprobación previa a publicación.

El flujo se descompone en cinco nodos secuenciales.

El primer nodo es un trigger cron que se ejecuta los lunes a las ocho horas.

El segundo nodo consulta la base Clientes Activos para obtener la lista de clientes con estado Activo y tier Basic o superior.

El tercer nodo, por cada cliente, recupera el system_prompt y la lista de servicios del taller.

El cuarto nodo invoca al modelo de lenguaje principal con instrucciones de generar dos posts orientados a SEO conversacional, con extensión y formato adaptados a publicación GMB.

El quinto nodo envía los posts propuestos al chat Telegram del dueño. Tras aprobación del dueño, un sub-flujo publica los posts en la ficha GMB mediante la API de Google Business Profile.

El impacto operativo del Workflow 2 es la reducción del tiempo dedicado por el operador a generación de contenido semanal desde un orden de tres a cuatro horas hasta un orden de veinte minutos de revisión.

La condición de adopción del Workflow 2 es la existencia de al menos tres clientes activos.

## Workflow 3: Reporte mensual automatizado

El Workflow 3 produce el reporte mensual de cada cliente el día primero de cada mes. Este workflow opera como arma anti-churn principal del Tier Basic: la entrega puntual el día primero con datos reales constituye la razón número uno de renovación.

El flujo se descompone en seis nodos secuenciales.

El primer nodo es un trigger cron que se ejecuta el día primero de cada mes a las nueve horas.

El segundo nodo consulta la API de Google My Business Insights solicitando métricas del mes completado: llamadas, búsquedas directas, búsquedas de descubrimiento, rutas pedidas, reseñas nuevas, reseñas respondidas.

El tercer nodo compara las métricas del mes con las del mes anterior calculando variaciones porcentuales.

El cuarto nodo invoca al modelo de lenguaje principal solicitando redacción de análisis positivo del mes con foco en métricas que mejoraron, manteniendo veracidad sobre las que no mejoraron.

El quinto nodo crea un nuevo registro en la base Reportes Mensuales de Notion con todos los campos rellenos, incluyendo URL al PDF generado.

El sexto nodo envía el PDF al dueño por email y por WhatsApp con un mensaje breve de acompañamiento.

La condición de adopción del Workflow 3 es la existencia de al menos un cliente activo y la disponibilidad de la base Reportes Mensuales en Notion.

## Workflow 4: Tier PRO Receptor

El Workflow 4 implementa la recepción asíncrona del pipeline del Tier PRO. Captura las fotografías enviadas por técnicos al bot Telegram, dispara el procesamiento de las once etapas del pipeline, y entrega el Pack al dueño.

El flujo se inicia mediante webhook Telegram que recibe el evento de mensaje con dos fotografías y un caption con cliente_id. El webhook reenvía el evento al endpoint FastAPI `/pro/reception` que ejecuta las once etapas del pipeline definido en la metodología Tier PRO.

La condición de adopción del Workflow 4 es la existencia de al menos un cliente Tier PRO firmado y el cumplimiento de los nueve pre-requisitos del Tier PRO para ese cliente.

## Workflow 5: Tier PRO Publicación

El Workflow 5 implementa la publicación condicional de fotografías procesadas en la galería GMB del cliente. Se activa únicamente cuando el dueño marca explícitamente la opción de publicar tras aprobar un Pack.

El flujo se descompone en tres nodos secuenciales.

El primer nodo es un trigger por evento que escucha aprobación con flag de publicación.

El segundo nodo invoca al modelo de lenguaje principal solicitando un caption corto adaptado al formato de post GMB para la imagen branded.

El tercer nodo invoca a la API de Google Business Profile para publicar la imagen branded con el caption en la galería del taller.

La regla operativa del Workflow 5 establece que la publicación requiere aprobación explícita del dueño en cada ocurrencia. La publicación nunca es automática para evitar publicaciones no supervisadas.

La condición de adopción del Workflow 5 es la operación estable del Workflow 4 con al menos un cliente Tier PRO.

## Reglas operativas comunes a todos los workflows

Los workflows se exportan periódicamente como archivos JSON al directorio `n8n/workflows/` del repositorio del proyecto. Esta exportación opera como backup y como recurso de auditoría técnica en escenarios de due diligence.

Los workflows se versionan mediante git junto al resto del código del proyecto. Cada modificación significativa de un workflow se acompaña de commit descriptivo y nota en el changelog operativo.

Los workflows incluyen nodos de logging hacia un archivo de log local con rotación diaria. Los logs se persisten durante treinta días para diagnóstico de incidencias.

Los workflows incluyen nodos de captura de errores con notificación al operador vía Telegram cuando un nodo falla. La notificación incluye el cliente afectado, el nodo fallido, y el mensaje de error.

## Agentes y automatización avanzada

Más allá de los cinco workflows base, CMAI prevé incorporar agentes con tool calling a partir del momento en que la cartera de clientes alcance umbrales operativos definidos.

El motor base previsto para tool calling local es un modelo Hermes 3 en el rango de setenta mil millones de parámetros, fine-tuned específicamente para function calling. Hermes 3 presenta mayor estabilidad que modelos generalistas en secuencias largas de invocaciones de herramientas.

Los agentes en consideración incluyen: agente de auditorías GMB automáticas para nuevos prospectos, agente de respuestas a reseñas con tool calling sobre la API de Google Business Profile, y agentes de scout de prospectos que identifican talleres candidatos automáticamente sobre Google Maps.

La adopción de agentes con computer use local sobre aplicaciones de escritorio se difiere a momentos en que las herramientas correspondientes alcancen madurez de producción. Las herramientas en estado beta o experimental no se incorporan al stack productivo.

## División de tareas entre workflows e intervención manual

La división de tareas entre automatización y operador humano sigue una regla operativa explícita.

Los workflows automatizan tareas con tres propiedades: alta repetitividad, bajo riesgo de error con consecuencias graves, y posibilidad de revisión por el cliente final antes de ejecución externa.

El operador humano interviene en tareas con propiedades opuestas: baja repetitividad o creatividad estratégica, alto riesgo de error con consecuencias graves, e incapacidad de revisión previa por terceros.

La regla deriva en que la generación de contenido es automatizable con revisión, pero la negociación comercial no se automatiza. La respuesta a una reseña se automatiza con aprobación, pero la decisión de aceptar o no a un prospecto no se automatiza.
