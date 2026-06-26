# Plantilla 3 — AI Product Canvas
## Framework PROMPT | Fase O — Oportunidad de IA
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Giancarlo Humberto Ferreyra Uribe
- Integrante 2: Sebastian Leonardo Muñico Diaz
- Integrante 3: Fernando Maquera

**Fecha de entrega:** 26/06/2026 — Semana 14

**Versión del canvas:** v2 (final PC2)

> **Actualización PC2:** Se confirma el stack realmente construido (modelo `epp-casco-v3` en Docker local expuesto con Pinggy, frontend en Netlify + alertas n8n/Twilio), se precisa el alcance ejecutado (foco en casco) y los OKRs se mantienen **idénticos** a los comprometidos en la PC1 — son inamovibles.

---

## SECCIÓN 1 — Identidad del producto

### 1.1 Nombre del producto / MVP
```
EPP-Guard
```

### 1.2 Problema que resuelve

```
"El supervisor SSO de obras de construcción en Lima Metropolitana tiene
dificultad para verificar de forma continua y simultánea el uso correcto de
EPP en todos los frentes de trabajo porque la supervisión humana es serial e
intermitente, lo que genera detecciones tardías de incumplimientos asociados
a multas SUNAFIL superiores a S/ 100,000, paralizaciones de obra y un
contexto sectorial de 139 accidentes mortales solo en el trimestre
abril-junio 2025."
```

### 1.3 Usuario principal
```
Supervisor de Seguridad y Salud Ocupacional (SSO) de obras de construcción
medianas (30–100 trabajadores) en Lima Metropolitana, responsable ante SUNAFIL
del cumplimiento de la Norma G.050.
```

### 1.4 Tipo de IA
- [ ] IA Generativa
- [X] ML Tradicional — Clasificación *(detección de objetos)*
- [ ] ML Tradicional — Regresión
- [ ] ML Tradicional — Clustering
- [ ] Combinación GenAI + ML

---

## SECCIÓN 2 — Tech & Cost Overview

### 2.1 Categorías de IA que podrían aplicar

| Categoría | ¿Podría aplicar? | Razonamiento |
|---|---|---|
| **Modelos de lenguaje (LLM)** | TAL VEZ | Solo para redactar la notificación de la alerta (componente accesorio). |
| **Visión computacional** | SÍ | Es el núcleo: detectar casco/sin casco sobre imágenes. |
| **ML supervisado tabular** | NO | No hay datos tabulares; el insumo es imagen. |
| **ML no supervisado** | NO | El problema requiere etiquetas (con/sin casco). |
| **Automatización de flujos con IA** | SÍ | Orquestación de la alerta (n8n) tras la detección. |
| **Clasificación de audio / voz** | NO | El problema es visual. |

### 2.2 Estimación de costo del MVP

| Componente | Categoría de herramienta | Nivel de costo | Comentario |
|---|---|---|---|
| Motor de IA principal | Visión computacional (RF-DETR en Docker local) | 🟢 | S/ 0 — inferencia self-hosted |
| Interfaz / frontend | Web HTML + JS | 🟢 | S/ 0 |
| Almacenamiento de datos | Google Sheets / log local | 🟢 | S/ 0 free tier |
| Orquestación / alertas | n8n + Twilio (sandbox) | 🟢 | S/ 0 en MVP |
| Otros (cámara, túnel) | Cámara IP / Pinggy / Netlify | 🟡 | S/ 0 (Pinggy y Netlify free) + S/ 200–400 cámara si no hay CCTV |

**Costo total estimado del MVP:**
```
Mínimo: S/ 0 / mes      Máximo: S/ 250 / mes
Supuestos: obra con CCTV existente = S/ 0; sin CCTV = costo único de cámara.
Inferencia local en Docker → sin costo recurrente de API.
```

### 2.3 Complejidad de implementación percibida

| Dimensión | Nivel | Comentario |
|---|---|---|
| Curva de aprendizaje | Media | Roboflow y n8n son no-code/low-code; Docker + túnel requirió algo más. |
| Documentación disponible | Alta | Roboflow, n8n y Twilio bien documentados. |
| Dependencia de conocimiento técnico externo | Media | Despliegue self-hosted exigió aprender Docker/Pinggy sobre la marcha. |
| Viabilidad de construir el MVP en 7 semanas | Alta | Modelo preentrenado + fine-tuning lo hizo viable. |

