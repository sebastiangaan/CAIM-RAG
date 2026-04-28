# KPIs y Métricas Operativas

## Estructura del sistema de métricas

El sistema de métricas de CMAI se organiza en cuatro niveles de agregación: métricas semanales del funnel comercial, KPIs operativos por tier de servicio, KPIs de validación de tesis SEO, y KPIs financieros mensuales para evaluación estratégica.

Cada métrica se define con: nombre normalizado, fórmula de cálculo, periodicidad de medición, umbral saludable, y umbral de riesgo. Las métricas se persisten en la base Notion correspondiente con histórico mensual para auditoría posterior.

## Métricas SaaS estándar

CMAI utiliza el conjunto estándar de métricas SaaS para reporting y para presentación a compradores potenciales en escenario de exit.

### MRR (Monthly Recurring Revenue)

El MRR se define como la suma de los importes mensuales contratados (`mrr` en la base Clientes Activos) de todos los clientes con `estado` igual a Activo. Se calcula con periodicidad mensual.

### ARR (Annual Recurring Revenue)

El ARR se define como el MRR multiplicado por doce. Es la métrica de referencia para cálculos de valoración mediante múltiplo SaaS.

### ARPU (Average Revenue Per User)

El ARPU se define como MRR dividido por el número total de clientes activos. Se calcula con periodicidad mensual.

### Churn mensual

El churn mensual se define como el número de clientes que cambiaron a estado Baja en el mes en curso, dividido por el número de clientes activos al inicio del mes. Se expresa como porcentaje.

El umbral saludable de churn mensual es menor o igual al tres por ciento. El umbral de excelencia para presentación a compradores es menor o igual al uno punto cinco por ciento. Un churn superior al cinco por ciento mensual constituye señal de problema estructural.

### CAC (Customer Acquisition Cost)

El CAC se define como el coste total de marketing y ventas en un periodo dividido por el número de nuevos clientes adquiridos en ese periodo. En operación con operador único sin gasto de marketing significativo, el CAC se aproxima al coste imputado del tiempo del operador en actividades comerciales.

### LTV (Lifetime Value)

El LTV se define como ARPU multiplicado por margen bruto, dividido por churn mensual. Es una estimación del valor económico esperado de un cliente medio durante su vida útil.

### Ratio LTV/CAC

El ratio LTV/CAC mide la eficiencia de adquisición. El umbral saludable es mayor o igual a tres veces. Un ratio inferior a uno indica que el negocio pierde dinero en cada nuevo cliente.

### NRR (Net Revenue Retention)

El NRR mide la evolución del revenue de una cohorte de clientes existentes. Se calcula como el MRR del mes actual de una cohorte dividido por el MRR del mes inicial de esa cohorte. Un NRR superior al cien por ciento indica que el revenue de expansion supera al revenue perdido por churn.

### NPS (Net Promoter Score)

El NPS se mide trimestralmente sobre la base de clientes activos mediante encuesta directa. La pregunta canónica solicita probabilidad de recomendación en escala de cero a diez. Los promotores son clientes con puntuación nueve o diez; los detractores son clientes con puntuación cero a seis. NPS se calcula como porcentaje de promotores menos porcentaje de detractores.

El umbral saludable de NPS por cliente individual a noventa días es mayor o igual a ocho sobre diez. Un valor inferior o igual a seis sobre diez constituye señal temprana de churn.

## Los siete KPIs del dashboard semanal

El dashboard semanal del operador presenta siete KPIs principales, evaluados cada lunes en sesión de revisión de aproximadamente treinta minutos.

KPI uno: MRR actual (suma del campo mrr de Clientes Activos con estado Activo).

KPI dos: número de clientes nuevos en el mes en curso.

KPI tres: churn del mes (umbral saludable inferior al tres por ciento).

KPI cuatro: CAC medio del mes (coste de marketing dividido por nuevos clientes).

KPI cinco: LTV estimado (ARPU multiplicado por margen, dividido por churn).

KPI seis: ratio LTV sobre CAC (umbral saludable mayor o igual a tres).

KPI siete: fotografías por cliente y semana en clientes Tier PRO (umbral saludable mayor o igual a quince; predictor número uno de churn en el tier).

## KPIs del Tier PRO

El Tier PRO opera con cinco KPIs específicos del pipeline de Reputación Visual.

### Fotos enviadas por cliente y semana

Umbral saludable mayor o igual a quince. Umbral de riesgo menor a quince (predictor número uno de churn). Si un cliente Tier PRO presenta este indicador en zona de riesgo, se activa contacto con el embajador interno del taller.

### Porcentaje de variantes aprobadas sin edición

Umbral saludable mayor o igual al sesenta por ciento. Umbral de riesgo menor al cuarenta por ciento. Indicador de calidad del system_prompt del cliente y de calidad del prompt de generación.

