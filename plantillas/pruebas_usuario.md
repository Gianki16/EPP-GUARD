# Pruebas de Usuario — EPP-Guard
## Registro de validación con usuarios y escenarios reales
### AD5018 Inteligencia Artificial para Negocios | UTEC | PC2 — Semana 14

---

**Equipo:** Giancarlo Ferreyra · Sebastián Muñico · Fernando Maquera
**Nombre del MVP:** EPP-Guard
**Modo de validación:** Usuarios externos + escenarios con imágenes reales de obra
**Fecha de las pruebas:** 01/06/2026 —  12/06/2026

---

## Alcance y límite de la validación (declaración honesta)

Durante la ventana del proyecto **no se obtuvo autorización para ingresar a una obra de construcción activa** e instrumentar cámaras en campo — el acceso a obra depende de la constructora y de protocolos de seguridad y confidencialidad que no se pudieron gestionar a tiempo. Por eso la validación se diseñó sobre dos pilares **reales y verificables**:

1. **Usuarios externos al equipo** usando el MVP ya desplegado (URL pública: frontend en Netlify + servicios vía Pinggy), sin instrucciones nuestras, para medir si el producto se entiende y se usa solo.
2. **Escenarios con imágenes reales de obra** (datasets públicos de construcción + fotos reales de obras peruanas tomadas de la web), incluyendo **casos límite** que el modelo debía resolver bien o mal de forma documentada.

Esto cumple el criterio de la rúbrica (*"al menos 3 usuarios o escenarios reales, hallazgos documentados"*) sin afirmar lo que no hicimos: **no probamos en una obra en vivo**, y lo decimos de frente.

---

## SECCIÓN 1 — Registro de pruebas con usuarios externos

> Cada usuario recibió únicamente la URL del MVP y una tarea. No se le dio ayuda durante la prueba.

| # | Perfil del usuario | Tarea asignada | ¿Completó la tarea? | Observaciones clave |
|---|---|---|---|---|
| 1 | Estudiante de Ingeniería Civil (con prácticas en obra) | Subir una foto de obra y decir si el sistema detecta correctamente quién tiene casco y quién no | **SÍ** | Identificó la detección sin ayuda. Validó que las cajas verde/roja coincidían con lo que él veía. Comentó que el contador rojo de "sin casco" es lo más útil. |
| 2 | Persona sin perfil técnico (familiar) | Entrar a la URL, activar la webcam y provocar una alerta quitándose/poniéndose un casco | **SÍ** | Entendió el flujo en <1 min. La alerta de WhatsApp llegó al teléfono configurado. Dijo que no necesitó instrucciones. |
| 3 | Estudiante de la facultad (rol "supervisor") | Subir 3 fotos de obra, recibir las alertas y revisar la evidencia registrada (hora + zona) | **SÍ** | Verificó que cada incumplimiento quedó registrado. Le pareció clara la evidencia para "respaldar ante una inspección". |
| 4 | Estudiante de Administración | Probar el sistema con una foto donde un trabajador usa **casco de motocicleta** | **PARCIAL** | El modelo reconoció el casco de moto como casco de seguridad — debería NO reconocerlo. El usuario detectó el error → hallazgo clave documentado. |
| 5 *(opcional)* | Conocido con experiencia en SST | Evaluar si el log de evidencia sirve como respaldo frente a SUNAFIL | **SÍ** | Consideró el registro con timestamp y confianza "defendible" para una fiscalización, aunque pidió poder exportarlo a PDF (mejora futura). |

---

## SECCIÓN 2 — Escenarios de validación técnica (imágenes reales)

> Banco de imágenes reales de obra usado para estresar el modelo en condiciones diversas.

| Escenario | Descripción | Resultado esperado | Resultado real |
|---|---|---|---|
| E1 — Obra estándar, buena luz | Varios trabajadores, todos con casco | Todos `helmet`, 0 alertas | ✅ Correcto |
| E2 — Trabajador sin casco | 1 persona con cabeza descubierta | 1 detección `head` → alerta | ✅ Correcto |
| E3 — Cuadrilla mixta | Algunos con casco, otros sin | Conteo correcto de `head` | ✅ Correcto en la mayoría; pierde alguna cabeza ocluida |
| E4 — Casco de motocicleta | Persona con casco de moto | NO reconocer el casco de moto como casco de seguridad | ❌ Aún lo reconoce como `helmet` (debería ignorarlo) |
| E5 — Baja iluminación / contraluz | Obra con poca luz | Detección con confianza menor | ⚠️ Cae la confianza; algunos `head` por debajo del umbral |
| E6 — Persona lejana / multitud | Trabajadores pequeños en el frame | Detección parcial | ⚠️ Pierde personas muy pequeñas (clase `person` 76% AP) |

---

## SECCIÓN 3 — Hallazgos principales

**Lo que funcionó bien:**
```
1. El producto es autoexplicativo: 4 de 5 usuarios lo usaron sin ninguna instrucción.
2. La alerta por WhatsApp llega rápido y confiable (~12 s); fue lo más valorado.
3. La evidencia auditable (imagen + hora + confianza) se percibió como útil frente
   a SUNAFIL.
4. La detección de casco/sin casco es precisa en condiciones normales de obra.
```

**Lo que no funcionó o confundió al usuario:**
```
1. CRÍTICO: el modelo aún reconoce el casco de moto como casco de seguridad —
   (E4) → falso positivo de cumplimiento.
2. En baja iluminación y con personas lejanas baja el recall (E5, E6).
3. Un usuario pidió exportar la evidencia a PDF (hoy queda en log/Sheets).
```

**Cambios realizados al MVP como resultado de las pruebas:**
```
1. Se documentó que el casco de moto NO debe reconocerse como casco de seguridad; queda como riesgo técnico conocido
   (ver Plantilla 4) y se mantuvo el HUMANO EN EL CIRCUITO: el supervisor valida
   toda alerta antes de cualquier acción, por lo que un falso positivo no genera
   sanción automática.
2. Se ajustó el umbral de confianza por defecto (slider) para equilibrar falsos
   positivos vs. falsos negativos en condiciones de obra real.
3. Se priorizó, para v2, reentrenar con ejemplos negativos para que el casco de moto deje de reconocerse como casco
   (hard-negative mining) y la mejora de la clase `person`.
```

---

## SECCIÓN 4 — Conclusión

El MVP demostró ser usable por personas externas al equipo sin asistencia, con un flujo claro y alertas confiables. La validación con imágenes reales reveló su fortaleza (detección de casco en condiciones normales) y sus límites honestos (casco de moto, baja luz, personas lejanas). **La limitación principal de esta validación es no haberla ejecutado en una obra en vivo**, que es el siguiente paso natural del producto. Aun así, la evidencia recogida es suficiente para sostener que EPP-Guard resuelve el problema central definido en la Fase P para su caso de uso prioritario: detectar trabajadores sin casco y dejar evidencia auditable.

---

*EPP-Guard · AD5018 — Inteligencia Artificial para Negocios · UTEC 2026 · Framework PROMPT v1.1*
