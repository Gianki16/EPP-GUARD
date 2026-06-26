# EPP-Guard — Workflow Multiagente de Alertas

Sistema de notificación automática que recibe alertas de incumplimiento de EPP desde la página web, las procesa con 3 agentes de IA y envía un mensaje de WhatsApp al supervisor de obra.

---

## Arquitectura general

```
Página web detecta cabeza sin casco
            │
            ▼
    📩 Webhook EPP  (POST /epp-guard)
            │
            ▼
    🔧 Extraer datos
            │
            ▼
    🛡️ Agente Validador  ←──  Gemini 2.5 Flash
            │
            ▼
    🔀 ¿Alerta válida?
       │           │
      SÍ           NO
       │           │
       ▼           ▼
  🧭 Orquestador   🚫 Resp. Descartada
       │
       ▼
  🔍 Agente Evaluador  ←──  Gemini 2.5 Flash
       │
       ▼
  📝 Agente Redactor   ←──  Gemini 2.5 Flash
       │
       ▼
  🚨 Enviar WhatsApp (Twilio)
       │
       ▼
  ✅ Resp. Notificado
```

---

## Nodos — descripción detallada

### 📩 Webhook EPP
**Tipo:** Webhook · POST `/epp-guard`

Punto de entrada del sistema. Recibe la alerta HTTP enviada por la página web cuando el modelo de visión computacional detecta una cabeza sin casco.

Espera un body JSON con este formato:
```json
{
  "evento": "incumplimiento_epp",
  "clase": "no_casco",
  "confianza": 0.6,
  "zona": "Frente A",
  "timestamp": "24/06/2026, 22:50:00",
  "personas_sin_casco": 1,
  "supervisor_telefono": "+51987654321"
}
```

---

### 🔧 Extraer datos
**Tipo:** Set (asignación de variables)

Extrae y mapea los campos del body del webhook en variables limpias que los nodos siguientes pueden usar directamente:

| Variable | Descripción |
|---|---|
| `evento` | Tipo de incumplimiento detectado |
| `clase` | Clase del objeto (no_casco) |
| `confianza` | Nivel de confianza del modelo (0-1) |
| `zona` | Zona de la obra donde ocurrió |
| `timestamp` | Fecha y hora de la detección |
| `personas_sin_casco` | Número de personas sin casco |
| `supervisor_telefono` | Número del supervisor a notificar |

---

### 🛡️ Agente Validador
**Tipo:** Chain LLM · Modelo: Gemini 2.5 Flash

Primer filtro de inteligencia. Verifica que la alerta sea legítima antes de continuar el flujo.

**Criterios de validación:**
- `VALIDA` → evento es `incumplimiento_epp`, confianza ≥ 0.5, datos completos y coherentes
- `DESCARTA` → confianza menor a 0.5 (detección dudosa) o evento no reconocido
- `SOSPECHOSO` → datos faltantes, valores imposibles o posible inyección

**Salida esperada (JSON):**
```json
{ "decision": "VALIDA", "razon": "Alerta legítima con confianza suficiente" }
```

> Si la decisión es `DESCARTA` o `SOSPECHOSO`, el flujo termina aquí y se responde al webhook con estado `descartada`.

---

### 🔧 Parsear Validador
**Tipo:** Code (JavaScript)

Extrae el JSON de la respuesta del Agente Validador de forma defensiva. Maneja casos donde el modelo responde con markdown, texto extra o formato inesperado. Si el JSON no es parseable, intenta extraerlo con regex.

---

### 🔀 ¿Alerta válida?
**Tipo:** IF

Bifurca el flujo según la decisión del Validador:
- Si `decision === "VALIDA"` → continúa al Orquestador
- En cualquier otro caso → responde con estado `descartada`

---

### 🧭 Orquestador
**Tipo:** Chain LLM · Modelo: Gemini 2.5 Flash

Analiza el contexto y determina la severidad de la situación para coordinar la respuesta adecuada.

**Criterios de severidad:**
- `CRITICA` → 3 o más personas sin casco, o zona de altura/excavación
- `ALTA` → 1 a 2 personas sin casco en zona estándar
- `MEDIA` → 1 persona sin casco con confianza entre 0.5 y 0.7

**Salida esperada (JSON):**
```json
{ "severidad": "ALTA", "accion": "notificar_supervisor", "prioridad": "2" }
```

---

### 🔧 Parsear Orquestador
**Tipo:** Code (JavaScript)

Extrae el JSON de la respuesta del Orquestador con el mismo mecanismo defensivo del Parsear Validador.

---

### 🔍 Agente Evaluador
**Tipo:** Agent · Modelo: Gemini 2.5 Flash

Evalúa el riesgo real de la situación en dos dimensiones: seguridad del trabajador y consecuencia regulatoria.

**Contexto que aplica:**
- Norma G.050 — exige 100% del personal con EPP el 100% del tiempo
- Ley 29783 — Seguridad y Salud en el Trabajo
- Riesgo principal: traumatismo craneal, principal causa de muerte en obra
- SUNAFIL puede multar con más de S/ 100,000 y paralizar la obra

