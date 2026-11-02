# Guía de cambios: Tarea 2 para Excelente

Son **2 commits**. Todo lo que hay que cambiar está en esta guía, en forma de diff (`-` es la línea que se borra y `+` la que se agrega).
Si no quieres editar a mano, junto a esta guía están los archivos ya corregidos, con la misma ruta que en el repo (`archivos_corregidos/buscador-js/...`, `archivos_corregidos/README.md`, etc.). Puedes copiarlos encima de los tuyos. **No copies esta guía al repo.**

Los cambios se probaron en una copia del zip:
- Sass compila.
- Los 5 módulos JS tienen sintaxis válida.
- Las utilidades nuevas (`object-fit-contain`, `text-bg-*`) se generan en el CSS.
- No hay `!important` en tus `.scss`.

Lo que **no** probé en un navegador es el clic en "Quitar" ni los estilos visuales. Esa prueba está en el paso 1.8.

---

## ⚠️ Antes de empezar: la etiqueta `tarea-2` y la fecha de entrega

La entrega cerró el **25 de octubre**. Si mueves la etiqueta `tarea-2` a un commit posterior, el profesor va a ver commits con fecha después del cierre.
- **Si todavía no entregaste o el profesor lo permite:** haz los 2 commits y mueve la etiqueta (paso 3).
- **Si ya entregaste:** pregúntale al profesor antes de mover la etiqueta. Si no lo permite, no la muevas. Igual te sirve aplicar los cambios para la defensa, en una rama aparte (`git switch -c mejoras-defensa`), y saber explicar estos bugs si te los señalan.

Abre una terminal en la raíz de tu repo y actualízalo:

```bash
git pull
```

---

## Commit 1: Parte A (compilación, animación, validación y utilidades)

### 1.1 `saas` → `sass` (el más importante)

```bash
cd buscador-js
```

```bash
npm uninstall saas
```

```bash
npm install -D sass
```

Comprueba que `package.json` ahora diga `"sass": "^1.x"` en `devDependencies`. Luego compila:

```bash
npm run sass
```

Tiene que aparecer `css/main.css` sin errores. Para probar desde cero como lo haría el profesor, borra `node_modules` y `css`, corre `npm install` y después `npm start`.

### 1.2 `js/render.js`: `transitionend` burbujea desde los botones

Reemplaza la función completa `quitarFilaConTransicion` (al final del archivo):

```js
// Primero corre la transición y cuando termina se saca la fila del DOM
export function quitarFilaConTransicion(fila, alTerminar) {
  let terminado = false;
  const terminar = () => {
    if (terminado) return;
    terminado = true;
    fila.removeEventListener('transitionend', alTerminarTransicion);
    fila.remove();
    alTerminar();
  };

  // transitionend burbujea: los botones de la fila (.btn) también tienen transiciones.
  // Solo cuenta la transición de opacidad de la propia fila.
  function alTerminarTransicion(evento) {
    if (evento.target === fila && evento.propertyName === 'opacity') terminar();
  }

  fila.addEventListener('transitionend', alTerminarTransicion);
  // Respaldo por si la transición no llega a ejecutarse (por ejemplo, con animaciones desactivadas)
  setTimeout(terminar, 1000);
  fila.classList.add('fila-saliendo');
}
```

**Por qué (para la defensa):** el botón "Quitar" tiene la transición de Bootstrap (0.15 s). Al deshabilitarlo, su `transitionend` sube hasta el `<tr>` y el `{ once: true }` se consumía con ese evento, así que la fila desaparecía a la mitad de su animación de 0.35 s.

### 1.3 `js/render.js`: colores de estado con utilidades de Bootstrap

Reemplaza la función `claseEstado`:

```diff
-function claseEstado(estado) {
-  // 'Leído' -> 'estado--leido'
-  const nombre = estado.normalize('NFD').replace(/[̀-ͯ]/g, '').toLowerCase();
-  return `badge estado estado--${nombre}`;
-}
+// Color de Bootstrap para cada estado (text-bg-* ya pone fondo y texto con buen contraste)
+const COLOR_ESTADO = {
+  'Pendiente': 'text-bg-warning',
+  'Leyendo': 'text-bg-primary',
+  'Leído': 'text-bg-success'
+};
+
+function claseEstado(estado) {
+  return `badge estado ${COLOR_ESTADO[estado] ?? 'text-bg-secondary'}`;
+}
```

La clase `estado` se mantiene porque `actualizarEstadoFila` la busca con `querySelector('.estado')`.
**Para la defensa:** si te piden agregar un 4.º estado, ahora basta con agregarlo en `ESTADOS` (`lista.js`) y en `COLOR_ESTADO`.

### 1.4 `js/render.js`: tarjetas con utilidades en lugar de clases propias

En `crearTarjetaLibro`:

