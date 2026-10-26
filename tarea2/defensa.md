# Guía de defensa de la Tarea 2

Este archivo es para preparar la defensa oral. No se sube al repositorio.

La guía para construir el proyecto commit por commit está en `GUIA_COMMITS.md`. Esta guía supone que ya lo construiste (o que lo estás construyendo) y te prepara para explicarlo, defenderlo y modificarlo **sin ayuda de IA y sin autocompletado**.

## Índice

1. Cómo es la defensa y qué se evalúa
2. Mapa del proyecto
3. Parte A archivo por archivo, función por función
4. Parte B componente por componente
5. Recorridos completos de interacciones
6. Los 13 criterios técnicos: qué decir y dónde mostrarlo
7. Decisiones de diseño, alternativas y limitaciones
8. Modificaciones en caliente paso a paso
9. Banco de preguntas con respuestas
10. Glosario
11. Si algo sale mal durante la defensa
12. Plan de práctica y checklist

---

# 1. Cómo es la defensa y qué se evalúa

La defensa vale **la mitad de la nota** (criterios 14 al 17). Además, si no logras explicar algo de los criterios 1 al 13, **ese criterio del producto también baja** al nivel que demuestres. Es decir: un código perfecto que no sabes explicar puede terminar con nota baja.

Condiciones:
- Se hace sobre el commit con la etiqueta `tarea-2`.
- Las dos partes tienen que estar **corriendo** en tu computadora al iniciar.
- **Sin IA ni autocompletado** (Copilot, ChatGPT, Claude, Cursor). Si los usas: Deficiente en 14 al 17.
- Solo puedes consultar la documentación oficial (MDN, Bootstrap, Sass, React, Vite).
- Las modificaciones en caliente se hacen en una **rama aparte**.

| Criterio | Qué tienes que lograr para Excelente | Dónde te preparas en esta guía |
|---|---|---|
| 14. Explicación del código | Explicar cualquier fragmento que te señalen, recorrer una interacción completa en las dos partes y decir qué requisito cumple cada fragmento | Secciones 3, 4 y 5 |
| 15. Dominio técnico | Responder con precisión sobre delegación, propagación, validación de restricciones, async/await, `response.ok`, variables de Sass, grilla, inmutabilidad, `key` y estado elevado, **mostrando dónde está en tu código** | Secciones 6 y 9 |
| 16. Decisiones de diseño | Defender módulos, Bootstrap/Sass, componentes y ubicación del estado frente a alternativas, reconocer limitaciones y explicar qué cambia de DOM manual a React | Sección 7 |
| 17. Modificaciones en caliente | Ubicar rápido dónde cambiar, hacerlo respetando el modelo de cada parte (delegación y DOM en A, estado inmutable en B) sin romper nada, y demostrarlo | Sección 8 |

**La regla de oro para responder:** concepto → dónde está → por qué así. Por ejemplo:

> "La delegación es poner una sola escucha en un contenedor y aprovechar que los eventos burbujean. **En mi código** está en `main.js`, en `tablaLecturas.addEventListener('click', ...)`. **Lo hice así** porque las filas se crean después de cargar la página y no quería registrar una escucha por cada botón."

---

# 2. Mapa del proyecto

## 2.1 Estructura

```
tarea-2/
├── README.md                    instalación, ejecución, "Del DOM manual a React"
├── .gitignore                   node_modules, css generado, dist
├── docs/
│   ├── depuracion.md            error real, panel de red, optimización
│   └── capturas/                7 capturas de DevTools
├── buscador-js/                 PARTE A: JavaScript sin framework
│   ├── index.html               estructura fija de la página
│   ├── package.json             Bootstrap, Sass, serve y scripts
│   ├── img/sin-portada.svg      portada alternativa
│   ├── scss/
│   │   ├── main.scss            junta todo y elige qué partes de Bootstrap importar
│   │   ├── _variables.scss      variables de origen de Bootstrap + propias
│   │   ├── _mixins.scss         función a-rem, mixins desde y sombra-suave
│   │   ├── _componentes.scss    .estante (Grid), .libro, colores de estados
│   │   └── _animaciones.scss    keyframes y transiciones de la lista
│   └── js/
│       ├── main.js              conecta eventos con los demás módulos
│       ├── api.js               fetch a Open Library y JSONPlaceholder
│       ├── render.js            crea y actualiza elementos del DOM
│       ├── lista.js             estado en memoria de la lista
│       └── formulario.js        validación, autores dinámicos, envío
└── tablero-react/               PARTE B: React con Vite
    ├── vite.config.js
    └── src/
        ├── main.jsx             monta App en #root
        ├── App.jsx              estado (libros, filtros), derivados y handlers
        ├── constantes.js        ESTADOS y claseEstado
        ├── data/libros.js       miembros y libros iniciales
        ├── styles/main.scss     Bootstrap + parciales de la Parte A
        └── components/
            ├── Panel/Panel.jsx
            ├── Filtros/Filtros.jsx
            ├── Resumen/Resumen.jsx
            ├── ListaLibros/ListaLibros.jsx
            ├── ListaLibros/LibroItem.jsx
            └── FormularioLibro/FormularioLibro.jsx
```

## 2.2 Quién usa a quién en la Parte A

```
index.html
   └── carga main.js (type="module")
          ├── importa api.js        → buscarLibros
          ├── importa lista.js      → agregarLectura, obtenerLectura, siguienteEstado, quitarLectura, cantidadLecturas
          ├── importa render.js     → mostrarResultados, mostrarEstado, mostrarDetalle, crearAlerta,
          │                           crearFilaLectura, actualizarEstadoFila, quitarFilaConTransicion
          └── importa formulario.js → iniciarFormulario
                    ├── importa api.js    → enviarPropuesta
                    └── importa render.js → crearAlerta
```

Reglas que se cumplen y conviene decir:
- `api.js` y `lista.js` **no tocan el DOM**. Solo datos.
- `render.js` **no sabe de eventos ni de la API**. Recibe datos y crea o cambia elementos.
- `main.js` es el **único** que conecta eventos de la página con los demás.
- `formulario.js` maneja todo lo del formulario de propuesta, incluidos sus propios eventos, porque es una pieza independiente de la página.

## 2.3 Árbol de componentes de la Parte B

```
App                         estado: libros, filtros
├── Panel "Resumen"         children ↓
│   └── Resumen             props: resumen, estadoActivo, onElegirEstado(fn)
├── Panel "Filtros"
│   └── Filtros             props: filtros, miembros, onCambiar(fn)
├── Panel "Libros del club (n)"
│   └── ListaLibros         props: libros (filtrados), miembros, onCambiarEstado(fn), onQuitar(fn)
│       └── LibroItem × n   props: libro, nombreMiembro, onCambiarEstado(fn), onQuitar(fn)   key=libro.id
└── Panel "Agregar un libro"
    └── FormularioLibro     props: miembros, onAgregar(fn)     estado local: datos, errores
```

Flechas para explicar: **los datos bajan** por props (App → hijos) y **los eventos suben** llamando a funciones que el padre pasó (hijos → App).

---

# 3. Parte A archivo por archivo, función por función

Para cada función: **qué recibe**, **qué devuelve o hace**, **quién la llama** y **qué hay que saber**. Si el profe señala cualquier línea, ubica primero en qué función está y arranca por ahí.

## 3.1 `index.html`

- **`<link rel="stylesheet" href="css/main.css">`**: el CSS compilado por Sass. Si no corriste `npm run sass` (o `npm start`), no existe y la página sale sin estilos.
- **Navbar**: `navbar-expand-md` (menú completo desde md, hamburguesa en celular). El botón hamburguesa usa `data-bs-toggle="collapse"` y `data-bs-target="#menu"`; Bootstrap lo maneja solo. Los enlaces `#buscar`, `#mi-lista`, `#proponer` son anclas a los `id` de las secciones.
- **Grilla**: `<div class="row g-4">` con `<section class="col-12 col-lg-8">` y `<aside class="col-12 col-lg-4">`.
- **Formulario de búsqueda**: `input type="search" required minlength="2"`. **No** tiene `novalidate`, así que si está vacío el navegador mismo impide el `submit` y muestra su globito.
- **`#estado-busqueda`**: contenedor donde `mostrarEstado` pone spinner o alertas. `aria-live="polite"` para lectores de pantalla.
- **`#resultados.estante`**: el componente propio donde van las tarjetas.
- **Tabla**: `<tbody id="tabla-lecturas">` vacío; JavaScript le agrega filas. `#aviso-lista` (alerta de duplicado) y `#lista-vacia` (mensaje cuando no hay libros).
- **Formulario de propuesta** con `novalidate`. Cada campo tiene `name` y un `<div class="invalid-feedback">` al lado. Los autores están dentro de `#lista-autores`, cada uno en un `.campo-autor` con su botón `data-accion="quitar-autor"`.
- **Modal** `#modal-detalle` con `#detalle-titulo`, `#detalle-portada` y `#detalle-datos` (un `<dl>`).
- **Scripts**: primero `bootstrap.bundle.min.js` (normal, crea la variable global `bootstrap`), después `js/main.js` con `type="module"`.

## 3.2 `js/api.js`

| Elemento | Qué es |
|---|---|
| `URL_BUSQUEDA`, `URL_PORTADAS`, `URL_PROPUESTAS` | Constantes con las URL. Si cambia una URL, se cambia aquí y en ningún otro lado. No se exportan. |
| `buscarLibros(texto)` | **Recibe** el texto de búsqueda. **Devuelve** una promesa con un arreglo de libros ya convertidos. **La llama** `main.js` en el `submit` del buscador. **Lanza error** si `!respuesta.ok`. |
| `convertirLibro(doc)` | **Recibe** un objeto crudo de Open Library. **Devuelve** `{ id, titulo, autores, anio, portada, ediciones, idiomas }`. Interna (no exportada). Se usa en `datos.docs.map(convertirLibro)`. |
| `enviarPropuesta(propuesta)` | **Recibe** el objeto armado por `leerDatos()`. Hace POST con JSON. **Devuelve** la respuesta del servidor (con `id`). **La llama** `manejarEnvio` en `formulario.js`. **Lanza error** si `!respuesta.ok`. |

Detalles por línea de `buscarLibros`:
1. `const url = \`${URL_BUSQUEDA}?q=${encodeURIComponent(texto)}&limit=12\`;` arma la URL. `limit=12` lo pide la consigna.
2. `const respuesta = await fetch(url);` espera la respuesta HTTP (solo los encabezados; el cuerpo todavía no).
3. `if (!respuesta.ok) throw ...` revisa el código HTTP.
4. `const datos = await respuesta.json();` lee el cuerpo y lo convierte de JSON a objeto. También es asincrónico, por eso lleva `await`.
5. `return datos.docs.map(convertirLibro);` Open Library pone los resultados en la propiedad `docs`.

