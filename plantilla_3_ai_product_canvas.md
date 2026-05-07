# Plantilla 3 — AI Product Canvas
## Framework PROMPT | Fase O — Oportunidad de IA
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: [Giancarlo Humberto Ferreyra Uribe]
- Integrante 2: [Sebastian Leonardo Muñico Diaz]
- Integrante 3: [Fernando Maquera]

**Fecha de entrega:** [07/05/26]
**Versión del canvas:** v1

---

> **Instrucción general:**
> Este canvas se completa DESPUÉS de tener aprobados el Problem Statement Canvas y el Data Readiness Checklist.

---

## SECCIÓN 1 — Identidad del producto

### 1.1 Nombre del producto / MVP
```
Nombre: EPP-Guard
```

### 1.2 Problema que resuelve
> *Copia exactamente la declaración del Problem Statement Canvas. No parafrasear.*

```
"El supervisor SSO de obras de construcción en Lima Metropolitana tiene
dificultad para verificar de forma continua y simultánea el uso correcto de
EPP en todos los frentes de trabajo porque la supervisión humana es serial e
intermitente, lo que genera detecciones tardías de incumplimientos asociados
a multas SUNAFIL superiores a S/ 100,000, paralizaciones de obra y un contexto
sectorial de 139 accidentes mortales solo en el trimestre abril-junio 2025."
```

### 1.3 Usuario principal
> *¿Quién usa el producto directamente? Sé específico — no "las empresas" sino el rol exacto.*

```
Supervisor de Seguridad y Salud Ocupacional (SSO) o Prevencionista de Riesgos
de obra. Usuario secundario: Residente de obra y Gerente de SST de la
constructora (recibe reportes consolidados).
```

### 1.4 Tipo de IA
- [ ] IA Generativa
- [X] ML Tradicional — Clasificación *(detección de objetos / visión computacional)*
- [ ] ML Tradicional — Regresión
- [ ] ML Tradicional — Clustering
- [ ] Combinación GenAI + ML

---

## SECCIÓN 2 — Tech & Cost Overview *(visión de temperatura)*

> **Esta sección NO es la selección definitiva del stack — esa se cierra en Fase M1.** Aquí se mapea qué categorías podrían aplicar y cuánto podría costar el MVP a nivel general.

---

### 2.1 Categorías de IA que podrían aplicar al problema

| Categoría | ¿Podría aplicar? | Razonamiento en 1 línea |
|---|---|---|
| **Modelos de lenguaje (LLM)** — texto, conversación, generación | NO | El núcleo del problema es percepción visual, no lenguaje. |
| **Visión computacional** — imágenes, video, detección visual | **SÍ** | Es la categoría central del proyecto: detectar EPP sobre frames de CCTV en tiempo real. |
| **ML supervisado tabular** — predicción con datos históricos | NO | No hay variable tabular a predecir; el insumo es video, no filas/columnas. |
| **ML no supervisado** — segmentación, clustering | NO | El problema tiene etiquetas claras (con/sin EPP), no se busca descubrir grupos. |
| **Automatización de flujos con IA** — orquestación de agentes | TAL VEZ | Útil para enrutar alertas a WhatsApp/email del supervisor cuando se detecta incumplimiento. |
| **Clasificación de audio / voz** | NO | El problema no involucra señales de audio. |

---

### 2.2 Estimación de costo rough del MVP

> *Nivel de precisión esperado: orden de magnitud, no presupuesto formal.*
> *Escala: 🟢 Gratuito o casi / 🟡 Freemium con límites / 🔴 Pago con costo significativo*

| Componente | Categoría de herramienta | Nivel de costo estimado | Comentario |
|---|---|---|---|
| Motor de IA principal | Roboflow RF-DETR Small + API hosted (plan público gratuito) | 🟢 | Entrenado con 4,000–8,000 imágenes EPP. mAP@0.5 baseline: 74.4% (v1). Costo cero en plan académico. |
| Cómputo / inferencia | GPU local o en la nube (Google Colab Pro, Kaggle, AWS Spot) | 🟡 | Colab gratuito alcanza para entrenamiento del MVP; Colab Pro (~S/ 40/mes) si se necesita más tiempo. |
| Interfaz o frontend | Dashboard web simple para el supervisor | 🟢 | Streamlit (gratis) o Bubble (freemium) son suficientes para MVP. |
| Almacenamiento de datos | Logs de detecciones + frames de incidentes | 🟢 | Google Drive / Airtable / Google Sheets en plan gratuito alcanza. |
| Orquestación / automatización | Envío de alertas en tiempo real | 🟢 | n8n self-hosted o cloud free tier; integración con WhatsApp Business API es opcional. |
| Otros *(APIs, integraciones)* | Cámara IP / acceso a CCTV | 🟡 | Si la obra ya tiene CCTV → costo cero. Si no, una cámara IP HD ronda S/ 200-400. |

