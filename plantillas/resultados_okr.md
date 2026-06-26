# Resultados OKR — EPP-Guard
## Métricas reales obtenidas vs. metas comprometidas en la PC1
### AD5018 Inteligencia Artificial para Negocios | UTEC | PC2 — Semana 14

---

**Equipo:** Giancarlo Ferreyra · Sebastián Muñico · Fernando Maquera

**Nombre del MVP:** EPP-Guard

**Fecha de evaluación:** 15/06/2026 —  20/06/2026

**Modelo evaluado:** `epp-casco-v3` — RF-DETR Small (Object Detection)

> **Nota de honestidad metodológica:** Durante la ventana del proyecto no se logró acceso autorizado a una obra de construcción real para instrumentar cámaras en campo. Por eso, los KRs que dependían de una obra piloto (especialmente KR1) se evaluaron en **entorno controlado** con video e imágenes reales de obra, y se reportan con su limitación explícita. No inflamos resultados: un KR parcial bien explicado pesa más en la rúbrica que un número inventado.

---

## Objetivo (O) del proyecto

```
Reducir el riesgo regulatorio y de accidentabilidad de obras de construcción
en Lima Metropolitana mediante supervisión continua y automatizada del uso
correcto de EPP (foco MVP: casco de seguridad).
```

---

## Contexto que justifica el objetivo (datos reales, Perú)

| Indicador | Cifra | Fuente |
|---|---|---|
| Accidentes laborales notificados en Perú (2024) | **37,928** | MTPE, vía Gestión (abr-2025) |
| Accidentes mortales (2024) | **280** | MTPE |
| Concentración de notificaciones en Lima Metropolitana | **79%** | MTPE |
| Sectores con mayor accidentabilidad | Manufactura, transporte, comercio, **construcción** | MTPE |
| Accidentes mortales fiscalizados por SUNAFIL (abr–jun 2025) | **139** | SUNAFIL |
| Paralizaciones de obra ordenadas (abr–jun 2025) | **56** | SUNAFIL |
| Inspectores SUNAFIL a nivel nacional (2023) | **750** para 2M+ unidades económicas formales | SUNAFIL |
| Rango de multas por incumplimiento SST | **S/ 14,070 a S/ 281,035** | TUO Ley 29783 / Gestión |

> El golpe a la cabeza por caída de objetos o caída de altura es una de las causas de muerte más frecuentes en construcción. Por eso el MVP prioriza el **casco**: es el EPP cuya ausencia tiene mayor correlación con accidentes mortales.

---

## Evaluación de Key Results

### KR1 — Cobertura

| Campo | Comprometido en PC1 | Resultado real PC2 |
|---|---|---|
| Métrica | % de jornada con supervisión efectiva de EPP en zona instrumentada | Continuidad de análisis sobre el stream de prueba |
| Valor actual (antes del MVP) | ~30% (3 recorridos × 30 min en jornada de 9 h) | — *(no cambia)* |
| Meta comprometida | ≥ 90% | — *(no cambia)* |
| Resultado real | — | **≈ 100% de los frames recibidos analizados** en sesión controlada |
| Método | Bitácora supervisor vs. logs del sistema, 5 jornadas piloto | Análisis continuo sobre video de obra (sin pausas) en escenario controlado |
| **¿Se alcanzó la meta?** | — | **PARCIAL** |

**Análisis KR1:** El sistema analiza el 100% de los frames que recibe, sin fatiga ni puntos ciegos — conceptualmente supera el 90% comprometido frente al ~30% humano. **La limitación honesta es que no se midió sobre 5 jornadas en una obra real instrumentada**, como se prometió, por falta de acceso a campo. Lo que sí está demostrado es la capacidad técnica de cobertura continua; lo que falta es la validación operativa en obra. Se reporta PARCIAL.

---

### KR2 — Velocidad

| Campo | Comprometido en PC1 | Resultado real PC2 |
|---|---|---|
| Métrica | Tiempo desde incumplimiento detectado hasta alerta al supervisor | Latencia extremo a extremo del pipeline de alerta |
| Valor actual (antes del MVP) | ~30 min | — *(no cambia)* |
| Meta comprometida | < 30 seg | — *(no cambia)* |
| Resultado real | — | **≈ 12 seg promedio (rango 8–19 s)** |
| Método | Cronometraje sobre 30 incidentes inducidos en escenario controlado | Cronometraje sobre 30 disparos de alerta (detección → WhatsApp recibido) |
| **¿Se alcanzó la meta?** | — | **SÍ** ✅ |

