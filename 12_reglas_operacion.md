# Reglas de Operación y Antipatrones

## Reglas no negociables del negocio

CMAI opera bajo un conjunto de reglas no negociables que estructuran las decisiones técnicas, comerciales y operativas. Estas reglas se aplican consistentemente y no se relajan por presión externa o conveniencia táctica.

### Regla uno: Producto, no servicio

Cualquier flujo manual que se repite tres veces se automatiza al cuarto. La automatización puede materializarse como workflow n8n, script en el código FastAPI, o agente con tool calling. El criterio operativo es: si una operación se ejecutó manualmente tres veces, la cuarta ejecución debe estar parcial o totalmente automatizada.

### Regla dos: IA local por defecto

Las APIs externas se utilizan únicamente en escenarios críticos y como fallback de calidad. El uso por defecto es modelo local. Cualquier nueva funcionalidad se diseña primero asumiendo modelo local y solo migra a API externa si el resultado del modelo local es insuficiente tras tres intentos.

### Regla tres: Datos del cliente nunca salen del hardware local

Las fotografías de clientes, system_prompts, embeddings, y cualquier dato derivado de la operación de un cliente residen exclusivamente en el hardware del operador. La transmisión de estos datos a servicios externos requiere aprobación explícita del cliente y operación.

### Regla cuatro: Orden estricto de fases

Las fases del roadmap se ejecutan en orden estricto. No se salta una fase para acelerar otra. La condición para iniciar una fase es la completitud verificable de la fase anterior. Saltar fases genera deuda operativa que se manifiesta en escalado.

### Regla cinco: Documentar mientras se construye

Cada script cuenta con README mínimo que incluye descripción funcional, instrucciones de ejecución, y dependencias. La documentación se escribe simultáneamente con el código, no como tarea posterior.

### Regla seis: Una métrica por semana

Cada semana se prioriza una métrica única que define el éxito de la semana. La métrica deriva de la fase actual del roadmap. Las decisiones tácticas se evalúan contra el impacto sobre esa métrica única.

### Regla siete: Cuentas auditadas desde el primer ejercicio

Las cuentas anuales se auditan desde el primer ejercicio cerrado, incluso cuando el coste del auditor parece elevado respecto al volumen de operación. La auditoría externa aporta entre diez y quince por ciento adicional de valoración en escenario de exit y reduce el riesgo de incidencias en due diligence.

### Regla ocho: Histórico de git inviolable

El histórico de git no se reescribe. No se ejecutan operaciones de force push sobre ramas compartidas. No se elimina histórico de commits. Los commits son atómicos y describen el cambio realizado.

### Regla nueve: Secretos nunca en repositorio

Los secretos (claves API, tokens, credenciales) nunca se incluyen en el repositorio. El archivo `.env` se incluye en `.gitignore` y nunca se sube. Los secretos se gestionan en la página Claves API de Notion con acceso restringido.

### Regla diez: Test antes de producción

Cualquier cambio que afecta a un cliente real se prueba previamente en entorno de test o con cliente cómplice antes de despliegue a producción. El Tier PRO se valida con cliente cómplice gratuito antes de su lanzamiento comercial.

## Antipatrones operativos generales

CMAI rechaza explícitamente cuatro categorías de antipatrones operativos.

### Antipatrón uno: Asumir conocimiento técnico no demostrado

Las respuestas y procesos no asumen conocimiento técnico previo del operador o del cliente. Los conceptos técnicos se introducen con analogías y ejemplos concretos del dominio CMAI. El glosario técnico opera como referencia compartida.

### Antipatrón dos: Optimización prematura

No se optimizan operaciones cuyo cuello de botella no está demostrado mediante métricas. La optimización se aplica donde se mide impacto, no donde se intuye potencial.

### Antipatrón tres: Dispersión de temas en una respuesta

Una respuesta operativa aborda un tema único. Las respuestas que mezclan estrategia, táctica, y técnica se descomponen en respuestas separadas para cada nivel.

### Antipatrón cuatro: Optimizar para fases futuras antes de cerrar fase actual

Las decisiones técnicas no anticipan necesidades de fases futuras si existen tareas pendientes en la fase actual. La fase futura puede no llegar; la fase actual es lo que existe.

## Antipatrones técnicos

CMAI rechaza explícitamente cinco antipatrones técnicos.

### Uso de servicios externos cuando existe alternativa local viable

Si una funcionalidad puede implementarse con modelo local con calidad aceptable, no se utiliza API externa. El criterio de migración a API externa es tres intentos fallidos del modelo local sobre tarea crítica.

### Adopción de tecnología beta o experimental en stack productivo

Las tecnologías en estado beta o experimental no se incorporan al stack productivo. La evaluación de tecnologías nuevas se realiza en entorno aislado con criterios de adopción explícitos.

### Lock-in en proveedor único de modelo

El stack se diseña con principio de agnosticismo de modelos. La sustitución de un modelo por otro debe ser modificación de configuración, no reescritura de código.

### Mezcla de datos de clientes distintos en mismo storage

Cada cliente opera con su propia colección ChromaDB, su propio directorio en filesystem, y su propio system_prompt. La mezcla genera riesgos de fuga de información entre clientes.

### Despliegue sin rollback documentado

