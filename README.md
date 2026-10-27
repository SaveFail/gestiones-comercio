# Gestiones Comercio — Dashboard

Panel web autocontenido para **filtrar, medir y graficar** la hoja de Google de
gestión comercial (KAM, estatus, reuniones). Lee la hoja **en solo lectura** y
no modifica nada en Drive.

Se abre en el navegador desde **cualquier equipo**, sin instalar nada, y siempre
muestra los datos más recientes de la hoja.

## Cómo abrirlo

- **En línea (recomendado):** entra a la URL publicada:
  `https://savefail.github.io/gestiones-comercio/`
  Funciona igual en cualquier computadora, tableta o teléfono; al abrirla verás
  los datos vigentes.
- **Local:** haz doble clic en `index.html` (no necesita servidor).

## Actualización de los datos

- Botón **↻ Actualizar**: vuelve a descargar la hoja cuando alguien la edita.
- Casilla **Auto** (opcional): refresca solo cada 30 s, 1 min o 5 min.

> **Importante:** la vista pública de Google Sheets se sirve desde una caché y
> puede tardar **entre 1 y 5 minutos** en reflejar los cambios. Es una limitación
> de Google, no del dashboard.

## Qué muestra

- **6 métricas (KPI):** gestiones totales, empresas únicas (por RIF o nombre),
  KAM activos, *Aceptó*, próximas reuniones y *No contesta*.
- **4 gráficas interactivas:** estatus (dona), gestiones por KAM (barras), por
  cargo, y tendencia por día (línea). Clic en una porción o barra **filtra**.
- **Detallados · Desglose por categoría:** tabla que **especifica y contabiliza
  todo** agrupando por KAM, estatus, cargo o día, con cantidad, empresas únicas,
  porcentaje y *Aceptó*, más una fila de **totales**. Clic en cualquier fila (o
  en *Ver →*) para filtrar los registros por esa categoría.
- **Tabla completa de registros**, ordenable y paginada, con todos los campos
  (fecha, nombre comercial, razón social, RIF, contacto, cargo, teléfono, KAM,
  estatus, próxima reunión, hora y observaciones) y botón **Exportar CSV**.

Las métricas, gráficas, desglose y tabla **respetan los filtros activos**.

## Filtros

Búsqueda de texto, KAM, estatus, cargo y rango de fechas. Todo se recalcula al
instante, sin volver a descargar la hoja.

## Notas sobre los datos

- La pestaña *Nuevos 1* tiene **427 registros**, pero solo ~47 tienen *Fecha de
  gestión* rellena; la tendencia por día crece conforme se completan fechas.
- La cabecera `J-313127396` en realidad es el **Contacto**; se renombra solo.
- Hay 3 pestañas: **Nuevos 1**, **Nuevo 2** y **Proximas reuniones**; cada una
  se adapta a sus propias columnas.

## Publicar / actualizar en la web

El dashboard está publicado con GitHub Pages. Para publicar cambios:

```bash
git add .
git commit -m "Actualización"
git push origin main
```

En un par de minutos la URL `https://savefail.github.io/gestiones-comercio/`
refleja la nueva versión en todos los equipos.

## Cambiar la hoja o las pestañas

Edita el bloque `CONFIG` al inicio del `<script>` de `index.html`:

```js
const CONFIG = {
  sheetId: '1lwaneMVbV-PYbTFlcv9DlSMX4N0bc96Nzt-rO5xlDKY', // ID de la hoja
  title: 'Gestiones Comercio',
  sheets: [
    { name: 'Nuevos 1',           gid: '0' },
    { name: 'Nuevo 2',            gid: '1976128283' },
    { name: 'Proximas reuniones', gid: '1059294359' },
  ],
  pageSize: 25,
};
```

- `sheetId`: la parte de la URL después de `/spreadsheets/d/`.
- Cada pestaña tiene un `gid` (se ve en la URL como `#gid=...`).
- La hoja debe estar compartida como **«Cualquier persona con el enlace →
  Lector»**.

## Requisitos

Navegador moderno y conexión a internet (para leer Google y cargar Chart.js).

## Cómo funciona (técnico)

- Descarga los datos con la **API de visualización de Google** (`gviz/tq`) vía
  JSONP, lo que permite abrir el archivo desde `file://` sin problemas de CORS.
- Normaliza cabeceras (ignora acentos y mayúsculas) a campos canónicos.
- Chart.js (CDN) dibuja las gráficas; los filtros se aplican en memoria.