Datos de la API que se usan:
- `key` → `id` (ej. `/works/OL27448W`)
- `title` → `titulo`
- `author_name` (arreglo) → `autores`
- `first_publish_year` → `anio`
- `cover_i` (número) → URL de la portada
- `edition_count` → `ediciones`
- `language` (arreglo de códigos como `"spa"`, `"eng"`) → `idiomas`

## 3.3 `js/lista.js`

| Elemento | Qué es |
|---|---|
| `ESTADOS` | `['Pendiente', 'Leyendo', 'Leído']`. Exportado. El orden define el ciclo de estados. |
| `lecturas` | Arreglo privado del módulo con las lecturas. **No** se exporta: solo se cambia con las funciones. |
| `obtenerLectura(id)` | Devuelve la lectura con ese id o `undefined`. La usan `agregarLectura`, `siguienteEstado` y el detalle en `main.js`. |
| `agregarLectura(libro)` | Si ya existe devuelve `null`. Si no, crea `{ ...libro, estado: 'Pendiente' }`, la guarda con `push` y la devuelve. |
| `siguienteEstado(id)` | Cambia `lectura.estado` al siguiente de `ESTADOS` (con `%` vuelve al inicio) y devuelve la lectura. |
| `quitarLectura(id)` | Busca la posición con `findIndex` y la borra con `splice`. Si no existe (`-1`) no hace nada. |
| `cantidadLecturas()` | Devuelve `lecturas.length`. La usa `actualizarMensajeListaVacia`. |

Por qué `cantidadLecturas()` y no exportar `lecturas`: si `main.js` tuviera el arreglo, podría modificarlo sin pasar por las reglas (por ejemplo, agregar duplicados).

## 3.4 `js/render.js`

| Función | Recibe | Hace / devuelve | La llama |
|---|---|---|---|
| `crear(etiqueta, clases, texto)` | nombre de etiqueta, clases y texto opcionales | Devuelve un elemento nuevo | Todas las de este archivo |
| `crearPortada(libro, clases)` | libro y clases | Devuelve un `<img>` con la portada o la alternativa, `alt`, `loading="lazy"` y fallback en `error` | `crearTarjetaLibro` |
| `crearTarjetaLibro(libro)` | un libro | Devuelve un `<article class="card libro">` con `data-id`, `data-detalle`, portada, título, autores, año y botón `data-accion="agregar"` | `mostrarResultados` (con `map`) |
| `mostrarResultados(contenedor, libros)` | contenedor y arreglo | Reemplaza el contenido del contenedor por las tarjetas | `main.js` (submit) |
| `mostrarEstado(contenedor, tipo, mensaje)` | contenedor, `'cargando' \| 'error' \| 'vacio' \| 'ninguno'` y texto | Pone spinner, alerta roja, alerta azul o vacía el contenedor | `main.js` (submit) |
| `crearAlerta(color, mensaje)` | clase de color y texto | Devuelve un `<div class="alert ...">` con `role="alert"` | `mostrarEstado`, `main.js`, `formulario.js` |
| `mostrarDetalle(libro)` | libro o lectura | Llena título, portada grande y `<dl>` del modal, y lo abre con `bootstrap.Modal.getOrCreateInstance(...).show()` | `main.js` (escucha en `<main>`) |
| `claseEstado(estado)` | texto del estado | Devuelve `'badge estado estado--leido'` (sin tildes) | `crearFilaLectura`, `actualizarEstadoFila` |
| `crearFilaLectura(lectura)` | una lectura | Devuelve un `<tr>` con `data-id`, `data-detalle`, clase `fila-entrando` (animación), celdas y dos botones con `data-accion` | `agregarALista` en `main.js` |
| `actualizarEstadoFila(fila, lectura)` | la fila y la lectura | Cambia clase y texto de la etiqueta de **esa** fila y dispara la animación `estado-cambiado` | `main.js` (tbody) |
| `quitarFilaConTransicion(fila, alTerminar)` | la fila y una función | Agrega `fila-saliendo`; en `transitionend` hace `fila.remove()` y llama a `alTerminar()` | `main.js` (tbody) |

Puntos finos:
- `mostrarDetalle` usa `document.getElementById` porque el modal es fijo en el HTML. Está bien en la Parte A; lo prohibido es `innerHTML` con datos de la API.
- En `crearFilaLectura` se muestra solo `lectura.autores[0]` (el primer autor) para que la tabla no quede muy ancha.
- `etiqueta.className = claseEstado(...)` **reemplaza todas** las clases (por eso borra también `estado-cambiado` si quedó de antes) y luego se vuelve a agregar `estado-cambiado`.

## 3.5 `js/main.js`

Orden del archivo:
1. **Imports** de los cuatro módulos.
2. **Referencias a elementos**: `formBusqueda`, `textoBusqueda`, `estadoBusqueda`, `resultados`, `tablaLecturas`, `listaVacia`, `avisoLista`, `contenido` (el `<main>`). Se buscan una vez al inicio y se reutilizan.
3. **`let librosEncontrados = []`**: los libros de la última búsqueda.
4. **Escucha `submit` del buscador** (async): valida largo, pone `'cargando'`, limpia resultados, `await buscarLibros`, decide vacío/resultados, `catch` para error.
5. **`buscarPorId(id)`**: busca en `librosEncontrados`.
6. **Escucha `click` en `<main>`**: `closest('[data-detalle]')` → `mostrarDetalle(obtenerLectura(id) ?? buscarPorId(id))`.
7. **Escucha `hide.bs.modal`**: `document.activeElement.blur()` para evitar el aviso de `aria-hidden`.
8. **Escucha `click` en `#resultados`**: `closest('[data-accion="agregar"]')` → `stopPropagation()` → `agregarALista(...)`.
9. **`actualizarMensajeListaVacia()`**: `listaVacia.classList.toggle('d-none', cantidadLecturas() > 0)`.
10. **`agregarALista(libro)`**: llama a `agregarLectura`; si da `null` muestra aviso; si no, limpia aviso, hace `append` de la fila nueva y actualiza el mensaje.
11. **Escucha `click` en el `tbody`**: `closest('button[data-accion]')` → `stopPropagation()` → `closest('tr').dataset.id` → según `data-accion` cambia estado o quita.
12. **`iniciarFormulario()`**.

Hay **exactamente una** escucha de clic por contenedor: `<main>`, `#resultados` y `tbody`. Ninguna se registra dentro de un bucle ni al crear filas. Eso es "sin escuchas duplicadas" (criterio 2).

**¿Por qué se pueden usar funciones antes de declararlas?** `agregarALista` se usa en la escucha de `#resultados`, que está escrita antes que la función. Funciona porque las declaraciones `function` se "elevan" (hoisting) y además la escucha se ejecuta mucho después, cuando alguien hace clic.

## 3.6 `js/formulario.js`

| Elemento | Qué es |
|---|---|
| `MIN_AUTORES`, `MAX_AUTORES` | 1 y 5 |
| `formulario`, `listaAutores`, `botonAgregarAutor`, `botonEnviar`, `alerta` | Referencias a elementos |
| `hoyComoTexto()` | Devuelve la fecha local como `"yyyy-mm-dd"` |
| `reglasExtra(campo)` | Devuelve el mensaje de la regla de año o de fecha, o `''` |
| `mensajeDeError(campo)` | Lee `campo.validity` y devuelve el mensaje propio |
| `validarCampo(campo)` | `setCustomValidity` + `checkValidity` + escribe el feedback + `is-invalid`/`is-valid`. Devuelve `true`/`false` |
| `validarFormulario()` | Valida todos los `input, select, textarea`. Devuelve si todos están bien |
| `camposAutor()` | `NodeList` con los `.campo-autor` actuales |
| `actualizarBotonesAutor()` | Deshabilita "+ Agregar" en 5 y los × en 1 |
| `agregarCampoAutor()` | Clona el primero, lo limpia, lo agrega, actualiza botones, le da foco |
| `quitarCampoAutor(boton)` | Quita el `.campo-autor` del botón, si quedan más de 1 |
| `leerDatos()` | Con `FormData` arma el objeto a enviar (con `getAll('autor')`) |
| `reiniciarFormulario()` | `reset()`, deja un autor, quita clases de validación |
| `manejarEnvio(evento)` | `preventDefault`, valida, estado "Enviando...", `await enviarPropuesta`, alerta, `finally` |
| `iniciarFormulario()` | Pone `max` del año y `min` de la fecha, registra escuchas (`input` en el form, `click` en `#lista-autores`, `click` en "+ Agregar", `submit`) y actualiza botones |

**Qué campo valida qué:**

| Campo | HTML | Regla JS extra | Mensajes posibles |
|---|---|---|---|
| Título | `required minlength="2" maxlength="120"` | — | obligatorio, al menos 2 |
| Autor (1 a 5) | `required minlength="3" maxlength="80" pattern` | — | obligatorio, al menos 3, solo letras |
| Año | `type="number" required min="1450" step="1"` + `max` desde JS | no futuro | obligatorio, mínimo 1450, no puede ser futuro, número entero |
| Páginas | `type="number" required min="10" max="3000" step="1"` | — | obligatorio, mínimo 10, máximo 3000, número entero |
| Género | `<select required>` | — | obligatorio |
| Correo | `type="email" required` | — | obligatorio, correo válido |
| Fecha | `type="date" required` + `min` desde JS | no anterior a hoy | obligatorio, no puede ser anterior a hoy |
| Motivo | `<textarea required minlength="20" maxlength="500">` | — | obligatorio, al menos 20 |

## 3.7 Sass

**`_variables.scss`**
- Variables de origen de Bootstrap: `$primary`, `$secondary`, `$success`, `$danger`, `$warning`, `$light`, `$body-bg`, `$font-family-sans-serif`, `$headings-font-family`, `$border-radius`, `$border-radius-lg`, `$enable-shadows`.
- Propias: `$espacio-base`, `$alto-portada`, `$duracion-animacion`.

**`_mixins.scss`**
- `@function a-rem($px)` → `math.div($px, 16px) * 1rem`.
- `@mixin desde($punto)` → `@media (min-width: map.get($grid-breakpoints, $punto)) { @content; }`.
- `@mixin sombra-suave` → una `box-shadow` reutilizable.

