# Plantilla 2 — Data Readiness Checklist
## Framework PROMPT | Fase R — Recursos de Datos
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Giancarlo Humberto Ferreyra Uribe
- Integrante 2: Sebastian Leonardo Muñico Diaz
- Integrante 3: Fernando Maquera

**Fecha de entrega:** 26/06/2026 — Semana 14

**Tipo de IA del proyecto:** ML Tradicional — Visión Computacional / Detección de objetos (clasificación supervisada)

**Versión:** v2 (final PC2)

> **Actualización PC2:** Se confirma con datos reales el dataset efectivamente usado para entrenar el modelo final (`epp-casco-v3`) y se documenta el bloqueante de inferencia que apareció en la Fase M (agotamiento de créditos de Roboflow) y su resolución (despliegue self-hosted con Docker + Pinggy y frontend en Netlify).

---

## SECCIÓN 1 — Inventario de datos

### Para IA Generativa — Inventario de conocimiento
*(No aplica — el proyecto es ML Tradicional con visión computacional)*

---

### Para ML Tradicional — Inventario de datos históricos

| # | Dataset | Fuente | Formato | N° de registros aprox. | ¿Tiene etiquetas? |
|---|---|---|---|---|---|
| 1 | Dataset combinado de detección de casco/EPP en construcción | Roboflow Universe / Kaggle (datasets abiertos PPE) | Imágenes JPG/PNG + anotaciones (bounding boxes) | **~53,700 imágenes** (con augmentación 3x sobre el set base) | SÍ — clases `head`, `helmet`, `person` |
| 2 | Imágenes de validación en contexto peruano (luz, polvo, atuendo) | Captura/recolección propia + web de obras peruanas | JPG | 100–250 (set de prueba) | SÍ — etiquetado manual del equipo |

**Distribución de clases del modelo final (`epp-casco-v3`):**
```
helmet (con casco) ........ ~35,200 instancias
person (persona) .......... ~27,735 instancias
head   (cabeza sin casco) . ~14,011 instancias
```

**Variable objetivo (target):**

```
Por cada persona detectada en un frame, determinar si su cabeza tiene casco
(`helmet`) o está descubierta (`head`). La presencia de una o más detecciones
`head` constituye un incumplimiento que dispara la alerta.

Alcance MVP: el casco de seguridad — el EPP con mayor correlación con accidentes
mortales por golpe/caída. Chaleco reflectivo, calzado y guantes quedan para v2.

Output: bounding box por objeto + etiqueta (head/helmet/person) + confianza.
```

**Tipo de problema confirmado:**
- [X] Clasificación — predice una categoría (con casco / sin casco)
- [ ] Regresión
- [ ] Clustering

> *Subtipo técnico: detección de objetos (object detection) = localización espacial + clasificación multiclase.*

---

### Para Combinación
*(No aplica — la GenAI en n8n solo redacta la alerta; no participa en la detección)*

---

## SECCIÓN 2 — Evaluación de calidad con semáforo

> 🟢 listo · 🟡 necesita trabajo (con plan) · 🔴 bloqueante

### Dataset / Fuente principal: Dataset combinado de detección de casco (Roboflow)

| Dimensión | Semáforo | Evidencia que respalda la evaluación | Plan de acción |
|---|---|---|---|
| **Disponibilidad** | 🟢 | Datasets públicos abiertos de PPE/helmet detection en Roboflow Universe y Kaggle, con licencias de uso. | — |
| **Volumen** | 🟢 | ~53,700 imágenes tras augmentación; suficiente para entrenar RF-DETR Small con buen desempeño (mAP@50 90.9%). | — |
| **Calidad** | 🟡 | Las anotaciones variaban entre datasets; se consolidó una taxonomía única de 3 clases y se descartaron imágenes ambiguas. **Falla residual:** el casco de moto aún se reconoce como casco de seguridad (no debería). | Reentrenar con ejemplos negativos (hard-negatives) para que el casco de moto deje de reconocerse como casco. |
| **Relevancia** | 🟡 | La mayoría de imágenes provienen de obras no peruanas; uniformes/luz/polvo locales pueden diferir. | Set de validación peruano + reentrenamiento si cae el desempeño. |
| **Legalidad** | 🟢 | Datasets bajo licencias CC / MIT / Apache (modelo final etiquetado Apache-2.0). Para imágenes propias se exige consentimiento y anonimización. | — |

---

## SECCIÓN 3 — Plan de resolución de bloqueantes

### Bloqueante de inferencia (aparecido en Fase M)
```
Dimensión afectada: Disponibilidad de inferencia (no del dato, sino del servicio).
Descripción: El workspace de Roboflow agotó los créditos gratuitos (error 402),
  bloqueando toda inferencia hosted (inferencejs, REST, SDK) y la descarga de pesos.
Acción concreta: Correr el modelo en un contenedor Docker local (Roboflow Inference
  Server) y exponerlo a internet con Pinggy. Segundo túnel para n8n. Frontend en Netlify.
Responsable: Sebastián Muñico.
Fecha límite: resuelto antes de la Semana 14.
Plan B (si falla): cuenta nueva de Roboflow con créditos frescos + fork del proyecto.
Estado: RESUELTO — inferencia local a costo S/ 0, MVP accesible por URL pública.
```

> Los semáforos de calidad de datos son 🟡 con plan asignado — no hay 🔴 de datos.

---

## SECCIÓN 4 — Privacidad y legalidad de los datos

| Pregunta | Respuesta | Detalle |
|---|---|---|
| ¿Los datos contienen información personal de usuarios? | SÍ | Imágenes de obra contienen rostros → datos personales/biométricos. |
| ¿Se cuenta con consentimiento explícito para usar esos datos? | PARCIAL | Datasets públicos con licencia; para imágenes de obra real se requiere consentimiento de la constructora. |
| ¿Los datos serán anonimizados antes de usarlos? | SÍ | Blur automático de rostros como paso del pipeline antes de almacenar. |
| ¿Aplica la Ley N° 29733 de Protección de Datos Personales del Perú? | SÍ | Aplica plenamente al tratar datos biométricos de trabajadores en territorio peruano. |
| ¿Hay alguna restricción contractual o de confidencialidad? | SÍ (esperado) | No se publican imágenes identificables ni datos de la obra. |

---

## SECCIÓN 5 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Cada semáforo tiene evidencia concreta que lo respalda? | SÍ |
| ¿Todos los 🔴 tienen un plan de acción con fecha y responsable? | SÍ (el bloqueante de inferencia está resuelto) |
| ¿El equipo verificó el acceso real a los datos antes de completar este checklist? | SÍ — datasets verificados y modelo entrenado; obra real no instrumentada |
| ¿La estrategia de tipo de ML es coherente con los datos disponibles? | SÍ — detección de objetos supervisada coincide con datasets etiquetados |

---

*Framework PROMPT v1.0 — AD5018 UTEC | Plantilla 2 de 4*