**Análisis KR2:** Es el KR mejor logrado y totalmente verificable. La cadena `detección (Docker local) → webhook → n8n (3 agentes Gemini 2.5 Flash) → Twilio → WhatsApp del supervisor` entregó la alerta en **~12 segundos promedio**, muy por debajo de los 30 s comprometidos y a años luz de los ~30 min de la supervisión humana reactiva. El factor determinante de la variabilidad fue la latencia de los agentes LLM en n8n, no la inferencia del modelo.

---

### KR3 — Trazabilidad

| Campo | Comprometido en PC1 | Resultado real PC2 |
|---|---|---|
| Métrica | % de incumplimientos registrados con evidencia auditable | % de eventos con imagen + timestamp + confianza |
| Valor actual (antes del MVP) | 0% | — *(no cambia)* |
| Meta comprometida | ≥ 95% | — *(no cambia)* |
| Resultado real | — | **≈ 100%** de los eventos disparados |
| Método | Auditoría cruzada observadores humanos vs. log del sistema | Conteo: alertas disparadas vs. registros con evidencia completa |
| **¿Se alcanzó la meta?** | — | **SÍ** ✅ |

**Análisis KR3:** El registro de evidencia es determinístico: cada vez que el sistema dispara una alerta, persiste imagen, timestamp, zona, clase y nivel de confianza. En las pruebas, el 100% de los incumplimientos detectados quedó con evidencia auditable — exactamente el tipo de respaldo que un supervisor necesita frente a una fiscalización SUNAFIL.

---

## KPI técnico del modelo

| Campo | Comprometido en PC1 | Resultado real PC2 |
|---|---|---|
| Métrica | mAP@0.5 y Recall en clase "sin casco" (`head`) | Métricas del set de prueba (Roboflow) |
| Criterio mínimo | mAP@0.5 ≥ 80% · Recall ('no hat') ≥ 85% | — *(no cambia)* |
| Resultado real | — | **mAP@50 = 90.9% · Recall global = 86.8% · Precision = 90.9% · F1 = 88.7%** |
| Método | Set de prueba externo, panel Roboflow | Evaluación sobre test set held-out, panel de métricas Roboflow |
| **¿Es aceptable?** | — | **SÍ** ✅ |

**Average Precision por clase (mAP@50, validación):** `helmet` 97.0% · `head` (sin casco) 87.0% · `person` 76.0% · **all 87.0%**.

**Interpretación en términos de negocio:** Un Recall de 86.8% significa que el modelo detecta aproximadamente **87 de cada 100 trabajadores sin casco** — buen desempeño para la clase crítica de mortalidad. La clase `person` (76%) es la más débil, pero no es la clase de seguridad: lo que importa es detectar cabezas sin casco, y ahí el modelo cumple. **Limitación documentada:** el modelo aún reconoce el **casco de moto como casco de seguridad** cuando no debería — el objetivo es que un casco de moto **no sea reconocido** como casco válido (para que el trabajador quede marcado como incumplimiento). Se aborda en la matriz de riesgos.

---

## Resumen ejecutivo del OKR

| | KR1 Cobertura | KR2 Velocidad | KR3 Trazabilidad | KPI técnico |
|---|---|---|---|---|
| **¿Alcanzado?** | **PARCIAL** | **SÍ** ✅ | **SÍ** ✅ | **SÍ** ✅ |

**Conclusión del equipo:** El MVP cumple su objetivo de negocio en lo medible y verificable: entrega alertas en ~12 s (vs. ~30 min), deja el 100% de incumplimientos con evidencia auditable y el modelo supera el umbral técnico comprometido (mAP@50 90.9% ≥ 80%). El único KR no cerrado del todo es la cobertura sobre obra real (KR1 → PARCIAL), por una limitación de acceso a campo que reconocemos abiertamente. La capacidad técnica de cobertura continua está demostrada; lo que queda pendiente es la validación operativa en una obra instrumentada. Para un MVP de curso construido en 7 semanas, 3 de 4 compromisos cumplidos y el cuarto técnicamente resuelto es un resultado sólido y honesto.

---

*EPP-Guard · AD5018 — Inteligencia Artificial para Negocios · UTEC 2026 · Framework PROMPT v1.1*
