# Plantilla 2 — Data Readiness Checklist
## Framework PROMPT | Fase R — Recursos de Datos
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: [Giancarlo Humberto Ferreyra Uribe]
- Integrante 2: [Sebastian Leonardo Muñico Diaz]
- Integrante 3: [Fernando Maquera]

**Fecha de entrega:** 07/05/2026
**Tipo de IA del proyecto:** ML Tradicional — Visión Computacional / Clasificación supervisada (detección de objetos)

---

> **Instrucción general:**
> Completa esta plantilla SOLO después de haber verificado la existencia y calidad de los datos — no antes. Cada semáforo debe estar respaldado por evidencia concreta. Un checklist optimista sin evidencia no tiene valor.

---

## SECCIÓN 1 — Inventario de datos

### Para IA Generativa — Inventario de conocimiento
*(No aplica — el proyecto es ML Tradicional con visión computacional)*

---

### Para ML Tradicional — Inventario de datos históricos

| # | Dataset | Fuente | Formato | N° de registros aprox. | ¿Tiene etiquetas? |
|---|---|---|---|---|---|
| 1 | Dataset público de detección de EPP en construcción | Roboflow Universe / Kaggle (datasets abiertos de PPE detection) | Imágenes JPG/PNG + anotaciones YOLO/COCO (.txt o .json) | 4,000 – 15,000 imágenes | SÍ (bounding boxes etiquetadas: casco, chaleco reflectivo, calzado, guantes) |
| 2 | Imágenes de validación en condiciones locales (luz, polvo, atuendo peruano) | Captura propia en obras piloto | JPG | 300-500 imágenes para set de prueba | SÍ — etiquetado manual del equipo |

**Variable objetivo (target):**
> *¿Qué es exactamente lo que el modelo debe predecir o clasificar?*

```
Por cada persona detectada en un frame de video, clasificar la presencia o
ausencia correcta de los EPP exigidos por la Norma G.050. En el alcance del MVP
se priorizan tres EPPs de mayor impacto:

  - Casco de seguridad (con/sin)
  - Chaleco reflectivo (con/sin)
  - Calzado de seguridad (con/sin)
  - Guantes de seguridad (con/sin)

Output esperado: bounding box por persona + etiqueta de cumplimiento por EPP.
```

**Tipo de problema confirmado:**
- [X] Clasificación — predice una categoría (con casco / sin casco, etc.)
- [ ] Regresión
- [ ] Clustering

> *Subtipo técnico: detección de objetos (object detection), que combina localización espacial + clasificación multiclase.*

---

### Para Combinación — completa ambas secciones anteriores y describe la conexión:
*(No aplica)*

---

## SECCIÓN 2 — Evaluación de calidad con semáforo

> *Instrucciones para el semáforo:*
> 🟢 **Verde** = listo, sin acciones pendientes
> 🟡 **Amarillo** = necesita trabajo, hay un plan concreto
> 🔴 **Rojo** = bloqueante, requiere replantear antes de continuar

### Dataset / Fuente principal: Datasets públicos de PPE detection (Roboflow / Kaggle)

| Dimensión | Semáforo | Evidencia que respalda la evaluación | Plan de acción (si es 🟡 o 🔴) |
|---|---|---|---|
| **Disponibilidad** — ¿Los datos existen y son accesibles? | 🟢 | Existen datasets públicos abiertos de PPE detection en Roboflow Universe (>20 datasets indexados) y Kaggle, con licencias de uso para investigación/educación. | — |
| **Volumen** — ¿Hay suficientes datos para el tipo de IA elegido? | 🟢 | Datasets públicos suman 4,000-15,000 imágenes etiquetadas. Suficiente para fine-tuning de un modelo preentrenado en MVP. | — |
| **Calidad** — ¿Los datos están completos y son consistentes? | 🟡 | Las anotaciones varían en calidad entre datasets; algunos tienen oclusiones mal etiquetadas o clases inconsistentes. | Filtrar datasets por calidad de anotación, consolidar taxonomía de clases única, descartar imágenes ambiguas. Responsable: [PENDIENTE]. |
| **Relevancia** — ¿Los datos representan el problema definido en Fase P? | 🟡 | La mayoría de datasets públicos provienen de obras en Asia, EE.UU. o Europa. Los uniformes, condiciones de luz, tipo de andamiaje y polvo de obras peruanas pueden diferir. | Complementar con dataset propio de validación capturado en obra peruana (Dataset 3) para medir caída de desempeño y reentrenar si es necesario. |
| **Legalidad** — ¿Hay autorización para usar estos datos? | 🟢 | Datasets bajo licencias Creative Commons / MIT / Apache. Para imágenes propias se requerirá consentimiento de la empresa constructora y anonimización de rostros. | — |

---

---

## SECCIÓN 3 — Plan de resolución de bloqueantes

> *Por cada semáforo 🔴 en cualquier dimensión, el equipo debe completar este plan.*

### Bloqueante 1
*(No aplica — los demás semáforos son 🟡 con plan asignado, no bloqueantes 🔴.)*

### Bloqueante 2 *(si aplica)*
*(No aplica — los demás semáforos son 🟡 con plan asignado, no bloqueantes 🔴.)*

---

## SECCIÓN 4 — Privacidad y legalidad de los datos

| Pregunta | Respuesta | Detalle |
|---|---|---|
| ¿Los datos contienen información personal de usuarios? | SÍ | Imágenes de obra contienen rostros y características físicas de trabajadores → datos biométricos. |
| ¿Se cuenta con consentimiento explícito para usar esos datos? | NO  | Uso de datasets publicos. |
| ¿Los datos serán anonimizados antes de usarlos en el proyecto? | SÍ | Se aplicará blur automático sobre rostros como paso obligatorio del pipeline antes de almacenar. |
| ¿Aplica la Ley N° 29733 de Protección de Datos Personales del Perú? | SÍ | Aplica plenamente porque se tratan datos biométricos de trabajadores en territorio peruano. |
| ¿Hay alguna restricción contractual o de confidencialidad? | SÍ (esperado) | El equipo no publicará imágenes identificables ni datos de la obra. |

---

## SECCIÓN 5 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Cada semáforo tiene evidencia concreta que lo respalda? | SÍ |
| ¿Todos los 🔴 tienen un plan de acción con fecha y responsable? | SÍ |
| ¿El equipo verificó el acceso real a los datos antes de completar este checklist? | PARCIAL — datasets públicos verificados; obra piloto en gestión |
| ¿La estrategia de contexto o tipo de ML es coherente con los datos disponibles? | SÍ — detección de objetos supervisada coincide con disponibilidad de datasets etiquetados |

> **Si alguna respuesta es NO → el checklist no está listo para entregar.**

---

*Framework PROMPT v1.0 — AD5018 UTEC | Plantilla 2 de 4*
