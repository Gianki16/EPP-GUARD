# Resumen Ejecutivo — EPP-Guard
## AD5018 Inteligencia Artificial para Negocios | UTEC | 2026

---

## Nombre del MVP y usuario principal

**MVP:** EPP-Guard — Sistema de detección automática de equipos de protección personal en obras de construcción.

**Usuario principal:** Supervisor de Seguridad y Salud Ocupacional (SSO) de obras de construcción medianas (30–100 trabajadores) en Lima Metropolitana, responsable ante SUNAFIL del cumplimiento de la Norma G.050.

---

## Declaración del problema

> *"El supervisor SSO de obras de construcción en Lima Metropolitana tiene dificultad para verificar de forma continua y simultánea el uso correcto de EPP en todos los frentes de trabajo porque la supervisión humana es serial e intermitente, lo que genera detecciones tardías de incumplimientos asociados a multas SUNAFIL superiores a S/ 100,000, paralizaciones de obra y un contexto sectorial de 139 accidentes mortales solo en el trimestre abril–junio 2025."*

---

## Tipo de IA elegido y justificación

**Tipo:** ML Tradicional — Clasificación supervisada (visión computacional).

El problema es de percepción visual a escala: identificar si una persona usa o no EPP en una imagen. No requiere generación de lenguaje (descarta IA Generativa) ni explicación conversacional del resultado (descarta Combinación). Un modelo de clasificación supervisada aprende de imágenes etiquetadas a reconocer patrones visuales — casco presente vs. ausente, chaleco presente vs. ausente — con alta velocidad y costo operativo cercano a cero. Es el tipo de tarea para el que este enfoque fue diseñado.

---

## Estado del diagnóstico de datos

| Fuente | Imágenes | Disponib. | Volumen | Calidad | Relevancia | Legalidad |
|---|---|---|---|---|---|---|
| Datasets públicos PPE (Roboflow / Kaggle) | 10,000–22,000 | 🟢 | 🟢 | 🟡 | 🟡 | 🟢 |
| Imágenes propias — obra piloto Lima | 1,000–2,000 (meta) | 🟡 | 🟡 | 🟡 | 🟢 | 🔴 |
| Set validación — contexto peruano | 300–500 (meta) | 🟡 | 🟡 | 🟡 | 🟢 | 🟡 |

**Bloqueante activo:** Las imágenes propias de obra contienen datos biométricos (rostros) sujetos a la Ley N° 29733. Plan de mitigación: protocolo de consentimiento informado + blur automático de rostros antes de cualquier captura. Fecha límite: Semana 5.

**Variable objetivo:** Clasificar presencia/ausencia de EPP por imagen. Clases MVP: casco, chaleco reflectivo, calzado, guantes.

---

## Descripción del producto

**Qué hace:** EPP-Guard analiza imágenes de cámaras de obra, detecta si los trabajadores usan correctamente su EPP y notifica al supervisor automáticamente cuando detecta un incumplimiento, generando evidencia auditable.

**Flujo de interacción:**
```
Cámara captura imagen → Modelo clasifica EPP → ¿Cumple? 
  → SÍ: registra OK
  → NO: alerta al supervisor + almacena imagen con timestamp
         → Supervisor valida y actúa
```

**Alcance del MVP:**
- Clasificación de 4 clases: casco, chaleco reflectivo, calzado, guantes
- Demo en navegador: subir foto o activar cámara web
- Alerta automática por WhatsApp/email al detectar incumplimiento
- Registro de evidencia con imagen + timestamp + nivel de confianza
- Dashboard básico de cumplimiento diario

**Fuera del MVP (versión 2.0):** localización espacial con bounding box, reconocimiento de identidad, múltiples cámaras simultáneas, integración con ERP/SST.

**Criterio de éxito:** Un supervisor externo al equipo puede subir una foto de obra, recibir la clasificación en menos de 5 segundos y descargar la evidencia, sin instrucciones adicionales.

---

## Decisión tecnológica Build / Buy / Integrate

**Estrategia elegida: Integrate**

| Opción | Razón de descarte |
|---|---|
| Buy | Soluciones comerciales (Avigilon, Viso.ai) superan los $1,000/mes y son cajas negras inadaptables al contexto peruano. |
| Build | Entrenar un modelo desde cero es inviable en 7 semanas y desperdicia décadas de progreso en visión computacional. |
| **Integrate** ✓ | Modelo preentrenado (open-source, costo cero) + fine-tuning con datos locales + flujo de producto propio. El valor diferencial está en el flujo y la adaptación local, no en el modelo. |

**Stack referencial por componente:**

| Componente | Herramienta | Costo estimado |
|---|---|---|
| Motor IA — prototipo | Teachable Machine (Google) | S/ 0 |
| Motor IA — producción | Roboflow RF-DETR Small + API hosted | S/ 0 |
| Frontend / demo | HTML + JS sobre Roboflow API | S/ 0 |
| Alertas | n8n cloud free tier | S/ 0 |
| Almacenamiento logs | Google Sheets free tier | S/ 0 |
| Cámara (si no hay CCTV) | Cámara IP HD | S/ 200–400 (único) |

**Costo total del MVP: S/ 0 / mes** en escenario base (obra con CCTV existente). **Máximo: S/ 250 / mes** si se necesita cámara y cómputo adicional.

---

## OKRs y KPIs

**Objetivo:** Reducir el riesgo regulatorio y de accidentabilidad de obras en Lima mediante supervisión continua y automatizada del uso correcto de EPP.

| KR | Métrica | Valor actual | Meta |
|---|---|---|---|
| KR1 — Cobertura | % de jornada con supervisión efectiva | ~30% | ≥ 90% |
| KR2 — Velocidad | Tiempo de detección hasta alerta | ~30 min | < 30 seg |
| KR3 — Trazabilidad | % incumplimientos con evidencia auditable | 0% | ≥ 95% |

**KPI técnico del modelo:**

| Métrica | Criterio mínimo | Método de medición |
|---|---|---|
| Accuracy global | ≥ 80% | Set de prueba externo — mín. 50 imágenes no vistas durante entrenamiento |
| Recall en clase "no hat" | ≥ 85% | Panel de evaluación Teachable Machine / Roboflow |

> Los valores actuales de KR1 y KR2 son estimaciones basadas en el patrón típico de supervisión (3 recorridos de 30 min en jornada de 9 h). Se confirmarán con bitácora real de la obra piloto antes de la sustentación.

---

*AD5018 — Inteligencia Artificial para Negocios | UTEC | Framework PROMPT v1.1*