**`_componentes.scss`**
- `.estante`: Grid con `--columnas` y `--separacion`, mobile-first con `desde(sm)`, `desde(lg)`, `desde(xxl)`.
- `[data-detalle] { cursor: pointer; }`.
- `.libro` con `&:hover`, `&__portada`, `&__titulo`, `&__autores`.
- `.estado` con `&--pendiente`, `&--leyendo`, `&--leido`.
- `.detalle-portada`.

**`_animaciones.scss`**
- `@keyframes aparecer` + `.fila-entrando`.
- `.fila-lectura` (la `transition`) + `.fila-saliendo` (el estado final).
- `@keyframes resaltar` + `.estado-cambiado`.

**`main.scss`**: orden variables → configuración de Bootstrap → parciales de Bootstrap usados → `utilities/api` → mixins → componentes → animaciones.

Puntos de corte de Bootstrap (apréndetelos): `sm` 576 px, `md` 768 px, `lg` 992 px, `xl` 1200 px, `xxl` 1400 px.

---

# 4. Parte B componente por componente

## 4.1 `main.jsx`
Importa los estilos (`./styles/main.scss`) y monta `<App />` dentro de `#root` con `createRoot`. `StrictMode` activa revisiones extra en desarrollo. Es el único `getElementById` de la Parte B y está fuera de los componentes.

## 4.2 `data/libros.js` y `constantes.js`
- `miembros`: 4 objetos `{ id, nombre }`.
- `librosIniciales`: 6 objetos `{ id, titulo, autor, estado, miembroId }`.
- `ESTADOS` y `claseEstado(estado)`: los mismos de la Parte A.

## 4.3 `App.jsx`

| Parte | Código | Qué es |
|---|---|---|
| Estado | `const [libros, setLibros] = useState(librosIniciales)` | La lista de libros |
| Estado | `const [filtros, setFiltros] = useState({ estado: 'todos', miembro: 'todos' })` | Los filtros, elevados aquí porque los usan Filtros, Resumen y la lista |
| Derivado | `librosDelMiembro` | `libros` filtrados solo por miembro |
| Derivado | `librosFiltrados` | `librosDelMiembro` filtrados también por estado; es lo que se muestra |
| Derivado | `resumen` | Objeto `{ Pendiente: n, Leyendo: n, Leído: n }` contado sobre `librosDelMiembro` |
| Handler | `cambiarEstado(id)` | `map` + objeto nuevo con el siguiente estado |
| Handler | `quitarLibro(id)` | `filter` |
| Handler | `agregarLibro(datos)` | `[...libros, { ...datos, id: crypto.randomUUID() }]` |
| Handler | `elegirEstado(estado)` | `setFiltros({ ...filtros, estado })` |
| JSX | navbar, `row` con `col-12 col-lg-8` (Resumen, Filtros, Lista) y `col-12 col-lg-4` (Formulario), cada uno en un `Panel` |

**Sobre `resumen`:** se crea un objeto vacío y se llena con `forEach`. Eso es mutar un objeto, pero **no es estado**: es una variable local que se crea de nuevo en cada render. La regla de inmutabilidad aplica al estado de `useState`.

## 4.4 `Panel.jsx`
`function Panel({ titulo, children })`. Pone una `card` con `card-header` (el título) y `card-body` (los `children`). Es el componente que **recibe children** (requisito).

## 4.5 `Filtros.jsx`
- Props: `filtros`, `miembros`, `onCambiar`.
- Sin estado propio.
- Dos `<select>` controlados (`value` + `onChange`) con `name="estado"` y `name="miembro"`.
- `manejarCambio`: `onCambiar({ ...filtros, [name]: value })`.
- La opción `"todos"` significa "sin filtro".

## 4.6 `Resumen.jsx`
- Props: `resumen`, `estadoActivo`, `onElegirEstado`.
- Un botón por estado con el número y el nombre.
- Clic: `onElegirEstado(estadoActivo === estado ? 'todos' : estado)` (interruptor).
- Clase `activo` en el botón del estado elegido.

## 4.7 `ListaLibros.jsx`
- Props: `libros`, `miembros`, `onCambiarEstado`, `onQuitar`.
- Si `libros.length === 0`: devuelve el mensaje "Ningún libro coincide con los filtros elegidos." (**renderizado condicional**).
- `nombreDe(miembroId)`: busca el nombre del miembro.
- `libros.map(libro => <LibroItem key={libro.id} ... />)`.
- Pasa las funciones a cada `LibroItem` sin modificarlas.

## 4.8 `LibroItem.jsx`
- Props: `libro`, `nombreMiembro`, `onCambiarEstado`, `onQuitar` (**props de tipo función**).
- Muestra título, autor, quién lo propuso y la etiqueta de estado.
- Botones con `onClick={() => onCambiarEstado(libro.id)}` y `onClick={() => onQuitar(libro.id)}`.

## 4.9 `FormularioLibro.jsx`
- Constante `VACIO` con los valores iniciales.
- Función `validar(datos)` fuera del componente: devuelve `{ campo: mensaje }`.
- Estado local: `datos` (valores) y `errores` (mensajes).
- `manejarCambio`: `setDatos({ ...datos, [name]: value })`.
- `manejarEnvio`: `preventDefault`, valida, guarda errores, si no hay errores llama `onAgregar` con los textos sin espacios y limpia.
- `clase(campo, base)`: agrega `is-invalid` si ese campo tiene error.
- Campos: título, autor, "Propuesto por" (select de miembros) y estado inicial (select de `ESTADOS`).

---

# 5. Recorridos completos de interacciones

El profe va a pedir "desde el evento del usuario hasta la actualización de la interfaz, en ambas partes". Practica cada uno **en voz alta, con el archivo abierto, señalando la línea**.

## 5.1 Cambiar el estado de un libro (Parte A)

1. **Evento**: el usuario hace clic en "Cambiar estado" de una fila.
2. **Propagación**: el evento `click` nace en el `<button>` y burbujea: `button` → `td` → `tr` → `tbody`.
3. **Escucha**: en `main.js`, `tablaLecturas.addEventListener('click', ...)` es la única escucha de la tabla.
4. **Identificar el botón**: `evento.target.closest('button[data-accion]')`. Si se tocó otra cosa (no un botón), da `null` y se sale con `return`; el clic sigue subiendo y en `<main>` abre el detalle.
5. **Cortar propagación**: `evento.stopPropagation()`, porque la fila tiene `data-detalle` y sin esto se abriría el modal.
6. **Identificar el libro**: `boton.closest('tr').dataset.id`.
7. **Identificar la acción**: `boton.dataset.accion === 'cambiar-estado'`.
8. **Actualizar los datos**: `siguienteEstado(id)` en `lista.js` busca la lectura, calcula `(posicion + 1) % ESTADOS.length` y le asigna el nuevo estado.
9. **Actualizar la pantalla**: `actualizarEstadoFila(fila, lectura)` en `render.js` busca `.estado` dentro de esa fila y cambia `className` (color) y `textContent` (texto).
10. **Animación**: agrega `estado-cambiado`; CSS corre `@keyframes resaltar`; en `animationend` se quita la clase.

**Resultado**: solo cambió una etiqueta en el DOM. Ni la tabla ni la fila se redibujaron.

## 5.2 Cambiar el estado de un libro (Parte B)

1. **Evento**: clic en "Cambiar estado" de un `LibroItem`.
2. **Manejador**: `onClick={() => onCambiarEstado(libro.id)}`. `LibroItem` sabe su `libro.id` porque lo recibió por props.
3. **La función viene de arriba**: `onCambiarEstado` es `cambiarEstado` de `App`, que pasó por `ListaLibros` hasta `LibroItem`.
4. **Nuevo estado inmutable**: `cambiarEstado(id)` hace `libros.map(...)`. Para el libro con ese id devuelve `{ ...libro, estado: siguiente }` (objeto nuevo); los demás quedan igual.
5. **Avisar a React**: `setLibros(nuevoArreglo)`.
6. **Nuevo render**: React vuelve a ejecutar `App` con el `libros` nuevo.
7. **Derivados**: se recalculan `librosDelMiembro`, `librosFiltrados` y `resumen`.
8. **JSX**: `App` devuelve el árbol con los nuevos valores; `Resumen` recibe otros números y `ListaLibros` recibe la lista nueva.
9. **Reconciliación**: React compara con el render anterior usando las `key` y cambia en el DOM solo la etiqueta de ese libro y los números del resumen.
10. Si hay un filtro por estado activo, el libro **desaparece** de la lista (ya no cumple el filtro) y no hubo que escribir nada para eso.

**La diferencia en una frase**: en A le digo al navegador *qué cambiar*; en B le digo a React *cómo debe verse según los datos* y él calcula qué cambiar.

## 5.3 Buscar libros (Parte A)

1. El usuario escribe y presiona Enter o el botón "Buscar".
2. Si el input está vacío, el navegador bloquea el envío por el `required` (el formulario de búsqueda no tiene `novalidate`).
3. Se dispara `submit` en `#form-busqueda`. Escucha en `main.js`.
4. `evento.preventDefault()` evita la recarga.
5. `texto = textoBusqueda.value.trim()`. Si tiene menos de 2 letras (por ejemplo, solo espacios): alerta de error y `return`.
6. `mostrarEstado(estadoBusqueda, 'cargando')` → spinner.
7. `resultados.replaceChildren()` → borra la búsqueda anterior.
8. `await buscarLibros(texto)` en `api.js`: arma la URL con `encodeURIComponent`, hace `fetch`, revisa `respuesta.ok`, `await respuesta.json()`, convierte cada `doc` con `convertirLibro`.
9. De vuelta en `main.js`:
   - Arreglo vacío → `mostrarEstado(..., 'vacio', ...)`.
   - Con libros → `mostrarEstado(..., 'ninguno')` y `mostrarResultados(resultados, librosEncontrados)`, que hace `replaceChildren(...libros.map(crearTarjetaLibro))`.
   - Excepción (sin red o `!ok`) → `catch` → `mostrarEstado(..., 'error', ...)`.
10. Cada tarjeta carga su portada con `loading="lazy"`; si falla, el evento `error` pone `sin-portada.svg`.

## 5.4 Agregar un libro a la lista (Parte A)

