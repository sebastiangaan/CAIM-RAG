# Metodología Tier PRO — Pipeline de Reputación Visual

## Resumen operativo del pipeline

El pipeline del Tier PRO transforma una captura fotográfica del técnico mecánico en un Pack de activos digitales listo para entrega al cliente final del taller. El flujo se inicia cuando el técnico envía dos fotografías (estado antes de la intervención y estado tras la intervención) al bot Telegram del sistema, acompañadas del identificador de cliente como caption. El flujo finaliza cuando el dueño del taller recibe en su Telegram personal un Pack que incluye dos imágenes con branding aplicado, tres variantes de reseña generadas, y un mensaje WhatsApp sugerido para enviar al cliente final.

El SLA operativo del pipeline establece un tiempo objetivo desde captura hasta entrega del Pack inferior o igual a noventa segundos en hora punta.

## Pre-requisitos del cliente Tier PRO

Un cliente del Tier PRO debe cumplir nueve pre-requisitos antes de la activación del pipeline. La ausencia de cualquiera de ellos bloquea la activación.

El primer pre-requisito es el acceso de tipo Gestor concedido a CMAI sobre la ficha de Google My Business del taller.

El segundo pre-requisito es la asignación de un cliente_id en formato slug del taller seguido de número correlativo (patrón ejemplificable como `tallerperez_001`).

El tercer pre-requisito es la obtención del gmb_location_id mediante la API de Google My Business, almacenado en la base Clientes Activos de Notion.

El cuarto pre-requisito es la redacción del system_prompt del taller en sesión de aproximadamente treinta minutos con el dueño, capturando voz, valores y atributos diferenciales.

El quinto pre-requisito es el registro del telegram_bot_chat_id del dueño y de los técnicos autorizados a enviar fotografías.

El sexto pre-requisito es la realización de una visita física al taller con duración aproximada de sesenta minutos.

El séptimo pre-requisito es la formación operativa a los técnicos con duración aproximada de treinta minutos.

El octavo pre-requisito es la identificación de un embajador interno en el taller, responsable de mantener la cadencia fotográfica.

El noveno pre-requisito es la firma del addendum contractual de protección de datos que cubre el tratamiento de imágenes con matrículas y empleados.

## Las once etapas del pipeline

### Etapa uno: Captura

El técnico mecánico envía dos fotografías al bot Telegram del sistema, acompañadas del cliente_id como caption del mensaje. El bot valida en Notion que el cliente_id existe y que tiene tier igual a PRO. Si la validación es positiva, el bot responde "recibido" en menos de dos segundos.

### Etapa dos: Webhook

El bot Telegram reenvía el evento a un endpoint FastAPI (POST `/pro/reception`). El servicio descarga las imágenes a una ruta del filesystem local con estructura `~/conversational-motors/clientes/<cliente_id>/fotos/YYYY-MM-DD/`.

### Etapa tres: Privacidad

Las imágenes pasan por un módulo `privacy.py` que utiliza un modelo YOLO ligero para detectar matrículas. Las matrículas detectadas se enmascaran mediante Gaussian blur con sigma quince. Los archivos resultantes se persisten con sufijo `_clean`. Las versiones originales se retienen únicamente durante siete días por minimización RGPD, tras los cuales se eliminan automáticamente mediante cron.

### Etapa cuatro: Extracción visual

Cada imagen procesada se envía a un modelo multimodal local con un prompt técnico que solicita extracción estructurada en formato JSON estricto. Los campos extraídos son: pieza_principal, marca_pieza_visible, modelo_pieza_visible, estado, contexto_reparacion, y observaciones_tecnicas. El resultado se persiste en el mismo directorio con sufijo `.json`.

### Etapa cinco: Búsqueda RAG

El sistema consulta la colección ChromaDB del cliente (nombrada como `<cliente_id>_resenas`) con un query construido a partir de la pieza identificada y el contexto de reparación. La consulta recupera entre tres y cinco reseñas previas del mismo taller con semántica similar, para mantener consistencia de voz. Adicionalmente se recupera el system_prompt del taller desde Notion.

### Etapa seis: Generación de variantes de reseña

El sistema invoca al modelo de lenguaje principal con un prompt que incluye el JSON antes, el JSON después, las reseñas RAG, y el system_prompt del taller. El prompt solicita tres variantes de reseña con cinco restricciones: lenguaje natural sin marcas de generación automática, mínimo tres entidades técnicas extraídas del JSON, longitud entre cuarenta y ochenta palabras, mención del nombre del taller exactamente una vez, y ausencia de superlativos vacíos.

### Etapa siete: Branding de imagen

