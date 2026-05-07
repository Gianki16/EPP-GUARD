# EPP-Guard — Detección Automática de EPP en Obras de Construcción

**Curso:** AD5018 Inteligencia Artificial para Negocios | UTEC  
**Equipo:** 
- Integrante 1: [Giancarlo Humberto Ferreyra Uribe]
- Integrante 2: [Sebastian Leonardo Muñico Diaz]
- Integrante 3: [Fernando Maquera]
**Fecha de entrega:** [07/05/26]  
**Semestre:** 2026-1

---

## ¿De qué trata este proyecto?

EPP-Guard es un sistema de visión computacional que detecta automáticamente el uso correcto de equipos de protección personal (EPP) en obras de construcción en Lima Metropolitana. Analiza imágenes de cámaras de obra, clasifica si los trabajadores usan casco y chaleco correctamente, y notifica al supervisor SSO cuando detecta un incumplimiento — generando evidencia auditable para SUNAFIL.

**Problema que resuelve:** El supervisor SSO de obras no puede vigilar simultáneamente todos los frentes de trabajo. La supervisión humana cubre solo ~30% de la jornada. La Norma G.050 exige 100% del personal el 100% del tiempo. EPP-Guard cierra esa brecha.

**Tipo de IA:** ML Tradicional — Clasificación supervisada (visión computacional).

---

## Estructura del repositorio

```
📁 repositorio/
│
├── README.md                              ← Este archivo
├── resumen_ejecutivo.md                   ← Síntesis ejecutiva del proyecto (2 páginas)
├── presentacion.pdf                       ← Deck de 8 slides para la sustentación
│
└── 📁 plantillas/
    ├── plantilla_1_problem_statement.md   ← Fase P — Problema de negocio
    ├── plantilla_2_data_readiness.md      ← Fase R — Recursos de datos
    └── plantilla_3_ai_product_canvas.md   ← Fase O — Oportunidad de IA
```

---

## Archivos del repositorio

### [`resumen_ejecutivo.md`](./resumen_ejecutivo.md)
Síntesis del proyecto en 2 páginas. Cubre: nombre del MVP, declaración del problema, tipo de IA y justificación, diagnóstico de datos con semáforo, descripción del producto y flujo, decisión tecnológica Build/Buy/Integrate con costos en soles, y OKRs con valores actuales y metas.

### [`presentacion.pdf`](./presentacion.pdf)
Deck de 8 slides para la sustentación oral del PC1. Estructura: Portada → El problema → ¿Por qué IA? → Los datos → El producto → Decisión tecnológica → Compromisos → Próximos pasos.

### [`plantillas/plantilla_1_problem_statement.md`](./plantillas/plantilla_1_problem_statement.md)
**Fase P — Problema de negocio.** Define el usuario afectado (supervisor SSO), el problema específico (supervisión serial e intermitente), la causa raíz (cuello de botella de atención humana), la consecuencia medible (accidentes, multas), la declaración formal del problema, el filtro de validación de IA y el tipo de IA elegido con justificación.

### [`plantillas/plantilla_2_data_readiness.md`](./plantillas/plantilla_2_data_readiness.md)
**Fase R — Recursos de datos.** Inventario de las 3 fuentes de datos (datasets públicos PPE, imágenes propias de obra piloto, set de validación peruano), semáforo de calidad por dimensión (disponibilidad, volumen, calidad, relevancia, legalidad), plan de resolución del bloqueante legal (Ley N° 29733), y protocolo de privacidad y anonimización.

### [`plantillas/plantilla_3_ai_product_canvas.md`](./plantillas/plantilla_3_ai_product_canvas.md)
**Fase O — Oportunidad de IA.** Identidad del producto (EPP-Guard), Tech & Cost Overview con stack referencial y costos en soles, flujo de interacción del usuario, decisión Build/Buy/Integrate justificada, especificaciones del modelo (Teachable Machine para prototipo → Roboflow RF-DETR para producción), alcance del MVP con criterio de éxito, y OKRs con KRs medibles y KPI técnico del modelo.

---

## Estado actual del prototipo

| Componente | Estado |
|---|---|
| Dataset combinado | ✅ 8,313 imágenes (Roboflow) |
| Modelo entrenado | ✅ EPP Detection Lima 1 — RF-DETR Small |
| Métricas modelo v1 | mAP@0.5: 97.7% · Precision: 99.6% · Recall: 97.6% |
| Iteración pendiente | 🔄 Unificar clases duplicadas → Version 2 (meta: mAP ≥ 80%) |

---

## Regulación aplicable

- **Ley N° 29733** — Protección de Datos Personales (Perú): aplica a imágenes de trabajadores.
- **D.S. 003-2013-JUS** — Reglamento de la Ley 29733: datos biométricos sensibles.
- **Ley N° 29783** — Seguridad y Salud en el Trabajo: marco de responsabilidad del empleador.
- **Norma G.050** — Seguridad durante la Construcción (DS N° 010-2009-VIVIENDA): EPP exigidos.

---

*Framework PROMPT v1.1 — AD5018 UTEC | PC1 Semana 6*
