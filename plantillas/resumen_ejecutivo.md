# Resumen Ejecutivo — EPP-Guard
## AD5018 Inteligencia Artificial para Negocios | UTEC | 2026 | PC2

---

## Nombre del MVP y usuario principal

**MVP:** EPP-Guard — Sistema de detección automática de uso de casco de seguridad en obras de construcción, con alerta inmediata y evidencia auditable.

**Usuario principal:** Supervisor de Seguridad y Salud Ocupacional (SSO) de obras medianas (30–100 trabajadores) en Lima Metropolitana, responsable ante SUNAFIL del cumplimiento de la Norma G.050.

---

## Declaración del problema

> *"El supervisor SSO de obras de construcción en Lima Metropolitana tiene dificultad para verificar de forma continua y simultánea el uso correcto de EPP en todos los frentes de trabajo porque la supervisión humana es serial e intermitente, lo que genera detecciones tardías de incumplimientos asociados a multas SUNAFIL de hasta S/ 281,035, paralizaciones de obra y un contexto sectorial de 280 accidentes mortales en 2024 y 139 solo en el trimestre abril–junio 2025."*

---

## Tipo de IA elegido y justificación

**Tipo:** ML Tradicional — Detección de objetos (clasificación supervisada / visión computacional).

El problema es de percepción visual a escala: identificar si una persona tiene casco en una imagen. No requiere generación de lenguaje (descarta IA Generativa como núcleo) ni razonamiento conversacional (descarta Combinación). La GenAI aparece solo como componente accesorio para redactar la notificación. El motor que resuelve el problema es un modelo de detección de objetos.

---

## Estado del diagnóstico de datos

| Fuente | Imágenes | Disponib. | Volumen | Calidad | Relevancia | Legalidad |
|---|---|---|---|---|---|---|
| Dataset combinado de casco (Roboflow/Kaggle) | ~53,700 (aug. 3x) | 🟢 | 🟢 | 🟡 | 🟡 | 🟢 |
| Set validación — contexto peruano | 100–250 | 🟡 | 🟡 | 🟡 | 🟢 | 🟡 |

**Clases del modelo:** `head` (sin casco) · `helmet` (con casco) · `person`.
**Bloqueante resuelto (Fase M):** se agotaron los créditos de Roboflow (error 402) → se migró a inferencia local en Docker expuesta con Pinggy (frontend en Netlify), a costo S/ 0.

---

## Descripción del producto y resultados

**Qué hace:** Analiza imágenes de cámara/webcam, detecta quién usa casco y quién no, y al detectar un incumplimiento alerta al supervisor por WhatsApp en ~12 segundos, dejando evidencia con imagen, hora y confianza.

**Flujo:**
```
Cámara/foto → Modelo epp-casco-v3 (Docker local) → ¿hay cabeza sin casco?
  → SÍ: webhook → n8n (agentes Gemini) → Twilio → WhatsApp + evidencia
  → NO: registra OK
```

**Resultados del modelo (test set):** mAP@50 **90.9%** · Precision 90.9% · Recall 86.8% · F1 88.7%.

**Validación:** 5 usuarios externos probaron el MVP desplegado y escenarios con imágenes reales de obra. Fortaleza: detección precisa de casco en condiciones normales. Límite documentado: el casco de moto aún se reconoce como casco de seguridad (debería NO reconocerse) → el supervisor valida toda alerta (humano en el circuito).

---

## Decisión tecnológica Build / Buy / Integrate

**Estrategia: Integrate.** Modelo preentrenado (RF-DETR Small) afinado con datos de casco + flujo de producto propio. El valor está en el flujo y la adaptación local, no en el modelo base.

| Componente | Herramienta | Costo |
|---|---|---|
| Motor IA | RF-DETR Small en Docker local (Roboflow) | S/ 0 |
| Frontend/demo | HTML + JS | S/ 0 |
| Alertas | n8n + Twilio (sandbox) | S/ 0 |
| Despliegue | Netlify (frontend) + Pinggy (2 túneles) | S/ 0 |
| Almacenamiento | Google Sheets / log | S/ 0 |
| Cámara (si no hay CCTV) | Cámara IP HD | S/ 200–400 (único) |

**Costo total del MVP: S/ 0 / mes** (obra con CCTV). Máximo S/ 250/mes si se necesita cámara.

---

## OKRs — comprometido vs. real

**Objetivo:** Reducir el riesgo regulatorio y de accidentabilidad de obras en Lima mediante supervisión continua y automatizada del uso de casco.

| KR | Métrica | Valor actual | Meta | Resultado real | ¿Logrado? |
|---|---|---|---|---|---|
| KR1 — Cobertura | % jornada con supervisión efectiva | ~30% | ≥ 90% | ≈100% de frames (escenario controlado, sin obra real) | 🟡 PARCIAL |
| KR2 — Velocidad | Tiempo detección → alerta | ~30 min | < 30 seg | **≈ 12 seg** | ✅ SÍ |
| KR3 — Trazabilidad | % incumplimientos con evidencia | 0% | ≥ 95% | **≈ 100%** | ✅ SÍ |

**KPI técnico:** mAP@0.5 ≥ 80% → **90.9%** ✅ · Recall ('no hat') ≥ 85% → **86.8%** ✅.

**Conclusión:** EPP-Guard cumple su objetivo de negocio en lo medible (alerta inmediata, evidencia auditable, modelo sobre el umbral). El único KR no cerrado del todo es la cobertura sobre obra real, por falta de acceso a campo — limitación que reconocemos abiertamente. Para un MVP de 7 semanas, 3 de 4 compromisos cumplidos y el cuarto técnicamente resuelto.

---

*AD5018 — Inteligencia Artificial para Negocios | UTEC | Framework PROMPT v1.1 | PC2*
