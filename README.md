# Tablero de Stock · IDIOT

App de una sola página (HTML) para llevar el stock de componentes y descontar automáticamente al cargar envíos. Funciona en GitHub Pages, sin servidor.

## 🤖 Asistente (lo principal)

La pestaña **Asistente** es un chat: le escribís los movimientos en texto y él actualiza el stock solo. Entiende tus alias (Tótem KC50 = POS V7, Ops = OPS 15", Check In = OPS 10", Turnero = TUR V6) y siempre te muestra qué entendió y te pide confirmar antes de tocar nada.

Ejemplos que entiende:

- `Tótems KC50: 2, Ops: 1, Check In: 1, Turnero: 1` → descuenta esos 4 productos.
- `salieron 3 turneros a Grido Rosario` → envío con cliente y tienda.
- `entraron 50 Fuente Huntkey` → suma stock a ese componente.
- `se rompieron 2 scanners` → ajuste negativo.

**Devices terminados (equipos armados), 2 etapas:**

- `armé 5 POS V7 Blanco` → resta los componentes del BOM y suma 5 a Terminados.
- `vendí 100 POS V7 Blanco a Grido` (o "reservé") → suma 100 a Comprometidos.
- `envié 3 POS V7 Blanco a Grido` → resta 3 de Terminados (y de Comprometidos si hay).

Los 3 números que ves por producto (en Resumen y Productos):

- **En stock (capacidad)** = Terminados armados + los que puedo armar con componentes.
- **Para la venta** = capacidad − Comprometidos (lo que todavía podés vender).
- **Listos para enviar** = Terminados armados (los que podés despachar ya).

**Componentes bloqueantes:** "Puedo armar" solo cuenta los componentes marcados como **Bloq.** (los difíciles de conseguir). En POS V7 son: KC50, todo el sheet metal, la impresora (del color), HUB USB C y PCB Splitter V3. Los demás se asumen disponibles y no frenan el armado. Podés marcar/desmarcar el "Bloq." de cada componente en la pestaña Productos.

**Semáforo de stock (🔴🟡🟢):** cada componente se pinta según su stock. Default global 🔴 ≤ 5, 🟡 ≤ 15, 🟢 mayor — configurable con el botón **⚙ Semáforo**. Podés poner umbrales propios por componente en la pestaña Componentes (columnas Rojo≤ / Amar≤).

**Para pedir 🛒:** pestaña con todos los componentes en rojo o amarillo, ordenados (primero los urgentes), con proveedor y cuánto pedir para volver a verde.

**Chapa por color:** el sheet metal de POS V7 Blanco y Negro es distinto (no compartido). La chapa de Negro arrancó en 0 — cargale el stock real en Productos → POS V7 Negro.

Nota: POS V7 quedó dividido en **POS V7 Blanco** y **POS V7 Negro** (comparten los mismos componentes). Si escribís "Tótem KC50" sin color, el asistente te pregunta cuál.

## Qué hace

- **Stock único por componente**: si un componente se usa en varios productos (ej. *Fuente 12v 5a Huntkey* en OPS 15", OPS 10" y TUR V6), hay **un solo número de stock**. Al enviar cualquier producto que lo usa, se descuenta en todos.
- **Resumen**: cuántas unidades de cada producto podés armar/enviar hoy, valor total del stock, y qué componente limita cada producto.
- **Movimientos**:
  - *Envío que sale* → elegís producto + cantidad + cliente + tienda y descuenta cada componente según el BOM (cantidad × qty por producto). Te muestra un preview y avisa si algo queda negativo.
  - *Entrada de stock* → por conjunto/kit (suma a todos los componentes de un grupo) o a un componente puntual.
  - *Ajuste manual* → sumar/restar a un componente (conteo, rotura, etc.).
- **ABM**: crear/borrar productos, asignar/quitar/crear/borrar componentes.
- **Historial** de todos los movimientos, con botón *revertir* y exportar a CSV.

## Cómo usarlo

La app carga los datos de `data.json` (mismo repo/carpeta) al abrir, y guarda tus cambios en el navegador. Cuando yo (o vos) actualizamos `data.json`, la app lo toma al recargar. Para que el auto-cargado de `data.json` funcione tiene que estar servido por http (GitHub Pages, o un servidor local) — si abrís el archivo con doble clic (file://), el navegador bloquea la lectura y podés cargar el JSON a mano desde ⚙ → Cargar JSON.

### Subirlo a GitHub Pages
1. Creá un repo (ej. `stock-idiot`) y subí `index.html` y `data.json`.
2. En el repo → **Settings → Pages** → Source: rama `main`, carpeta `/root`. Queda publicado en `https://TU_USUARIO.github.io/stock-idiot/`.

### Guardar los datos en GitHub (persistencia real entre dispositivos)
Para que los cambios se guarden en el repo y no solo en tu navegador:
1. En GitHub: **Settings → Developer settings → Fine-grained tokens → Generate new token**.
   - *Repository access*: solo el repo del stock.
   - *Permissions → Contents: **Read and write***.
2. En la app, botón **⚙** → completá owner, repo, rama (`main`), archivo (`data.json`) y pegá el token → **Guardar config**.
3. Usás **⬆ Guardar en GitHub** para subir ca.