1. Clic en "Agregar a mi lista" dentro de una tarjeta.
2. El evento burbujea: `button` → `div.card-body` → `article.libro` → `#resultados`.
3. Escucha en `#resultados`: `closest('[data-accion="agregar"]')` encuentra el botón.
4. `stopPropagation()`: el evento no llega a `<main>`, así que no se abre el detalle.
5. `boton.closest('.libro').dataset.id` → `buscarPorId(id)` en `librosEncontrados`.
6. `agregarALista(libro)` → `agregarLectura(libro)` en `lista.js`:
   - Ya estaba → `null` → alerta amarilla en `#aviso-lista`.
   - Nuevo → crea `{ ...libro, estado: 'Pendiente' }`, `push`, la devuelve.
7. `tablaLecturas.append(crearFilaLectura(lectura))`: la fila nace con `fila-entrando` → animación `aparecer`.
8. `actualizarMensajeListaVacia()` oculta "Todavía no agregaste libros.".

## 5.5 Quitar un libro (Parte A)

1. Clic en "Quitar" → burbujea hasta `tbody`.
2. `closest('button[data-accion]')`, `stopPropagation()`, `id` de la fila.
3. `boton.disabled = true` (evita doble clic).
4. `quitarLectura(id)`: `findIndex` + `splice` en el arreglo.
5. `quitarFilaConTransicion(fila, actualizarMensajeListaVacia)`:
   - Registra `transitionend` con `{ once: true }`.
   - Agrega `fila-saliendo` → `opacity: 0` y `transform: translateX(1rem)`.
   - La `transition` de `.fila-lectura` anima el cambio durante 0.35 s.
6. Al terminar: `fila.remove()` y `actualizarMensajeListaVacia()` (si era el último, aparece el mensaje).

## 5.6 Validar un campo mientras se escribe (Parte A)

1. El usuario escribe en "Año": `2030`.
2. Se dispara `input` en el campo y burbujea hasta el `<form>`.
3. Escucha en `iniciarFormulario`: `evento.target.matches('input, select, textarea')` → `validarCampo(evento.target)`.
4. `campo.setCustomValidity(reglasExtra(campo))`: `reglasExtra` ve `name === 'anio'` y `2030 > 2026` → devuelve "El año de publicación no puede ser futuro." → el campo queda con `customError`.
5. `campo.checkValidity()` → `false`.
6. `feedback = campo.parentElement.querySelector('.invalid-feedback')`.
7. `feedback.textContent = mensajeDeError(campo)`: como `validity.customError` es `true`, devuelve `campo.validationMessage` (el mensaje propio).
8. `classList.toggle('is-invalid', true)` → borde rojo y Bootstrap muestra el feedback.
9. Si corrige a `1990`, `reglasExtra` devuelve `''`, `setCustomValidity('')` limpia, `checkValidity()` da `true`, se borra el mensaje y se pone `is-valid`.

## 5.7 Enviar la propuesta (Parte A)

1. Clic en "Enviar propuesta" → `submit` del formulario → `manejarEnvio`.
2. `preventDefault()` y limpiar alerta anterior.
3. `validarFormulario()` recorre todos los campos (incluidos autores agregados) con `map(validarCampo)`. Si alguno falla: foco al primer `.is-invalid` y `return`.
4. Botón deshabilitado con texto "Enviando...".
5. `leerDatos()`: `new FormData(formulario)`, `getAll('autor')` para todos los autores, `Number()` para año y páginas.
6. `await enviarPropuesta(datos)` en `api.js`: `fetch` con `POST`, `Content-Type: application/json` y `JSON.stringify`. Revisa `ok`.
7. Éxito: alerta verde con `respuesta.id` y `reiniciarFormulario()`.
8. Error: alerta roja y `console.warn`.
9. `finally`: rehabilitar el botón.

## 5.8 Filtrar desde el Resumen (Parte B)

1. Clic en el botón "Pendiente" de `Resumen`.
2. `onClick={() => onElegirEstado(...)}`: como no estaba activo, manda `'Pendiente'`.
3. `onElegirEstado` es `elegirEstado` de `App` → `setFiltros({ ...filtros, estado: 'Pendiente' })`.
4. `App` se vuelve a dibujar con `filtros.estado = 'Pendiente'`.
5. `librosFiltrados` ahora solo tiene pendientes → `ListaLibros` muestra esos.
6. `Filtros` recibe `filtros` nuevos → su `<select value={filtros.estado}>` muestra "Pendiente" **sin que `Filtros` haga nada**.
7. `Resumen` recibe `estadoActivo = 'Pendiente'` → ese botón lleva la clase `activo`.

Esto demuestra que dos hermanos (`Resumen` y `Filtros`) se sincronizan por el estado elevado.

## 5.9 Agregar un libro (Parte B)

1. El usuario escribe en "Título": cada tecla dispara `onChange` → `manejarCambio` → `setDatos({ ...datos, titulo: valor })` → el input se vuelve a dibujar con el valor del estado.
2. Clic en "Agregar libro" → `onSubmit={manejarEnvio}`.
3. `preventDefault()`, `validar(datos)` → objeto de errores → `setErrores(...)`.
4. Si hay errores: `return`; en el render, cada campo con error recibe `is-invalid` y su mensaje.
5. Sin errores: `onAgregar({ ...datos, titulo: ..., autor: ... })` → es `agregarLibro` de `App` → `setLibros([...libros, { ...datos, id: crypto.randomUUID() }])`.
6. `setDatos(VACIO)` limpia el formulario.
7. `App` se vuelve a dibujar: el libro aparece en la lista (si cumple los filtros) y el resumen suma uno.

---

# 6. Los 13 criterios técnicos: qué decir y dónde mostrarlo

Para cada criterio: **en 30 segundos** (lo que dices si te preguntan en general), **dónde está** (lo que abres) y **preguntas trampa**.

## Criterio 1. Manipulación del DOM y generación dinámica

**En 30 segundos:** "Todo lo dinámico se genera desde arreglos con `createElement`, `textContent` y `append`. Nunca uso `innerHTML` con datos de la API para evitar XSS. Las funciones de renderizado son reutilizables y las actualizaciones son precisas: al cambiar un estado solo toco esa etiqueta."

**Dónde:** `render.js`: `crear`, `crearTarjetaLibro`, `mostrarResultados` (`libros.map(crearTarjetaLibro)`), `crearFilaLectura`, `actualizarEstadoFila`.

**Trampas:**
- *¿`replaceChildren` no es como `innerHTML`?* No: recibe nodos, no texto HTML. No interpreta nada.
- *¿Y si un título trae `<b>`?* Se ve el texto `<b>` literal, porque `textContent` no interpreta HTML.
- *¿Dónde está lo de "actualización precisa"?* `actualizarEstadoFila` cambia una etiqueta; `agregarALista` hace `append` de una fila; quitar elimina una fila. Nunca se reconstruye la tabla completa.
- *¿Por qué al buscar sí reemplazas todo el estante?* Porque es una búsqueda nueva: todos los resultados cambian.

## Criterio 2. Eventos, propagación y delegación

**En 30 segundos:** "Uso delegación: una escucha por contenedor (`<main>`, `#resultados` y `tbody`). Identifico el origen con `closest()` y la acción con `data-accion`. Corto la propagación con `stopPropagation()` en los botones que están dentro de elementos que abren el detalle."

**Dónde:** `main.js`, las tres escuchas de clic. Y `formulario.js`: escucha de `input` en el form y de `click` en `#lista-autores`.

**Trampas:**
- *¿`target` vs `currentTarget`?* `target` es donde se hizo clic (puede ser un hijo); `currentTarget` es el elemento que tiene la escucha (el contenedor).
- *¿Qué pasa si quitas el `stopPropagation` de Agregar?* Se agrega el libro **y** se abre el modal, porque el clic llega a `<main>`.
- *¿Por qué no usaste `onclick` en el HTML?* La rúbrica lo pone como deficiente, mezcla HTML con JS y solo permite una función por elemento.
- *¿Por qué no funcionó `stopPropagation` con `data-bs-toggle`?* Porque Bootstrap escucha en fase de captura en `document`, que ocurre antes de la burbuja.
- *¿Hay escuchas duplicadas?* No. Ninguna escucha se registra al crear elementos (las únicas son las `{ once: true }` de animación, que se borran solas y son de cada fila, no del clic).

## Criterio 3. Validación de formularios

**En 30 segundos:** "Combino atributos HTML5 con la API de validación de restricciones. `novalidate` apaga los mensajes del navegador; con `validity` sé qué regla falla y pongo un mensaje propio en el `invalid-feedback` de cada campo. Las reglas de año no futuro y fecha no pasada se agregan con `setCustomValidity`."

**Dónde:** `index.html` (atributos) y `formulario.js` (`reglasExtra`, `mensajeDeError`, `validarCampo`, `validarFormulario`).

**Trampas:**
- *¿Si el form tiene `novalidate`, se valida?* Sí, `checkValidity()` y `validity` siguen funcionando; solo se apagan los globitos y el bloqueo automático del envío.
- *¿Por qué `setCustomValidity('')` cada vez?* Si no se limpia, el campo queda inválido para siempre.
- *¿Por qué el `max` del año no está en el HTML?* Porque cambia cada año. Lo pone `iniciarFormulario`.
- *¿Por qué no `toISOString()`?* Usa UTC; en Costa Rica después de las 6 p. m. da el día siguiente.

## Criterio 4. Campos dinámicos

**En 30 segundos:** "Los autores se agregan clonando el primero con `cloneNode(true)`, así heredan las mismas reglas. Respeto mínimo 1 y máximo 5 deshabilitando los botones y revisando en las funciones. Todos se llaman `autor`, entonces la validación los encuentra y `FormData.getAll('autor')` los envía todos."

**Dónde:** `formulario.js`: `agregarCampoAutor`, `quitarCampoAutor`, `actualizarBotonesAutor`, `leerDatos`.

**Trampas:**
- *¿El clon trae las escuchas?* No, `cloneNode` no copia escuchas. No hace falta porque se usa delegación.
- *¿Qué pasa si clonas un autor con error?* Se limpian valor, clases y mensaje del clon.
- *¿Cómo verificas que se envían?* En Network → petición POST → Payload: `autores` es un arreglo con todos.

## Criterio 5. Comunicación asincrónica con fetch

**En 30 segundos:** "La búsqueda y el envío usan `fetch` con `async/await`. `api.js` revisa `response.ok` y lanza un error si no; quien llama tiene `try/catch`. La búsqueda distingue cargando, error, vacío y resultados. Todo sin recargar gracias a `preventDefault`."

**Dónde:** `api.js` (`buscarLibros`, `enviarPropuesta`), `main.js` (submit) y `formulario.js` (`manejarEnvio`).