**Costo total estimado del MVP (rango aproximado):**
```
Mínimo: S/ 0     / mes      (todo en planes gratuitos, CCTV existente)
Máximo: S/ 250   / mes      (Colab Pro + cámara extra + plan freemium escalado)

Supuestos clave de esta estimación:
- Se usa modelo open-source preentrenado (no se paga API por inferencia).
- La obra colaboradora ya cuenta con CCTV operativo.
- El MVP corre sobre 1-2 cámaras simultáneas, no 20.
- No se incluye costo de hardware dedicado para producción —es un MVP, no
  despliegue industrial.
```

---

### 2.3 Complejidad de implementación percibida

| Dimensión | Nivel | Comentario |
|---|---|---|
| Curva de aprendizaje de las herramientas | Media | Detección de objetos requiere conocer fine-tuning, anotación, métricas mAP. Hay tutoriales abundantes. |
| Disponibilidad de tutoriales y documentación | Alta | "PPE detection" es un caso de uso muy documentado en Roboflow, Kaggle y YouTube. |
| Dependencia de conocimiento técnico externo | Media | El equipo necesitará apoyarse en tutoriales y posiblemente en asesoría de un docente con perfil técnico. |
| Viabilidad de construir el MVP en 7 semanas | Media | Es viable si se usa modelo preentrenado + fine-tuning. Construir desde cero no lo es. |

---

> **\* Análisis financiero detallado:** se complementará con la **Plantilla 5 — Financial & Tech Feasibility**.

---

## SECCIÓN 3 — Experiencia del usuario

### 3.1 Input del usuario
> *¿Qué hace o ingresa el usuario para activar la IA?*

- [ ] Escribe texto en un chat o formulario
- [ ] Sube un archivo (imagen, PDF, CSV, audio)
- [ ] Hace clic en un botón o selecciona una opción
- [ ] Habla o graba un audio
- [X] El sistema se activa automáticamente (sin acción del usuario)
- [ ] Otro

**Descripción detallada del input:**
```
El sistema se conecta directamente al feed de CCTV de la obra (RTSP/HTTP) y
procesa frames en tiempo real sin intervención del supervisor. El supervisor
solo configura una vez al inicio: zonas de obra, EPP exigido por zona, número
de teléfono/email para alertas.
```

### 3.2 Output de la IA
> *¿Qué recibe el usuario como resultado de la interacción con la IA?*

- [ ] Texto / respuesta en lenguaje natural
- [ ] Número o predicción
- [ ] Clasificación o categoría
- [X] Alerta o notificación
- [ ] Recomendación con opciones
- [ ] Imagen generada
- [X] Acción ejecutada automáticamente *(registro guardado en log auditable)*
- [X] Otro: dashboard con estadísticas en vivo + frame de evidencia con bounding box

**Descripción detallada del output:**
```
Tres outputs combinados:
1. Alerta inmediata (WhatsApp/email/dashboard) cuando se detecta incumplimiento:
   "[14:32] Trabajador sin casco — Frente Norte — Cámara 2".
2. Frame de evidencia capturado y almacenado con bounding box dibujado, marca
   de tiempo y referencia de cámara → trazabilidad para SUNAFIL.
3. Dashboard con KPIs del día: % cumplimiento EPP, incidentes por zona,
   tendencia semanal.
```

---

## SECCIÓN 4 — Flujo del producto

### 4.1 Diagrama de flujo