Cada despliegue significativo cuenta con procedimiento de rollback documentado. Si el rollback no está claro, el despliegue no se ejecuta.

## Antipatrones comerciales

CMAI rechaza explícitamente seis antipatrones comerciales.

### Llamada en frío como canal principal

WhatsApp opera con mejor tasa de respuesta en el nicho de talleres mecánicos que la llamada telefónica en frío.

### Envío de PDFs no solicitados

La auditoría se envía únicamente tras solicitud explícita del prospecto. El envío sin permiso se interpreta como spam y daña la marca.

### Mención de tecnología en venta

El dueño del taller compra resultados (incremento de llamadas), no stack técnico. Los términos como "modelo de IA", "ChromaDB", "FastAPI" no aparecen en mensajes ni llamadas comerciales.

### Promesa de "primer puesto en Google"

CMAI promete incremento de llamadas cualificadas medible en una ventana de noventa días, no posiciones absolutas en SERP. Las promesas no medibles dañan credibilidad y exponen a disputas.

### Envío de propuesta escrita previa a la llamada

La propuesta se explica oralmente en llamada, no se lee desde documento. La propuesta escrita se entrega únicamente tras llamada y como confirmación.

### Cierre por email

El cierre comercial opera con confirmación verbal seguida de confirmación escrita. El email aislado como medio de cierre presenta tasa de conversión significativamente menor.

## Antipatrones de pricing

CMAI rechaza explícitamente tres antipatrones de pricing.

### Reducción de precio por regateo

El precio no se reduce por presión negociadora del cliente. Si el cliente percibe el precio como elevado, la respuesta operativa es explicar el valor entregado o aceptar la pérdida del prospecto.

### Descuentos no estructurados

Los descuentos solo aplican como precio de cohorte temprana definido explícitamente. Los descuentos discrecionales por cliente erosionan unit economics y comunican debilidad estructural.

### Ampliación de scope sin compensación

La retención de un cliente en riesgo de churn no se opera mediante adición de servicios fuera del scope contractual. La retención se opera sobre calidad de entrega del scope existente.

## Antipatrones de catálogo

CMAI rechaza explícitamente cinco antipatrones del catálogo de productos.

### Lanzamiento simultáneo de múltiples tiers

Los tiers se lanzan en cascada con validación previa de cada nivel. El lanzamiento simultáneo dispersa esfuerzo y genera procesos no probados.

### Operación del Tier PRO sin onboarding ritualizado completo

El onboarding ritualizado es condición necesaria del Tier PRO, no opcional. Un cliente Tier PRO sin onboarding completo presenta riesgo elevado de churn temprano.

### Oferta de Tier Enterprise antes de activación de Ruta 2

El Tier Enterprise no figura en el catálogo de oferta estándar. Su activación requiere capital y equipo correspondiente a Ruta 2.

### Modificación de mrr por procesos automáticos

El campo mrr en la base Clientes Activos se modifica exclusivamente por intervención humana del operador. Los procesos automáticos no escriben sobre este campo.

### Eliminación de registros de bases CRM o Clientes Activos

Los registros no se eliminan. Las bajas se gestionan mediante cambio de estado, no mediante borrado. La preservación del histórico es necesaria para análisis de cohortes y due diligence.

## Antipatrones de la tesis SEO

CMAI rechaza explícitamente cuatro antipatrones de la tesis SEO Conversacional.

### Operación como agencia de volumen

La cartera amplia de clientes a precio bajo es commodity y no genera moat acumulativo. CMAI opera con cartera reducida de clientes premium con tickets elevados.

### Competencia en precio

El precio de entrada del Tier Basic se sitúa explícitamente por encima del precio medio de mercado para reforzar diferenciación por calidad. La competencia se ejecuta sobre dimensión de calidad, no sobre dimensión de precio.

### SEO black-hat

Las reseñas falsas, los links comprados, y el contenido fabricado están explícitamente prohibidos. Todo el contenido producido es inducido sobre trabajo real ejecutado por el taller.

### Promesas no medibles

Las promesas comerciales se formulan en términos de métricas verificables (incremento de llamadas en GMB Insights), no en términos absolutos no verificables (primer puesto en Google).

## Reglas para el asistente de IA del operador

El asistente de IA que apoya al operador opera bajo restricciones explícitas. Estas restricciones se especifican en el system prompt del workspace correspondiente.

El asistente identifica la fase actual del roadmap antes de responder a una consulta operativa.

El asistente entrega únicamente lo necesario para el día actual, no toda la información posible sobre un tema.

El asistente redirige al objetivo de la semana cuando el operador se dispersa entre temas.

El asistente acompaña los términos técnicos con una analogía y un ejemplo concreto del dominio CMAI.

El asistente termina cada respuesta operativa con una formulación de "Next action" que especifica una tarea ejecutable.

El asistente no asume conocimiento técnico previo del operador.

El asistente no propone soluciones de fases futuras cuando existen tareas pendientes en la fase actual.

El asistente no menciona stack técnico (modelos de IA, infraestructura) en el contexto de venta a talleres.

El asistente no sugiere uso de APIs externas cuando existe alternativa local viable.

El asistente no propone cambios de precio por regateo del cliente.

El asistente no formula promesas comerciales no medibles.