**Trampas:**
- *¿`fetch` lanza error con un 404?* No. Solo con error de red. Por eso se revisa `ok`.
- *¿`await` bloquea la página?* No, solo pausa esa función. El resto de la página sigue respondiendo.
- *¿Qué devuelve una función `async`?* Siempre una promesa.
- *¿Qué es CORS y por qué funciona con Open Library?* Es la política que decide si una página puede leer respuestas de otro dominio. Open Library y JSONPlaceholder envían el encabezado `Access-Control-Allow-Origin: *`, que permite leerlas desde cualquier origen.
- *¿Cómo pruebas el estado de error?* Network → Offline.

## Criterio 6. Transiciones y animaciones

**En 30 segundos:** "Las animaciones están en CSS. JavaScript solo agrega y quita clases: `fila-entrando` al agregar, `estado-cambiado` al cambiar y `fila-saliendo` al quitar. Al quitar, espero `transitionend` y recién ahí hago `remove()`."

**Dónde:** `_animaciones.scss` y `render.js` (`crearFilaLectura`, `actualizarEstadoFila`, `quitarFilaConTransicion`).

**Trampas:**
- *¿Por qué `{ once: true }`?* `transitionend` se dispara una vez por propiedad (opacity y transform).
- *¿Qué pasa si quitas la fila sin esperar?* Desaparece de golpe, sin animación.
- *¿Por qué no `elemento.style.opacity = 0`?* Son estilos en línea; la rúbrica los pone en Regular. Además mezcla presentación con lógica.

## Criterio 7. Organización, depuración y rendimiento

**En 30 segundos:** "Hay cinco módulos ES con una responsabilidad cada uno; `api.js` y `lista.js` no tocan el DOM. La consola queda sin errores. En `depuracion.md` documenté el error real del botón Agregar con breakpoints, el análisis de red y la optimización de importar solo los parciales de Bootstrap, medida antes y después."

**Dónde:** carpeta `js/`, `docs/depuracion.md`, historial de git (commits 9 y 10).

**Trampas:**
- *¿Cómo pones un breakpoint?* Sources → archivo → clic en el número de línea. También se puede escribir `debugger;` en el código.
- *¿Qué hace F8, F10, F11?* Continuar, pasar a la siguiente línea, entrar a la función.
- *¿Qué mediste?* Tamaño de `main.css` antes y después, cantidad de peticiones y peso total.

## Criterio 8. Bootstrap: grilla, componentes y utilidades

**En 30 segundos:** "Uso la grilla con `row` y `col-12 col-lg-8 / col-lg-4`, más columnas con `sm` y `md` en el formulario. Están todos los componentes pedidos: navbar, tarjetas, modal, formulario, alerta y tabla. Uso utilidades para márgenes, display y flex en vez de clases propias."

**Dónde:** `index.html` y las clases en `render.js`.

**Trampas:**
- *¿Cuántas columnas tiene la grilla?* 12.
- *¿Qué hace `g-4`?* El espacio entre columnas (gutter).
- *¿Qué pasa con `col-lg-8` en un celular?* No aplica (es desde 992 px); aplica `col-12`.

## Criterio 9. Sass, personalización y diseño adaptable

**En 30 segundos:** "Personalizo Bootstrap con variables de origen definidas antes de importarlo, aprovechando `!default`. Organizo parciales con variable, mixin, función y anidamiento BEM. El componente `.estante` usa CSS Grid, variables CSS, rem y es mobile-first con `min-width`."

**Dónde:** `scss/` completo.

**Trampas:**
- *¿Qué es `!default`?* "Usa este valor solo si la variable no tiene uno".
- *¿Qué pasa si importas tus variables después de Bootstrap?* Bootstrap ya generó el CSS con sus valores; tus colores no se aplicarían a los componentes.
- *¿Variable Sass vs variable CSS?* La de Sass desaparece al compilar; la de CSS existe en el navegador y se puede cambiar en tiempo real (por eso `--columnas` cambia en cada media query).
- *¿Función vs mixin?* La función devuelve un valor; el mixin inserta reglas.

## Criterio 10. Componentes y props en React

**En 30 segundos:** "Siete componentes funcionales en carpetas, cada uno con una responsabilidad. `Panel` recibe `children`. `Filtros`, `Resumen`, `ListaLibros`, `LibroItem` y `FormularioLibro` reciben funciones por props para avisar cambios a `App`. Ningún componente usa `querySelector` ni `getElementById`."

**Dónde:** `src/components/`.

**Trampas:**
- *¿Qué es `children`?* Lo que se escribe entre la etiqueta de apertura y cierre del componente.
- *¿Por qué `onClick={() => fn(id)}` y no `onClick={fn(id)}`?* La segunda ejecuta la función al dibujar.
- *¿Un hijo puede cambiar las props?* No; las props son de solo lectura. Para cambiar algo, el hijo llama a una función del padre.

## Criterio 11. Estado, eventos y renderizado de listas

**En 30 segundos:** "El estado está en `useState` y se actualiza de forma inmutable con `map`, `filter` y spread. El resumen y la lista filtrada se calculan en cada render, no son estado. Las listas usan `key={libro.id}` y hay renderizado condicional cuando el filtro no deja libros."

**Dónde:** `App.jsx` y `ListaLibros.jsx`.

**Trampas:**
- *¿Por qué no `push`?* React compara referencias; mismo arreglo = no detecta cambio.
- *¿Por qué no el índice como key?* Al quitar uno se corren los índices y React asocia mal los elementos (puede dejar estados o inputs pegados al elemento equivocado).
- *¿El `forEach` de `resumen` no es mutación?* Es una variable local recreada en cada render, no estado.

## Criterio 12. Formularios controlados y comunicación entre componentes

**En 30 segundos:** "`FormularioLibro` es controlado: `value` del estado y `onChange` que lo actualiza. Valida con una función que devuelve un objeto de errores y muestra cada mensaje bajo su campo. Los filtros están elevados a `App`, el ancestro común de `Filtros` y `Resumen`, y por eso se sincronizan."

**Dónde:** `FormularioLibro.jsx`, `Filtros.jsx`, `Resumen.jsx` y `App.jsx`.

**Trampas:**
- *¿Por qué el estado del formulario no está en App?* Porque nadie más lo necesita. El estado va lo más abajo posible.
- *¿Qué pasa si `Filtros` tuviera su propio `useState`?* `Resumen` no se enteraría y la lista tampoco.

## Criterio 13. Repositorio y documentación

**En 30 segundos:** "El README explica instalación, compilación de Sass, ejecución de las dos partes y tiene la sección 'Del DOM manual a React' con referencias a archivos. Está la etiqueta `tarea-2` y 21 commits en seis días con mensajes descriptivos."

**Dónde:** GitHub: README, pestaña Tags, `git log --oneline`.

**Trampas:**
- *¿Qué es una etiqueta?* Un nombre fijo para un commit; no se mueve como una rama.
- *¿Por qué no subes `node_modules`?* Se regenera con `npm install` desde `package-lock.json`.
- *¿Por qué no subes `css/main.css`?* Es generado; se crea con `npm run sass`.

---

# 7. Decisiones de diseño, alternativas y limitaciones

El criterio 16 pide defender las decisiones **frente a alternativas** y **reconocer limitaciones**. Para cada decisión, ten lista la frase "elegí X en vez de Y porque Z, y el costo es W".

## 7.1 Organización de módulos (Parte A)

| Decisión | Alternativa | Por qué la mía | Costo o limitación |
|---|---|---|---|
| Separar por responsabilidad: api, lista, render, formulario, main | Un solo `app.js` | Cada archivo se entiende solo; la consigna lo pide; un cambio en la API no toca el DOM | Hay que seguir imports entre archivos |
| | Separar por pantalla (buscador.js, lista.js) | La consigna pide separar acceso a API, renderizado, formulario y estado | — |
| `api.js` lanza errores y no los atrapa | `try/catch` dentro de `api.js` | Quien llama decide qué mostrar; `api.js` no conoce la interfaz | Quien llame **tiene** que acordarse del `try/catch` |
| `lista.js` encapsula el arreglo | Exportar el arreglo | Nadie puede agregar duplicados o romper el formato por fuera | Un poco más de código (funciones para todo) |
| El estado en un módulo y no en el DOM | Leer el estado desde el texto de las etiquetas | Una sola fuente de verdad | Hay que mantener sincronizados datos y DOM a mano (justo lo que React resuelve) |
| `formulario.js` maneja sus propios eventos | Que `main.js` registre todo | El formulario es una pieza independiente; `main.js` queda más corto | `main.js` ya no es el único lugar con escuchas |
| `render.js` usa ids del modal | Pasar los elementos como parámetros | Más simple | Si cambia el HTML del modal, hay que cambiar `render.js` |

## 7.2 Eventos

| Decisión | Alternativa | Por qué la mía | Costo |
|---|---|---|---|
| Delegación en contenedores | Una escucha por botón | Funciona con elementos creados después; menos escuchas; no hay que limpiarlas | Hay que identificar el origen con `closest` |
| `data-accion` para saber la acción | Leer el texto del botón o las clases | No depende del idioma ni del estilo | Un atributo extra en cada botón |
| Modal abierto desde una escucha propia en `<main>` | `data-bs-toggle` de Bootstrap | Con `data-bs-toggle` el `stopPropagation` no sirve (Bootstrap escucha en captura) | Un poco más de código |
| `submit` y no `input` para buscar | Buscar mientras se escribe | Lo pide la consigna; no se hacen decenas de peticiones | El usuario tiene que presionar Enter |

## 7.3 Bootstrap y Sass

| Decisión | Alternativa | Por qué la mía | Costo |
|---|---|---|---|
| Bootstrap con npm + Sass | Bootstrap por CDN | Con CDN no se pueden cambiar las variables de origen | Hay que compilar |
| Variables de origen | Sobrescribir con `!important` o clases propias | El CSS se genera correcto desde el inicio; sin peleas de especificidad | Hay que respetar el orden de imports |
| Solo parciales usados | Bootstrap completo | ~24 % menos CSS | Si agregas un componente nuevo de Bootstrap, hay que importar su parcial |
| Componente propio con Grid | Solo la grilla de Bootstrap (`row-cols-*`) | La consigna pide un componente propio con Grid/Flexbox, variables CSS y mobile-first | Dos sistemas de layout en la página |
| `@import` | `@use` / `@forward` (lo moderno de Sass) | Bootstrap 5 está hecho con `@import` y las variables tienen que compartirse | Sass marca `@import` como obsoleto (se silencia el aviso) |
| `bootstrap.bundle.min.js` completo | Importar solo Modal y Collapse | La Parte A no tiene empaquetador | Se descarga JS que no se usa |

