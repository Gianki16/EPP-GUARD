# Plantilla 1 — Problem Statement Canvas
## Framework PROMPT | Fase P — Problema de Negocio
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: [Giancarlo Humberto Ferreyra Uribe]
- Integrante 2: [Sebastian Leonardo Muñico Diaz]
- Integrante 3: [Fernando Maquera]

**Fecha de entrega:** [08/05/26]
**Versión del canvas:** v1

---

## SECCIÓN 1 — Definición del problema

### 1.1 Usuario afectado
> *¿Quién sufre el problema? Sé específico: no "las empresas" sino "el jefe de ventas de una pyme retail".*

```
El supervisor de Seguridad y Salud Ocupacional (SSO) de obras de construcción
medianas (30 a 100+ trabajadores) en Lima Metropolitana, responsable directo
ante SUNAFIL del cumplimiento de la Norma G.050 y de la integridad física del
personal en frente de trabajo.
```

---

### 1.2 Problema específico
> *¿Qué está pasando exactamente? Describe la situación actual sin mencionar soluciones.*

```
La supervisión del uso correcto de EPP (casco, chaleco reflectivo, calzado, guantes) depende de un único
inspector humano con atención serial: cuando audita un frente de obra, no ve
los demás. La fiscalización es intermitente, con puntos ciegos inevitables, y
opera de forma reactiva —se detectan incumplimientos después del incidente o
por muestreo— en obras donde la Norma G.050 exige cumplimiento del 100% del
personal el 100% del tiempo.
```

---

### 1.3 Causa raíz
> *¿Por qué ocurre el problema? No el síntoma — la causa real.*

```
Cuello de botella estructural de atención humana: un solo par de ojos no puede
vigilar simultáneamente múltiples frentes de trabajo. El déficit no se resuelve
contratando más supervisores —el sector tiene escasez crónica de inspectores
y SUNAFIL apenas cuenta con 750 fiscalizadores para más de 2 millones de
unidades económicas formales—.
```

---

### 1.4 Consecuencia medible
> *¿Qué pierde el usuario sin solución? Expresa en números cuando sea posible (tiempo, dinero, clientes, errores).*

```
Entre enero y abril 2025 se registraron 6,954 accidentes laborales en Perú
(MTPE). En el trimestre abril-junio 2025, SUNAFIL reportó 139 accidentes
mortales y ordenó 56 paralizaciones de obra (SUNAFIL). El 79% de accidentes
en Lima se concentran en manufactura y construcción. Cada incumplimiento
detectado puede generar multas SUNAFIL >S/ 100,000, paralización de obra y
responsabilidad penal del supervisor (Ley 29783).
```

---

### 1.5 Declaración del problema — formato obligatorio
> *Completa la siguiente frase. Esta declaración debe sintetizar las 4 secciones anteriores en una sola oración.*

**"[Tipo de usuario] tiene dificultad para [acción específica] porque [causa raíz], lo que genera [consecuencia medible]."**

```
"El supervisor SSO de obras de construcción en Lima Metropolitana tiene
dificultad para verificar de forma continua y simultánea el uso correcto de
EPP en todos los frentes de trabajo porque la supervisión humana es serial e
intermitente, lo que genera detecciones tardías de incumplimientos asociados
a multas SUNAFIL superiores a S/ 100,000, paralizaciones de obra y un
contexto sectorial de 139 accidentes mortales solo en el trimestre
abril-junio 2025."
```

---

## SECCIÓN 2 — Filtro de validación IA

> *Responde cada pregunta con SÍ o NO y una justificación breve. Sé honesto — una respuesta incorrecta aquí arruina todo lo que sigue.*

| Pregunta | SÍ / NO | Justificación (1 línea) |
|---|---|---|
| ¿Una hoja de cálculo o formulario resuelve esto? | **NO** | El problema no es de registro, es de detección visual continua y simultánea sobre video en vivo. |
| ¿El problema escala con volumen de datos o usuarios? | **SÍ** | A más cámaras y más trabajadores, la supervisión humana se vuelve exponencialmente inviable. |
| ¿Hay un patrón repetitivo difícil de procesar manualmente? | **SÍ** | Identificar visualmente "casco sí/no", "chaleco sí/no", "arnés sí/no" sobre decenas de personas en simultáneo durante 10 horas diarias. |
| ¿El problema requiere generar contenido o razonar en lenguaje natural? | **NO** | El núcleo del problema es percepción visual, no generación de lenguaje. |
| ¿Necesitas predecir Y luego explicar o actuar sobre el resultado? | **SÍ (parcial)** | Se necesita detectar (clasificar) y luego accionar una alerta — pero la explicación en lenguaje natural no es el núcleo crítico. |

**Conclusión del filtro:**
> *¿Por qué la IA es la respuesta correcta y no otra solución más simple?*

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

Marca con una X la opción elegida:

- [ ] **IA Generativa** — el problema involucra lenguaje natural, conversación o generación de contenido
- [X] **ML Tradicional** — el problema requiere predecir, clasificar o segmentar con datos históricos
- [ ] **Combinación** — se necesita predecir Y comunicar/actuar sobre el resultado

### Justificación de la elección
> *¿Por qué este tipo de IA y no los otros dos? Argumenta en función del problema, no de la preferencia del equipo.*

```
El problema es de clasificación visual sobre imágenes de video (CCTV) — no de
generación de lenguaje ni de razonamiento conversacional. La IA Generativa no
aporta valor al núcleo del problema (detectar EPP en tiempo real); su uso
introduciría latencia y costos sin beneficio. La Combinación tampoco se justifica:
una alerta de "trabajador sin casco en zona X a las 14:32" no requiere
generación de lenguaje natural — un mensaje estructurado tipo notificación es
suficiente y más confiable. ML Tradicional con visión computacional es la
respuesta directa y proporcionada al problema definido.
```

### Si elegiste ML Tradicional — especifica el tipo:

- [X] Supervisado — Clasificación *(detección de objetos sobre frames de video: ¿hay casco? ¿hay chaleco? ¿hay arnés?)*
- [ ] Supervisado — Regresión
- [ ] No Supervisado — Clustering

> *Nota técnica: la detección de objetos es un subtipo de clasificación supervisada que ubica espacialmente cada elemento (bounding box) y le asigna una etiqueta. Requiere imágenes históricas etiquetadas — encaja en el cuadrante de aprendizaje supervisado.*

### Si elegiste Combinación — describe el flujo:
*(No aplica)*

---

## SECCIÓN 4 — Autoevaluación del equipo

> *Antes de entregar, el equipo responde estas preguntas internamente.*

| Pregunta de control | Respuesta |
|---|---|
| ¿El problema está descrito sin mencionar tecnología? | SÍ |
| ¿La declaración del problema sigue el formato exacto? | SÍ |
| ¿La elección del tipo de IA se justifica con el problema, no con la preferencia? | SÍ |
| ¿Todos los integrantes pueden explicar este canvas sin leerlo? | [PENDIENTE] |

> **Si alguna respuesta es NO → el canvas no está listo para entregar.**

---

*Framework PROMPT v1.0 — AD5018 UTEC | Plantilla 1 de 4*
