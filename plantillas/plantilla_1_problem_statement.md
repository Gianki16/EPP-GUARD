# Plantilla 1 — Problem Statement Canvas
## Framework PROMPT | Fase P — Problema de Negocio
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Giancarlo Humberto Ferreyra Uribe
- Integrante 2: Sebastian Leonardo Muñico Diaz
- Integrante 3: Fernando Maquera

**Fecha de entrega:** 26/06/2026 — Semana 14
**Versión del canvas:** v2 (final PC2)

> **Actualización PC2:** El problema y el usuario no cambian respecto a la PC1. Se actualizaron las cifras de consecuencia con datos oficiales más recientes y citables, y se precisa que el MVP implementado se concentró en el **casco de seguridad** — el EPP de mayor impacto en mortalidad — como decisión de alcance de la Fase M.

---

## SECCIÓN 1 — Definición del problema

### 1.1 Usuario afectado

```
El supervisor de Seguridad y Salud Ocupacional (SSO) de obras de construcción
medianas (30 a 100+ trabajadores) en Lima Metropolitana, responsable directo
ante SUNAFIL del cumplimiento de la Norma G.050 y de la integridad física del
personal en frente de trabajo.
```

---

### 1.2 Problema específico

```
La supervisión del uso correcto de EPP (casco, chaleco reflectivo, calzado,
guantes) depende de un único inspector humano con atención serial: cuando audita
un frente de obra, no ve los demás. La fiscalización es intermitente, con puntos
ciegos inevitables, y opera de forma reactiva —se detectan incumplimientos
después del incidente o por muestreo— en obras donde la Norma G.050 exige
cumplimiento del 100% del personal el 100% del tiempo.
```

---

### 1.3 Causa raíz

```
Cuello de botella estructural de atención humana: un solo par de ojos no puede
vigilar simultáneamente múltiples frentes de trabajo. El déficit no se resuelve
contratando más supervisores —el sector tiene escasez crónica de inspectores
y SUNAFIL apenas cuenta con ~750 fiscalizadores para más de 2 millones de
unidades económicas formales (SUNAFIL, 2023)—.
```

---

### 1.4 Consecuencia medible

```
En 2024 se notificaron 37,928 accidentes laborales en Perú, de los cuales 280
fueron mortales (MTPE); el 79% de las notificaciones se concentra en Lima
Metropolitana y la construcción está entre los sectores más afectados. Solo en
el trimestre abril–junio 2025, SUNAFIL registró 139 accidentes mortales y ordenó
56 paralizaciones de obra. Cada incumplimiento en SST puede derivar en multas de
S/ 14,070 hasta S/ 281,035 (TUO Ley 29783), paralización de obra y
responsabilidad penal del supervisor.
```

---

### 1.5 Declaración del problema — formato obligatorio

**"[Tipo de usuario] tiene dificultad para [acción específica] porque [causa raíz], lo que genera [consecuencia medible]."**

```
"El supervisor SSO de obras de construcción en Lima Metropolitana tiene
dificultad para verificar de forma continua y simultánea el uso correcto de
EPP en todos los frentes de trabajo porque la supervisión humana es serial e
intermitente, lo que genera detecciones tardías de incumplimientos asociados
a multas SUNAFIL de hasta S/ 281,035, paralizaciones de obra y un contexto
sectorial de 280 accidentes mortales en 2024 y 139 solo en el trimestre
abril–junio 2025."
```

---

## SECCIÓN 2 — Filtro de validación IA

| Pregunta | SÍ / NO | Justificación (1 línea) |
|---|---|---|
| ¿Una hoja de cálculo o formulario resuelve esto? | **NO** | El problema no es de registro, es de detección visual continua y simultánea sobre video en vivo. |
| ¿El problema escala con volumen de datos o usuarios? | **SÍ** | A más cámaras y más trabajadores, la supervisión humana se vuelve exponencialmente inviable. |
| ¿Hay un patrón repetitivo difícil de procesar manualmente? | **SÍ** | Identificar "casco sí/no" sobre decenas de personas en simultáneo durante 10 horas diarias. |
| ¿El problema requiere generar contenido o razonar en lenguaje natural? | **NO** | El núcleo del problema es percepción visual, no generación de lenguaje. |
| ¿Necesitas predecir Y luego explicar o actuar sobre el resultado? | **SÍ (parcial)** | Se necesita detectar (clasificar) y accionar una alerta — la explicación en lenguaje natural no es el núcleo crítico. |

**Conclusión del filtro:**

```
Porque el problema es estructuralmente uno de percepción visual a escala: vigilar
simultáneamente 30-100+ personas sobre múltiples frentes durante toda la jornada.
Ni una hoja de cálculo, ni un checklist digital, ni más personal humano resuelven
el cuello de botella —solo un sistema que vea por todos los puntos al mismo tiempo
y emita alertas en segundos. La visión por computadora es la única tecnología que
multiplica la capacidad de observación sin multiplicar el costo lineal de inspectores.
```

---

## SECCIÓN 3 — Tipo de IA elegido

- [ ] **IA Generativa**
- [X] **ML Tradicional** — el problema requiere predecir, clasificar o segmentar con datos
- [ ] **Combinación**

### Justificación de la elección

```
El problema es de clasificación visual sobre imágenes de video (CCTV) — no de
generación de lenguaje ni de razonamiento conversacional. La IA Generativa no
aporta valor al núcleo del problema (detectar EPP en tiempo real); su uso
introduciría latencia y costos sin beneficio. La Combinación tampoco se justifica:
una alerta de "trabajador sin casco en zona X a las 14:32" no requiere generación
de lenguaje natural — un mensaje estructurado tipo notificación es suficiente y más
confiable. ML Tradicional con visión computacional es la respuesta directa y
proporcionada al problema definido.
```

> **Nota PC2:** En la construcción del MVP usamos agentes GenAI (Gemini en n8n) **solo para redactar la notificación de WhatsApp** a partir de la detección. El núcleo de IA que resuelve el problema sigue siendo ML/visión computacional; la GenAI es un componente accesorio de comunicación, no el motor de detección.

### Si elegiste ML Tradicional — especifica el tipo:

- [X] Supervisado — Clasificación *(detección de objetos sobre frames: ¿hay casco? ¿cabeza sin casco?)*
- [ ] Supervisado — Regresión
- [ ] No Supervisado — Clustering

> *Nota técnica: la detección de objetos es un subtipo de clasificación supervisada que ubica espacialmente cada elemento (bounding box) y le asigna una etiqueta.*

---

## SECCIÓN 4 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿El problema está descrito sin mencionar tecnología? | SÍ |
| ¿La declaración del problema sigue el formato exacto? | SÍ |
| ¿La elección del tipo de IA se justifica con el problema, no con la preferencia? | SÍ |
| ¿Todos los integrantes pueden explicar este canvas sin leerlo? | SÍ |

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 1 de 4*