## 7.4 Componentes y estado (Parte B)

| Decisión | Alternativa | Por qué la mía | Costo |
|---|---|---|---|
| Estado de `libros` y `filtros` en `App` | Estado en cada componente | Lo usan varios hermanos; el ancestro común es `App` | `App` concentra la lógica |
| Estado del formulario dentro de `FormularioLibro` | En `App` | Nadie más lo necesita; `App` no se redibuja por cada tecla | — |
| Resumen y lista filtrada derivados | Guardarlos en `useState` | Imposible que se desincronicen | Se recalculan en cada render (insignificante con pocos datos) |
| Props para pasar datos | Context API | Solo hay dos niveles; Context sería complicar | Si el árbol crece mucho, pasar props se vuelve incómodo ("prop drilling") |
| `crypto.randomUUID()` como id | `libros.length + 1` o `Date.now()` | Único siempre | Ids largos (no importa) |
| Validación propia en JS | Atributos HTML5 como en la Parte A | La consigna pide validación en el cliente con mensajes por campo, y en React lo natural es validar desde el estado | Más código que un `required` |
| `Panel` con `children` | Repetir la tarjeta en cada lugar | Reutilizable | — |
| `LibroItem` aparte de `ListaLibros` | Todo en `ListaLibros` | Cada uno hace una cosa: la lista decide qué mostrar, el ítem cómo se ve un libro | Un archivo más |

## 7.5 Limitaciones que debes reconocer (sin que te las pregunten, suma)

1. La lista de la Parte A se pierde al recargar (la consigna no pide persistencia; se resolvería con `localStorage`).
2. Los datos de la Parte B también vuelven al inicio al recargar.
3. JSONPlaceholder no guarda nada; solo simula.
4. Si la API de Open Library está lenta, la búsqueda tarda (no hay tiempo límite ni cancelación de la búsqueda anterior).
5. Si haces dos búsquedas seguidas muy rápido, podría mostrarse la que llegue última aunque no sea la última que pediste.
6. Las tarjetas abren el detalle con el mouse, pero no con el teclado (un `<article>` no recibe foco). Se mejoraría con un botón "Ver detalle".
7. Los estilos de la Parte B dependen de la carpeta de la Parte A (se importan con rutas relativas).
8. Bootstrap sigue pesando aunque se importe parcialmente.

## 7.6 Qué cambia al pasar del DOM manual a React (la pregunta central del 16)

Dilo así, con el README abierto:

1. **En la Parte A los datos y la pantalla son dos cosas separadas** que yo sincronizo. Para cambiar un estado necesito: cambiar el dato (`siguienteEstado`), encontrar la fila (`data-id`, `closest`), cambiar el DOM (`actualizarEstadoFila`) y usar delegación porque las filas se crean después.
2. **En la Parte B solo cambio el dato** (`setLibros` con un arreglo nuevo). React vuelve a ejecutar los componentes y calcula qué cambiar en el DOM.
3. **Deja de ser necesario:** buscar elementos, `data-id` y `closest`, delegación manual, actualizar a mano el resumen o el contador.
4. **Aparecen reglas nuevas:** no mutar el estado, `key` en las listas, no tocar el DOM desde los componentes, pensar dónde vive el estado.
5. **Lo que no cambia:** la regla de negocio (el siguiente estado sale de `ESTADOS` con `%`).

---

# 8. Modificaciones en caliente paso a paso

Antes de cualquier modificación:

```powershell
git switch -c defensa
```

Deja corriendo en una terminal `npm run sass:watch` (en `buscador-js`) y en otra `npm start`. En una tercera, `npm run dev` (en `tablero-react`). Así los cambios se ven al recargar.

Para cada modificación: **dónde**, **qué escribir**, **por qué respeta el modelo** y **cómo demostrarlo**.

## 8.1 Agregar un estado nuevo en las dos partes ("Abandonado")

**Parte A**

1. `buscador-js/js/lista.js`:
   ```js
   export const ESTADOS = ['Pendiente', 'Leyendo', 'Leído', 'Abandonado'];
   ```
2. `buscador-js/scss/_componentes.scss`, dentro de `.estado { ... }`, después de `&--leido`:
   ```scss
     &--abandonado {
       background-color: $danger;
     }
   ```
3. Recarga (si el watch está corriendo, ya compiló).

**Parte B**

1. `tablero-react/src/constantes.js`:
   ```js
   export const ESTADOS = ['Pendiente', 'Leyendo', 'Leído', 'Abandonado'];
   ```
2. Nada más: el color viene del mismo `_componentes.scss`; el resumen, el filtro y el select del formulario recorren `ESTADOS`.

**Por qué respeta el modelo:** en A no se tocó el DOM ni las escuchas; la delegación ya maneja el botón. En B el estado se sigue actualizando con `map`.

**Demostrar:** en A, agrega un libro y cámbiale el estado 4 veces: pasa por Abandonado y vuelve a Pendiente. En B, el resumen muestra 4 botones y el filtro tiene la opción.

**Si preguntan "¿y si quisieran elegir el estado en vez de ciclar?":** en A, reemplazar el botón por un `<select>` en `crearFilaLectura` y escuchar `change` en el `tbody` (también burbujea). En B, un `<select>` en `LibroItem` con `onChange={(e) => onCambiarEstado(libro.id, e.target.value)}` y que `cambiarEstado` reciba el estado.

## 8.2 Agregar una regla de validación con mensaje propio (Parte A)

Todas van en `reglasExtra(campo)` de `formulario.js`, **antes** del `return '';` final. `validarCampo` ya llama a `setCustomValidity` con lo que devuelva.

**Ejemplo 1: el título no puede ser solo números**
```js
  if (campo.name === 'titulo' && /^\d+$/.test(campo.value.trim())) {
    return 'El título no puede tener solo números.';
  }
```

**Ejemplo 2: el correo tiene que ser de un dominio específico**
```js
  if (campo.name === 'correo' && campo.value && !campo.value.endsWith('@ucenfotec.ac.cr')) {
    return 'Use su correo de la universidad (@ucenfotec.ac.cr).';
  }
```

**Ejemplo 3: el motivo debe tener al menos 5 palabras**
```js
  if (campo.name === 'motivo' && campo.value.trim().split(/\s+/).length < 5) {
    return 'Explique el motivo con al menos 5 palabras.';
  }
```

**Ejemplo 4: la sesión debe ser como máximo en 6 meses**
```js
  if (campo.name === 'fecha' && campo.value) {
    const limite = new Date();
    limite.setMonth(limite.getMonth() + 6);
    if (new Date(campo.value + 'T00:00') > limite) {
      return 'La sesión no puede ser dentro de más de 6 meses.';
    }
  }
```
(Se agrega `'T00:00'` para que la fecha se lea en hora local.)

**Ejemplo 5: regla con atributo, sin JavaScript.** Si piden "máximo 1000 páginas", basta con cambiar `max="3000"` por `max="1000"` en `index.html`. El mensaje "El valor máximo es 1000." ya sale de `mensajeDeError` (`rangeOverflow`).

**Ejemplo 6: un autor no puede repetirse.** Esta compara campos entre sí:
```js
  if (campo.name === 'autor' && campo.value.trim()) {
    const iguales = [...formulario.querySelectorAll('[name="autor"]')]
      .filter((otro) => otro.value.trim().toLowerCase() === campo.value.trim().toLowerCase());
    if (iguales.length > 1) return 'Este autor ya está en la lista.';
  }
```

**Demostrar:** escribe un valor que rompa la regla y muestra el mensaje bajo el campo; corrígelo y se pone verde; intenta enviar con la regla rota y no se envía.

## 8.3 Mostrar un dato adicional de la API (Parte A)

Campos útiles de Open Library: `number_of_pages_median` (páginas), `publisher` (arreglo de editoriales), `subject` (temas), `ratings_average` (calificación)... Para ver todos: abre en el navegador `https://openlibrary.org/search.json?q=tolkien&limit=1`.

**Ejemplo: páginas en el modal y en la tarjeta**

1. `api.js`, dentro de `convertirLibro`, agrega una propiedad:
   ```js
       paginas: doc.number_of_pages_median ?? 'Sin dato',
   ```
2. **En el modal** (`render.js`, arreglo `datos` de `mostrarDetalle`):
   ```js
       ['Páginas', libro.paginas],
   ```
3. **En la tarjeta** (`render.js`, `crearTarjetaLibro`), después de la línea de `anio`:
   ```js
     const paginas = crear('p', 'card-text small text-muted', `Páginas: ${libro.paginas}`);
   ```
   y cambia `cuerpo.append(titulo, autores, anio, boton);` por:
   ```js
     cuerpo.append(titulo, autores, anio, paginas, boton);
   ```

**Ejemplo: editorial (primer elemento de un arreglo)**
```js
    editorial: doc.publisher?.[0] ?? 'Sin dato',
```
y en `datos`: `['Editorial', libro.editorial],`

**Por qué respeta el modelo:** el dato se agrega en `api.js` (conversión) y se muestra con `textContent` en `render.js`. No hay `innerHTML`.

## 8.4 Agregar un filtro en React con estado elevado

**Ejemplo: buscar por título**

1. `App.jsx`, el estado inicial de filtros:
   ```jsx
   const [filtros, setFiltros] = useState({ estado: 'todos', miembro: 'todos', texto: '' });
   ```
2. `App.jsx`, `librosFiltrados`:
   ```jsx
   const librosFiltrados = librosDelMiembro.filter(
     (libro) => (filtros.estado === 'todos' || libro.estado === filtros.estado)
       && libro.titulo.toLowerCase().includes(filtros.texto.toLowerCase())
   );
   ```
3. `Filtros.jsx`, dentro del `<div className="row g-2">`, al final:
   ```jsx
   <div className="col-12">
     <label htmlFor="filtro-texto" className="form-label small">Título</label>
     <input id="filtro-texto" name="texto" className="form-control"
       value={filtros.texto} onChange={manejarCambio} />
   </div>
   ```

`manejarCambio` ya sirve porque usa `name`. El filtro **no** se guarda en `Filtros`: queda en `App` (estado elevado).

**Ejemplo: filtro por autor con un select** (si piden otro):
1. Estado: agregar `autor: 'todos'`.
2. En `App`, crear la lista de autores derivada: `const autores = [...new Set(libros.map((l) => l.autor))];` y pasarla a `Filtros` como prop `autores={autores}`.
3. Condición: `&& (filtros.autor === 'todos' || libro.autor === filtros.autor)`.
4. En `Filtros`, recibir `autores` y agregar un `<select name="autor">` igual a los otros.

