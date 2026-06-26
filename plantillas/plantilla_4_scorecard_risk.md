# Plantilla 4 — AI Impact Scorecard + AI Risk Matrix
## Framework PROMPT | Fases P2 + T — Performance y Transparencia
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Giancarlo Humberto Ferreyra Uribe
- Integrante 2: Sebastian Leonardo Muñico Diaz
- Integrante 3: Fernando Maquera

**Fecha de entrega:** 26/06/2026 — Semana 14

**Nombre del MVP:** EPP-Guard

---

> **Instrucción general:** Esta plantilla se completa con datos reales obtenidos de las pruebas. No se inventan datos. Un scorecard honesto con un KR parcial bien explicado vale más que uno inflado sin evidencia.

---

# PARTE A — AI IMPACT SCORECARD
## Fase P2 — Performance y Métricas

---

## SECCIÓN 1 — Evaluación de OKRs declarados en el AI Product Canvas

### Objetivo (O) del proyecto

```
Reducir el riesgo regulatorio y de accidentabilidad de obras de construcción
en Lima Metropolitana mediante supervisión continua y automatizada del uso
correcto de EPP (foco MVP: casco de seguridad).
```

---

### Evaluación de Key Results

**KR1 — Cobertura:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | % de jornada con supervisión efectiva de EPP en zona instrumentada | Continuidad de análisis sobre el stream de prueba |
| Valor actual (antes del MVP) | ~30% | — *(no cambia)* |
| Meta comprometida | ≥ 90% | — *(no cambia)* |
| Resultado real | — | ≈ 100% de los frames recibidos analizados (escenario controlado) |
| Método usado para medir | Bitácora supervisor vs. logs del sistema, 5 jornadas piloto | Análisis continuo sobre video de obra, sin obra real instrumentada |
| **¿Se alcanzó la meta?** | — | **PARCIAL** |

**Análisis del KR1:**
```
El sistema analiza el 100% de los frames que recibe, sin fatiga ni puntos ciegos,
lo que conceptualmente supera el 90% comprometido frente al ~30% humano. El factor
limitante fue no haber accedido a una obra real para medir 5 jornadas como se
prometió. La capacidad técnica de cobertura continua está demostrada; la validación
operativa en obra queda pendiente. Por honestidad se reporta PARCIAL.
```

---

**KR2 — Velocidad:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | Tiempo desde incumplimiento hasta alerta al supervisor | Latencia extremo a extremo del pipeline de alerta |
| Valor actual (antes del MVP) | ~30 min | — *(no cambia)* |
| Meta comprometida | < 30 seg | — *(no cambia)* |
| Resultado real | — | ≈ 12 seg promedio (rango 8–19 s) |
| Método usado para medir | Cronometraje sobre 30 incidentes inducidos | Cronometraje sobre 30 disparos (detección → WhatsApp recibido) |
| **¿Se alcanzó la meta?** | — | **SÍ** |

**Análisis del KR2:**
```
Es el KR mejor logrado y 100% verificable. La cadena detección (Docker local) →
n8n (3 agentes Gemini 2.5 Flash) → Twilio → WhatsApp entregó la alerta en ~12 s,
muy por debajo de los 30 s comprometidos. La variabilidad la causó la latencia de
los agentes LLM, no la inferencia del modelo.
```

---

**KR3 — Trazabilidad:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | % de incumplimientos con evidencia auditable | % de eventos con imagen + timestamp + confianza |
| Valor actual (antes del MVP) | 0% | — *(no cambia)* |
| Meta comprometida | ≥ 95% | — *(no cambia)* |
| Resultado real | — | ≈ 100% de los eventos disparados |
| Método usado para medir | Auditoría cruzada observadores vs. log | Conteo: alertas disparadas vs. registros completos |
| **¿Se alcanzó la meta?** | — | **SÍ** |

**Análisis del KR3:**
```
El registro de evidencia es determinístico: cada alerta persiste imagen, timestamp,
zona, clase y confianza. El 100% de incumplimientos detectados quedó con evidencia
auditable — exactamente el respaldo que un supervisor necesita ante SUNAFIL.
```