```diff
-  const titulo = crear('h3', 'card-title libro__titulo', libro.titulo);
-  const autores = crear('p', 'card-text libro__autores mb-1', libro.autores.join(', '));
+  const titulo = crear('h3', 'card-title fs-6 mb-1', libro.titulo);
+  const autores = crear('p', 'card-text small text-secondary mb-1', libro.autores.join(', '));
```

```diff
-  tarjeta.append(crearPortada(libro, 'card-img-top libro__portada'), cuerpo);
+  tarjeta.append(crearPortada(libro, 'card-img-top libro__portada object-fit-contain bg-light p-2'), cuerpo);
```

### 1.5 `js/render.js`: dos detalles pequeños

En `mostrarDetalle`, después de `portada.alt = ...`:

```diff
   portada.alt = `Portada de ${libro.titulo}`;
+  // Propiedad y no addEventListener: así cada apertura del modal reemplaza la escucha en vez de acumularla
+  portada.onerror = () => {
+    portada.onerror = null;
+    portada.src = PORTADA_ALTERNATIVA;
+  };
```

En `actualizarEstadoFila`, antes de `etiqueta.classList.add('estado-cambiado');`:

```diff
   etiqueta.textContent = lectura.estado;
 
+  // Leer offsetWidth fuerza un reflow: así la animación se reinicia aunque se hagan clics seguidos
+  void etiqueta.offsetWidth;
   etiqueta.classList.add('estado-cambiado');
```

### 1.6 `scss/_componentes.scss` y `scss/_animaciones.scss`

En `_componentes.scss`, **borra** `&__titulo`, `&__autores` y todo el bloque `.estado { ... }`. Deja así el final del archivo:

```scss
  // Bootstrap no tiene una utilidad para un alto fijo; el resto (fondo, padding, object-fit) son utilidades en render.js
  &__portada {
    height: a-rem($alto-portada);
  }
}

.detalle-portada {
  max-height: a-rem(320px);
}
```

`.estante`, `[data-detalle]` y `.libro` con su `&:hover` quedan igual. En `_animaciones.scss`, borra la línea `display: inline-block;` de `.estado-cambiado`, porque `.badge` ya es `inline-block`.

### 1.7 `index.html`, `js/api.js`, `js/formulario.js` y `js/main.js`

**`index.html` (línea 183):** se quita el `src` que descargaba el SVG aunque el modal estuviera cerrado.

```diff
-              <img id="detalle-portada" class="img-fluid detalle-portada" src="img/sin-portada.svg" alt="">
+              <img id="detalle-portada" class="img-fluid object-fit-contain detalle-portada" alt="">
```

**`js/api.js`:**

```diff
-    titulo: doc.title,
+    titulo: doc.title ?? 'Sin título',
```

**`js/formulario.js`**, al inicio de `reglasExtra`:

```diff
 function reglasExtra(campo) {
+  // required y minlength aceptan espacios; aquí se rechaza un campo que solo tiene espacios
+  if (campo.required && campo.value !== '' && campo.value.trim() === '') {
+    return 'Este campo no puede quedar en blanco.';
+  }
   if (campo.name === 'anio' && ...
```

**`js/main.js`:**

```diff
 const textoBusqueda = document.getElementById('texto-busqueda');
+const botonBuscar = formBusqueda.querySelector('button[type="submit"]');
```

```diff
   mostrarEstado(estadoBusqueda, 'cargando');
   resultados.replaceChildren();
+  // Mientras espera la respuesta no se puede lanzar otra búsqueda que la pise
+  botonBuscar.disabled = true;
 
   try {
```

```diff
     console.warn('Error en la búsqueda:', error.message);
+  } finally {
+    botonBuscar.disabled = false;
   }
 });
```

El `finally` también corre cuando el `try` hace `return` en el caso de resultado vacío.

### 1.8 Probar antes del commit

```bash
npm run sass
```

```bash
npm start
```

Abre http://localhost:3000 con la consola abierta y revisa:
- [ ] La página se ve con estilos y la consola no muestra errores.
- [ ] Buscar "tolkien": el botón se deshabilita mientras carga y las tarjetas se ven igual que antes (título, autores en gris, portada con fondo claro).
- [ ] Agregar 2 libros, hacer clic varias veces seguidas en "Cambiar estado" (el badge salta en cada clic y cambia de color) y luego "Quitar" (la fila se desliza **completa** antes de desaparecer).
- [ ] Formulario: escribir solo espacios en título, motivo o autor muestra "Este campo no puede quedar en blanco.".
- [ ] Abrir el detalle de un libro sin portada muestra la imagen alternativa.

### 1.9 Commit

Vuelve a la raíz del repo:

```bash
cd ..
```

```bash
git add buscador-js
```

```bash
git commit -m "Corrige compilación de Sass (sass en vez de saas), animación al quitar y validación de campos en blanco; usa utilidades de Bootstrap en tarjetas y estados"
```