```
Paso 1: Cámara CCTV de obra → captura video continuo
Paso 2: → Sistema extrae frames (1 cada N segundos)
Paso 3: → Modelo de visión computacional procesa el frame y detecta personas + EPP
Paso 4: → Lógica de validación: ¿cumple los EPP exigidos en esta zona?
            ├─ SÍ → registra OK en log, no notifica
            └─ NO → registra incidente + frame con bounding box → enruta alerta
Paso 5: → Supervisor SSO recibe notificación (WhatsApp/dashboard) y actúa
Paso 6: → Supervisor marca el incidente como atendido / falso positivo (feedback)
Paso 7: → Sistema acumula métricas para reporte diario / semanal a Gerencia SST
```

**Versión visual (opcional pero recomendada):**
```
Link o imagen: [PENDIENTE — recomendado dibujar en draw.io o Lucidchart antes
del Entregable 1]
```

### 4.2 Humano en el circuito
> *¿En qué punto del flujo interviene un humano para validar o corregir antes de que la IA actúe?*

- [ ] No hay intervención humana — la IA actúa de forma autónoma
- [X] El humano revisa el output antes de que llegue al usuario final *(parcial: el supervisor valida si el incidente es real antes de aplicar sanción/llamado de atención al trabajador)*
- [ ] El humano puede aprobar o rechazar la recomendación de la IA
- [ ] El humano interviene solo cuando la IA no tiene respuesta
- [ ] Otro

**Justificación de la decisión:**
```
La IA emite la alerta de forma autónoma (no espera aprobación humana para
notificar) porque el valor del producto está en la velocidad. Pero la decisión
de acción correctiva (llamar al trabajador, aplicar sanción, paralizar la zona)
es siempre humana — la IA no toma decisiones disciplinarias. Esto evita falsos
positivos que dañen la relación con trabajadores y mantiene al supervisor como
responsable final ante la Ley 29783.
```

### 4.3 Plan de contingencia
> *¿Qué pasa si la IA falla, no tiene respuesta o la API no está disponible?*

```
1. Si el modelo no logra clasificar un frame con confianza ≥ umbral, no genera
   alerta (no inunda al supervisor con falsos positivos) y registra el frame
   como "ambiguo" para revisión.
2. Si la cámara CCTV se desconecta, el sistema notifica al supervisor que
   perdió cobertura de esa zona (no falla en silencio).
3. Si la infraestructura completa falla, el supervisor vuelve al método actual
   (recorrido manual) — el producto complementa, no sustituye, la presencia
   física en obra.
```

---

## SECCIÓN 5 — Decisión estratégica Build / Buy / Integrate

Marca con una X:

- [ ] **Buy** — usar herramienta existente sin modificar
- [X] **Integrate** — conectar API de IA a flujo o interfaz propia
- [ ] **Build** — entrenar modelo propio con datos del equipo
- [ ] **Combinación** — *(parcial: hay fine-tuning, pero sobre modelo preentrenado)*

**Justificación (obligatoria):**
```
Buy se descarta porque las soluciones comerciales de detección de EPP existentes
(Avigilon, Viso.ai, Forsight) cuestan miles de dólares al mes y son cajas
negras inadaptables al contexto peruano. Build desde cero es inviable en 7
semanas y desperdiciaría décadas de progreso en visión computacional. Integrate
es la decisión correcta: tomar un modelo open-source preentrenado, hacerle
fine-tuning con datos relevantes al contexto de obra peruana, y construir
encima la lógica de negocio (zonas, alertas, dashboard, log auditable). El
valor diferencial del MVP no está en el modelo — está en el flujo de producto
y en la adaptación al contexto local.
```

---

## SECCIÓN 6 — Diseño específico por tipo de IA

### Si es IA Generativa — System Prompt
*(No aplica — el proyecto es ML Tradicional / Visión Computacional)*

---

### Si es ML Tradicional — Especificaciones del modelo

