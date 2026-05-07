# EPP-Guard — Demo de Detección de EPP

Aplicación web para detectar equipos de protección personal (EPP) en obras de
construcción usando visión computacional.

---

## ¿Qué hace esta aplicación?

EPP-Guard analiza imágenes o video en tiempo real y detecta automáticamente
los siguientes elementos en una obra de construcción:

| Elemento | Color del recuadro |
|---|---|
| Humano | 🟢 Verde |
| Casco | 🔵 Azul |
| Chaleco | 🟣 Morado |
| Botas | 🩵 Celeste |
| Guantes | 🟡 Amarillo |

Cada detección muestra un recuadro sobre el objeto con su nombre y el
porcentaje de confianza del modelo (ej: `Casco 95%`).

El modelo fue entrenado con **8,431 imágenes** y obtuvo:
- **mAP@50:** 97.7%
- **Precision:** 99.6%
- **Recall:** 97.6%

---

## Cómo abrir la aplicación

### Requisito único: tener Python instalado

Para verificar que tienes Python, abre una terminal y escribe:
```
python --version
```
Si ves algo como `Python 3.x.x`, estás listo.

> Si no tienes Python, descárgalo gratis en https://www.python.org/downloads/

---

### Paso 1 — Abrir la terminal en la carpeta correcta

**Windows:**
1. Abre la carpeta donde está el archivo `epp_guard_demo.html`
2. Haz clic en la barra de direcciones del explorador de archivos
3. Escribe `cmd` y presiona Enter

**Mac:**
1. Abre la carpeta en Finder
2. Click derecho → "Abrir Terminal aquí"

---

### Paso 2 — Iniciar el servidor local

Escribe este comando en la terminal y presiona Enter:

```
python -m http.server 8080
```

Verás algo como:
```
Serving HTTP on :: port 8080 ...
```

Deja esa ventana abierta mientras uses la aplicación.

---

### Paso 3 — Abrir la aplicación en el navegador

Abre **Google Chrome** y entra a esta dirección:

```
http://localhost:8080/epp_guard_demo.html
```

La aplicación carga en pocos segundos y ya está lista para usar.

> ⚠️ No abras el archivo haciendo doble clic — no funcionará.
> Siempre accede desde `http://localhost:8080/...`
> Importante tener en cuenta que el prototipo debe de lanzarse en un navegador con la aceleración de hardware activada.

---

## Cómo usar la aplicación

### Opción 1 — Subir una foto

1. Click en el botón **"Subir Foto"**
2. Selecciona cualquier imagen de obra desde tu computadora
3. El modelo analiza la imagen automáticamente
4. Los recuadros de detección aparecen sobre la imagen
5. En el panel derecho ves la lista de objetos detectados con su confianza

También puedes **arrastrar y soltar** una imagen directamente sobre la pantalla.

### Opción 2 — Webcam en tiempo real

1. Click en el botón **"Webcam"**
2. El navegador pedirá permiso para acceder a la cámara — acepta
3. La detección corre automáticamente sobre el video en vivo
4. Click en **"Detener"** para apagar la cámara

---

### Panel lateral

| Elemento | Descripción |
|---|---|
| **Confianza mínima** | Slider que controla qué tan seguro debe estar el modelo para mostrar una detección. Sube el valor para menos falsos positivos. Baja el valor para detectar más objetos. |
| **Detecciones** | Lista de todos los objetos detectados en la imagen actual con su porcentaje de confianza. |
| **Clases del modelo** | Referencia de colores para identificar cada tipo de EPP en los recuadros. |

---

## Para cerrar la aplicación

Cierra la pestaña del navegador y luego cierra la terminal (o presiona `Ctrl + C`).

---

*EPP-Guard · AD5018 Inteligencia Artificial para Negocios · UTEC 2026*
