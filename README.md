# EPP-Guard — Detección Automática de EPP en Obras de Construcción

**Curso:** AD5018 Inteligencia Artificial para Negocios | UTEC
**Equipo:**
- Integrante 1: Giancarlo Humberto Ferreyra Uribe
- Integrante 2: Sebastian Leonardo Muñico Diaz
- Integrante 3: Fernando Maquera

**Entrega:** PC2 — Semana 14 (Fases M + P2 + T)
**Semestre:** 2026-1

---

## 🔗 URL del MVP

> El frontend está en **Netlify** (URL fija) y los servicios de inferencia (Docker local) y n8n se exponen con **Pinggy** al inicio de la sustentación. Detalle y pasos en [`mvp/README_mvp.md`](./mvp/README_mvp.md).

```
Frontend (Netlify): https://luminous-klepon-0fdaec.netlify.app/
Inferencia (Roboflow, Pinggy :9001): https://________.pinggy.link
Webhook n8n (Pinggy :5678): https://________.pinggy.link/webhook/epp-guard
```

---

## ¿De qué trata este proyecto?

EPP-Guard es un sistema de visión computacional que detecta automáticamente si los trabajadores de una obra usan **casco de seguridad**. Analiza imágenes de cámara o webcam, marca quién tiene casco (`helmet`) y quién no (`head`), y al detectar un incumplimiento notifica al supervisor SSO por WhatsApp en segundos, generando evidencia auditable para SUNAFIL.

**Problema que resuelve:** El supervisor SSO no puede vigilar todos los frentes a la vez; la supervisión humana es serial e intermitente. La Norma G.050 exige el 100% del personal protegido el 100% del tiempo. EPP-Guard cierra esa brecha para el EPP de mayor impacto en mortalidad: el casco.

**Tipo de IA:** ML Tradicional — Detección de objetos (clasificación supervisada).

---

## Estado final del proyecto (PC2)

| Componente | Estado |
|---|---|
| Dataset | ✅ ~53,700 imágenes (augmentación 3x) — clases head/helmet/person |
| Modelo final | ✅ `epp-casco-v3` — RF-DETR Small |
| Métricas (test set) | ✅ mAP@50 **90.9%** · Precision 90.9% · Recall 86.8% · F1 88.7% |
| Despliegue | ✅ Frontend en Netlify + inferencia Docker local expuesta con Pinggy (S/ 0) |
| Alertas | ✅ n8n (agentes Gemini 2.5 Flash) → Twilio → WhatsApp (~12 s) |
| Validación | ✅ 5 usuarios externos + escenarios con imágenes reales |
| Limitación conocida | ⚠️ Aún reconoce el casco de moto como casco de seguridad — debe dejar de reconocerlo (documentado) |

**Resultado OKR:** KR2 (velocidad) ✅ · KR3 (trazabilidad) ✅ · KPI técnico ✅ · KR1 (cobertura) 🟡 PARCIAL (sin obra real instrumentada).

---

## Estructura del repositorio

```
📁 repositorio/
├── README.md                          ← Este archivo (índice + estado final + URL MVP)
├── resumen_ejecutivo.md               ← Síntesis con resultados reales
├── presentacion_pc2.pdf               ← Deck de sustentación final
│
├── 📁 plantillas/
│   ├── plantilla_1_problem_statement.md   ← Fase P (versión final)
│   ├── plantilla_2_data_readiness.md      ← Fase R (versión final)
│   ├── plantilla_3_ai_product_canvas.md   ← Fase O (versión final)
│   └── plantilla_4_scorecard_risk.md      ← Fases P2 + T (datos reales)
│
└── 📁 mvp/
    ├── README_mvp.md                  ← URL de despliegue + instrucciones para el evaluador
    └── 📁 evidencia/
        ├── pruebas_usuario.md         ← Validación con usuarios y escenarios reales
        └── resultados_okr.md          ← Métricas reales vs. metas comprometidas en PC1
```

---

## Qué se actualizó vs. qué es nuevo en la PC2

| Se actualizó del PC1 | Es nuevo en el PC2 |
|---|---|
| README.md (estado final + URL del MVP) | presentacion_pc2.pdf |
| resumen_ejecutivo.md (resultados reales) | plantillas/plantilla_4_scorecard_risk.md |
| plantillas 1, 2 y 3 (versión final) | mvp/README_mvp.md |
| | mvp/evidencia/pruebas_usuario.md |
| | mvp/evidencia/resultados_okr.md |

---

## Regulación aplicable

- **Ley N° 29733** — Protección de Datos Personales (imágenes de trabajadores).
- **D.S. 003-2013-JUS** — Reglamento de la Ley 29733 (datos biométricos).
- **Ley N° 29783** — Seguridad y Salud en el Trabajo (responsabilidad del empleador).
- **Norma G.050** — Seguridad durante la Construcción (DS N° 010-2009-VIVIENDA): EPP exigidos.

---

*Framework PROMPT v1.1 — AD5018 UTEC | PC2 Semana 14*