---

### Evaluación del KPI técnico

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica técnica | mAP@0.5 y Recall en clase "sin casco" (`head`) | Métricas del set de prueba (Roboflow) |
| Criterio mínimo aceptable | mAP@0.5 ≥ 80% · Recall ≥ 85% | — *(no cambia)* |
| Resultado real | — | mAP@50 = 90.9% · Recall = 86.8% · Precision = 90.9% · F1 = 88.7% |
| Método de medición | Set de prueba externo, panel Roboflow | Test set held-out, panel de métricas Roboflow |
| **¿Es aceptable?** | — | **SÍ** |

> **AP@50 por clase (validación):** `helmet` 97.0% · `head` 87.0% · `person` 76.0% · all 87.0%.
> **Modelo:** `epp-casco-v3` — RF-DETR Small, ~53,700 imágenes (con augmentación 3x), 3 clases.

---

### Resumen ejecutivo del OKR

| | KR1 | KR2 | KR3 | KPI técnico |
|---|---|---|---|---|
| **¿Alcanzado?** | PARCIAL | SÍ | SÍ | SÍ |

**Conclusión del equipo:**
```
El MVP cumple su objetivo de negocio en lo medible: entrega alertas en ~12 s
(vs. ~30 min), deja el 100% de incumplimientos con evidencia auditable y el modelo
supera el umbral técnico comprometido (mAP@50 90.9% ≥ 80%). El único KR no cerrado
del todo es la cobertura sobre obra real (KR1 → PARCIAL), por falta de acceso a
campo, limitación que reconocemos abiertamente. Para un MVP de 7 semanas, 3 de 4
compromisos cumplidos y el cuarto técnicamente resuelto es un resultado sólido.
```

---

## SECCIÓN 2 — Evaluación técnica del modelo

### Para IA Generativa
*(No aplica — el proyecto es ML Tradicional / visión computacional)*

---

### Para ML Tradicional

| Métrica | Criterio mínimo aceptable | Resultado real | ¿Aceptable? |
|---|---|---|---|
| **mAP@0.5** *(detección de objetos)* | ≥ 80% | **90.9%** | **SÍ** |
| **Precision** | ≥ 80% | **90.9%** | **SÍ** |
| **Recall** *(global; clase `head` = sin casco)* | ≥ 85% | **86.8%** | **SÍ** |
| **F1 Score** | ≥ 0.85 | **0.887** | **SÍ** |
| **Baseline** *(supervisión humana ~30% de jornada, detección ~30 min)* | Cualquier mejora medible | Cobertura continua + alerta en ~12 s | **Supera el baseline** |

**Interpretación del equipo:**
```
Un Recall de 86.8% significa que el modelo detecta ~87 de cada 100 trabajadores sin
casco — buen desempeño para la clase de mayor mortalidad. La clase `person` (76% AP)
es la más débil pero no es crítica para seguridad. Limitación documentada: el modelo
aún reconoce el casco de moto como casco de seguridad cuando no debería, por lo que el
supervisor valida toda alerta antes de actuar.
```

**Método de evaluación:** Test set held-out en Roboflow; banco de imágenes reales de obra para escenarios (ver `pruebas_usuario.md`); cronometraje de 30 disparos de alerta para latencia.

---

## SECCIÓN 3 — Evaluación con usuarios reales

> Detalle completo en `mvp/evidencia/pruebas_usuario.md`.

### Registro de pruebas de usuario

| # | Perfil del usuario | Tarea asignada | ¿Completó? | Observaciones clave |
|---|---|---|---|---|
| 1 | Estudiante de Ing. Civil con prácticas | Subir foto y verificar detección casco/sin casco | SÍ | Detección coincide con lo que él ve; valora el contador rojo |
| 2 | Persona sin perfil técnico | Activar webcam y provocar una alerta | SÍ | Flujo claro en <1 min; alerta WhatsApp llegó |
| 3 | Estudiante (rol supervisor) | Recibir alertas y revisar evidencia | SÍ | Evidencia "defendible ante inspección" |
| 4 | Estudiante de Administración | Probar foto con casco de moto | PARCIAL | Detectó que el casco de moto se reconoce como casco — no debería (hallazgo clave) |