| Campo | Detalle |
|---|---|
| Tipo de modelo | Detección de objetos supervisada (subtipo de clasificación con localización espacial). |
| Herramienta de entrenamiento | **Roboflow** — entrenamiento con RF-DETR Small sobre checkpoint MS COCO. Dataset de 4,000–8,000 imágenes EPP (Roboflow Universe + Kaggle). API hosted para inferencia. Frontend: página web HTML + JS sobre Roboflow API. Alertas: n8n cloud free tier. |
| Variable objetivo (target) | Etiqueta de cumplimiento por frame: {Helmet (casco presente), no hat (sin casco), Safety Vest (chaleco presente), no vest (sin chaleco)}. Clases reales entrenadas en Roboflow. En versión 2.0 se agrega arnés y EPP secundarios. |
| Features principales | Píxeles del frame de video (input crudo). El modelo aprende automáticamente qué patrones visuales corresponden a cada clase — no se hace ingeniería manual de features. |
| Métrica de evaluación principal | mAP@0.5 (mean Average Precision con IoU 0.5) — métrica estándar en detección de objetos. Complementaria: Recall por clase (priorizado: queremos detectar todos los incumplimientos, aunque haya falsos positivos). |
| Criterio mínimo de aceptación | mAP@0.5 ≥ 0.80 sobre las clases priorizadas (casco, chaleco reflectivo, calzado, guantes). Recall ≥ 0.85 en clase "no hat" (la más crítica para mortalidad). Latencia ≤ 2 segundos por frame. Nota: modelo v1 obtuvo mAP 74.4% — versión 2 con clases unificadas apunta a ≥ 80%. |

---

## SECCIÓN 7 — Alcance del MVP

### Lo que SÍ incluye el MVP
> *Lista las funcionalidades que estarán listas para la sustentación de la Semana 14.*

```
1. Detección de 4 clases EPP: casco, chaleco reflectivo, calzado, guantes sobre
   imágenes y video.
2. Detección con bounding box: localización espacial del EPP en el frame,
   evidencia visual precisa para SUNAFIL.
3. Procesamiento de imágenes subidas manualmente o feed de video (1 cámara).
4. Generación de alerta automática (WhatsApp o email) cuando se detecta
   incumplimiento.
5. Almacenamiento del frame de evidencia con bounding box, timestamp y zona.
6. Dashboard simple: % cumplimiento del día, lista de incidentes, evidencia.
```

### Lo que NO incluye el MVP *(pero podría incluir una versión futura)*
> *Declarar esto explícitamente demuestra madurez en la gestión del proyecto.*

```
1. Detección de EPP secundarios (lentes, protección auditiva,
   barbiquejo del casco) — versión 2.0.
2. Reconocimiento de identidad del trabajador para reincidencias — versión 2.0
   (requiere análisis legal adicional bajo Ley 29733).
3. Integración con sistemas ERP/SST de la constructora.
4. Procesamiento simultáneo de múltiples cámaras (>5).
5. Análisis predictivo de zonas de mayor riesgo basado en histórico.
6. App móvil nativa para el supervisor (el MVP usa web responsive).
7. Arnés de seguridad (versión 2.0) — clases de altura requieren más
   datos de contexto peruano.
```

### Criterio de éxito del MVP
> *¿Cómo sabrá el equipo que el MVP está listo para ser sustentado?*

```
"El MVP está listo cuando un supervisor SSO externo al equipo puede subir
una imagen o video de obra, recibir la clasificación de EPP con bounding box
en menos de 2 segundos, ver el dashboard del día y descargar la evidencia
visual — sin instrucciones adicionales del equipo y con mAP@0.5 ≥ 0.80
sobre set de prueba externo."
```

---

## SECCIÓN 8 — OKRs y KPIs del producto

> **Esta sección define los compromisos de impacto del producto antes de construir.**

---

### Estructura obligatoria: O + KR + KPI

### OKR + KPI del proyecto

**Objetivo (O):**
> *Una frase que describe la mejora de negocio que el producto persigue.*

```
O: Reducir el riesgo regulatorio y de accidentabilidad de obras de
   construcción en Lima Metropolitana mediante supervisión continua y
   automatizada del uso correcto de EPP.
```

---

**Key Result 1 (KR1):**

| Campo | Detalle |
|---|---|
| Métrica | % de tiempo de jornada con cobertura efectiva de supervisión de EPP en frente de trabajo. |
| Valor actual | ~30% (estimación basada en patrón típico: 3 recorridos de 30 min en jornada de 9 h). Valor a confirmar con bitácora real de obra piloto antes de la sustentación. |
| Meta con el MVP | ≥ 90% de la jornada con cobertura efectiva sobre las zonas instrumentadas con cámara. |
| Método de medición | Comparación entre minutos de jornada cubiertos por inspección humana (registro en bitácora) vs. minutos cubiertos por el sistema (logs automáticos de detección). |
| Período de medición | Semana 12-13, sobre 5 jornadas piloto. |

