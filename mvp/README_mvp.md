# README MVP — EPP-Guard
## URL de despliegue + instrucciones de uso para el evaluador
### AD5018 Inteligencia Artificial para Negocios | UTEC | PC2 — Semana 14

---

**Equipo:** Giancarlo Ferreyra · Sebastián Muñico · Fernando Maquera

**Nombre del MVP:** EPP-Guard

---

## URL pública del MVP

| Servicio | URL | Qué expone |
|---|---|---|
| **Frontend (página web)** | https://luminous-klepon-0fdaec.netlify.app/ | Interfaz del demo (subir foto / webcam) — desplegada en Netlify |
| **Inferencia (Roboflow)** | `https://________.pinggy.link` *(Pinggy, puerto 9001)* | Servidor de inferencia Docker local con el modelo `epp-casco-v3` |
| **Webhook de alertas (n8n)** | `https://________.pinggy.link/webhook/epp-guard` | Flujo n8n que arma y envía la alerta de WhatsApp |

> El frontend está fijo en Netlify. Los servicios de inferencia (Docker) y n8n se exponen con **Pinggy** al inicio de la sustentación (las URLs se generan en ese momento).

---

## Arquitectura del despliegue

```
┌─────────────┐   foto/webcam    ┌──────────────────────────────┐
│  Navegador  │ ───────────────▶ │  Frontend (Netlify)          │
│  evaluador  │ ◀─────────────── │  luminous-klepon-0fdaec...   │
└─────────────┘   detecciones    └──────────────────────────────┘
       │                                   │ inferencia
       │                                   ▼  (Pinggy → :9001)
       │                          ┌──────────────────────────────┐
       │                          │  Docker local: modelo         │
       │                          │  epp-casco-v3 (RF-DETR Small) │
       │                          └──────────────────────────────┘
       │ incumplimiento (head detectado)
       │                          (Pinggy → :5678 /webhook/epp-guard)
       │                          ┌──────────────────────────────┐
       │                          │  n8n (multiagente Gemini)     │
       │                          │  Validador→Evaluador→Redactor │
       │                          │  → Twilio (WhatsApp sandbox)  │
       │                          └──────────────────────────────┘
       ▼                                   ▼
  Evidencia (log)                 WhatsApp del supervisor
```

**Por qué este despliegue (decisión de Fase M):** El plan original era usar la inferencia *hosted* de Roboflow, pero el workspace agotó los créditos gratuitos (error 402) y la descarga de pesos también está bloqueada en el plan free. Solución: correr el modelo en un **contenedor Docker local** y exponerlo a internet con **Pinggy**, junto con un segundo túnel para n8n. El frontend se aloja en **Netlify**. Resultado: inferencia a costo S/ 0 y MVP accesible por URL pública sin depender de créditos de terceros.

---

## Pasos para levantar el MVP en red pública

### 0) Preparar Docker y n8n
Encender el contenedor Docker con los **dos puertos** expuestos (inferencia `9001` y servicio) y dejar n8n configurado y corriendo.

### 1) Levantar Twilio (sandbox de WhatsApp)
El teléfono que **recibirá** las alertas debe unirse al sandbox de Twilio. Desde WhatsApp, enviar el mensaje:

```
join helpful-complete
```

al número:

```
+1 (415) 523-8886
```

> Sin este paso, Twilio no entrega los mensajes de WhatsApp al supervisor. Cada número que deba recibir alertas tiene que unirse una vez.

### 2) Abrir los túneles con Pinggy

**Roboflow (inferencia, puerto 9001):**
```
ssh -p 443 -R0:localhost:9001 a.pinggy.io
```

**n8n (puerto 5678, con reintento automático en PowerShell):**
```powershell
while ($true) { ssh -p 443 -R0:localhost:5678 a.pinggy.io; Start-Sleep -Seconds 2 }
```

> *Comandos estándar de Pinggy. Si usas un token o un comando Pinggy específico, reemplázalo aquí tal cual lo copias del panel de Pinggy.*

### 3) Validar el acceso del túnel
Entrar a cada link generado por Pinggy en el navegador y **aceptar** la página de aviso inicial. Sin este paso, las URLs pueden no responder a las peticiones.

### 4) Abrir el frontend
Pasar/abrir el link de la página:
```
https://luminous-klepon-0fdaec.netlify.app/
```

### 5) Configurar la URL del servidor de inferencia
En el campo **"URL del servidor"** de la página, pegar la **URL de Roboflow** (la de Pinggy del puerto 9001) generada en el paso 2.

### 6) Configurar la URL del webhook de n8n
Publicar el flujo en n8n (**published**) y tomar su URL. En el campo de n8n de la página, pegar esa URL **añadiendo al final**:
```
/webhook/epp-guard
```
Es decir: `https://________.pinggy.link/webhook/epp-guard`

### 7) Probar la web
Subir una foto de obra o activar la webcam y verificar: detección visual, alerta de WhatsApp (~12 s) y registro de evidencia.

> ✅ Con estos pasos el MVP corre en red pública.

---

## Cómo usar el MVP (para el evaluador)

### Opción 1 — Subir una foto
1. Abrir el frontend en **Google Chrome** (con aceleración por hardware activada).
2. Esperar a que el indicador diga **"Modelo listo"** (verde).
3. Click en **"Subir Foto"** o arrastrar una imagen de obra.
4. El sistema marca: 🟢 con casco (`helmet`), 🔴 sin casco (`head`), 🔵 persona (`person`).
5. Si hay alguien sin casco, el contador se pone en rojo y se dispara la alerta.

### Opción 2 — Webcam en vivo
1. Click en **"Webcam"** y aceptar el permiso de cámara.
2. Ponerse y quitarse un casco frente a la cámara para provocar la detección.
3. Verificar que la alerta de WhatsApp llega al teléfono que se unió al sandbox.

---

## Limitaciones conocidas (declaradas)
- El modelo **aún reconoce el casco de moto como casco de seguridad** cuando no debería (un casco de moto no es EPP de obra válido); el objetivo es que **no sea reconocido como casco**. Hasta corregirlo, el **supervisor valida toda alerta** antes de actuar.
- En **baja iluminación** o con **personas muy lejanas** baja el recall.
- Al usar **Pinggy + Docker local**, el MVP requiere que la máquina del equipo esté encendida, los túneles activos y la página de aviso aceptada; es adecuado para la demo, no para producción 24/7.
- Twilio opera en **modo sandbox**: solo recibe alertas quien previamente envió `join helpful-complete`.

---

## Clases del modelo

| Clase | Significado | Color |
|---|---|---|
| `helmet` | Persona con casco de seguridad | 🟢 Verde |
| `head` | Cabeza sin casco → incumplimiento | 🔴 Rojo |
| `person` | Persona detectada | 🔵 Azul |

---

*EPP-Guard · AD5018 — Inteligencia Artificial para Negocios · UTEC 2026 · Framework PROMPT v1.1*