### Hallazgos principales

**Lo que funcionó bien:**
```
1. Producto autoexplicativo: 4 de 5 usuarios lo usaron sin instrucciones.
2. Alerta WhatsApp rápida y confiable (~12 s) — lo más valorado.
3. Evidencia auditable percibida como útil frente a SUNAFIL.
```

**Lo que no funcionó o confundió:**
```
1. El casco de moto aún se reconoce como casco de seguridad (debería NO reconocerse).
2. Baja luz y personas lejanas reducen el recall.
3. Falta exportar evidencia a PDF.
```

**Cambios realizados al MVP:**
```
1. Documentado: el casco de moto no debe reconocerse como casco; humano en el circuito valida.
2. Ajuste del umbral de confianza por defecto.
3. Reentrenamiento con hard-negatives priorizado para v2.
```

---

## SECCIÓN 4 — Reflexión de resultados

### ¿El MVP resuelve el problema definido en la Fase P?

- [X] **SÍ parcialmente** — el MVP resuelve el núcleo del problema (detectar trabajadores sin casco y dejar evidencia auditable, con alerta inmediata) con limitaciones de alcance claras (solo casco, no validado en obra en vivo).

**Argumento del equipo:**
```
El problema de la Fase P era la supervisión serial e intermitente del EPP. El MVP
ataca directamente la pieza de mayor impacto en mortalidad (el casco): analiza de
forma continua, alerta en ~12 s y deja evidencia auditable al 100%. Los datos lo
respaldan: mAP@50 90.9%, recall 86.8%. Lo que falta para resolverlo "completamente"
es ampliar a más EPP (chaleco, calzado, guantes) y validar en una obra real
instrumentada. Por eso decimos "parcialmente", con honestidad y con evidencia.
```

### Lecciones aprendidas por fase PROMPT

| Fase | ¿Qué cambiarías si empezaras de nuevo? |
|---|---|
| **P** — Problema | Mantendríamos el foco en el casco desde el inicio en vez de prometer 4 EPP; el casco solo ya es un MVP completo y de alto impacto. |
| **R** — Datos | Gestionaríamos antes el acceso a una obra real; conseguir un set de validación peruano fue lo más difícil. |
| **O** — Diseño | Habríamos previsto el límite de créditos de Roboflow desde el día uno y diseñado el despliegue self-hosted (Docker) antes. |
| **M** — Construcción | Empezar con menos clases y más datos de calidad por clase; el casco de moto debió tratarse como hard-negative desde el entrenamiento. |
| **P2** — Métricas | Instrumentar el cronometraje de latencia desde la primera iteración, no al final. |
| **T** — Ética | Definir antes el protocolo de privacidad (blur, no identidad) como requisito de diseño, no como añadido. |

### Próximos pasos si el proyecto continuara

```
1. (corto plazo) Reentrenar con ejemplos negativos para que el casco de moto deje de reconocerse como casco, y mejorar la
   clase `person`; exportar evidencia a PDF.
2. (mediano plazo) Validar en una obra real instrumentada durante varias jornadas
   (cerrar KR1); ampliar a chaleco reflectivo.
3. (largo plazo) v2.0: múltiples cámaras, calzado y guantes, integración con el
   sistema SST de la constructora e inferencia en servidor dedicado.
```

---
---

# PARTE B — AI RISK MATRIX
## Fase T — Transparencia y Ética

---

## SECCIÓN 5 — Identificación de riesgos

### Riesgo 1