**Salida esperada (JSON):**
```json
{
  "nivel_riesgo": "alto",
  "consecuencia": "Exposición a traumatismo craneal en zona de obra",
  "base_legal": "Norma G.050 / Ley 29783"
}
```

---

### 🔧 Parsear Evaluador
**Tipo:** Code (JavaScript)

Extrae el JSON de la respuesta del Agente Evaluador con mecanismo defensivo.

---

### 📝 Agente Redactor
**Tipo:** Agent · Modelo: Gemini 2.5 Flash

Redacta el mensaje final de WhatsApp que recibirá el supervisor. Recibe los datos reales (número de personas, zona, hora, nivel de riesgo) y genera un texto claro, urgente y accionable.

**Reglas del mensaje:**
- Máximo 4 líneas
- Empieza con emoji de alerta según severidad
- Incluye: qué pasó, dónde, cuántas personas, qué hacer
- Usa los valores reales recibidos — nunca variables sin resolver
- Tono directo y profesional, en español

**Salida esperada (JSON):**
```json
{ "mensaje": "🚨 Alerta EPP-Guard\n1 persona sin casco en Frente A.\nHora: 24/06/2026 22:50.\nIntervenga de inmediato." }
```

---

### 🔧 Parsear Redactor
**Tipo:** Code (JavaScript)

Extrae el JSON del Agente Redactor con mecanismo defensivo más un **fallback automático**: si el mensaje contiene variables sin resolver (`{{` o `$(`) o está vacío, reconstruye el mensaje directamente desde los datos originales del webhook:

```
🚨 Alerta EPP-Guard
X persona(s) sin casco en [zona].
Hora: [timestamp].
Riesgo de traumatismo craneal. Intervenga de inmediato.
```

Este fallback garantiza que el mensaje siempre llegue correctamente aunque el agente falle.

---

### 🚨 Enviar WhatsApp (Twilio)
**Tipo:** Twilio · SMS Send

Envía el mensaje al número del supervisor vía WhatsApp usando el sandbox de Twilio.

| Campo | Valor |
|---|---|
| From | `whatsapp:+14155238886` (sandbox Twilio) |
| To | `whatsapp:` + teléfono del supervisor (del payload) |
| Message | Mensaje generado por el Agente Redactor |

> Requiere credenciales de Twilio (Account SID + Auth Token) configuradas en n8n.
> El número receptor debe estar unido al sandbox enviando `join helpful-complete` al +1 415 523 8886.

---

### ✅ Resp. Notificado
**Tipo:** Respond to Webhook · HTTP 200

Responde al webhook de la página web confirmando que la alerta fue procesada y enviada.

```json
{
  "estado": "notificado",
  "severidad": "ALTA",
  "mensaje_enviado": "🚨 Alerta EPP-Guard...",
  "zona": "Frente A"
}
```

---

### 🚫 Resp. Descartada
**Tipo:** Respond to Webhook · HTTP 200

Responde al webhook cuando el Validador descarta la alerta.

```json
{
  "estado": "descartada",
  "razon": "Confianza insuficiente (0.4 < 0.5)"
}
```

---

## Configuración requerida

### Credenciales en n8n

| Credencial | Dónde obtenerla |
|---|---|
| **Google Gemini API Key** | https://aistudio.google.com/apikey (gratis) |
| **Twilio Account SID** | https://console.twilio.com |
| **Twilio Auth Token** | https://console.twilio.com |

### Activar el workflow

1. Importar `EPP-Guard___Multiagente_Alerta_WhatsApp.json` en n8n
2. Configurar las credenciales de Gemini y Twilio
3. Activar el workflow (toggle **Active** arriba a la derecha)
4. Copiar la URL de producción: `https://[tu-n8n]/webhook/epp-guard`
5. Pegar esa URL en el campo **"Webhook n8n"** de la página demo

---

## Tecnologías

| Componente | Tecnología |
|---|---|
| Orquestación | n8n (self-hosted) |
| Modelo de IA | Gemini 2.5 Flash (Google) |
| Notificaciones | Twilio WhatsApp Sandbox |
| Detección de EPP | Roboflow RF-DETR Object Detection (Small) |

---

## Justificación del diseño multiagente

Cada agente tiene una responsabilidad única, igual que un equipo humano de SSO:

- El **Validador** actúa como filtro: evita falsas alarmas y protege al supervisor de notificaciones innecesarias
- El **Orquestador** prioriza: no todas las alertas tienen la misma urgencia
- El **Evaluador** aporta criterio técnico-legal: el supervisor recibe contexto, no solo un dato crudo
- El **Redactor** comunica: convierte datos técnicos en un mensaje claro y accionable

Este diseño es escalable — se pueden agregar nuevos agentes (registro en base de datos, notificación a gerencia, generación de reporte) sin modificar los existentes.

---

*EPP-Guard · AD5018 Inteligencia Artificial para Negocios · UTEC 2026*