`css/` y `node_modules/` no se suben porque están en el `.gitignore`. Revísalo con `git status` antes del commit.

---

## Commit 2: README y depuración

### 2.1 `README.md` a UTF-8

Está guardado en UTF-16 y GitHub puede mostrarlo mal. En VS Code, haz clic en **"UTF-16 LE"** en la barra inferior derecha, elige **"Save with Encoding"** y luego **"UTF-8"**.
También puedes copiar `archivos_corregidos/README.md`, que ya está en UTF-8 y trae los 3 cambios de abajo.

Cambios de texto:

**Tabla de organización de la Parte A:**

```diff
-| `scss/_componentes.scss` | Componente propio `.estante` (CSS Grid, mobile-first) y colores de estados |
+| `scss/_componentes.scss` | Componente propio `.estante` (CSS Grid, mobile-first) y ajustes de tarjetas que Bootstrap no trae como utilidad |
```

**Sección "Parte B → Ejecutar"**, después de "Vite compila el Sass por su cuenta, no hace falta un paso aparte.", agrega:

> `src/styles/main.scss` reutiliza los parciales de `buscador-js/scss/`, así que la Parte B necesita el repositorio completo (no solo la carpeta `tablero-react/`).

**"Del DOM manual a React", viñeta del resumen:**

```diff
-en la Parte B el resumen se calcula desde `libros` en cada render, así que nunca queda desactualizado.
+en la Parte B el resumen se calcula en cada render a partir de `libros` (contando los que pasan el filtro por miembro, `librosDelMiembro` en `App.jsx`), así que nunca queda desactualizado.
```

### 2.2 Medir la red de nuevo (sin extensiones)

Tus capturas actuales incluyen archivos de una extensión de Adobe (`vendor.js`, `otherVendors.js`, fuentes `AdobeClean-*.otf`), así que los totales no son de tu sitio.

1. Con `npm start` corriendo, abre una **ventana de incógnito** (Cmd+Shift+N). Ahí las extensiones vienen desactivadas.
2. Abre DevTools, ve a **Network** y marca **Disable cache**. Recarga con Cmd+Shift+R.
3. **Captura** el panel completo, con la barra inferior visible ("X requests | Y kB transferred"), y guárdala como `docs/capturas/07-red_inicial.png`.
4. Busca "tolkien" y baja hasta el final de los resultados para que carguen todas las portadas.
5. **Captura** de nuevo, con `search.json` y las peticiones a `covers` visibles en la lista, y guárdala como `docs/capturas/08-red_busqueda.png`.
6. Anota los números.

### 2.3 `docs/depuracion.md`

Copia `archivos_corregidos/docs/depuracion.md` y **reemplaza cada `XX`** de la tabla nueva con tus números del paso 2.2. Respecto del original, el archivo trae estos cambios:
- Pie de la captura 01: ya no dice que la consola muestra `modal-open`, porque en la imagen no se ve.
- Sección 2: pide la ventana de incógnito y corrige lo de `sin-portada.svg`. Agrega la tabla de peticiones y peso, y referencia las capturas 07 y 08.
- Sección 3: pone 35.0 kB → 27.6 kB (−21 %), que es lo que muestran tus capturas 04 y 06 (antes decía 32 → 27).

La captura 05 ya no se usa. Bórrala:

```bash
git rm "docs/capturas/05-busqueda_css_antes.png"
```

**Opcional, si tienes tiempo:** rehaz la captura 01 mostrando `document.body.classList.contains('modal-open')` → `true` en la consola. Para reproducir el bug:
1. Crea una rama temporal.
2. Vuelve a poner `data-bs-toggle="modal"` y `data-bs-target="#modal-detalle"` en la tarjeta.
3. Pon el breakpoint en `stopPropagation()` y toma la captura.
4. Vuelve a `main`.

Si la haces, puedes volver a poner el pie de foto original.

### 2.4 Commit

```bash
git add README.md docs
```

```bash
git commit -m "Documenta análisis de red sin extensiones, corrige medición de CSS y guarda README en UTF-8"
```

---

## 3. Etiqueta y push (solo si el profesor lo permite; ver la advertencia del inicio)

```bash
git push
```

```bash
git tag -f tarea-2
```

```bash
git push -f origin tarea-2
```

Comprueba en GitHub, en **Tags**, que `tarea-2` apunte al último commit, y que el README se vea bien.

## 4. Revisa en GitHub (no se podía ver en el zip)

- [ ] 15 commits o más, con mensajes descriptivos y repartidos en varios días: `git log --oneline --format="%ad %s" --date=short`
- [ ] `node_modules/` y `buscador-js/css/` nunca se subieron: `git log --all --oneline -- "*/node_modules/*" buscador-js/css`, que no debe mostrar nada.
- [ ] Clonar en una carpeta nueva y seguir el README al pie de la letra: las dos partes deben levantar.