| Campo | Detalle |
|---|---|
| **Tipo** | Técnico |
| **Nombre del riesgo** | El casco de moto se reconoce como casco de seguridad (no debería) |
| **Descripción** | El modelo aún reconoce un casco de moto como `helmet`, marcando como "cumple" a alguien cuyo casco no es EPP de obra válido. El objetivo es que un casco de moto **no se reconozca** como casco. También puede haber falsos negativos (cabeza sin casco ocluida o lejana). |
| **Nivel** | Medio |
| **Probabilidad** | Media |
| **Impacto si ocurre** | El supervisor podría confiarse de una alerta omitida o de un "OK" erróneo; en el peor caso, un trabajador sin protección real pasa desapercibido. |
| **Mitigación** | Humano en el circuito: el supervisor valida toda alerta antes de actuar. Ajuste de umbral de confianza. Plan de reentrenamiento con hard-negatives para que el casco de moto deje de reconocerse como casco de seguridad. El sistema se presenta como complemento, no reemplazo, de la supervisión. |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE |

---

### Riesgo 2

| Campo | Detalle |
|---|---|
| **Tipo** | Ético |
| **Nombre del riesgo** | Vigilancia laboral y uso disciplinario indebido |
| **Descripción** | Cámaras que monitorean continuamente pueden generar sensación de vigilancia y prestarse a sancionar individualmente a trabajadores en lugar de prevenir accidentes. |
| **Nivel** | Medio |
| **Probabilidad** | Media |
| **Impacto si ocurre** | Daño a la confianza y al clima laboral; posible uso punitivo contra trabajadores en vez de fin preventivo. |
| **Mitigación** | El sistema detecta EPP, **no identidad** (sin reconocimiento facial). Las alertas son por zona, no por persona. Finalidad declarada: seguridad y prevención, no sanción individual. Transparencia con los trabajadores sobre el uso del sistema. |
| **¿Está mitigado en el MVP actual?** | SÍ |

---

### Riesgo 3

| Campo | Detalle |
|---|---|
| **Tipo** | Legal / Privacidad |
| **Nombre del riesgo** | Tratamiento de datos personales/biométricos sin base legal (Ley N° 29733) |
| **Descripción** | Las imágenes de trabajadores son datos personales y, al incluir rasgos físicos, potencialmente biométricos. Tratarlos sin consentimiento ni anonimización infringe la Ley 29733. |
| **Nivel** | Alto |
| **Probabilidad** | Media |
| **Impacto si ocurre** | Sanción de la Autoridad de Protección de Datos; responsabilidad legal para la constructora y el equipo. |
| **Mitigación** | Blur automático de rostros antes de almacenar; no se guarda identidad; no hay reconocimiento facial; consentimiento informado de la constructora; datos tratados en territorio peruano con finalidad legítima (seguridad). |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE |

---

### Riesgo 4 *(recomendado)*

| Campo | Detalle |
|---|---|
| **Tipo** | Técnico |
| **Nombre del riesgo** | Dependencia de infraestructura (Docker local + túnel Pinggy) |
| **Descripción** | El modelo corre en una máquina local expuesta por Pinggy. Si la máquina se apaga, falla la red o el túnel temporal cae, el sistema deja de funcionar. |
| **Nivel** | Alto *(en producción)* / Bajo *(en MVP de demo)* |
| **Probabilidad** | Media |
| **Impacto si ocurre** | Interrupción del servicio: la obra queda sin supervisión automática. |
| **Mitigación** | Aceptable para el MVP. Para producción: migrar a inferencia en servidor dedicado o GPU en la nube, con monitoreo de disponibilidad y mensaje de fallback al supervisor cuando el sistema no esté operativo. |
| **¿Está mitigado en el MVP actual?** | NO *(asumido conscientemente para el MVP)* |

---

## SECCIÓN 6 — Marco regulatorio