### Ratio entre reseñas publicadas y Packs enviados

Umbral saludable mayor o igual al veinticinco por ciento. Umbral de riesgo menor al diez por ciento. Indicador de comportamiento del dueño respecto a solicitar reseñas a clientes finales.

### Tiempo medio entre captura y entrega del Pack

Umbral saludable menor o igual a noventa segundos. Umbral de riesgo mayor a tres minutos. Indicador de salud de la infraestructura.

### NPS del cliente a noventa días

Umbral saludable mayor o igual a ocho sobre diez. Umbral de riesgo menor o igual a seis sobre diez.

## KPI canario de validación de tesis SEO

El KPI canario que valida la tesis SEO Conversacional es el ratio entre reseñas con tres o más entidades técnicas y reseñas genéricas, medido por cliente.

El baseline observado en talleres sin intervención CMAI se sitúa en torno al quince por ciento de reseñas con entidades.

El umbral de validación a tres meses para un cliente Tier PRO es mayor o igual al sesenta por ciento.

Si a tres meses el ratio no asciende, la tesis falla para ese cliente y se considera caso anómalo. La investigación de causa evalúa: frecuencia fotográfica del técnico, comportamiento del dueño en solicitud de reseñas, calidad de extracción del modelo multimodal sobre el tipo de intervenciones del taller.

## Métricas semanales del funnel comercial

El funnel comercial se evalúa semanalmente con seis métricas operativas.

Prospectos nuevos cargados en CRM: umbral saludable mayor o igual a diez por semana.

Mensajes tipo 1 enviados: umbral saludable mayor o igual a cinco por semana.

Auditorías enviadas: umbral saludable mayor o igual a tres por semana.

Llamadas realizadas: umbral saludable mayor o igual a dos por semana.

Tasa de respuesta a Mensaje 1: umbral saludable mayor o igual al treinta por ciento.

Tasa de conversión de llamada a piloto: umbral saludable mayor o igual al treinta por ciento.

Pipeline total con estado mayor o igual a Interesado: umbral saludable mayor o igual a quince prospectos vivos en simultáneo.

Si dos semanas consecutivas fallan un umbral, el operador revisa la causa: mensajes (iterar copy), llamadas (iterar estructura), auditorías (mejorar calidad).

## Métricas financieras mensuales

Las métricas financieras se evalúan el día primero de cada mes en sesión de revisión de aproximadamente noventa minutos.

P&L mensual: ingresos clientes más ingresos otros, menos gastos infraestructura, gastos legales, gastos marketing y gastos personales necesarios.

Beneficio neto: ingresos totales menos gastos totales.

Ahorro acumulado: campo manual actualizado el día primero.

Runway en meses: ahorro acumulado dividido por el promedio mensual de gastos sin ingresos.

Evaluación de trayectoria: comparación de MRR actual versus objetivo del roadmap para el mes en curso.

## Métricas de auditoría técnica

Las métricas de auditoría técnica se calculan automáticamente y se persisten en logs.

Uptime de los servicios principales (Ollama, ChromaDB, n8n, FastAPI): umbral saludable mayor o igual al noventa y nueve por ciento.

Latencia media de inferencia del modelo principal: umbral saludable depende del tamaño de prompt; valores típicos para prompts de quinientos tokens son menores a veinte segundos.

Tasa de error en webhooks de Telegram: umbral saludable menor al uno por ciento.

Volumen de tokens procesados por mes: métrica para capacity planning, sin umbral fijo.

## Métricas de cohorte para due diligence

En contexto de preparación para venta, las métricas se segmentan por cohorte (mes de alta del cliente) para evaluar evolución de unit economics.

LTV por cohorte: el LTV calculado únicamente sobre clientes de una cohorte específica.

Tasa de upgrade por cohorte: porcentaje de clientes de una cohorte que ascendió de tier desde su alta.

Churn por cohorte: porcentaje de clientes de una cohorte que cambió a estado Baja por mes desde su alta.

Tiempo medio en tier por cohorte: meses promedio que un cliente de una cohorte permanece en su tier inicial antes de upgrade o baja.

## Frecuencia de revisión y rituales

La frecuencia de revisión de métricas sigue cuatro rituales operativos.

Ritual diario: actualización del estado del Prompt CEO con el último avance del día (duración aproximada diez minutos).

Ritual semanal (lunes): revisión de los siete KPIs del dashboard, evaluación del pipeline comercial, identificación de riesgos, decisión sobre prioridades de la semana (duración aproximada treinta minutos).

Ritual mensual (día primero): cálculo de P&L mensual, evaluación de runway, comparación versus objetivo del roadmap (duración aproximada noventa minutos).

Ritual trimestral: retrospective con OKRs (Objectives and Key Results) para el trimestre siguiente, incluyendo definición de tres OKRs principales (duración aproximada media jornada).