**Si piden que el resumen también respete el nuevo filtro:** calcular `resumen` sobre una lista filtrada por miembro **y** por el nuevo filtro (pero no por estado), igual que ahora con `librosDelMiembro`.

## 8.5 Cambiar una variable de origen de Bootstrap y recompilar

1. `buscador-js/scss/_variables.scss`, por ejemplo:
   ```scss
   $primary: #6f42c1;
   ```
   o `$border-radius: 0;` (todo cuadrado), o `$font-family-sans-serif: "Courier New", monospace;`.
2. En la terminal, dentro de `buscador-js`: `npm run sass` (o deja corriendo `npm run sass:watch`).
3. Recarga: cambian navbar, botones, enlaces, spinner, badge "Leyendo"...
4. En la Parte B, Vite lo detecta solo porque importa el mismo archivo.

**Explicar:** el valor llega antes que el `!default` de Bootstrap, entonces Bootstrap genera todo con él. Muestra en `css/main.css` que `--bs-primary` cambió.

**Variante: cambiar un punto de corte.** Por ejemplo que `lg` empiece en 1100 px:
```scss
$grid-breakpoints: (
  xs: 0,
  sm: 576px,
  md: 768px,
  lg: 1100px,
  xl: 1200px,
  xxl: 1400px
);
```
Va en `_variables.scss`. Afecta la grilla **y** el mixin `desde(lg)` del estante, porque los dos usan `$grid-breakpoints`.

## 8.6 Otras modificaciones posibles

**Cambiar el máximo de autores a 3:** `formulario.js` → `const MAX_AUTORES = 3;`.

**Mostrar cuántos libros hay en la lista (Parte A):**
1. `index.html`: en el título de la tarjeta, `<h2 class="h5 mb-0">Mi lista de lecturas (<span id="contador-lista">0</span>)</h2>`.
2. `main.js`: `const contadorLista = document.getElementById('contador-lista');` y dentro de `actualizarMensajeListaVacia()` agrega `contadorLista.textContent = cantidadLecturas();`.
3. Como esa función ya se llama al agregar y al terminar de quitar, el contador queda bien. (Explica que en React esto saldría solo con `libros.length`.)

**Ordenar la lista de React por título:** en `App.jsx`, derivar sin mutar:
```jsx
const librosOrdenados = [...librosFiltrados].sort((a, b) => a.titulo.localeCompare(b.titulo));
```
y pasar `librosOrdenados` a `ListaLibros`. **Ojo:** `sort` muta el arreglo, por eso se copia con `[...]` antes.

**Agregar un campo al formulario de React (por ejemplo, año):**
1. `VACIO`: agregar `anio: ''`.
2. `validar`: `if (!datos.anio || Number(datos.anio) > new Date().getFullYear()) errores.anio = 'Año inválido.';`
3. JSX: un `<input name="anio" type="number" className={clase('anio')} value={datos.anio} onChange={manejarCambio} />` con su `invalid-feedback`.
4. Opcional: mostrarlo en `LibroItem`.

**Botón "Quitar todos" (React):** en `App`, `function vaciar() { setLibros([]); }` y un botón que la llame. Es inmutable porque se pasa un arreglo nuevo.

**Botón "Quitar todos" (Parte A):** un botón en el HTML con `id`, una escucha en `main.js` que recorra las filas y use `quitarFilaConTransicion` en cada una, y en `lista.js` una función `vaciarLecturas()` que haga `lecturas.length = 0`.

## 8.7 Cómo actuar durante la modificación

1. **Di en voz alta dónde vas a tocar y por qué** antes de escribir. Eso ya demuestra que sabes ubicarte (criterio 17: "ubica con rapidez").
2. Haz un cambio a la vez y prueba.
3. Ten la consola abierta. Si sale un error rojo, lee el archivo y la línea que indica.
4. Al terminar, **demuestra** que funciona y que lo demás sigue funcionando (agrega, cambia estado y quita un libro).
5. Si se rompe algo y no sabes qué fue: `git diff` muestra lo que cambiaste; `git restore archivo` lo deshace.

---

# 9. Banco de preguntas con respuestas

Practica respondiendo en voz alta **antes** de leer la respuesta.

## 9.1 JavaScript y el DOM

**¿Qué es el DOM?**
La representación en memoria del HTML como un árbol de objetos. JavaScript lo lee y lo modifica, y el navegador redibuja lo que cambió.

**¿`textContent` vs `innerHTML` vs `innerText`?**
`textContent` pone o lee texto plano sin interpretar. `innerHTML` interpreta el texto como HTML (riesgo de XSS). `innerText` es parecido a `textContent` pero toma en cuenta el CSS (no lee texto oculto) y es más lento.

**¿Qué es XSS?**
Cross-site scripting: inyectar código en una página a través de datos que la página muestra como HTML. Si el título de un libro trajera un `<script>` o un `<img onerror=...>` y se metiera con `innerHTML`, se ejecutaría.

**¿`append` vs `appendChild`?**
`append` acepta varios nodos y también texto; `appendChild` acepta uno solo y devuelve el nodo. Uso `append` porque meto varios hijos a la vez.

**¿Qué hace `dataset`?**
Da acceso a los atributos `data-*`. `elemento.dataset.id` lee o escribe `data-id`. Los nombres con guiones pasan a camelCase: `data-bs-toggle` ↔ `dataset.bsToggle`.

**¿Qué hace `closest()`?**
Busca hacia arriba (el mismo elemento y sus ancestros) el primero que coincida con el selector. Devuelve `null` si no hay.

**¿`querySelector` vs `getElementById`?**
`getElementById` busca solo por id y es un poco más rápido; `querySelector` acepta cualquier selector CSS. Los dos devuelven el primer elemento o `null`.

**¿`querySelectorAll` devuelve un arreglo?**
No, un `NodeList`. Tiene `forEach` pero no `map` ni `filter`. Por eso uso `[...campos]` para convertirlo.

**¿`const` permite modificar un arreglo?**
Sí. `const` impide reasignar la variable (`lecturas = otro`), no modificar el contenido (`lecturas.push(x)`).

**¿Qué es `??`?**
Operador de coalescencia nula: si lo de la izquierda es `null` o `undefined`, usa lo de la derecha. A diferencia de `||`, no reemplaza `0` ni `''`.

**¿Qué es `?.`?**
Encadenamiento opcional: si lo de la izquierda es `null` o `undefined`, devuelve `undefined` en vez de lanzar un error.

**¿Qué es el spread `...`?**
"Expande" un arreglo u objeto. `[...a, x]` crea un arreglo nuevo con todo `a` y `x`. `{ ...obj, clave: v }` copia `obj` y cambia `clave`. `fn(...arr)` pasa cada elemento como argumento.

**¿Qué es un módulo ES?**
Un archivo con su propio alcance que exporta lo que quiere compartir (`export`) e importa lo que necesita (`import`). Se carga con `type="module"`.

**¿Por qué los módulos no funcionan con doble clic en el HTML?**
Los módulos se cargan con las reglas de CORS, y una página abierta como `file://` no tiene un origen válido. Por eso se usa un servidor local (`serve`).

## 9.2 Eventos

**¿Cuáles son las fases de un evento?**
Captura (baja de `window` al elemento), objetivo (llega al elemento) y burbuja (sube de vuelta). `addEventListener` escucha en burbuja por defecto; con `{ capture: true }` o `true` como tercer parámetro, en captura.

**¿Qué hace `stopPropagation()`?**
Detiene el recorrido del evento hacia los siguientes elementos. No cancela lo que ya pasó ni la acción por defecto.

**¿Y `stopImmediatePropagation()`?**
Además de lo anterior, impide que se ejecuten **otras escuchas del mismo elemento** registradas después.

**¿Y `preventDefault()`?**
Cancela la acción por defecto del navegador: enviar el formulario y recargar, seguir un enlace, marcar un checkbox, etc.

**¿Todos los eventos burbujean?**
No. `focus`, `blur`, `mouseenter`, `mouseleave` y `load` no burbujean (existen `focusin` y `focusout` que sí). `click`, `input`, `change`, `submit` y `keydown` sí.

**¿Por qué `submit` en el formulario y no `click` en el botón?**
`submit` también se dispara con Enter y es el evento propio del formulario. Además respeta la validación nativa (en el buscador).

**¿Qué es `{ once: true }`?**
Una opción de `addEventListener` que borra la escucha después de la primera vez.

**¿Qué es `evento.target` y `evento.currentTarget`?**
`target`: el elemento donde se originó (el más profundo). `currentTarget`: el elemento que tiene la escucha que se está ejecutando.

**¿Cómo sabes que no hay escuchas duplicadas?**
Todas las escuchas de clic se registran una vez al cargar el módulo, en contenedores que siempre existen. Ninguna se registra dentro de una función que se llame varias veces.

## 9.3 Formularios y validación

**¿Qué es la API de validación de restricciones?**
El conjunto de propiedades y métodos que el navegador da a cada campo para validar: `validity`, `checkValidity()`, `reportValidity()`, `setCustomValidity()`, `validationMessage` y `willValidate`.

**¿`checkValidity()` vs `reportValidity()`?**
Los dos devuelven si es válido. `reportValidity()` además muestra el globito del navegador. Uso `checkValidity()` porque los mensajes los pongo yo.

**¿Por qué el año usa `type="number"` y no un `pattern`?**
`number` ya obliga a que sea número y permite `min`, `max` y `step`. Además en celular abre el teclado numérico.

**¿Qué hace `step="1"`?**
Solo acepta enteros. Con 3.5 se activa `stepMismatch`.

**¿Qué pasa si alguien quita `required` desde DevTools?**
El formulario podría enviarse sin ese campo. La validación en el cliente es para la experiencia de usuario; un servidor real tendría que validar de nuevo. (JSONPlaceholder no valida nada.)

**¿Qué hace `FormData`?**
Lee todos los campos de un formulario por su `name`. `get` devuelve el primero y `getAll` todos los que tienen ese nombre.

## 9.4 Asincronía

**¿Qué es una promesa?**
Un objeto que representa un valor que va a estar disponible más adelante (o un error). Puede estar pendiente, cumplida o rechazada.

**¿Qué hace `await`?**
Espera a que la promesa se resuelva y devuelve su valor. Si se rechaza, lanza el error (por eso funciona con `try/catch`). Solo se usa dentro de funciones `async` (o en la raíz de un módulo).