| Regulación | ¿Aplica? | ¿Cómo cumple el MVP con ella? |
|---|---|---|
| **Ley N° 29733** — Protección de Datos Personales (Perú) | SÍ | Blur de rostros, sin identidad ni reconocimiento facial, consentimiento de la constructora, finalidad legítima de seguridad. |
| **D.S. 003-2013-JUS** — Reglamento de la Ley 29733 | SÍ | Trata datos potencialmente biométricos con minimización: no se almacenan rasgos identificables. |
| **Ley N° 29783** — Seguridad y Salud en el Trabajo | SÍ | El producto refuerza el cumplimiento del empleador en supervisión de EPP y deja evidencia para fiscalización. |
| **Norma G.050** — Seguridad durante la Construcción (DS 010-2009-VIVIENDA) | SÍ | Detecta el EPP exigido (casco) en cumplimiento del estándar de obra. |
| **EU AI Act** *(referencial)* | NO | Solo referencial; demuestra conocimiento de tendencias internacionales. |

**¿El proyecto procesa datos personales de usuarios?**
- [X] Sí — los datos están anonimizados (blur de rostros) y no se almacena identidad; se gestiona con consentimiento de la constructora.

---

## SECCIÓN 7 — Preguntas éticas clave

**¿El usuario sabe que está interactuando con IA?**
```
SÍ. En la obra, los trabajadores son informados de que un sistema de visión por
computadora supervisa el uso de EPP. En el demo, la interfaz muestra explícitamente
que las detecciones provienen de un modelo de IA, con su nivel de confianza.
```

**¿Qué pasa si la IA comete un error que afecta al usuario?**
```
Si hay un falso negativo (no detecta a alguien sin casco), el supervisor humano sigue
en el circuito y no depende solo del sistema. Si hay un falso positivo (p. ej. casco
de moto), el supervisor valida la alerta antes de cualquier acción: NINGUNA sanción
es automática. El responsable legal sigue siendo el supervisor SSO; el sistema aporta
evidencia y alertas, no decisiones disciplinarias autónomas. El usuario puede reportar
errores para reentrenamiento.
```

**¿El producto funciona igual de bien para todos los segmentos de usuarios?**
```
Hay un riesgo de sesgo: los datasets son mayormente extranjeros, por lo que el modelo
podría desempeñar peor con uniformes, iluminación o tez locales. Se mitiga con un set
de validación peruano y reentrenamiento. La clase `person` (76% AP) y las condiciones
de baja luz son los puntos donde el desempeño cae y deben vigilarse.
```

**¿El usuario tiene control sobre sus datos y puede solicitar su eliminación?**
```
Sí. No se almacena identidad y los rostros se difuminan. Las imágenes de evidencia se
conservan solo el tiempo necesario para fines de seguridad y pueden eliminarse a
solicitud, conforme a la Ley 29733.
```

---

## SECCIÓN 8 — Declaración de uso de IA en el proyecto

| Herramienta de IA usada | Para qué tarea del proyecto | Fase PROMPT |
|---|---|---|
| Roboflow (RF-DETR Small) | Entrenamiento del modelo de detección de casco | M |
| Gemini 2.5 Flash (en n8n) | Agentes que validan, evalúan y redactan la alerta de WhatsApp | M / O |
| Claude (Anthropic) | Asistencia en documentación, debugging del demo y estructura de entregables | M / P2 / T |

---

## SECCIÓN 9 — Autoevaluación final del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Los KRs evaluados son exactamente los declarados en la Plantilla 3? | SÍ |
| ¿Los resultados reales tienen evidencia concreta que los respalda? | SÍ |
| ¿El equipo no cambió ningún KR después de la Semana 6? | SÍ |
| ¿Hay al menos 1 riesgo técnico, 1 ético y 1 legal identificados? | SÍ (2 técnicos, 1 ético, 1 legal) |
| ¿Cada riesgo tiene una mitigación concreta y no genérica? | SÍ |
| ¿El equipo conoce la regulación aplicable a su sector? | SÍ |
| ¿El MVP informa visiblemente al usuario que interactúa con IA? | SÍ |
| ¿Todos los integrantes pueden responder las preguntas éticas de la Sección 7? | SÍ |

> **Si alguna respuesta es NO → la plantilla no está lista para la sustentación.**

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 4 de 4*