---

**Key Result 2 (KR2):**

| Campo | Detalle |
|---|---|
| Métrica | Tiempo promedio de detección de un incumplimiento de EPP, desde que ocurre hasta que el supervisor recibe la alerta. |
| Valor actual | ~30 min estimados (depende del ciclo de recorrido del supervisor). Valor a confirmar con bitácora real de obra piloto antes de la sustentación. |
| Meta con el MVP | < 30 segundos desde el frame con incumplimiento hasta la notificación al supervisor. |
| Método de medición | Cronometraje manual sobre 30 incidentes inducidos en escenario controlado durante prueba piloto. |
| Período de medición | Semana 12. |

---

**Key Result 3 (KR3) — opcional pero recomendado:**

| Campo | Detalle |
|---|---|
| Métrica | Número de incidentes de incumplimiento de EPP detectados y registrados con evidencia auditable por jornada. |
| Valor actual | 0 (sistema actual no registra evidencia visual estructurada — solo bitácora de texto). |
| Meta con el MVP | ≥ 95% de los incumplimientos reales en zonas instrumentadas detectados y registrados con frame + timestamp + zona. |
| Método de medición | Auditoría cruzada: 2 observadores humanos cuentan incumplimientos en muestra de video y se compara contra el log del sistema. |
| Período de medición | Semana 13. |

---

**KPI técnico del modelo:**

| Campo | Detalle |
|---|---|
| Métrica técnica | mAP@0.5 (mean Average Precision con umbral IoU 0.5) sobre las 3 clases prioritarias. Complementaria: Recall en clase "sin casco". |
| Criterio mínimo aceptable | mAP@0.5 ≥ 0.80 — Recall clase "no hat" ≥ 0.85 — Latencia ≤ 2 s por frame. Baseline actual: mAP 74.4% (modelo v1 entrenado con 28,313 imágenes en Roboflow). |
| Método de medición | Evaluación sobre set de prueba externo (mínimo 50 imágenes no vistas durante entrenamiento). Panel de métricas Roboflow: "Average Precision by Class" sobre Validation Set y Test Set. |

---

### Verificación de coherencia interna

| Pregunta | Respuesta |
|---|---|
| ¿El Objetivo refleja directamente el problema de la Sección 1.2? | SÍ |
| ¿Los KRs son medibles con números concretos (no "mejorar" o "aumentar")? | SÍ |
| ¿El KPI técnico está conectado al tipo de IA elegido en la Sección 1.4? | SÍ — mAP es la métrica estándar en detección de objetos. |
| ¿El equipo puede obtener el valor actual de los KRs antes de la Semana 6? | PARCIAL — depende de obtener acceso a bitácora real del supervisor. Plan B: estimación con literatura sectorial. |

> **Si algún KR dice "mejorar", "aumentar" o "reducir" sin número → no es un KR válido.**

---

## SECCIÓN 9 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿El problema en la Sección 1.2 es copia exacta del Problem Statement Canvas? | SÍ |
| ¿El flujo de la Sección 4 es consistente con el tipo de IA elegido? | SÍ |
| ¿El stack tecnológico fue verificado (cuentas creadas, accesos confirmados)? | SÍ — Roboflow verificado: modelo v1 entrenado con 28,313 imágenes, mAP 74.4%. Cuenta activa. |
| ¿El alcance del MVP es realista para construir en 7 semanas? | SÍ — usando modelo preentrenado + fine-tuning |
| ¿El system prompt fue probado al menos una vez antes de entregar? | N/A (no hay system prompt — proyecto es ML Tradicional) |
| ¿El Objetivo del OKR refleja el problema definido en Fase P? | SÍ |
| ¿Los KRs tienen valores actuales concretos (no estimados)? | PARCIAL — pendiente confirmación con obra piloto |
| ¿Todos los integrantes entienden cada sección de este canvas? | [PENDIENTE] |

> **Si alguna respuesta es NO → el canvas no está listo para entregar.**

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 3 de 4*