---

## SECCIÓN 3 — Experiencia del usuario

### 3.1 Input del usuario
- [X] Sube un archivo (imagen)
- [X] El sistema se activa automáticamente (webcam/stream en vivo)

```
El usuario sube una foto de obra o activa la webcam. En operación real, la cámara
de obra alimenta el stream de forma continua sin acción del supervisor.
```

### 3.2 Output de la IA
- [X] Clasificación o categoría (con casco / sin casco)
- [X] Alerta o notificación (WhatsApp)
- [X] Acción ejecutada automáticamente (registro de evidencia)

```
El usuario ve las detecciones sobre la imagen (cajas + confianza), un contador de
trabajadores sin casco, y —ante un incumplimiento— recibe una alerta de WhatsApp
con zona, clase, confianza y timestamp, además del registro de evidencia.
```

---

## SECCIÓN 4 — Flujo del producto

### 4.1 Diagrama de flujo
```
Paso 1: Cámara de obra / usuario → captura imagen
Paso 2: → Modelo epp-casco-v3 (Docker local) clasifica head/helmet/person
Paso 3: → ¿Hay detecciones `head` (sin casco)?
Paso 4: → SÍ: dispara webhook → n8n (agentes Gemini) arma mensaje → Twilio
Paso 5: → Usuario recibe: alerta de WhatsApp + evidencia (imagen+hora+confianza)
```

### 4.2 Humano en el circuito
- [X] El humano puede aprobar o rechazar la recomendación de la IA

```
El supervisor valida toda alerta antes de actuar. El sistema no sanciona ni decide
de forma autónoma: detecta y notifica; la decisión y la responsabilidad legal son
del supervisor. Esto protege contra falsos positivos (p. ej. un casco de moto que el modelo aún reconoce como casco de seguridad y que no debería aceptar).
```

### 4.3 Plan de contingencia
```
Si el modelo no detecta con suficiente confianza, no dispara alerta y el frame
queda como "ambiguo" (la supervisión humana sigue activa como respaldo). Si el
servicio cae (máquina apagada o túnel Pinggy caído), el sistema deja de operar y
se recurre a la supervisión humana habitual; para producción se prevé inferencia
en servidor dedicado con monitoreo y fallback.
```

---

## SECCIÓN 5 — Decisión estratégica Build / Buy / Integrate

- [ ] **Buy**
- [X] **Integrate** — conectar/adaptar modelo preentrenado a flujo propio
- [ ] **Build**

**Justificación:**
```
Buy: soluciones comerciales (Viso.ai, Avigilon) superan los $1,000/mes, son cajas
negras y no se adaptan al contexto peruano. Build: entrenar desde cero es inviable
en 7 semanas y desperdicia años de progreso en visión computacional. Integrate:
tomamos un modelo preentrenado (RF-DETR), lo afinamos con datos de casco y
construimos el flujo de producto (detección → alerta → evidencia). El valor
diferencial está en el flujo y la adaptación local, no en el modelo base. Costo cero.
```

---

## SECCIÓN 6 — Diseño específico por tipo de IA

### ML Tradicional — Especificaciones del modelo

| Campo | Detalle |
|---|---|
| Tipo de modelo | Detección de objetos (clasificación supervisada) |
| Herramienta de entrenamiento | Roboflow — RF-DETR Small (pretrain Objects365/MS COCO, ~100 épocas) |
| Variable objetivo (target) | Clase por objeto: `head` (sin casco) / `helmet` (con casco) / `person` |
| Features principales | Píxeles de la imagen; el modelo aprende patrones visuales del casco |
| Métrica de evaluación principal | mAP@0.5 (+ Precision, Recall, F1) |
| Criterio mínimo de aceptación | mAP@0.5 ≥ 80% · Recall en clase `head` ≥ 85% |

> **Resultado alcanzado (`epp-casco-v3`):** mAP@50 90.9% · Precision 90.9% · Recall 86.8% · F1 88.7%.

**Decisión documentada (Fase M1):**
```
Herramienta elegida: Roboflow (RF-DETR Small) + Docker para inferencia local.
Alternativas descartadas:
  - Teachable Machine — solo clasifica imagen completa, no detecta objetos por persona.
  - YOLOv8 self-host puro — viable, pero Roboflow aceleró el etiquetado y el entrenamiento.
  - Inferencia hosted Roboflow — descartada en producción por límite de créditos free.
Justificación: RF-DETR dio mejor mAP en nuestras pruebas; Docker local resolvió el
costo de inferencia (S/ 0) y mantuvo el MVP accesible por URL pública (frontend en Netlify, servicios vía Pinggy).
```