**¿Por qué dos `await` en `buscarLibros`?**
El primero espera la respuesta HTTP (`fetch`); el segundo espera leer y convertir el cuerpo (`json()`), que también es asincrónico.

**¿Qué pasa si el JSON viene mal?**
`respuesta.json()` lanza un error, y se atrapa en el mismo `catch` de `main.js`.

**¿Qué códigos HTTP dan `ok`?**
Del 200 al 299.

**¿Qué hace `finally`?**
Se ejecuta siempre después del `try` o del `catch`. Ahí rehabilito el botón de enviar.

**¿GET vs POST?**
GET pide datos (los parámetros van en la URL). POST envía datos para crear algo (van en el cuerpo).

## 9.5 Bootstrap y Sass

**¿Qué es mobile-first?**
Escribir primero los estilos para pantallas chicas y agregar cambios para pantallas grandes con `min-width`. Así el celular carga lo mínimo y no hay que deshacer estilos.

**¿Por qué rem y no px?**
`rem` es relativo al tamaño de letra raíz. Si el usuario agranda la letra del navegador, todo escala en proporción.

**¿`rem` vs `em`?**
`rem` es relativo al elemento raíz (`html`); `em` al tamaño de letra del elemento padre, y se acumula al anidar.

**¿Qué es un parcial de Sass?**
Un archivo que empieza con `_` y no se compila solo; se importa en otro.

**¿Qué es la interpolación `#{}`?**
Inserta el resultado de una expresión Sass dentro de un texto. Se necesita para guardar un valor de Sass en una variable CSS.

**¿Qué hace `@content` en un mixin?**
Marca dónde se inserta el bloque que se escribe al usar `@include`.

**¿Qué es BEM?**
Una convención de nombres: bloque (`libro`), elemento (`libro__portada`) y modificador (`estado--leido`). Evita choques de nombres y deja claro qué pertenece a qué.

**¿Por qué Bootstrap no se personaliza editando sus archivos en `node_modules`?**
Porque `node_modules` no se sube y se reinstala; los cambios se perderían.

**¿Qué hace `table-responsive`?**
Agrega scroll horizontal a la tabla si no cabe en pantallas pequeñas.

## 9.6 React

**¿Qué es JSX?**
Una sintaxis que parece HTML dentro de JavaScript. Vite (con el plugin de React) la convierte en llamadas a funciones de React.

**¿Qué es un componente funcional?**
Una función que recibe props y devuelve JSX.

**¿Qué son las props?**
Los datos que un componente padre le pasa a un hijo. Son de solo lectura para el hijo.

**¿Qué es el estado?**
Datos que un componente guarda y que, al cambiar con su función `set`, provocan un nuevo render.

**¿Cuándo se vuelve a dibujar un componente?**
Cuando cambia su estado o cuando su padre se vuelve a dibujar.

**¿Por qué `setLibros` con una función nueva y no modificando `libros`?**
React compara la referencia nueva con la vieja. Si es el mismo arreglo, puede no volver a dibujar. Además, mutar rompe herramientas de React como `StrictMode`.

**¿Qué es "levantar el estado"?**
Mover el estado al ancestro común más cercano de los componentes que lo necesitan, y pasarlo hacia abajo por props.

**¿Qué es un valor derivado?**
Un valor que se puede calcular a partir del estado o las props. No se guarda como estado.

**¿Para qué sirve `key`?**
Para que React identifique cada elemento de una lista entre un render y otro, y sepa cuál se agregó, cuál se movió y cuál se quitó.

**¿Qué hace `StrictMode`?**
En desarrollo, ejecuta algunas cosas dos veces y avisa sobre prácticas peligrosas. No afecta la versión final.

**¿Qué es el Virtual DOM / reconciliación?**
React compara el árbol de JSX nuevo con el anterior y aplica al DOM real solo las diferencias.

**¿Por qué los componentes no pueden usar `querySelector`?**
Porque el DOM lo maneja React. Si lo cambias por fuera, React no se entera y en el siguiente render puede sobrescribir o chocar con tu cambio. Para casos especiales existe `useRef`.

**¿Qué es un formulario controlado?**
Uno en que el valor de cada campo sale del estado (`value`) y cada cambio actualiza el estado (`onChange`).

**¿Por qué `htmlFor` y `className`?**
Porque `for` y `class` son palabras reservadas de JavaScript.

**¿Qué es Vite?**
Una herramienta de desarrollo: servidor con recarga instantánea y empaquetado para producción. Entiende JSX y Sass.

## 9.7 Git

**¿Qué es un commit?** Una foto del proyecto en un momento, con mensaje, autor y fecha.

**¿Rama vs etiqueta?** La rama avanza con cada commit nuevo; la etiqueta queda fija en un commit.

**¿Cómo ves el historial?** `git log --oneline`.

**¿Cómo deshaces un cambio no confirmado?** `git restore archivo`.

**¿Por qué una rama para la defensa?** Para que las modificaciones no alteren el commit entregado.

**npm muestra vulnerabilidades al instalar, ¿es un problema?** Vienen de dependencias internas de `serve` (una herramienta de desarrollo, en `devDependencies`) que solo corre en mi computadora. No forman parte de lo que se entrega al navegador. No usé `npm audit fix --force` porque bajaría `serve` a una versión vieja con cambios incompatibles.

---

# 10. Glosario

| Término | Significado corto |
|---|---|
| Ancestro común | El componente más cercano que contiene a otros dos |
| `async` / `await` | Forma de escribir código asincrónico que se lee de arriba abajo |
| BEM | Convención de nombres bloque__elemento--modificador |
| Breakpoint (depuración) | Punto donde se detiene la ejecución para inspeccionar |
| Breakpoint (CSS) | Ancho de pantalla donde cambia el diseño |
| Burbuja | Fase en que el evento sube del elemento a sus ancestros |
| Captura | Fase en que el evento baja de `window` al elemento |
| `children` | Prop con el contenido entre las etiquetas de un componente |
| Componente controlado | Su valor lo decide el estado de React |
| CORS | Reglas que deciden si una página puede leer respuestas de otro dominio |
| Delegación | Una escucha en un contenedor que maneja eventos de sus hijos |
| Derivado | Valor calculado desde el estado, no guardado |
| `!default` | En Sass, "usa este valor si la variable no existe" |
| Estado elevado | Estado movido al ancestro común |
| Inmutabilidad | No modificar el valor; crear uno nuevo |
| Interpolación | `#{}` en Sass para insertar un valor en texto |
| `key` | Identificador estable de cada elemento de una lista en React |
| Mixin | Bloque de reglas reutilizable en Sass |
| Mobile-first | Estilos base para celular, ampliados con `min-width` |
| Módulo ES | Archivo JS con `import`/`export` y alcance propio |
| Parcial | Archivo Sass con `_` que se importa en otro |
| Promesa | Objeto que representa un valor futuro |
| Props | Datos que un componente recibe de su padre |
| Renderizado condicional | Mostrar una cosa u otra según una condición |
| Variable de origen | Variable de Bootstrap que se puede definir antes de importarlo |
| XSS | Inyección de código a través de HTML insertado sin control |

---

# 11. Si algo sale mal durante la defensa

| Problema | Causa probable | Qué hacer |
|---|---|---|
| La Parte A sale sin estilos | No se compiló Sass | `npm run sass` o `npm start` |
| Error `Failed to load module` o CORS en la consola | Se abrió el HTML con doble clic | Usar `npm start` y abrir `localhost:3000` |
| `bootstrap is not defined` | El script de Bootstrap no cargó o está después de `main.js` | Revisar el orden de los `<script>` y que exista `node_modules` |
| `Cannot read properties of null` | Un `getElementById` no encontró el id | Revisar que el id del HTML coincida exactamente |
| La búsqueda siempre da error | Sin internet o Open Library caída | Mostrar que el estado de error funciona; probar otra red |
| `SyntaxError` tras una modificación | Falta una coma, llave o paréntesis | Leer la línea que indica la consola; `git diff` |
| React: pantalla en blanco | Error en un componente | Mirar la consola del navegador y la terminal de Vite |
| React: aviso de `key` | Falta `key` en un `map` | Agregar `key` con un id estable |
| Sass: `Undefined variable` | Se usa una variable antes de definirla o en un archivo que no la importa | Revisar el orden de imports en `main.scss` |
| Sass: `Undefined mixin` | Se usa `desde` antes de importar `_mixins` | `_mixins` tiene que ir antes de `_componentes` |
| No sabes qué rompiste | — | `git diff` para ver cambios; `git restore archivo` para volver |

Y si te bloqueas con una pregunta: di lo que sí sabes, ubica el archivo, lee el código en voz alta. Razonar en voz alta sobre tu propio código cuenta.

---

# 12. Plan de práctica y checklist

## 12.1 Mientras construyes (20 al 25)

- Después de cada commit de `GUIA_COMMITS.md`, explica en voz alta lo que hiciste sin mirar.
- Al final de cada día, repasa las secciones de esta guía que corresponden a lo que construiste.

## 12.2 Después de entregar (antes de la defensa)

**Día 1**
- [ ] Recorridos 5.1 y 5.2 (cambiar estado en las dos partes) en voz alta, sin mirar la guía.
- [ ] Recorridos 5.3, 5.4 y 5.5.
- [ ] Sección 6, criterios 1 al 7: decir el "en 30 segundos" de cada uno.

**Día 2**
- [ ] Recorridos 5.6 al 5.9.
- [ ] Sección 6, criterios 8 al 13.
- [ ] Sección 7.6 (DOM manual vs React) en voz alta.

**Día 3**
- [ ] En una rama de práctica, hacer **sin mirar** las modificaciones 8.1, 8.2 (dos ejemplos), 8.3, 8.4 y 8.5. Medir cuánto tardas en cada una.
- [ ] Borrar la rama: primero `git switch main` y después `git branch -D practica` (en dos líneas; en PowerShell no se usa `&&`).
- [ ] Sección 9 completa: tapar las respuestas e ir respondiendo.

**Día de la defensa**
- [ ] Las dos partes corriendo (`npm start` en `buscador-js`, `npm run dev` en `tablero-react`).
- [ ] Copilot, Claude, ChatGPT y cualquier autocompletado con IA **desactivados**.
- [ ] Pestañas abiertas: MDN, documentación de Bootstrap (Sass y componentes), Sass y React.
- [ ] Terminal en la carpeta del repositorio, en la rama `main`, con `git status` limpio.
- [ ] DevTools abierto con la consola limpia.
- [ ] Respira. Sabes cómo funciona porque lo escribiste tú.