Un módulo `branding.py` utiliza la librería Pillow para procesar las imágenes limpias. El procesamiento añade el logo del taller (almacenado en `clientes/<cliente_id>/assets/logo.png`) en la esquina inferior derecha, una marca de agua sutil con el nombre del taller, redimensiona a resolución 1200x900 (formato óptimo para publicaciones GMB), y comprime a JPG con calidad ochenta y cinco. El output se persiste con sufijos `_branded_antes` y `_branded_despues`.

### Etapa ocho: Composición del Pack

El sistema compone un objeto JSON denominado Pack que contiene los siguientes campos: pack_id (combinación de cliente_id, fecha y timestamp), cliente_id, lista de rutas a imágenes branded, lista de variantes de reseña, mensaje WhatsApp sugerido para enviar al cliente final del taller, timestamp de generación, e identificador del técnico que originó la captura.

### Etapa nueve: Entrega al dueño

El bot Telegram envía al chat_id del dueño del taller las dos imágenes branded como adjuntos, las tres variantes de reseña como mensaje de texto numerado, y el mensaje WhatsApp sugerido como segundo mensaje. Los mensajes incluyen tres botones inline para acciones del dueño: aprobar y enviar al cliente, editar variante, descartar Pack.

### Etapa diez: Aprobación y registro

La acción del dueño se procesa según tres ramas. La aprobación cambia el estado del Pack a `enviado_cliente` en Notion e incrementa el contador `fotos_semana` del cliente. La edición invoca un flujo conversacional donde el dueño selecciona variante y aporta modificación libre. El descarte registra la razón del descarte como input para mejora de prompts. En las tres ramas el Pack se persiste en SQLite y ChromaDB con su estado final.

### Etapa once: Publicación condicional en GMB

Si el dueño activa explícitamente la opción de publicar la foto en la galería del taller, n8n recoge la orden y utiliza la API de Google Business Profile para publicar la imagen branded con un caption corto generado por el modelo de lenguaje. Esta publicación requiere aprobación explícita en cada ocurrencia para evitar publicaciones automáticas no supervisadas.

## SLAs operativos

El pipeline opera bajo tres SLAs cuantificados.

El SLA de latencia establece un tiempo desde envío de fotografía hasta entrega del Pack al dueño inferior o igual a noventa segundos en hora punta de operación.

El SLA de uptime del bot Telegram establece disponibilidad del noventa y nueve por ciento, con caída únicamente en ventanas de mantenimiento nocturno planificado.

El SLA de capacidad establece soporte de hasta diez Packs simultáneos sin degradación de latencia, con el modelo multimodal gestionando cola de procesamiento.

## KPIs del Tier PRO

El Tier PRO se evalúa mediante cinco KPIs con umbrales saludables y umbrales de riesgo definidos.

El KPI de fotos enviadas por cliente y semana presenta umbral saludable mayor o igual a quince. Un valor inferior a quince constituye el predictor número uno de churn del cliente y dispara alerta automática.

El KPI de porcentaje de variantes aprobadas sin edición presenta umbral saludable mayor o igual al sesenta por ciento. Un valor inferior al cuarenta por ciento indica baja calidad de prompts y dispara revisión del system_prompt o del prompt de generación.

El KPI de ratio entre reseñas publicadas y Packs enviados presenta umbral saludable mayor o igual al veinticinco por ciento. Un valor inferior al diez por ciento indica que el dueño no está solicitando reseñas a clientes finales o que la conversión cliente final a reseña es deficiente.

El KPI de tiempo medio entre captura y Pack presenta umbral saludable inferior o igual a noventa segundos. Un valor superior a tres minutos indica problema de infraestructura.

El KPI de NPS (Net Promoter Score) del cliente medido a noventa días presenta umbral saludable mayor o igual a ocho sobre diez. Un valor inferior o igual a seis sobre diez constituye señal temprana de churn.

## Onboarding ritualizado del Tier PRO

El onboarding del Tier PRO es ritualizado con duración total de treinta días y se compone de fases obligatorias.

La fase Día Cero comprende la firma del contrato, la concesión del acceso Gestor a la ficha GMB, y la pre-redacción del system_prompt por parte de CMAI con datos de la ficha actual del taller.

La fase Día Uno a Tres comprende la visita física al taller, la sesión de captura de voz con el dueño para refinar el system_prompt, la formación a los técnicos sobre el uso del bot Telegram, y la identificación del embajador interno responsable de mantener la cadencia fotográfica.

La fase Día Cuatro a Catorce comprende el periodo de captura supervisada con seguimiento diario de incidencias, ajuste fino del system_prompt mediante iteración con feedback del dueño, y validación de la calidad de extracción del modelo multimodal sobre las primeras intervenciones del taller.

La fase Día Quince a Treinta comprende la operación en autonomía con revisión semanal de KPIs y reunión de cierre de onboarding al final del periodo.