---

## SECCIÓN 7 — Alcance del MVP

### Lo que SÍ incluye el MVP
```
1. Detección de casco: clases head (sin casco) / helmet (con casco) / person.
2. Demo en navegador: subir foto o activar webcam, con cajas y confianza.
3. Alerta automática por WhatsApp ante un incumplimiento (~12 s).
4. Registro de evidencia: imagen + timestamp + zona + confianza.
```

### Lo que NO incluye el MVP (versión futura)
```
1. Chaleco reflectivo, calzado y guantes (las otras 3 clases del plan original).
2. Localización de identidad / reconocimiento facial.
3. Múltiples cámaras simultáneas e integración con ERP/SST de la constructora.
```

### Criterio de éxito del MVP
```
"El MVP está listo cuando un usuario externo al equipo puede subir una foto de obra,
recibir la clasificación de casco en <5 segundos, recibir la alerta y descargar la
evidencia, sin instrucciones adicionales."
```

---

## SECCIÓN 8 — OKRs y KPIs del producto

**Objetivo (O):**
```
Reducir el riesgo regulatorio y de accidentabilidad de obras de construcción en
Lima Metropolitana mediante supervisión continua y automatizada del uso correcto
de EPP (foco MVP: casco de seguridad).
```

**Key Result 1 (KR1) — Cobertura:**

| Campo | Detalle |
|---|---|
| Métrica | % de jornada con supervisión efectiva en zona instrumentada |
| Valor actual | ~30% |
| Meta con el MVP | ≥ 90% |
| Método de medición | Bitácora supervisor vs. logs del sistema, 5 jornadas |
| Período de medición | Semanas 12–13 |

**Key Result 2 (KR2) — Velocidad:**

| Campo | Detalle |
|---|---|
| Métrica | Tiempo desde incumplimiento hasta alerta al supervisor |
| Valor actual | ~30 min |
| Meta con el MVP | < 30 seg |
| Método de medición | Cronometraje sobre 30 incidentes inducidos |
| Período de medición | Semanas 12–13 |

**Key Result 3 (KR3) — Trazabilidad:**

| Campo | Detalle |
|---|---|
| Métrica | % de incumplimientos con evidencia auditable |
| Valor actual | 0% |
| Meta con el MVP | ≥ 95% |
| Método de medición | Auditoría cruzada observadores vs. log |
| Período de medición | Semanas 12–13 |

**KPI técnico del modelo:**

| Campo | Detalle |
|---|---|
| Métrica técnica | mAP@0.5 + Recall en clase `head` (sin casco) |
| Criterio mínimo aceptable | mAP@0.5 ≥ 80% · Recall ≥ 85% |
| Método de medición | Set de prueba externo (mín. 50 imágenes), panel Roboflow |

### Verificación de coherencia interna

| Pregunta | Respuesta |
|---|---|
| ¿El Objetivo refleja directamente el problema de la Sección 1.2? | SÍ |
| ¿Los KRs son medibles con números concretos? | SÍ |
| ¿El KPI técnico está conectado al tipo de IA elegido? | SÍ |
| ¿El equipo puede obtener el valor actual de los KRs? | SÍ (valores baseline estimados con patrón de supervisión) |

---

## SECCIÓN 9 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿El problema en la Sección 1.2 es copia exacta del Problem Statement Canvas? | SÍ |
| ¿El flujo de la Sección 4 es consistente con el tipo de IA elegido? | SÍ |
| ¿El stack tecnológico fue verificado (cuentas creadas, accesos confirmados)? | SÍ — modelo entrenado y desplegado |
| ¿El alcance del MVP es realista para construir en 7 semanas? | SÍ |
| ¿El system prompt fue probado? | N/A — proyecto ML Tradicional (la GenAI solo redacta la alerta) |
| ¿El Objetivo del OKR refleja el problema de la Fase P? | SÍ |
| ¿Los KRs tienen valores actuales concretos? | SÍ |
| ¿Todos los integrantes entienden cada sección de este canvas? | SÍ |

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 3 de 4*
