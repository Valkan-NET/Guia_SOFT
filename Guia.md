# Innovation Hub · Avance 1 · Guía de implementación completa

Every step below is one commit. Work inside your real git clone, not the unzipped folder in Downloads. Commit messages use two git trailers so the README table can be generated with six columns:

```bash
git commit -m "Mensaje corto" --trailer "Seccion: Estilos" --trailer "Cambio: Qué cambió en una frase"
```

Use `--trailer` and not a second `-m`. Git only reads trailers from the last paragraph of the message, so two separate `-m` flags would drop the first one.

---

## PASO 1 · Toolchain (root of the repo)

```bash
npm init -y
npm install --save-dev sass bootstrap@5.3.3
```

Replace the `"scripts"` block of the generated `package.json` with:

```json
"scripts": {
  "css": "sass --load-path=node_modules --no-source-map --quiet-deps --silence-deprecation=import avance1/scss/main.scss avance1/css/main.css",
  "css:watch": "npm run css -- --watch"
}
```

`package.json` exists only to compile Sass. Serving the prototype is done with the **Live Server** extension of VS Code (publisher Ritwick Dey, id `ritwickdey.LiveServer`): install it once, right-click `avance1/index.html` and choose **Open with Live Server**. It opens `http://127.0.0.1:5500/avance1/index.html` and reloads on every save, including the CSS that `npm run css:watch` recompiles.

Append to `.gitignore`:

```
# compilación de Sass
.sass-cache/
```

Commit: `package.json`, `package-lock.json`, `.gitignore`.
Trailers: `Seccion: Herramientas` · `Cambio: Sass y Bootstrap como dependencias de desarrollo`

---

## PASO 2 · Sass (7 files in `avance1/scss/`)

### `avance1/scss/main.scss`

```scss
// 1. Funciones de Bootstrap (necesarias para tint-color, shade-color, etc.)
@import "bootstrap/scss/functions";

// 2. Variables propias: sobrescriben las variables de origen de Bootstrap
@import "variables";

// 3. Bootstrap completo, ya con nuestros valores
@import "bootstrap/scss/bootstrap";

// 4. Parciales propios
@import "base";
@import "navbar";
@import "hero";
@import "tarjetas";
@import "formularios";
```

### `avance1/scss/_variables.scss`

```scss
// ---------- Paleta de marca ----------
$azul:     #2563EB;
$marino:   #1E3A8A;
$petroleo: #0E7490;
$cian:     #06B6D4;   // solo para degradados decorativos
$cielo:    #38BDF8;   // blob decorativo del hero
$menta:    #34D399;   // blob decorativo del hero
$tinta:    #0B1F4B;

// ---------- Colores de tema (variables de origen de Bootstrap) ----------
$primary:   $azul;
$secondary: #64748B;
$success:   #10B981;
$info:      $petroleo;
$warning:   #F59E0B;
$danger:    #EF4444;
$light:     #EEF2F9;
$dark:      $tinta;

$body-bg:               #F4F7FC;
$body-color:            #1E293B;
$body-secondary-color:  #475569;
$headings-color:        $tinta;
$link-color:            $azul;
$link-decoration:       none;
$link-hover-decoration: underline;
$border-color:          #E2E8F0;

// ---------- Tipografía ----------
$font-family-sans-serif: "DM Sans", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
$headings-font-family:   "Sora", $font-family-sans-serif;
$headings-font-weight:   700;
$font-size-base:         1rem;
$lead-font-size:         1.125rem;
$lead-font-weight:       400;

// ---------- Espaciado ----------
$spacer: 1rem;
$spacers: (
  0: 0,
  1: $spacer * .25,
  2: $spacer * .5,
  3: $spacer,
  4: $spacer * 1.5,
  5: $spacer * 3,
  6: $spacer * 4.5,
  7: $spacer * 6
);
$grid-gutter-width: 1.75rem;

// ---------- Formas y sombras ----------
$border-radius:    .75rem;
$border-radius-sm: .5rem;
$border-radius-lg: 1rem;
$border-radius-xl: 1.5rem;
$box-shadow-sm:    0 2px 8px rgba(11, 31, 75, .06);
$box-shadow:       0 10px 30px rgba(11, 31, 75, .10);
$box-shadow-lg:    0 20px 50px rgba(11, 31, 75, .16);

// ---------- Botones ----------
$btn-border-radius:    50rem;
$btn-border-radius-sm: 50rem;
$btn-border-radius-lg: 50rem;
$btn-font-weight:      600;
$btn-padding-x:        1.25rem;
$btn-padding-y:        .55rem;

// ---------- Tarjetas ----------
$card-border-width:  0;
$card-border-radius: $border-radius-lg;
$card-spacer-y:      1.25rem;
$card-spacer-x:      1.25rem;
$card-cap-bg:        transparent;

// ---------- Badges ----------
$badge-font-weight:   600;
$badge-font-size:     .72em;
$badge-padding-x:     .7em;
$badge-padding-y:     .4em;
$badge-border-radius: 50rem;

// ---------- Formularios ----------
$input-border-radius:      $border-radius-sm;
$input-padding-y:          .625rem;
$input-padding-x:          .875rem;
$input-border-color:       #CBD5E1;
$input-focus-border-color: tint-color($primary, 25%);
$input-focus-box-shadow:   0 0 0 .25rem rgba($primary, .18);
$form-label-font-weight:   600;
$form-label-font-size:     .875rem;
$form-feedback-font-size:  .8125rem;

// ---------- Navegación ----------
$navbar-padding-y:          .75rem;
$navbar-light-color:        #334155;
$navbar-light-hover-color:  $primary;
$navbar-light-active-color: $primary;
$nav-link-font-weight:      500;
$nav-tabs-border-color:     $border-color;
$nav-tabs-border-radius:    0;
$nav-tabs-link-active-color: $primary;
$nav-tabs-link-active-bg:   transparent;
$nav-tabs-link-active-border-color: transparent transparent $primary;

// ---------- Modal, toast, alertas ----------
$modal-content-border-radius:     $border-radius-lg;
$modal-content-border-width:      0;
$modal-content-box-shadow-sm-up:  $box-shadow-lg;
$toast-border-radius:             $border-radius;
$toast-border-width:              0;
$toast-box-shadow:                $box-shadow;
$alert-border-radius:             $border-radius;
$alert-border-width:              0;
```

### `avance1/scss/_base.scss`

```scss
h1, h2, h3, .navbar-brand { letter-spacing: -.02em; }

main { min-height: 60vh; }

// Las utilidades de Bootstrap (.d-flex, .d-block) también usan !important y se
// declaran antes que este parcial, por lo que esta regla gana y respeta [hidden].
[hidden] { display: none !important; }

:focus-visible {
  outline: 3px solid rgba($primary, .45);
  outline-offset: 2px;
}

.bi { vertical-align: -.125em; }

.texto-gradiente {
  background: linear-gradient(90deg, shade-color($azul, 25%), $cian);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.seccion-titulo {
  font-size: .75rem;
  font-weight: 600;
  letter-spacing: .06em;
  text-transform: uppercase;
  color: $body-secondary-color;
  margin-bottom: .5rem;
}

.lista-chips {
  display: flex;
  flex-wrap: wrap;
  gap: .4rem;
  padding: 0;
  margin: 0;
  list-style: none;
}

.estado-vacio img,
.estado-error img { max-width: 220px; height: auto; }

.pie {
  border-top: 1px solid $border-color;
  color: $body-secondary-color;
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { transition: none !important; animation: none !important; }
}
```

### `avance1/scss/_navbar.scss`

```scss
.navbar {
  position: sticky;
  top: 0;
  z-index: $zindex-sticky;
  background: rgba(#fff, .88);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid $border-color;

  .navbar-brand {
    display: inline-flex;
    align-items: center;
    gap: .5rem;
    font-family: $headings-font-family;
    font-weight: 800;
    img { width: 28px; height: 28px; }
    span { @extend .texto-gradiente; }
  }

  .nav-link {
    position: relative;
    padding-inline: .75rem;
    .bi { margin-right: .25rem; }
  }

  @include media-breakpoint-up(md) {
    .nav-link.active::after {
      content: "";
      position: absolute;
      left: .75rem;
      right: .75rem;
      bottom: 0;
      height: 3px;
      border-radius: 3px;
      background: $primary;
    }
  }

  .ver-como {
    display: flex;
    align-items: center;
    gap: .5rem;
    font-size: .8125rem;
    color: $body-secondary-color;
    .form-select {
      width: auto;
      padding-block: .3rem;
      border-radius: 50rem;
      font-size: .8125rem;
    }
  }
}
```

### `avance1/scss/_hero.scss`

```scss
.hero {
  position: relative;
  overflow: hidden;
  color: #fff;
  padding: 3.5rem 0 4rem;
  background: linear-gradient(135deg, $marino 0%, $azul 55%, $petroleo 100%);
  border-radius: 0 0 $border-radius-xl $border-radius-xl;

  @include media-breakpoint-up(md) { padding: 5.5rem 0 6rem; }

  h1 {
    color: #fff;
    font-size: clamp(2rem, 5vw, 3.25rem);
    font-weight: 800;
    line-height: 1.1;
  }
  .lead { color: rgba(#fff, .9); }

  &::before,
  &::after {
    content: "";
    position: absolute;
    border-radius: 50%;
    filter: blur(60px);
    opacity: .5;
    pointer-events: none;
  }
  &::before { width: 22rem; height: 22rem; background: $cielo; top: -8rem; right: -6rem; }
  &::after  { width: 18rem; height: 18rem; background: $menta; bottom: -9rem; left: -5rem; }

  .container { position: relative; z-index: 1; }

  .hero-imagen {
    max-width: 100%;
    height: auto;
    filter: drop-shadow(0 20px 40px rgba(0, 0, 0, .25));
  }
}

.paso-numero {
  width: 2.75rem;
  height: 2.75rem;
  display: grid;
  place-items: center;
  font-family: $headings-font-family;
  font-weight: 800;
  color: #fff;
  border-radius: 50%;
  background: linear-gradient(135deg, shade-color($azul, 25%), $cian);
}

.paso-imagen {
  width: 100%;
  height: 140px;
  object-fit: contain;
}

.estadistica {
  font-family: $headings-font-family;
  font-weight: 800;
  font-size: 2rem;
  line-height: 1;
  color: $tinta;
}
```

### `avance1/scss/_tarjetas.scss`

```scss
.tarjeta-iniciativa {
  box-shadow: $box-shadow-sm;
  border-top: 4px solid transparent;
  transition: transform .2s ease, box-shadow .2s ease;

  &:hover,
  &:focus-within { transform: translateY(-4px); box-shadow: $box-shadow; }

  &--idea      { border-top-color: $primary; }
  &--necesidad { border-top-color: $success; }
  &--reto      { border-top-color: $warning; }

  .card-title { font-size: 1rem; }
}

.chip {
  display: inline-flex;
  align-items: center;
  gap: .3rem;
  padding: .3em .75em;
  font-size: .78rem;
  font-weight: 500;
  border-radius: 50rem;
  background: tint-color($primary, 88%);
  color: shade-color($primary, 30%);

  &--neutro { background: $light; color: $tinta; }
  &--nivel  { opacity: .75; font-weight: 400; }
}

.avatar {
  width: 2.25rem;
  height: 2.25rem;
  border-radius: 50%;
  display: inline-grid;
  place-items: center;
  font-weight: 700;
  font-size: .8rem;
  color: #fff;
  background: $primary;
  flex-shrink: 0;

  &--lg {
    width: 4.5rem;
    height: 4.5rem;
    font-size: 1.5rem;
    font-family: $headings-font-family;
    font-weight: 800;
    background: linear-gradient(135deg, shade-color($azul, 25%), $cian);
  }
}

.metrica {
  background: $body-bg;
  border-radius: $border-radius;
  padding: .75rem 1rem;

  .valor {
    font-family: $headings-font-family;
    font-weight: 800;
    font-size: 1.5rem;
    line-height: 1.1;
    color: $tinta;
  }

  &--acento {
    background: tint-color($primary, 88%);
    .valor, .seccion-titulo { color: shade-color($primary, 30%); }
  }
}

.bloque-acento {
  background: #fff;
  border-left: 4px solid $primary;
  border-radius: 0 $border-radius $border-radius 0;
  padding: .75rem 1rem;
  &--teal { border-left-color: $petroleo; }
}

.fila-lista {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  padding: .75rem .25rem;
  border-bottom: 1px solid $border-color;
  color: inherit;
  text-decoration: none;
  &:hover { background: rgba($primary, .03); color: inherit; }
}

@include media-breakpoint-up(lg) {
  .tarjeta-lateral { position: sticky; top: 5.5rem; }
}
```

### `avance1/scss/_formularios.scss`

```scss
.formulario-tarjeta {
  background: #fff;
  border-radius: $border-radius-lg;
  box-shadow: $box-shadow-sm;
  padding: 1.5rem;
  @include media-breakpoint-up(md) { padding: 2rem; }
}

.invalid-feedback { font-weight: 500; }

.contador {
  display: block;
  text-align: right;
  font-size: .75rem;
  color: $body-secondary-color;
  margin-top: .25rem;
}

.competencia-elegida {
  display: inline-flex;
  align-items: center;
  gap: .5rem;
  padding: .45rem .9rem;
  .btn-close { width: .6em; height: .6em; }
}
```

Compile and check the output exists:

```bash
npm run css && ls -la avance1/css/main.css
```

Expected: a file of roughly 230 KB. If Dart Sass prints a wall of deprecation warnings that bother you, pin `npm install --save-dev sass@1.79.4`; both versions produce the same CSS.

Commit: `avance1/scss/*`, `avance1/css/main.css`.
Trailers: `Seccion: Estilos` · `Cambio: Sistema de diseño en Sass que sobrescribe variables de Bootstrap`

---

## PASO 3 · Imágenes (`avance1/img/`)

### 3.1 Hand-coded SVGs (do not generate these; paste as-is)

`avance1/img/logo.svg`

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" role="img" aria-label="Innovation Hub">
  <defs>
    <linearGradient id="g" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#1E40AF"/><stop offset="1" stop-color="#06B6D4"/>
    </linearGradient>
  </defs>
  <path d="M16 2 28 9v14l-12 7L4 23V9z" fill="none" stroke="url(#g)" stroke-width="2.5" stroke-linejoin="round"/>
  <path d="M16 10 10.5 20M16 10l5.5 10M10.5 20h11" stroke="#2563EB" stroke-width="1.6" stroke-linecap="round"/>
  <circle cx="16" cy="10" r="2.4" fill="#2563EB"/>
  <circle cx="10.5" cy="20" r="2.4" fill="#0E7490"/>
  <circle cx="21.5" cy="20" r="2.4" fill="#06B6D4"/>
</svg>
```

`avance1/img/vacio-catalogo.svg`

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 220 160" role="img" aria-label="Caja vacía">
  <rect x="50" y="70" width="120" height="70" rx="10" fill="#DBEAFE"/>
  <path d="M50 80 110 60l60 20-60 20z" fill="#BFDBFE"/>
  <path d="M110 100v40" stroke="#2563EB" stroke-width="2"/>
  <path d="M110 58c0-16 10-26 22-30" fill="none" stroke="#0E7490" stroke-width="3" stroke-linecap="round"/>
  <circle cx="134" cy="26" r="4" fill="#0E7490"/>
  <text x="110" y="128" text-anchor="middle" font-family="sans-serif" font-size="22" font-weight="700" fill="#1E40AF">?</text>
</svg>
```

`avance1/img/error-carga.svg`

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 220 160" role="img" aria-label="Cable desconectado">
  <path d="M10 80h60" stroke="#1E40AF" stroke-width="6" stroke-linecap="round"/>
  <rect x="70" y="64" width="34" height="32" rx="8" fill="#2563EB"/>
  <rect x="104" y="72" width="8" height="5" fill="#2563EB"/><rect x="104" y="83" width="8" height="5" fill="#2563EB"/>
  <path d="M210 80h-60" stroke="#0E7490" stroke-width="6" stroke-linecap="round"/>
  <rect x="122" y="64" width="34" height="32" rx="8" fill="#0E7490"/>
  <rect x="114" y="72" width="8" height="5" fill="#0E7490"/><rect x="114" y="83" width="8" height="5" fill="#0E7490"/>
  <path d="M112 50l4 8-6 2 5 8" fill="none" stroke="#F59E0B" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"/>
</svg>
```

### 3.2 Generated images

Paste this style block first in every prompt, then the per-image line. Export WebP at the listed size. If the generator cannot output transparent WebP, use PNG for `hero` and `paso-*`.

```
Flat vector illustration, modern editorial style, soft rounded geometric shapes, subtle paper grain,
limited palette: navy #1E3A8A, electric blue #2563EB, deep teal #0E7490, sky #38BDF8, mint #34D399,
warm accent coral #FB7185 used sparingly, off-white background #F4F7FC or transparent,
no text, no letters, no logos, no watermarks, clean negative space, consistent series style.
```

| File | Size | Used in | Alt text | Per-image prompt |
|---|---|---|---|---|
| `hero.webp` | 1200×900, transparent | `index.html` hero, right column, hidden below lg | Estudiantes de distintas carreras conectando ideas en una red | Four diverse university students in three-quarter view, each holding a glowing node, nodes joined by curved lines forming a constellation between them, slight isometric depth, sense of upward motion, transparent background |
| `paso-explorar.webp` | 480×360 | `index.html` step 1 card | Lupa sobre tarjetas de iniciativas | A large magnifying glass hovering over three floating rounded cards with abstract tag shapes, blue and teal, transparent background |
| `paso-publicar.webp` | 480×360 | `index.html` step 2 card | Persona lanzando una idea representada por una bombilla | A person launching a lightbulb-shaped paper plane upward, small sparkles, blue dominant with one coral spark, transparent background |
| `paso-postular.webp` | 480×360 | `index.html` step 3 card | Mano entregando una pieza de rompecabezas que encaja | A hand offering a puzzle piece that fits into a larger puzzle, teal piece into blue puzzle, transparent background |
| `paso-equipo.webp` | 480×360 | `index.html` step 4 card | Equipo interdisciplinario alrededor de una mesa con un cohete | Three people around a round table, a small rocket lifting off from the center of the table, mint and blue, transparent background |

Alt text for the hero and step images is written in the HTML of step 4. Users have no avatar image: the interface shows their initials in a colored circle, generated by `ui.js`.

Commit: `avance1/img/*`.
Trailers: `Seccion: Diseño visual` · `Cambio: Logotipo e ilustraciones`

---

## PASO 4 · HTML (6 pages, full replacement)

Common rules applied in every page: `lang="es"`, skip link, `<header id="navbar">` filled by JS, `<main id="contenido">`, `#cargando` and `#error` blocks, local compiled CSS instead of the CDN CSS, Google Fonts and Bootstrap Icons links, and page scripts wrapped in an IIFE (see step 5).

### `avance1/index.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Innovation Hub · Conectá ideas con personas</title>
  <meta name="description" content="Publicá ideas, necesidades y retos; encontrá competencias y formá equipos interdisciplinarios en la comunidad universitaria.">
  <link rel="icon" href="img/logo.svg" type="image/svg+xml">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Sora:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
  <link rel="stylesheet" href="css/main.css">
</head>
<body>
<a class="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">Saltar al contenido</a>
<header id="navbar"></header>

<main id="contenido">
  <section class="hero" aria-labelledby="t-hero">
    <div class="container">
      <div class="row align-items-center g-4">
        <div class="col-lg-7 text-center text-lg-start">
          <h1 id="t-hero">Conectá ideas con las personas que las hacen realidad</h1>
          <p class="lead mt-3 mb-4">Publicá ideas, necesidades y retos; encontrá las competencias que faltan y formá equipos interdisciplinarios dentro de la comunidad universitaria.</p>
          <div class="d-flex flex-column flex-sm-row gap-2 justify-content-center justify-content-lg-start">
            <a class="btn btn-light btn-lg" href="paginas/catalogo.html"><i class="bi bi-search" aria-hidden="true"></i> Explorar iniciativas</a>
            <a class="btn btn-outline-light btn-lg" href="paginas/formulario.html"><i class="bi bi-plus-circle" aria-hidden="true"></i> Publicar una iniciativa</a>
          </div>
        </div>
        <div class="col-lg-5 d-none d-lg-block">
          <img class="hero-imagen" src="img/hero.webp" alt="Estudiantes de distintas carreras conectando ideas en una red" width="600" height="450">
        </div>
      </div>
    </div>
  </section>

  <section class="container py-5" aria-labelledby="t-como">
    <h2 id="t-como" class="h3 text-center mb-2">¿Cómo funciona?</h2>
    <p class="text-center text-body-secondary col-lg-8 mx-auto mb-4">Cuatro pasos para pasar de una idea suelta a un equipo trabajando.</p>
    <div class="row row-cols-1 row-cols-sm-2 row-cols-lg-4 g-4">
      <div class="col">
        <article class="card h-100 text-center">
          <div class="card-body">
            <img class="paso-imagen mb-3" src="img/paso-explorar.webp" alt="Lupa sobre tarjetas de iniciativas" width="240" height="180" loading="lazy">
            <span class="paso-numero mx-auto mb-3" aria-hidden="true">1</span>
            <h3 class="h6">Explorá</h3>
            <p class="small text-body-secondary mb-0">Buscá ideas, necesidades y retos por texto, tipo, categoría o competencia.</p>
          </div>
        </article>
      </div>
      <div class="col">
        <article class="card h-100 text-center">
          <div class="card-body">
            <img class="paso-imagen mb-3" src="img/paso-publicar.webp" alt="Persona lanzando una idea representada por una bombilla" width="240" height="180" loading="lazy">
            <span class="paso-numero mx-auto mb-3" aria-hidden="true">2</span>
            <h3 class="h6">Publicá</h3>
            <p class="small text-body-secondary mb-0">Registrá tu iniciativa con el problema, los beneficiarios y las competencias que necesitás.</p>
          </div>
        </article>
      </div>
      <div class="col">
        <article class="card h-100 text-center">
          <div class="card-body">
            <img class="paso-imagen mb-3" src="img/paso-postular.webp" alt="Mano entregando una pieza de rompecabezas que encaja" width="240" height="180" loading="lazy">
            <span class="paso-numero mx-auto mb-3" aria-hidden="true">3</span>
            <h3 class="h6">Postulate</h3>
            <p class="small text-body-secondary mb-0">Solicitá participar indicando qué competencia aportás, tu rol deseado y tu disponibilidad.</p>
          </div>
        </article>
      </div>
      <div class="col">
        <article class="card h-100 text-center">
          <div class="card-body">
            <img class="paso-imagen mb-3" src="img/paso-equipo.webp" alt="Equipo interdisciplinario alrededor de una mesa con un cohete" width="240" height="180" loading="lazy">
            <span class="paso-numero mx-auto mb-3" aria-hidden="true">4</span>
            <h3 class="h6">Formá equipo</h3>
            <p class="small text-body-secondary mb-0">El autor acepta participantes y, con equipo, la iniciativa se convierte en proyecto.</p>
          </div>
        </article>
      </div>
    </div>
  </section>

  <section class="container pb-5" aria-labelledby="t-caminos">
    <h2 id="t-caminos" class="visually-hidden">Publicar o participar</h2>
    <div class="row g-4">
      <div class="col-md-6">
        <article class="card h-100">
          <div class="card-body">
            <h3 class="h5"><i class="bi bi-lightbulb-fill text-primary" aria-hidden="true"></i> Tenés una idea, una necesidad o un reto</h3>
            <ul class="small text-body-secondary mb-3">
              <li>Describí el problema y a quién beneficia.</li>
              <li>Indicá las competencias que buscás y cuántas personas necesitás.</li>
              <li>Elegí la visibilidad: pública, institucional, restringida o privada.</li>
              <li>Recibí solicitudes y armá tu equipo.</li>
            </ul>
            <a class="btn btn-primary" href="paginas/formulario.html">Publicar iniciativa <i class="bi bi-arrow-right" aria-hidden="true"></i></a>
          </div>
        </article>
      </div>
      <div class="col-md-6">
        <article class="card h-100">
          <div class="card-body">
            <h3 class="h5"><i class="bi bi-people-fill text-info" aria-hidden="true"></i> Querés sumarte a un proyecto real</h3>
            <ul class="small text-body-secondary mb-3">
              <li>Completá tu perfil con competencias e intereses.</li>
              <li>Filtrá el catálogo por lo que sabés hacer.</li>
              <li>Enviá una solicitud con tu mensaje de presentación.</li>
              <li>Cuando te acepten, pasás a formar parte del equipo.</li>
            </ul>
            <a class="btn btn-outline-primary" href="paginas/catalogo.html">Ver iniciativas <i class="bi bi-arrow-right" aria-hidden="true"></i></a>
          </div>
        </article>
      </div>
    </div>
  </section>

  <section id="estadisticas" class="container pb-5" aria-label="Cifras de la plataforma" hidden>
    <div class="row row-cols-2 row-cols-md-4 g-3 text-center">
      <div class="col"><div class="card"><div class="card-body"><p class="estadistica mb-1" id="n-iniciativas">0</p><p class="small text-body-secondary mb-0">iniciativas activas</p></div></div></div>
      <div class="col"><div class="card"><div class="card-body"><p class="estadistica mb-1" id="n-proyectos">0</p><p class="small text-body-secondary mb-0">convertidas en proyecto</p></div></div></div>
      <div class="col"><div class="card"><div class="card-body"><p class="estadistica mb-1" id="n-personas">0</p><p class="small text-body-secondary mb-0">personas registradas</p></div></div></div>
      <div class="col"><div class="card"><div class="card-body"><p class="estadistica mb-1" id="n-competencias">0</p><p class="small text-body-secondary mb-0">competencias en el catálogo</p></div></div></div>
    </div>
  </section>
</main>

<footer class="pie text-center py-4 small">
  <div class="container">Innovation Hub · Proyecto SOFT-12 · Universidad CENFOTEC · 2026</div>
</footer>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="js/ui.js"></script>
<script src="js/sesion.js"></script>
<script src="js/datos.js"></script>
<script src="js/almacen.js"></script>
<script src="js/inicio.js"></script>
</body>
</html>
```

### `avance1/paginas/catalogo.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Catálogo de iniciativas · Innovation Hub</title>
  <link rel="icon" href="../img/logo.svg" type="image/svg+xml">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Sora:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
  <link rel="stylesheet" href="../css/main.css">
</head>
<body>
<a class="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">Saltar al contenido</a>
<header id="navbar"></header>

<main id="contenido" class="container py-4">
  <div class="d-flex flex-wrap justify-content-between align-items-end gap-2 mb-4">
    <div>
      <h1 class="h3 mb-1">Catálogo de iniciativas</h1>
      <p class="text-body-secondary mb-0">Ideas, necesidades y retos que buscan equipo.</p>
    </div>
    <p class="mb-0 small text-body-secondary" id="conteo" aria-live="polite"></p>
  </div>

  <form id="filtros" class="formulario-tarjeta row g-3 mb-4" role="search" aria-label="Buscar y filtrar iniciativas">
    <div class="col-12 col-lg-4">
      <label class="form-label" for="buscar"><i class="bi bi-search" aria-hidden="true"></i> Buscar</label>
      <input id="buscar" name="texto" type="search" class="form-control" placeholder="Título, resumen o etiqueta" autocomplete="off">
    </div>
    <div class="col-6 col-md-4 col-lg-2">
      <label class="form-label" for="f-tipo">Tipo</label>
      <select id="f-tipo" name="tipo" class="form-select">
        <option value="">Todos</option>
        <option value="idea">Idea</option>
        <option value="necesidad">Necesidad</option>
        <option value="reto">Reto</option>
      </select>
    </div>
    <div class="col-6 col-md-4 col-lg-3">
      <label class="form-label" for="f-categoria">Categoría</label>
      <select id="f-categoria" name="categoria" class="form-select"><option value="">Todas</option></select>
    </div>
    <div class="col-12 col-md-4 col-lg-3">
      <label class="form-label" for="f-competencia">Competencia</label>
      <select id="f-competencia" name="competencia" class="form-select"><option value="">Todas</option></select>
    </div>
    <div class="col-12 d-flex justify-content-end">
      <button type="reset" class="btn btn-outline-secondary btn-sm"><i class="bi bi-x-circle" aria-hidden="true"></i> Limpiar filtros</button>
    </div>
  </form>

  <div id="cargando" class="text-center py-5" aria-live="polite">
    <div class="spinner-border text-primary" role="status"><span class="visually-hidden">Cargando iniciativas…</span></div>
  </div>
  <div id="error" class="estado-error text-center py-5" role="alert" hidden></div>
  <div id="vacio" class="estado-vacio text-center py-5" role="status" hidden>
    <img src="../img/vacio-catalogo.svg" alt="" width="220" height="160" class="mb-3">
    <h2 class="h5">No encontramos iniciativas con esos criterios</h2>
    <p class="text-body-secondary mb-3">Probá con otra palabra o quitá algún filtro.</p>
    <button type="button" class="btn btn-outline-primary btn-sm" id="limpiar-desde-vacio"><i class="bi bi-x-circle" aria-hidden="true"></i> Limpiar filtros</button>
  </div>
  <div id="lista" class="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-4"></div>
</main>

<footer class="pie text-center py-4 small">
  <div class="container">Innovation Hub · Proyecto SOFT-12 · Universidad CENFOTEC · 2026</div>
</footer>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="../js/ui.js"></script>
<script src="../js/sesion.js"></script>
<script src="../js/datos.js"></script>
<script src="../js/almacen.js"></script>
<script src="../js/reglas.js"></script>
<script src="../js/catalogo.js"></script>
</body>
</html>
```

### `avance1/paginas/detalle.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Detalle de iniciativa · Innovation Hub</title>
  <link rel="icon" href="../img/logo.svg" type="image/svg+xml">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Sora:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
  <link rel="stylesheet" href="../css/main.css">
</head>
<body>
<a class="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">Saltar al contenido</a>
<header id="navbar"></header>

<main id="contenido" class="container py-4">
  <div id="cargando" class="text-center py-5" aria-live="polite">
    <div class="spinner-border text-primary" role="status"><span class="visually-hidden">Cargando iniciativa…</span></div>
  </div>
  <div id="error" class="estado-error text-center py-5" role="alert" hidden></div>
  <div id="detalle" hidden></div>
</main>

<div class="modal fade" id="modal-confirmacion" tabindex="-1" aria-labelledby="modal-titulo" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header border-0 pb-0">
        <h2 class="modal-title h5" id="modal-titulo">Confirmar</h2>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
      </div>
      <div class="modal-body">
        <p class="fw-semibold mb-1" id="modal-nombre"></p>
        <p class="text-body-secondary mb-0" id="modal-mensaje"></p>
      </div>
      <div class="modal-footer border-0 pt-0">
        <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">Cancelar</button>
        <button type="button" class="btn btn-danger" id="modal-confirmar">Eliminar</button>
      </div>
    </div>
  </div>
</div>

<footer class="pie text-center py-4 small">
  <div class="container">Innovation Hub · Proyecto SOFT-12 · Universidad CENFOTEC · 2026</div>
</footer>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="../js/ui.js"></script>
<script src="../js/sesion.js"></script>
<script src="../js/datos.js"></script>
<script src="../js/almacen.js"></script>
<script src="../js/reglas.js"></script>
<script src="../js/detalle.js"></script>
</body>
</html>
```

### `avance1/paginas/formulario.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Publicar iniciativa · Innovation Hub</title>
  <link rel="icon" href="../img/logo.svg" type="image/svg+xml">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Sora:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
  <link rel="stylesheet" href="../css/main.css">
</head>
<body>
<a class="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">Saltar al contenido</a>
<header id="navbar"></header>

<main id="contenido" class="container py-4" style="max-width: 56rem;">
  <h1 class="h3 mb-1" id="titulo-pagina">Publicar iniciativa</h1>
  <p class="text-body-secondary mb-4">Los campos marcados con <span class="text-danger" aria-hidden="true">*</span><span class="visually-hidden">asterisco</span> son obligatorios.</p>

  <div id="cargando" class="text-center py-5" aria-live="polite">
    <div class="spinner-border text-primary" role="status"><span class="visually-hidden">Cargando catálogos…</span></div>
  </div>
  <div id="error" class="estado-error text-center py-5" role="alert" hidden></div>
  <div id="bloqueo" class="alert alert-warning d-flex gap-2" role="status" hidden></div>

  <form id="formulario-iniciativa" class="formulario-tarjeta" novalidate hidden>
    <div class="row g-3">
      <div class="col-12 col-md-8">
        <label class="form-label" for="titulo">Título <span class="text-danger" aria-hidden="true">*</span></label>
        <input id="titulo" class="form-control" autocomplete="off">
        <span class="contador" id="contador-titulo" aria-live="polite"></span>
        <div class="invalid-feedback" id="error-titulo" hidden></div>
      </div>
      <div class="col-12 col-md-4">
        <label class="form-label" for="tipo">Tipo <span class="text-danger" aria-hidden="true">*</span></label>
        <select id="tipo" class="form-select">
          <option value="">Elegí un tipo…</option>
          <option value="idea">Idea</option>
          <option value="necesidad">Necesidad</option>
          <option value="reto">Reto</option>
        </select>
        <div class="invalid-feedback" id="error-tipo" hidden></div>
      </div>

      <div class="col-12">
        <label class="form-label" for="resumen">Resumen <span class="text-danger" aria-hidden="true">*</span></label>
        <input id="resumen" class="form-control" placeholder="Una frase que explique la iniciativa">
        <span class="contador" id="contador-resumen" aria-live="polite"></span>
        <div class="invalid-feedback" id="error-resumen" hidden></div>
      </div>

      <div class="col-12">
        <label class="form-label" for="descripcion">Descripción detallada <span class="text-danger" aria-hidden="true">*</span></label>
        <textarea id="descripcion" class="form-control" rows="4"></textarea>
        <div class="invalid-feedback" id="error-descripcion" hidden></div>
      </div>

      <div class="col-12 col-md-6">
        <label class="form-label" for="problema">Problema o situación identificada <span class="text-danger" aria-hidden="true">*</span></label>
        <textarea id="problema" class="form-control" rows="3"></textarea>
        <div class="invalid-feedback" id="error-problema" hidden></div>
      </div>
      <div class="col-12 col-md-6">
        <label class="form-label" for="beneficiarios">Población o usuarios beneficiados <span class="text-danger" aria-hidden="true">*</span></label>
        <textarea id="beneficiarios" class="form-control" rows="3"></textarea>
        <div class="invalid-feedback" id="error-beneficiarios" hidden></div>
      </div>

      <div class="col-12 col-md-6">
        <label class="form-label" for="categoria">Categoría <span class="text-danger" aria-hidden="true">*</span></label>
        <select id="categoria" class="form-select"><option value="">Elegí una categoría…</option></select>
        <div class="invalid-feedback" id="error-categoria" hidden></div>
      </div>
      <div class="col-6 col-md-3">
        <label class="form-label" for="participantes">Integrantes estimados <span class="text-danger" aria-hidden="true">*</span></label>
        <input id="participantes" type="number" inputmode="numeric" class="form-control" placeholder="1 a 20">
        <div class="invalid-feedback" id="error-participantes" hidden></div>
      </div>
      <div class="col-6 col-md-3">
        <label class="form-label" for="visibilidad">Visibilidad <span class="text-danger" aria-hidden="true">*</span></label>
        <select id="visibilidad" class="form-select">
          <option value="pública">Pública</option>
          <option value="institucional">Institucional</option>
          <option value="restringida">Restringida</option>
          <option value="privada">Privada</option>
        </select>
        <div class="invalid-feedback" id="error-visibilidad" hidden></div>
      </div>

      <div class="col-12">
        <label class="form-label" for="competencia-nueva">Competencias necesarias <span class="text-danger" aria-hidden="true">*</span></label>
        <div class="input-group">
          <select id="competencia-nueva" class="form-select"><option value="">Elegí una competencia…</option></select>
          <button type="button" id="agregar-competencia" class="btn btn-outline-primary"><i class="bi bi-plus-lg" aria-hidden="true"></i> Agregar</button>
        </div>
        <ul id="competencias-elegidas" class="lista-chips mt-2" aria-live="polite" aria-label="Competencias elegidas"></ul>
        <div class="invalid-feedback d-block" id="error-competencias" hidden></div>
      </div>

      <div class="col-12">
        <label class="form-label" for="etiquetas">Etiquetas</label>
        <input id="etiquetas" class="form-control" placeholder="voluntariado, comunidad" aria-describedby="ayuda-etiquetas">
        <div id="ayuda-etiquetas" class="form-text">Separadas por coma, máximo 5. Letras, números, espacios o guiones.</div>
        <div class="invalid-feedback" id="error-etiquetas" hidden></div>
      </div>
    </div>

    <div class="mt-4 d-flex flex-column flex-sm-row gap-2">
      <button type="submit" class="btn btn-primary"><i class="bi bi-check2" aria-hidden="true"></i> Guardar iniciativa</button>
      <a href="catalogo.html" class="btn btn-outline-secondary">Cancelar</a>
    </div>
  </form>
</main>

<footer class="pie text-center py-4 small">
  <div class="container">Innovation Hub · Proyecto SOFT-12 · Universidad CENFOTEC · 2026</div>
</footer>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="../js/ui.js"></script>
<script src="../js/sesion.js"></script>
<script src="../js/datos.js"></script>
<script src="../js/almacen.js"></script>
<script src="../js/reglas.js"></script>
<script src="../js/validacion.js"></script>
<script src="../js/formulario.js"></script>
</body>
</html>
```

### `avance1/paginas/perfil.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Mi perfil · Innovation Hub</title>
  <link rel="icon" href="../img/logo.svg" type="image/svg+xml">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Sora:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
  <link rel="stylesheet" href="../css/main.css">
</head>
<body>
<a class="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">Saltar al contenido</a>
<header id="navbar"></header>

<main id="contenido" class="container py-4">
  <h1 class="h3 mb-4">Mi perfil</h1>

  <div id="cargando" class="text-center py-5" aria-live="polite">
    <div class="spinner-border text-primary" role="status"><span class="visually-hidden">Cargando perfil…</span></div>
  </div>
  <div id="error" class="estado-error text-center py-5" role="alert" hidden></div>
  <div id="sin-sesion" class="alert alert-warning d-flex gap-2" role="status" hidden>
    <i class="bi bi-person-lock" aria-hidden="true"></i>
    <div>Estás navegando como visitante. Elegí un usuario en <strong>Ver como</strong>, en la barra superior, para ver su perfil.</div>
  </div>

  <div id="perfil" class="row g-4" hidden>
    <div class="col-md-5 col-lg-4">
      <div class="card"><div class="card-body" id="tarjeta-perfil"></div></div>
    </div>
    <div class="col-md-7 col-lg-8">
      <div class="nav nav-tabs" id="pestanas" role="tablist" aria-label="Actividad del usuario">
        <button class="nav-link active" id="tab-propias" role="tab" type="button" data-tab="propias" aria-selected="true" aria-controls="contenido-pestanas">
          <i class="bi bi-lightbulb" aria-hidden="true"></i> Mis iniciativas <span class="badge text-bg-light border ms-1" id="n-propias">0</span>
        </button>
        <button class="nav-link" id="tab-participo" role="tab" type="button" data-tab="participo" aria-selected="false" aria-controls="contenido-pestanas" tabindex="-1">
          <i class="bi bi-people" aria-hidden="true"></i> Participo en <span class="badge text-bg-light border ms-1" id="n-participo">0</span>
        </button>
        <button class="nav-link" id="tab-solicitudes" role="tab" type="button" data-tab="solicitudes" aria-selected="false" aria-controls="contenido-pestanas" tabindex="-1">
          <i class="bi bi-send" aria-hidden="true"></i> Mis solicitudes <span class="badge text-bg-light border ms-1" id="n-solicitudes">0</span>
        </button>
      </div>
      <div id="contenido-pestanas" class="pt-2" role="tabpanel" aria-labelledby="tab-propias" tabindex="0"></div>

      <div class="d-flex flex-wrap justify-content-between align-items-center gap-2 mt-4 pt-3 border-top">
        <p id="nota-local" class="small text-body-secondary mb-0" hidden><i class="bi bi-database" aria-hidden="true"></i> Hay cambios guardados en este navegador.</p>
        <button type="button" id="restablecer" class="btn btn-outline-secondary btn-sm"><i class="bi bi-arrow-counterclockwise" aria-hidden="true"></i> Restablecer datos de ejemplo</button>
      </div>
    </div>
  </div>
</main>

<footer class="pie text-center py-4 small">
  <div class="container">Innovation Hub · Proyecto SOFT-12 · Universidad CENFOTEC · 2026</div>
</footer>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="../js/ui.js"></script>
<script src="../js/sesion.js"></script>
<script src="../js/datos.js"></script>
<script src="../js/almacen.js"></script>
<script src="../js/reglas.js"></script>
<script src="../js/perfil.js"></script>
</body>
</html>
```

### `avance1/paginas/solicitud.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Solicitar participación · Innovation Hub</title>
  <link rel="icon" href="../img/logo.svg" type="image/svg+xml">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Sora:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
  <link rel="stylesheet" href="../css/main.css">
</head>
<body>
<a class="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">Saltar al contenido</a>
<header id="navbar"></header>

<main id="contenido" class="container py-4">
  <h1 class="h3 mb-4">Solicitar participación</h1>

  <div id="cargando" class="text-center py-5" aria-live="polite">
    <div class="spinner-border text-primary" role="status"><span class="visually-hidden">Cargando iniciativa…</span></div>
  </div>
  <div id="error" class="estado-error text-center py-5" role="alert" hidden></div>

  <div class="row g-4">
    <div class="col-lg-7">
      <div id="bloqueo" class="alert alert-warning d-flex gap-2" role="status" hidden></div>

      <form id="formulario-solicitud" class="formulario-tarjeta" novalidate hidden>
        <div class="mb-3">
          <label class="form-label" for="mensaje">Mensaje de presentación <span class="text-danger" aria-hidden="true">*</span></label>
          <textarea id="mensaje" class="form-control" rows="4" placeholder="Contá por qué querés participar y qué podés aportar."></textarea>
          <span class="contador" id="contador-mensaje" aria-live="polite">0 / 300</span>
          <div class="invalid-feedback" id="error-mensaje" hidden></div>
        </div>
        <div class="row g-3 mb-3">
          <div class="col-md-6">
            <label class="form-label" for="competencia">Competencia principal que aportás <span class="text-danger" aria-hidden="true">*</span></label>
            <select id="competencia" class="form-select"></select>
            <div class="invalid-feedback" id="error-competencia" hidden></div>
          </div>
          <div class="col-md-6">
            <label class="form-label" for="rol">Rol que querés desempeñar <span class="text-danger" aria-hidden="true">*</span></label>
            <input id="rol" class="form-control" placeholder="Coordinadora, desarrollador, analista…">
            <div class="invalid-feedback" id="error-rol" hidden></div>
          </div>
        </div>
        <div class="mb-4">
          <label class="form-label" for="disponibilidad">Disponibilidad <span class="text-danger" aria-hidden="true">*</span></label>
          <input id="disponibilidad" class="form-control" placeholder="4 h/semana" aria-describedby="ayuda-disponibilidad">
          <div id="ayuda-disponibilidad" class="form-text">Formato: número de horas seguido de "h/semana".</div>
          <div class="invalid-feedback" id="error-disponibilidad" hidden></div>
        </div>
        <div class="d-flex flex-column flex-sm-row gap-2">
          <button type="submit" class="btn btn-primary"><i class="bi bi-send" aria-hidden="true"></i> Enviar solicitud</button>
          <a href="catalogo.html" class="btn btn-outline-secondary">Cancelar</a>
        </div>
      </form>
    </div>
    <aside class="col-lg-5">
      <div class="card tarjeta-lateral" id="resumen-iniciativa" hidden></div>
    </aside>
  </div>
</main>

<footer class="pie text-center py-4 small">
  <div class="container">Innovation Hub · Proyecto SOFT-12 · Universidad CENFOTEC · 2026</div>
</footer>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script src="../js/ui.js"></script>
<script src="../js/sesion.js"></script>
<script src="../js/datos.js"></script>
<script src="../js/almacen.js"></script>
<script src="../js/reglas.js"></script>
<script src="../js/validacion.js"></script>
<script src="../js/solicitud.js"></script>
</body>
</html>
```

Commit: the six HTML files.
Trailers: `Seccion: Interfaz` · `Cambio: Páginas con CSS compilado, fuentes, iconos, estados de carga y accesibilidad`

---

## PASO 5 · JavaScript

Delete `avance1/js/app.js` (it is empty). Its role is taken by `sesion.js`.

Shared modules are IIFEs attached to `window.IH`. Page scripts are wrapped in an IIFE too, so their `const` declarations never leak into the shared global scope.

### `avance1/js/ui.js` (full replacement)

```js
window.IH = window.IH || {};

IH.ui = (function () {
  let rutaBase = '';

  function escapar(texto) {
    return String(texto ?? '').replace(/[&<>"']/g, (c) => ({
      '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;'
    }[c]));
  }

  const ETIQUETAS = {
    tipo: { idea: 'Idea', necesidad: 'Necesidad', reto: 'Reto' },
    estado: {
      borrador: 'Borrador', publicada: 'Publicada', en_formacion: 'En formación de equipo',
      proyecto: 'Convertida en proyecto', archivada: 'Archivada'
    },
    visibilidad: { 'pública': 'Pública', institucional: 'Institucional', restringida: 'Restringida', privada: 'Privada' },
    nivel: { basico: 'básico', intermedio: 'intermedio', avanzado: 'avanzado' },
    solicitud: { pendiente: 'Pendiente', aceptada: 'Aceptada', rechazada: 'Rechazada', cancelada: 'Cancelada' },
    tipoMiembro: { estudiante: 'Estudiante', docente: 'Docente', administrativo: 'Administrativo' }
  };

  const ICONOS = {
    tipo: { idea: 'bi-lightbulb-fill', necesidad: 'bi-life-preserver', reto: 'bi-flag-fill' },
    estado: {
      borrador: 'bi-pencil-square', publicada: 'bi-broadcast', en_formacion: 'bi-people-fill',
      proyecto: 'bi-rocket-takeoff-fill', archivada: 'bi-archive'
    },
    visibilidad: { 'pública': 'bi-globe2', institucional: 'bi-building', restringida: 'bi-eye-slash', privada: 'bi-lock-fill' },
    solicitud: { pendiente: 'bi-hourglass-split', aceptada: 'bi-check-circle-fill', rechazada: 'bi-x-circle-fill', cancelada: 'bi-slash-circle' },
    tipoMiembro: { estudiante: 'bi-mortarboard', docente: 'bi-easel', administrativo: 'bi-briefcase' }
  };

  const COLOR = {
    tipo: { idea: 'primary', necesidad: 'success', reto: 'warning' },
    estado: { borrador: 'light', publicada: 'light', en_formacion: 'light', proyecto: 'info', archivada: 'secondary' },
    solicitud: { pendiente: 'warning', aceptada: 'success', rechazada: 'danger', cancelada: 'secondary' }
  };

  function icono(clase) {
    return `<i class="bi ${escapar(clase)}" aria-hidden="true"></i>`;
  }
  function iconoDe(grupo, clave) {
    return ICONOS[grupo]?.[clave] ?? 'bi-tag';
  }
  function etiqueta(grupo, clave) {
    return ETIQUETAS[grupo]?.[clave] ?? clave;
  }

  function badge(grupo, clave) {
    const color = COLOR[grupo]?.[clave] ?? 'light';
    const borde = color === 'light' ? ' border' : '';
    return `<span class="badge text-bg-${color}${borde}">${icono(iconoDe(grupo, clave))} ${escapar(etiqueta(grupo, clave))}</span>`;
  }
  function badgeTipo(tipo) { return badge('tipo', tipo); }
  function badgeEstado(estado) { return badge('estado', estado); }
  function badgeSolicitud(estado) { return badge('solicitud', estado); }
  function badgeVisibilidad(visibilidad) {
    return `<span class="chip">${icono(iconoDe('visibilidad', visibilidad))} ${escapar(etiqueta('visibilidad', visibilidad))}</span>`;
  }

  function chip(texto, opciones = {}) {
    const clases = 'chip' + (opciones.neutro ? ' chip--neutro' : '');
    const ic = opciones.icono ? icono(opciones.icono) + ' ' : '';
    const nivel = opciones.nivel ? ` <span class="chip--nivel">· ${escapar(etiqueta('nivel', opciones.nivel))}</span>` : '';
    return `<span class="${clases}">${ic}${escapar(texto)}${nivel}</span>`;
  }

  function iniciales(nombre) {
    return nombre.split(/\s+/).slice(0, 2).map((p) => p[0]).join('').toUpperCase();
  }

  // Sin imágenes de perfil: iniciales sobre un círculo de color.
  function avatar(usuario, grande = false) {
    const clase = 'avatar' + (grande ? ' avatar--lg' : '');
    if (!usuario) return `<span class="${clase}" aria-hidden="true">?</span>`;
    return `<span class="${clase}" role="img" aria-label="${escapar(usuario.nombre)}">${escapar(iniciales(usuario.nombre))}</span>`;
  }

  function formatearFecha(iso) {
    if (!iso) return 'sin fecha';
    return new Date(iso + 'T00:00:00').toLocaleDateString('es-CR', { day: 'numeric', month: 'short', year: 'numeric' });
  }

  function tarjetaIniciativa(ini, ctx) {
    const categoria = ctx.categorias.get(ini.categoria);
    const autor = ctx.usuarios.get(ini.propietario)?.nombre ?? 'Desconocido';
    const espacios = Math.max(0, ini.participantesEstimados - ini.equipo.length - 1);
    const competencias = ini.competencias.slice(0, 3)
      .map((id) => `<li>${chip(ctx.competencias.get(id)?.nombre ?? id)}</li>`).join('');
    return `<article class="card h-100 tarjeta-iniciativa tarjeta-iniciativa--${escapar(ini.tipo)}">
      <div class="card-body d-flex flex-column">
        <div class="d-flex justify-content-between align-items-start gap-2">
          <h2 class="card-title mb-0">${escapar(ini.titulo)}</h2>
          ${badgeTipo(ini.tipo)}
        </div>
        <p class="card-text small text-body-secondary mt-2">${escapar(ini.resumen)}</p>
        <ul class="lista-chips mb-3">${competencias}</ul>
        <p class="small mb-1">${icono(categoria?.icono ?? 'bi-tag')} ${escapar(categoria?.nombre ?? 'Sin categoría')} · ${icono('bi-person')} ${escapar(autor)}</p>
        <p class="small mb-3 d-flex flex-wrap gap-2 align-items-center">
          ${badgeEstado(ini.estado)}
          <span class="text-body-secondary">${icono('bi-person-plus')} ${ini.equipo.length + 1} ${ini.equipo.length + 1 === 1 ? 'integrante' : 'integrantes'} · ${espacios} ${espacios === 1 ? 'espacio' : 'espacios'}</span>
        </p>
        <a class="btn btn-outline-primary btn-sm mt-auto align-self-start" href="${rutaBase}paginas/detalle.html?id=${encodeURIComponent(ini.id)}">Ver detalle ${icono('bi-arrow-right')}</a>
      </div>
    </article>`;
  }

  function renderNavbar(paginaActual) {
    const enlaces = [
      ['index.html', 'Inicio', 'bi-house'],
      ['paginas/catalogo.html', 'Iniciativas', 'bi-grid-3x3-gap'],
      ['paginas/perfil.html', 'Mi perfil', 'bi-person-circle']
    ];
    const items = enlaces.map(([href, texto, ic]) => {
      const activo = href.endsWith(paginaActual);
      return `<li class="nav-item"><a class="nav-link${activo ? ' active' : ''}"${activo ? ' aria-current="page"' : ''} href="${rutaBase}${href}">${icono(ic)} ${texto}</a></li>`;
    }).join('');
    return `<nav class="navbar navbar-expand-md" aria-label="Navegación principal">
      <div class="container">
        <a class="navbar-brand" href="${rutaBase}index.html">
          <img src="${rutaBase}img/logo.svg" alt="" width="28" height="28"><span>Innovation Hub</span>
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navPrincipal" aria-controls="navPrincipal" aria-expanded="false" aria-label="Mostrar u ocultar la navegación">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navPrincipal">
          <ul class="navbar-nav me-auto mb-2 mb-md-0">${items}</ul>
          <div class="d-flex flex-column flex-md-row align-items-md-center gap-2">
            <label class="ver-como mb-0" for="ver-como">${icono('bi-person-badge')} Ver como
              <select id="ver-como" class="form-select form-select-sm"><option value="visitante">Visitante</option></select>
            </label>
            <a class="btn btn-primary btn-sm" href="${rutaBase}paginas/formulario.html">${icono('bi-plus-circle-fill')} Publicar iniciativa</a>
          </div>
        </div>
      </div>
    </nav>`;
  }

  function montarNavbar(paginaActual, base = '') {
    rutaBase = base;
    document.getElementById('navbar').innerHTML = renderNavbar(paginaActual);
    document.getElementById('ver-como').addEventListener('change', (e) => IH.sesion.cambiar(e.target.value));
  }

  function actualizarVerComo(usuarios) {
    const sel = document.getElementById('ver-como');
    if (!sel) return;
    sel.innerHTML = '<option value="visitante">Visitante</option>' +
      usuarios.map((u) => `<option value="${escapar(u.id)}">${escapar(u.nombre)}</option>`).join('');
    sel.value = IH.sesion.idActual();
    if (sel.selectedIndex < 0) sel.value = 'visitante';
  }

  function toast(mensaje, tipo = 'success') {
    let cont = document.getElementById('toasts');
    if (!cont) {
      cont = document.createElement('div');
      cont.id = 'toasts';
      cont.className = 'toast-container position-fixed bottom-0 end-0 p-3';
      document.body.appendChild(cont);
    }
    const textoOscuro = ['success', 'warning', 'light'].includes(tipo);
    const el = document.createElement('div');
    el.className = `toast align-items-center text-bg-${tipo} border-0`;
    el.setAttribute('role', 'status');
    el.setAttribute('aria-live', 'polite');
    el.innerHTML = `<div class="d-flex">
      <div class="toast-body">${mensaje}</div>
      <button type="button" class="btn-close ${textoOscuro ? '' : 'btn-close-white'} me-2 m-auto" data-bs-dismiss="toast" aria-label="Cerrar"></button>
    </div>`;
    cont.appendChild(el);
    el.addEventListener('hidden.bs.toast', () => el.remove());
    bootstrap.Toast.getOrCreateInstance(el, { delay: 4500 }).show();
  }

  const MENSAJES_URL = {
    creada: ['Iniciativa publicada.', 'success'],
    editada: ['Cambios guardados.', 'success'],
    eliminada: ['Iniciativa eliminada.', 'secondary'],
    archivada: ['La iniciativa tenía equipo o solicitudes: se archivó en lugar de eliminarse.', 'info'],
    solicitud: ['Solicitud enviada. Queda pendiente hasta que el propietario la resuelva.', 'success']
  };

  function mostrarMensajeDeUrl() {
    const params = new URLSearchParams(window.location.search);
    const clave = params.get('msg');
    if (!clave || !MENSAJES_URL[clave]) return;
    toast(...MENSAJES_URL[clave]);
    params.delete('msg');
    const consulta = params.toString();
    window.history.replaceState(null, '', window.location.pathname + (consulta ? '?' + consulta : ''));
  }

  async function cargarPagina(base, alCargar) {
    const cargando = document.getElementById('cargando');
    const error = document.getElementById('error');
    try {
      const datos = await IH.datos.cargarDatos(base + 'datos/');
      IH.almacen.inicializar(datos);
      actualizarVerComo(datos.usuarios);
      if (cargando) cargando.hidden = true;
      await alCargar(datos);
    } catch (e) {
      console.error(e);
      if (cargando) cargando.hidden = true;
      if (!error) return;
      const esArchivo = window.location.protocol === 'file:';
      error.hidden = false;
      error.innerHTML = `
        <img src="${base}img/error-carga.svg" alt="" width="220" height="160" class="mb-3">
        <h2 class="h5">No se pudieron cargar los datos</h2>
        <p class="text-body-secondary mb-3">${esArchivo
          ? 'El navegador bloquea la lectura de archivos JSON cuando la página se abre directamente desde el disco. Abrí el proyecto en VS Code, hacé clic derecho sobre <code>avance1/index.html</code> y elegí <strong>Open with Live Server</strong>.'
          : 'Ocurrió un error al leer los archivos JSON. Recargá la página para volver a intentar.'}</p>
        <button type="button" class="btn btn-outline-primary btn-sm" onclick="window.location.reload()">${icono('bi-arrow-clockwise')} Reintentar</button>`;
    }
  }

  return {
    escapar, icono, iconoDe, etiqueta, badgeTipo, badgeEstado, badgeSolicitud, badgeVisibilidad,
    chip, avatar, formatearFecha, tarjetaIniciativa, montarNavbar, actualizarVerComo,
    toast, mostrarMensajeDeUrl, cargarPagina
  };
})();
```

### `avance1/js/sesion.js` (new)

```js
window.IH = window.IH || {};

// Simulación de sesión para el Avance 1. No hay autenticación real:
// el selector "Ver como" del navbar guarda el usuario elegido en localStorage.
IH.sesion = (function () {
  const CLAVE = 'ih.sesion';
  const POR_DEFECTO = 'u1';

  function idActual() {
    return localStorage.getItem(CLAVE) ?? POR_DEFECTO;
  }

  function usuarioActual() {
    const id = idActual();
    if (id === 'visitante') return null;
    return IH.almacen.obtenerUsuario(id) ?? null;
  }

  function cambiar(id) {
    localStorage.setItem(CLAVE, id || 'visitante');
    window.location.reload();
  }

  return { idActual, usuarioActual, cambiar };
})();
```

### `avance1/js/datos.js` (unchanged, shown for completeness)

```js
window.IH = window.IH || {};

IH.datos = (function () {
  async function cargarJSON(ruta) {
    const respuesta = await fetch(ruta);
    if (!respuesta.ok) throw new Error(`No se pudo cargar ${ruta} (${respuesta.status})`);
    return respuesta.json();
  }

  async function cargarDatos(base = 'datos/') {
    const [iniciativas, categorias, competencias, usuarios, solicitudes] = await Promise.all([
      cargarJSON(base + 'iniciativas.json'),
      cargarJSON(base + 'categorias.json'),
      cargarJSON(base + 'competencias.json'),
      cargarJSON(base + 'usuarios.json'),
      cargarJSON(base + 'solicitudes.json')
    ]);
    return { iniciativas, categorias, competencias, usuarios, solicitudes };
  }

  return { cargarJSON, cargarDatos };
})();
```

### `avance1/js/almacen.js` (full replacement)

```js
window.IH = window.IH || {};

// Estado en memoria. Los JSON son el modelo principal; localStorage solo
// conserva altas, cambios y solicitudes entre páginas dentro del prototipo.
IH.almacen = (function () {
  const CLAVE = 'ih.datos.v1';
  let estado = { iniciativas: [], categorias: [], competencias: [], usuarios: [], solicitudes: [] };
  const mapas = {};

  function hoy() { return new Date().toISOString().slice(0, 10); }

  function leerLocal() {
    try { return JSON.parse(localStorage.getItem(CLAVE)); } catch { return null; }
  }
  function guardar() {
    localStorage.setItem(CLAVE, JSON.stringify({ iniciativas: estado.iniciativas, solicitudes: estado.solicitudes }));
  }
  function hayCambiosLocales() { return localStorage.getItem(CLAVE) !== null; }
  function restablecer() { localStorage.removeItem(CLAVE); }

  function inicializar(datos) {
    const local = leerLocal();
    estado = {
      ...datos,
      iniciativas: local?.iniciativas ?? datos.iniciativas,
      solicitudes: local?.solicitudes ?? datos.solicitudes
    };
    mapas.categorias = new Map(estado.categorias.map((c) => [c.id, c]));
    mapas.competencias = new Map(estado.competencias.map((c) => [c.id, c]));
    mapas.usuarios = new Map(estado.usuarios.map((u) => [u.id, u]));
  }

  function obtenerIniciativas() { return estado.iniciativas; }
  function obtenerIniciativa(id) { return estado.iniciativas.find((i) => i.id === id); }
  function obtenerSolicitudes() { return estado.solicitudes; }
  function obtenerUsuario(id) { return mapas.usuarios.get(id); }
  function contexto() {
    return { categorias: mapas.categorias, competencias: mapas.competencias, usuarios: mapas.usuarios };
  }

  function actualizarIniciativa(id, cambios) {
    estado.iniciativas = estado.iniciativas.map((i) => (i.id === id ? { ...i, ...cambios, fechaModificacion: hoy() } : i));
    guardar();
  }

  function agregarIniciativa(datos) {
    const ini = { ...datos, fechaCreacion: hoy(), fechaModificacion: hoy() };
    estado.iniciativas = [...estado.iniciativas, ini];
    guardar();
    return ini;
  }

  // Información que RF-I-INI-04 pide mostrar antes de eliminar o archivar.
  function relaciones(id) {
    const ini = obtenerIniciativa(id);
    return {
      miembros: ini?.equipo.length ?? 0,
      solicitudes: estado.solicitudes.filter((s) => s.iniciativaId === id).length,
      tieneProyecto: ini?.estado === 'proyecto'
    };
  }

  function eliminarOArchivar(id) {
    const r = relaciones(id);
    if (r.miembros > 0 || r.solicitudes > 0 || r.tieneProyecto) {
      actualizarIniciativa(id, { estado: 'archivada' });
      return 'archivada';
    }
    estado.iniciativas = estado.iniciativas.filter((i) => i.id !== id);
    guardar();
    return 'eliminada';
  }

  function agregarSolicitud(solicitud) {
    estado.solicitudes = [...estado.solicitudes, solicitud];
    guardar();
  }

  return {
    inicializar, obtenerIniciativas, obtenerIniciativa, obtenerSolicitudes, obtenerUsuario, contexto,
    actualizarIniciativa, agregarIniciativa, relaciones, eliminarOArchivar, agregarSolicitud,
    hayCambiosLocales, restablecer
  };
})();
```

### `avance1/js/reglas.js` (full replacement)

```js
window.IH = window.IH || {};

IH.reglas = (function () {
  function esPropietario(ini, usuario) {
    return !!usuario && ini.propietario === usuario.id;
  }
  function esMiembro(ini, usuario) {
    return !!usuario && ini.equipo.includes(usuario.id);
  }

  // RN-20: toda consulta respeta la visibilidad. Propietario y equipo siempre ven todo.
  function nivelDeAcceso(ini, usuario) {
    if (esPropietario(ini, usuario) || esMiembro(ini, usuario)) return 'completo';
    switch (ini.visibilidad) {
      case 'pública': return 'completo';
      case 'institucional': return usuario ? 'completo' : 'bloqueado';
      case 'restringida': return 'resumen';
      case 'privada': return 'bloqueado';
      default: return 'bloqueado';
    }
  }

  // Las privadas no aparecen en el catálogo general salvo para propietario y equipo.
  function visibleEnCatalogo(ini, usuario) {
    return ini.visibilidad !== 'privada' || esPropietario(ini, usuario) || esMiembro(ini, usuario);
  }

  // RN-16: el propietario cuenta como integrante sin solicitud.
  function espaciosDisponibles(ini) {
    return Math.max(0, ini.participantesEstimados - ini.equipo.length - 1);
  }

  // RN-10, RN-11, RN-15
  function puedeSolicitar(ini, usuario, solicitudes) {
    if (!usuario) return { ok: false, motivo: 'Elegí un usuario en "Ver como" para simular la sesión y poder solicitar participación.' };
    if (esPropietario(ini, usuario)) return { ok: false, motivo: 'No podés solicitar participación en tu propia iniciativa.' };
    if (esMiembro(ini, usuario)) return { ok: false, motivo: 'Ya formás parte del equipo de esta iniciativa.' };
    if (['archivada', 'proyecto'].includes(ini.estado)) return { ok: false, motivo: 'Esta iniciativa ya no recibe solicitudes.' };
    if (espaciosDisponibles(ini) === 0) return { ok: false, motivo: 'La iniciativa ya completó los espacios previstos.' };
    const yaPendiente = solicitudes.some(
      (s) => s.iniciativaId === ini.id && s.usuarioId === usuario.id && s.estado === 'pendiente'
    );
    if (yaPendiente) return { ok: false, motivo: 'Ya tenés una solicitud pendiente para esta iniciativa.' };
    return { ok: true, motivo: '' };
  }

  // RN-17, RN-18
  function puedeConvertirseEnProyecto(ini) {
    return ini.equipo.length >= 1 && !['proyecto', 'archivada'].includes(ini.estado);
  }

  // RF-A-INI-03
  function puedeEditarse(ini) {
    return ini.estado !== 'archivada';
  }

  return { esPropietario, esMiembro, nivelDeAcceso, visibleEnCatalogo, espaciosDisponibles, puedeSolicitar, puedeConvertirseEnProyecto, puedeEditarse };
})();
```

### `avance1/js/validacion.js` (full replacement)

```js
window.IH = window.IH || {};

IH.validacion = (function () {
  function requerido(valor) {
    return valor && String(valor).trim().length > 0 ? null : 'Este campo es obligatorio.';
  }
  function longitudMinima(valor, min) {
    return String(valor).trim().length >= min ? null : `Debe tener al menos ${min} caracteres.`;
  }
  function longitudMaxima(valor, max) {
    return String(valor).trim().length <= max ? null : `No debe superar los ${max} caracteres.`;
  }
  function rango(valor, min, max) {
    const n = Number(valor);
    if (valor === '' || Number.isNaN(n)) return 'Debe ser un número.';
    return n >= min && n <= max ? null : `Debe estar entre ${min} y ${max}.`;
  }
  function minimo(valor, min, mensaje) {
    return Number(valor) >= min ? null : mensaje;
  }
  function formato(valor, regex, mensaje) {
    return regex.test(String(valor).trim()) ? null : mensaje;
  }
  function seleccionValida(valor, opciones) {
    if (!valor) return 'Elegí una opción.';
    return opciones.includes(valor) ? null : 'Elegí una opción válida.';
  }
  function cantidadMinima(arreglo, min) {
    return arreglo.length >= min ? null : `Agregá al menos ${min} elemento${min === 1 ? '' : 's'}.`;
  }
  function cantidadMaxima(arreglo, max) {
    return arreglo.length <= max ? null : `Podés indicar como máximo ${max}.`;
  }
  // Ejecuta reglas en orden y devuelve el primer mensaje de error.
  function combinar(...reglas) {
    for (const regla of reglas) {
      const mensaje = regla();
      if (mensaje) return mensaje;
    }
    return null;
  }

  function mostrarError(idCampo, mensaje) {
    const campo = document.getElementById(idCampo);
    const feedback = document.getElementById('error-' + idCampo);
    if (feedback) {
      feedback.innerHTML = mensaje
        ? `<i class="bi bi-exclamation-triangle-fill" aria-hidden="true"></i> ${IH.ui.escapar(mensaje)}`
        : '';
      feedback.hidden = !mensaje;
    }
    if (campo) {
      campo.classList.toggle('is-invalid', !!mensaje);
      campo.setAttribute('aria-invalid', mensaje ? 'true' : 'false');
      if (feedback) campo.setAttribute('aria-describedby', feedback.id);
    }
  }

  // campos: [[idCampo, () => mensaje | null], ...]. Enfoca el primer campo inválido.
  function validarCampos(campos) {
    let primerInvalido = null;
    campos.forEach(([id, validar]) => {
      const mensaje = validar();
      mostrarError(id, mensaje);
      if (mensaje && !primerInvalido) primerInvalido = id;
    });
    if (primerInvalido) document.getElementById(primerInvalido)?.focus();
    return primerInvalido === null;
  }

  return {
    requerido, longitudMinima, longitudMaxima, rango, minimo, formato, seleccionValida,
    cantidadMinima, cantidadMaxima, combinar, mostrarError, validarCampos
  };
})();
```

### `avance1/js/inicio.js` (new; replaces the inline script of index.html)

```js
(() => {
  IH.ui.montarNavbar('index.html', '');

  // La portada funciona sin datos. Si el fetch falla (por ejemplo, abierta con
  // doble clic desde el disco), simplemente no se muestran las cifras.
  IH.datos.cargarDatos('datos/').then((datos) => {
    IH.almacen.inicializar(datos);
    IH.ui.actualizarVerComo(datos.usuarios);
    const activas = IH.almacen.obtenerIniciativas().filter((i) => i.estado !== 'archivada');
    document.getElementById('n-iniciativas').textContent = activas.length;
    document.getElementById('n-proyectos').textContent = activas.filter((i) => i.estado === 'proyecto').length;
    document.getElementById('n-personas').textContent = datos.usuarios.length;
    document.getElementById('n-competencias').textContent = datos.competencias.length;
    document.getElementById('estadisticas').hidden = false;
  }).catch((e) => console.warn('Cifras no disponibles:', e.message));
})();
```

### `avance1/js/catalogo.js` (full replacement)

```js
(() => {
  IH.ui.montarNavbar('catalogo.html', '../');

  const filtros = { texto: '', tipo: '', categoria: '', competencia: '' };
  const $ = (id) => document.getElementById(id);

  function llenarSelect(select, opciones, etiqueta) {
    select.innerHTML = `<option value="">${etiqueta}</option>` +
      opciones.map((o) => `<option value="${o.id}">${IH.ui.escapar(o.nombre)}</option>`).join('');
  }

  // Búsqueda textual sobre título, resumen y etiquetas (RF-I-INI-05).
  function coincide(ini) {
    const texto = filtros.texto.trim().toLowerCase();
    const enTexto = !texto || [ini.titulo, ini.resumen, ...ini.etiquetas].some((t) => t.toLowerCase().includes(texto));
    const enTipo = !filtros.tipo || ini.tipo === filtros.tipo;
    const enCategoria = !filtros.categoria || ini.categoria === filtros.categoria;
    const enCompetencia = !filtros.competencia || ini.competencias.includes(filtros.competencia);
    return enTexto && enTipo && enCategoria && enCompetencia;
  }

  function renderLista() {
    const ctx = IH.almacen.contexto();
    const usuario = IH.sesion.usuarioActual();
    const visibles = IH.almacen.obtenerIniciativas()
      .filter((i) => IH.reglas.visibleEnCatalogo(i, usuario))
      .filter(coincide);
    $('lista').innerHTML = visibles.map((i) => IH.ui.tarjetaIniciativa(i, ctx)).join('');
    $('vacio').hidden = visibles.length > 0;
    $('conteo').textContent = visibles.length === 1 ? '1 iniciativa' : `${visibles.length} iniciativas`;
  }

  function limpiar() {
    Object.keys(filtros).forEach((k) => (filtros[k] = ''));
    renderLista();
  }

  $('filtros').addEventListener('input', (e) => {
    if (e.target.name) filtros[e.target.name] = e.target.value;
    renderLista();
  });
  // El evento reset se dispara antes de que el navegador vacíe los campos.
  $('filtros').addEventListener('reset', () => setTimeout(limpiar, 0));
  $('limpiar-desde-vacio').addEventListener('click', () => $('filtros').reset());

  IH.ui.cargarPagina('../', (datos) => {
    llenarSelect($('f-categoria'), datos.categorias, 'Todas');
    llenarSelect($('f-competencia'), datos.competencias, 'Todas');
    renderLista();
    IH.ui.mostrarMensajeDeUrl();
  });
})();
```

### `avance1/js/detalle.js` (full replacement)

```js
(() => {
  IH.ui.montarNavbar('detalle.html', '../');
  const U = IH.ui;
  const R = IH.reglas;
  const $ = (id) => document.getElementById(id);

  function obtenerIdDeUrl() {
    return new URLSearchParams(window.location.search).get('id');
  }

  function renderBloqueado(ini) {
    const motivo = ini.visibilidad === 'institucional'
      ? 'requiere iniciar sesión. Elegí un usuario en "Ver como" para continuar.'
      : 'solo la pueden consultar el propietario y los miembros de su equipo.';
    $('detalle').innerHTML = `
      <div class="alert alert-danger d-flex gap-2" role="alert">
        ${U.icono('bi-lock-fill')}
        <div><strong>Sin acceso.</strong> Esta iniciativa es <strong>${U.escapar(U.etiqueta('visibilidad', ini.visibilidad).toLowerCase())}</strong> y ${motivo}</div>
      </div>
      <a class="btn btn-outline-primary" href="catalogo.html">${U.icono('bi-arrow-left')} Volver al catálogo</a>`;
  }

  function renderAcciones(ini, usuario) {
    if (!R.esPropietario(ini, usuario)) {
      return `<a class="btn btn-primary" href="solicitud.html?id=${encodeURIComponent(ini.id)}">${U.icono('bi-send')} Solicitar participación</a>`;
    }
    const puedeConvertir = R.puedeConvertirseEnProyecto(ini);
    const notaConvertir = puedeConvertir ? '' : `<p class="small text-body-secondary mb-0 mt-1">${
      ini.estado === 'proyecto' ? 'Esta iniciativa ya es un proyecto.'
      : ini.estado === 'archivada' ? 'Una iniciativa archivada no puede convertirse en proyecto.'
      : 'Para convertirla en proyecto necesita al menos un integrante además del propietario.'}</p>`;
    const editar = R.puedeEditarse(ini)
      ? `<a class="btn btn-outline-primary" href="formulario.html?id=${encodeURIComponent(ini.id)}">${U.icono('bi-pencil')} Editar</a>`
      : `<button type="button" class="btn btn-outline-primary" disabled>${U.icono('bi-pencil')} Editar</button>`;
    return `${editar}
      <button type="button" class="btn btn-outline-danger" id="btn-eliminar">${U.icono('bi-trash3')} Eliminar</button>
      <button type="button" class="btn btn-primary" id="btn-convertir" ${puedeConvertir ? '' : 'disabled'}>${U.icono('bi-rocket-takeoff')} Convertir en proyecto</button>
      ${notaConvertir}`;
  }

  function render(ini) {
    const ctx = IH.almacen.contexto();
    const usuario = IH.sesion.usuarioActual();
    const nivel = R.nivelDeAcceso(ini, usuario);

    if (nivel === 'bloqueado') { renderBloqueado(ini); return; }

    const categoria = ctx.categorias.get(ini.categoria);
    const propietario = ctx.usuarios.get(ini.propietario);
    const espacios = R.espaciosDisponibles(ini);

    const competencias = ini.competencias
      .map((id) => `<li>${U.chip(ctx.competencias.get(id)?.nombre ?? id, { icono: 'bi-stars' })}</li>`).join('');
    const etiquetas = ini.etiquetas.map((t) => `<li>${U.chip('#' + t, { neutro: true })}</li>`).join('')
      || '<li class="small text-body-secondary">Sin etiquetas</li>';
    const equipo = [ini.propietario, ...ini.equipo].map((id) => {
      const u = ctx.usuarios.get(id);
      const rol = id === ini.propietario ? 'propietario' : (u?.carrera ?? '');
      return `<li class="d-flex align-items-center gap-2 mb-2">${U.avatar(u)}
        <span>${U.escapar(u?.nombre ?? id)} <span class="small text-body-secondary">· ${U.escapar(rol)}</span></span></li>`;
    }).join('');

    const cuerpo = nivel === 'resumen'
      ? `<div class="alert alert-warning d-flex gap-2" role="status">
           ${U.icono('bi-info-circle-fill')}
           <div><strong>Iniciativa restringida:</strong> solo se muestra el resumen. La descripción completa es visible para el propietario y los miembros del equipo.</div>
         </div>
         <p class="lead">${U.escapar(ini.resumen)}</p>`
      : `<p class="lead">${U.escapar(ini.resumen)}</p>
         <p>${U.escapar(ini.descripcion)}</p>
         <div class="row g-3 mt-2">
           <div class="col-md-6"><div class="bloque-acento h-100"><h2 class="seccion-titulo">Problema identificado</h2><p class="mb-0">${U.escapar(ini.problema)}</p></div></div>
           <div class="col-md-6"><div class="bloque-acento bloque-acento--teal h-100"><h2 class="seccion-titulo">Beneficiarios</h2><p class="mb-0">${U.escapar(ini.beneficiarios)}</p></div></div>
         </div>
         <h2 class="seccion-titulo mt-4">Etiquetas</h2>
         <ul class="lista-chips">${etiquetas}</ul>`;

    $('detalle').innerHTML = `<article class="row g-4">
      <div class="col-lg-8">
        <a class="small text-body-secondary d-inline-block mb-2" href="catalogo.html">${U.icono('bi-arrow-left')} Volver al catálogo</a>
        <h1 class="h2 mb-2">${U.escapar(ini.titulo)}</h1>
        <div class="d-flex flex-wrap gap-2 mb-2">${U.badgeTipo(ini.tipo)} ${U.badgeEstado(ini.estado)} ${U.badgeVisibilidad(ini.visibilidad)}</div>
        <p class="small text-body-secondary d-flex flex-wrap gap-3 mb-3">
          <span>${U.icono(categoria?.icono ?? 'bi-tag')} ${U.escapar(categoria?.nombre ?? 'Sin categoría')}</span>
          <span>${U.icono('bi-person')} ${U.escapar(propietario?.nombre ?? 'Desconocido')}</span>
          <span>${U.icono('bi-calendar-event')} Publicada ${U.formatearFecha(ini.fechaCreacion)}</span>
          <span>${U.icono('bi-pencil')} Editada ${U.formatearFecha(ini.fechaModificacion)}</span>
        </p>
        ${cuerpo}
      </div>
      <aside class="col-lg-4">
        <div class="card tarjeta-lateral"><div class="card-body">
          <div class="row g-2 mb-3">
            <div class="col-6"><div class="metrica"><div class="seccion-titulo">Integrantes</div><div class="valor">${ini.equipo.length + 1} <small class="fs-6 fw-normal text-body-secondary">de ${ini.participantesEstimados}</small></div></div></div>
            <div class="col-6"><div class="metrica metrica--acento"><div class="seccion-titulo">Espacios</div><div class="valor">${espacios} <small class="fs-6 fw-normal">libres</small></div></div></div>
          </div>
          <h2 class="seccion-titulo">Equipo</h2>
          <ul class="list-unstyled mb-3">${equipo}</ul>
          <h2 class="seccion-titulo">Competencias requeridas</h2>
          <ul class="lista-chips mb-4">${competencias}</ul>
          <div class="d-grid gap-2">${renderAcciones(ini, usuario)}</div>
        </div></div>
      </aside>
    </article>`;

    $('btn-convertir')?.addEventListener('click', () => {
      IH.almacen.actualizarIniciativa(ini.id, { estado: 'proyecto' });
      U.toast('La iniciativa ahora es un proyecto.', 'info');
      render(IH.almacen.obtenerIniciativa(ini.id));
    });
    $('btn-eliminar')?.addEventListener('click', () => abrirConfirmacion(ini));
  }

  // RF-I-INI-04: mostrar título, acción y mensaje antes de confirmar.
  function abrirConfirmacion(ini) {
    const r = IH.almacen.relaciones(ini.id);
    const archivar = r.miembros > 0 || r.solicitudes > 0 || r.tieneProyecto;
    $('modal-titulo').innerHTML = `${U.icono('bi-exclamation-triangle-fill')} <span class="ms-1">${archivar ? 'Archivar iniciativa' : 'Eliminar iniciativa'}</span>`;
    $('modal-nombre').textContent = ini.titulo;
    $('modal-mensaje').textContent = archivar
      ? `Tiene ${r.miembros} integrante(s) y ${r.solicitudes} solicitud(es), así que se archivará en lugar de eliminarse. ¿Querés continuar?`
      : 'No tiene equipo ni solicitudes: se eliminará de forma definitiva. ¿Querés continuar?';
    const btn = $('modal-confirmar');
    btn.innerHTML = archivar ? `${U.icono('bi-archive')} Archivar` : `${U.icono('bi-trash3')} Eliminar`;
    // Asignación directa: nunca se acumulan escuchas al reabrir el modal.
    btn.onclick = () => {
      const resultado = IH.almacen.eliminarOArchivar(ini.id);
      window.location.href = `catalogo.html?msg=${resultado}`;
    };
    bootstrap.Modal.getOrCreateInstance($('modal-confirmacion')).show();
  }

  IH.ui.cargarPagina('../', () => {
    const ini = IH.almacen.obtenerIniciativa(obtenerIdDeUrl());
    $('detalle').hidden = false;
    if (!ini) {
      $('detalle').innerHTML = `
        <div class="alert alert-warning d-flex gap-2" role="alert">${U.icono('bi-question-circle')}<div>No encontramos esa iniciativa. Puede haberse eliminado.</div></div>
        <a class="btn btn-outline-primary" href="catalogo.html">${U.icono('bi-arrow-left')} Volver al catálogo</a>`;
      return;
    }
    document.title = `${ini.titulo} · Innovation Hub`;
    render(ini);
    IH.ui.mostrarMensajeDeUrl();
  });
})();
```

### `avance1/js/formulario.js` (full replacement)

```js
(() => {
  IH.ui.montarNavbar('formulario.html', '../');
  const U = IH.ui;
  const V = IH.validacion;
  const $ = (id) => document.getElementById(id);

  const LIMITES = { titulo: 80, resumen: 160, etiquetas: 5 };
  const REGEX_ETIQUETA = /^[\p{L}\p{N}][\p{L}\p{N}\- ]{1,29}$/u;
  const TIPOS = ['idea', 'necesidad', 'reto'];
  const VISIBILIDADES = ['pública', 'institucional', 'restringida', 'privada'];

  let ctx = null;
  let competenciasElegidas = [];
  let idEnEdicion = null;
  let iniciativaOriginal = null;

  const val = (id) => $(id).value;

  function renderCompetenciasElegidas() {
    $('competencias-elegidas').innerHTML = competenciasElegidas.map((id) => {
      const nombre = ctx.competencias.get(id)?.nombre ?? id;
      return `<li class="badge text-bg-primary competencia-elegida">${U.escapar(nombre)}
        <button type="button" class="btn-close btn-close-white" data-id="${id}" aria-label="Quitar ${U.escapar(nombre)}"></button></li>`;
    }).join('') || '<li class="small text-body-secondary">Todavía no agregaste competencias.</li>';
    // Las ya elegidas se deshabilitan en el select para evitar duplicados.
    [...$('competencia-nueva').options].forEach((o) => { if (o.value) o.disabled = competenciasElegidas.includes(o.value); });
  }

  function leerEtiquetas() {
    return val('etiquetas').split(',').map((t) => t.trim()).filter(Boolean);
  }

  function actualizarContador(id) {
    $('contador-' + id).textContent = `${val(id).length} / ${LIMITES[id]}`;
  }

  function validarFormulario() {
    const etiquetas = leerEtiquetas();
    const minimoIntegrantes = iniciativaOriginal ? iniciativaOriginal.equipo.length + 1 : 1;
    return V.validarCampos([
      ['titulo', () => V.combinar(() => V.requerido(val('titulo')), () => V.longitudMinima(val('titulo'), 5), () => V.longitudMaxima(val('titulo'), LIMITES.titulo))],
      ['tipo', () => V.seleccionValida(val('tipo'), TIPOS)],
      ['resumen', () => V.combinar(() => V.requerido(val('resumen')), () => V.longitudMinima(val('resumen'), 10), () => V.longitudMaxima(val('resumen'), LIMITES.resumen))],
      ['descripcion', () => V.combinar(() => V.requerido(val('descripcion')), () => V.longitudMinima(val('descripcion'), 20))],
      ['problema', () => V.combinar(() => V.requerido(val('problema')), () => V.longitudMinima(val('problema'), 10))],
      ['beneficiarios', () => V.requerido(val('beneficiarios'))],
      ['categoria', () => V.seleccionValida(val('categoria'), [...ctx.categorias.keys()])],
      ['participantes', () => V.combinar(
        () => V.rango(val('participantes'), 1, 20),
        () => V.minimo(val('participantes'), minimoIntegrantes, `El equipo ya tiene ${minimoIntegrantes} integrante(s), incluido el propietario. No podés indicar menos.`)
      )],
      ['visibilidad', () => V.seleccionValida(val('visibilidad'), VISIBILIDADES)],
      ['competencias', () => V.cantidadMinima(competenciasElegidas, 1)],
      ['etiquetas', () => V.combinar(
        () => V.cantidadMaxima(etiquetas, LIMITES.etiquetas),
        () => etiquetas.map((t) => V.formato(t, REGEX_ETIQUETA, `La etiqueta "${t}" solo puede tener letras, números, espacios o guiones (2 a 30 caracteres).`)).find(Boolean) ?? null
      )]
    ]);
  }

  function bloquear(mensaje) {
    $('bloqueo').hidden = false;
    $('bloqueo').innerHTML = `${U.icono('bi-person-lock')}<div>${mensaje}</div>`;
  }

  function cargarEnFormulario(ini) {
    const valores = {
      titulo: ini.titulo, tipo: ini.tipo, resumen: ini.resumen, descripcion: ini.descripcion,
      problema: ini.problema, beneficiarios: ini.beneficiarios, categoria: ini.categoria,
      participantes: ini.participantesEstimados, visibilidad: ini.visibilidad, etiquetas: ini.etiquetas.join(', ')
    };
    Object.entries(valores).forEach(([id, v]) => { $(id).value = v; });
    competenciasElegidas = [...ini.competencias];
  }

  $('agregar-competencia').addEventListener('click', () => {
    const sel = $('competencia-nueva');
    if (!sel.value) { V.mostrarError('competencias', 'Elegí una competencia de la lista antes de agregar.'); return; }
    if (!competenciasElegidas.includes(sel.value)) competenciasElegidas.push(sel.value);
    V.mostrarError('competencias', null);
    sel.value = '';
    renderCompetenciasElegidas();
  });

  // Delegación: un solo escucha para todos los botones de quitar.
  $('competencias-elegidas').addEventListener('click', (e) => {
    const boton = e.target.closest('button[data-id]');
    if (!boton) return;
    competenciasElegidas = competenciasElegidas.filter((id) => id !== boton.dataset.id);
    renderCompetenciasElegidas();
  });

  ['titulo', 'resumen'].forEach((id) => $(id).addEventListener('input', () => actualizarContador(id)));

  $('formulario-iniciativa').addEventListener('submit', (e) => {
    e.preventDefault();
    if (!validarFormulario()) return;

    const datos = {
      titulo: val('titulo').trim(),
      tipo: val('tipo'),
      resumen: val('resumen').trim(),
      descripcion: val('descripcion').trim(),
      problema: val('problema').trim(),
      beneficiarios: val('beneficiarios').trim(),
      categoria: val('categoria'),
      competencias: [...competenciasElegidas],
      participantesEstimados: Number(val('participantes')),
      visibilidad: val('visibilidad'),
      etiquetas: leerEtiquetas()
    };

    if (idEnEdicion) {
      IH.almacen.actualizarIniciativa(idEnEdicion, datos);
      window.location.href = `detalle.html?id=${encodeURIComponent(idEnEdicion)}&msg=editada`;
      return;
    }
    IH.almacen.agregarIniciativa({
      id: 'ini-' + Date.now(),
      ...datos,
      propietario: IH.sesion.usuarioActual().id,
      estado: 'publicada',
      equipo: []
    });
    window.location.href = 'catalogo.html?msg=creada';
  });

  IH.ui.cargarPagina('../', (datos) => {
    ctx = IH.almacen.contexto();
    const usuario = IH.sesion.usuarioActual();
    if (!usuario) {
      bloquear('Para publicar una iniciativa necesitás una sesión. Elegí un usuario en <strong>Ver como</strong>, en la barra superior.');
      return;
    }

    $('categoria').innerHTML += datos.categorias.map((c) => `<option value="${c.id}">${U.escapar(c.nombre)}</option>`).join('');
    $('competencia-nueva').innerHTML += datos.competencias.map((c) => `<option value="${c.id}">${U.escapar(c.nombre)}</option>`).join('');

    idEnEdicion = new URLSearchParams(window.location.search).get('id');
    if (idEnEdicion) {
      iniciativaOriginal = IH.almacen.obtenerIniciativa(idEnEdicion);
      if (!iniciativaOriginal) { bloquear('No encontramos la iniciativa que querés editar.'); return; }
      if (!IH.reglas.esPropietario(iniciativaOriginal, usuario)) { bloquear('Solo el propietario puede modificar esta iniciativa (RN-06).'); return; }
      if (!IH.reglas.puedeEditarse(iniciativaOriginal)) { bloquear('Una iniciativa archivada no se puede modificar.'); return; }
      $('titulo-pagina').textContent = 'Editar iniciativa';
      document.title = 'Editar iniciativa · Innovation Hub';
      cargarEnFormulario(iniciativaOriginal);
    }

    renderCompetenciasElegidas();
    ['titulo', 'resumen'].forEach(actualizarContador);
    $('formulario-iniciativa').hidden = false;
  });
})();
```

### `avance1/js/perfil.js` (full replacement)

```js
(() => {
  IH.ui.montarNavbar('perfil.html', '../');
  const U = IH.ui;
  const $ = (id) => document.getElementById(id);

  function renderTarjeta(usuario) {
    const ctx = IH.almacen.contexto();
    const disponible = (usuario.disponibilidad?.horasSemana ?? 0) > 0;
    const competencias = usuario.competencias
      .map((c) => `<li>${U.chip(ctx.competencias.get(c.id)?.nombre ?? c.id, { nivel: c.nivel })}</li>`).join('')
      || '<li class="small text-body-secondary">Sin competencias registradas.</li>';
    const intereses = usuario.intereses.map((i) => `<li>${U.chip('#' + i, { neutro: true })}</li>`).join('')
      || '<li class="small text-body-secondary">Sin intereses registrados.</li>';
    const portafolio = usuario.portafolio
      ? `<p class="small mb-2"><a href="${U.escapar(usuario.portafolio)}" target="_blank" rel="noopener">${U.icono('bi-link-45deg')} ${U.escapar(usuario.portafolio.replace(/^https?:\/\//, ''))}</a></p>`
      : '';

    $('tarjeta-perfil').innerHTML = `
      <div class="text-center">
        ${U.avatar(usuario, true)}
        <h2 class="h4 mt-3 mb-1">${U.escapar(usuario.nombre)}</h2>
        <div class="d-flex justify-content-center flex-wrap gap-2 mb-2">
          ${U.chip(U.etiqueta('tipoMiembro', usuario.tipoMiembro), { icono: U.iconoDe('tipoMiembro', usuario.tipoMiembro) })}
          <span class="badge ${disponible ? 'text-bg-success' : 'text-bg-secondary'}">${U.icono(disponible ? 'bi-check-circle-fill' : 'bi-pause-circle')} ${disponible ? 'Disponible' : 'Sin disponibilidad'}</span>
        </div>
        <p class="small text-body-secondary mb-1">${U.icono('bi-book')} ${U.escapar(usuario.carrera)}</p>
        <p class="small text-body-secondary mb-1">${U.icono('bi-clock')} ${disponible ? `${usuario.disponibilidad.horasSemana} h/semana · ${U.escapar(usuario.disponibilidad.detalle)}` : 'No disponible por ahora'}</p>
        ${portafolio}
        <p class="small">${U.escapar(usuario.bio || 'Sin descripción todavía.')}</p>
      </div>
      <h3 class="seccion-titulo mt-3">Competencias</h3>
      <ul class="lista-chips mb-3">${competencias}</ul>
      <h3 class="seccion-titulo">Intereses</h3>
      <ul class="lista-chips">${intereses}</ul>`;
  }

  function filaIniciativa(i, ctx) {
    const cat = ctx.categorias.get(i.categoria);
    return `<a class="fila-lista" href="detalle.html?id=${encodeURIComponent(i.id)}">
      <span>
        <span class="fw-semibold d-block">${U.escapar(i.titulo)}</span>
        <span class="small text-body-secondary">${U.icono(cat?.icono ?? 'bi-tag')} ${U.escapar(cat?.nombre ?? '')} · ${i.equipo.length + 1} de ${i.participantesEstimados} integrantes previstos</span>
      </span>
      <span class="d-flex gap-2 align-items-center flex-shrink-0">${U.badgeTipo(i.tipo)} ${U.badgeEstado(i.estado)} ${U.icono('bi-chevron-right')}</span>
    </a>`;
  }

  function vacio(texto) {
    return `<div class="estado-vacio text-center py-4">
      <img src="../img/vacio-catalogo.svg" alt="" width="160" height="116" class="mb-2">
      <p class="text-body-secondary mb-0">${texto}</p></div>`;
  }

  function renderPestana(tab, usuario) {
    const ctx = IH.almacen.contexto();
    const todas = IH.almacen.obtenerIniciativas();
    if (tab === 'propias') {
      const l = todas.filter((i) => i.propietario === usuario.id);
      return l.map((i) => filaIniciativa(i, ctx)).join('') || vacio('No has publicado iniciativas todavía.');
    }
    if (tab === 'participo') {
      const l = todas.filter((i) => i.equipo.includes(usuario.id));
      return l.map((i) => filaIniciativa(i, ctx)).join('') || vacio('Todavía no formás parte de ningún equipo.');
    }
    const mias = IH.almacen.obtenerSolicitudes().filter((s) => s.usuarioId === usuario.id);
    return mias.map((s) => {
      const ini = IH.almacen.obtenerIniciativa(s.iniciativaId);
      return `<div class="fila-lista">
        <span>
          <span class="fw-semibold d-block">${U.escapar(ini?.titulo ?? s.iniciativaId)}</span>
          <span class="small text-body-secondary">${U.escapar(s.rol)} · ${U.escapar(ctx.competencias.get(s.competencia)?.nombre ?? s.competencia)} · ${U.formatearFecha(s.fecha)}</span>
        </span>
        ${U.badgeSolicitud(s.estado)}
      </div>`;
    }).join('') || vacio('No has enviado solicitudes.');
  }

  function contar(usuario) {
    const todas = IH.almacen.obtenerIniciativas();
    return {
      propias: todas.filter((i) => i.propietario === usuario.id).length,
      participo: todas.filter((i) => i.equipo.includes(usuario.id)).length,
      solicitudes: IH.almacen.obtenerSolicitudes().filter((s) => s.usuarioId === usuario.id).length
    };
  }

  function activarPestana(boton, usuario) {
    document.querySelectorAll('#pestanas [role="tab"]').forEach((b) => {
      const activo = b === boton;
      b.classList.toggle('active', activo);
      b.setAttribute('aria-selected', String(activo));
      b.tabIndex = activo ? 0 : -1;
    });
    const panel = $('contenido-pestanas');
    panel.setAttribute('aria-labelledby', boton.id);
    panel.innerHTML = renderPestana(boton.dataset.tab, usuario);
  }

  IH.ui.cargarPagina('../', () => {
    const usuario = IH.sesion.usuarioActual();
    if (!usuario) { $('sin-sesion').hidden = false; return; }

    $('perfil').hidden = false;
    renderTarjeta(usuario);
    Object.entries(contar(usuario)).forEach(([k, v]) => { $('n-' + k).textContent = v; });

    const pestanas = $('pestanas');
    activarPestana(pestanas.querySelector('[data-tab="propias"]'), usuario);

    pestanas.addEventListener('click', (e) => {
      const b = e.target.closest('[role="tab"]');
      if (b) activarPestana(b, usuario);
    });
    // Navegación por teclado entre pestañas con flechas.
    pestanas.addEventListener('keydown', (e) => {
      if (!['ArrowRight', 'ArrowLeft'].includes(e.key)) return;
      const tabs = [...pestanas.querySelectorAll('[role="tab"]')];
      const i = tabs.indexOf(document.activeElement);
      if (i < 0) return;
      e.preventDefault();
      const siguiente = tabs[(i + (e.key === 'ArrowRight' ? 1 : tabs.length - 1)) % tabs.length];
      siguiente.focus();
      activarPestana(siguiente, usuario);
    });

    $('nota-local').hidden = !IH.almacen.hayCambiosLocales();
    $('restablecer').addEventListener('click', () => {
      IH.almacen.restablecer();
      U.toast('Datos de ejemplo restablecidos.', 'info');
      setTimeout(() => window.location.reload(), 700);
    });
  });
})();
```

### `avance1/js/solicitud.js` (full replacement)

```js
(() => {
  IH.ui.montarNavbar('solicitud.html', '../');
  const U = IH.ui;
  const V = IH.validacion;
  const $ = (id) => document.getElementById(id);
  const REGEX_DISPONIBILIDAD = /^\d{1,2}\s?h\/semana$/i;
  const val = (id) => $(id).value;

  function renderResumen(ini, ctx) {
    const propietario = ctx.usuarios.get(ini.propietario);
    const buscan = ini.competencias
      .map((id) => `<li>${U.chip(ctx.competencias.get(id)?.nombre ?? id, { icono: 'bi-stars' })}</li>`).join('');
    $('resumen-iniciativa').innerHTML = `<div class="card-body">
      <h2 class="seccion-titulo">Iniciativa</h2>
      <p class="fw-semibold mb-2"><a href="detalle.html?id=${encodeURIComponent(ini.id)}">${U.escapar(ini.titulo)}</a></p>
      <div class="d-flex flex-wrap gap-2 mb-3">${U.badgeTipo(ini.tipo)} ${U.chip(`${IH.reglas.espaciosDisponibles(ini)} espacios libres`, { icono: 'bi-person-plus' })}</div>
      <h2 class="seccion-titulo">Buscan</h2>
      <ul class="lista-chips mb-3">${buscan}</ul>
      <p class="small text-body-secondary mb-0">${U.icono('bi-person')} Tu solicitud llegará a <strong>${U.escapar(propietario?.nombre ?? 'el propietario')}</strong>.</p>
    </div>`;
    $('resumen-iniciativa').hidden = false;
  }

  function bloquear(mensaje) {
    $('bloqueo').hidden = false;
    $('bloqueo').innerHTML = `${U.icono('bi-info-circle-fill')}<div>${U.escapar(mensaje)}</div>`;
  }

  IH.ui.cargarPagina('../', () => {
    const ctx = IH.almacen.contexto();
    const id = new URLSearchParams(window.location.search).get('id');
    const ini = IH.almacen.obtenerIniciativa(id);
    if (!ini) { bloquear('No encontramos la iniciativa indicada.'); return; }

    renderResumen(ini, ctx);
    document.title = `Solicitar participación · ${ini.titulo}`;

    const usuario = IH.sesion.usuarioActual();
    const verificacion = IH.reglas.puedeSolicitar(ini, usuario, IH.almacen.obtenerSolicitudes());
    if (!verificacion.ok) { bloquear(verificacion.motivo); return; }

    const idsCompetencias = usuario.competencias.map((c) => c.id);
    $('competencia').innerHTML = '<option value="">Elegí una competencia…</option>' +
      usuario.competencias.map((c) => `<option value="${c.id}">${U.escapar(ctx.competencias.get(c.id)?.nombre ?? c.id)} · ${U.etiqueta('nivel', c.nivel)}</option>`).join('');
    if (usuario.disponibilidad?.horasSemana) $('disponibilidad').value = `${usuario.disponibilidad.horasSemana} h/semana`;
    $('formulario-solicitud').hidden = false;

    $('mensaje').addEventListener('input', () => { $('contador-mensaje').textContent = `${val('mensaje').length} / 300`; });

    $('formulario-solicitud').addEventListener('submit', (e) => {
      e.preventDefault();
      const ok = V.validarCampos([
        ['mensaje', () => V.combinar(() => V.requerido(val('mensaje')), () => V.longitudMinima(val('mensaje'), 15), () => V.longitudMaxima(val('mensaje'), 300))],
        ['competencia', () => V.seleccionValida(val('competencia'), idsCompetencias)],
        ['rol', () => V.combinar(() => V.requerido(val('rol')), () => V.longitudMaxima(val('rol'), 40))],
        ['disponibilidad', () => V.combinar(() => V.requerido(val('disponibilidad')), () => V.formato(val('disponibilidad'), REGEX_DISPONIBILIDAD, 'Usá el formato "N h/semana", por ejemplo 4 h/semana.'))]
      ]);
      if (!ok) return;

      IH.almacen.agregarSolicitud({
        id: 'sol-' + Date.now(),
        iniciativaId: ini.id,
        usuarioId: usuario.id,
        mensaje: val('mensaje').trim(),
        competencia: val('competencia'),
        rol: val('rol').trim(),
        disponibilidad: val('disponibilidad').trim(),
        estado: 'pendiente',
        fecha: new Date().toISOString().slice(0, 10)
      });
      window.location.href = `detalle.html?id=${encodeURIComponent(ini.id)}&msg=solicitud`;
    });
  });
})();
```

Commits for step 5 follow the order in the final table: shared modules first, then one commit per page script.

---

## PASO 6 · Datos JSON (`avance1/datos/`)

### `categorias.json` (the 8 mandatory categories, with icon)

```json
[
  { "id": "cat-tecnologia",     "nombre": "Tecnología",                "icono": "bi-cpu" },
  { "id": "cat-educacion",      "nombre": "Educación",                 "icono": "bi-mortarboard" },
  { "id": "cat-sostenibilidad", "nombre": "Sostenibilidad",            "icono": "bi-tree" },
  { "id": "cat-impacto-social", "nombre": "Impacto social",            "icono": "bi-people" },
  { "id": "cat-negocios",       "nombre": "Negocios y emprendimiento", "icono": "bi-graph-up-arrow" },
  { "id": "cat-salud",          "nombre": "Salud y bienestar",         "icono": "bi-heart-pulse" },
  { "id": "cat-cultura",        "nombre": "Cultura y creatividad",     "icono": "bi-palette" },
  { "id": "cat-otro",           "nombre": "Otro",                      "icono": "bi-three-dots" }
]
```

### `competencias.json`

```json
[
  { "id": "comp-javascript",     "nombre": "JavaScript",             "area": "Desarrollo" },
  { "id": "comp-diseno-ux",      "nombre": "Diseño UX",              "area": "Diseño" },
  { "id": "comp-diseno-grafico", "nombre": "Diseño gráfico",         "area": "Diseño" },
  { "id": "comp-analisis-datos", "nombre": "Análisis de datos",      "area": "Datos" },
  { "id": "comp-gestion",        "nombre": "Gestión de proyectos",   "area": "Gestión" },
  { "id": "comp-redes",          "nombre": "Redes y ciberseguridad", "area": "Infraestructura" },
  { "id": "comp-marketing",      "nombre": "Marketing digital",      "area": "Negocios" },
  { "id": "comp-comunicacion",   "nombre": "Comunicación",           "area": "Negocios" }
]
```

### `usuarios.json`

```json
[
  { "id": "u1", "nombre": "Ana Mora", "tipoMiembro": "estudiante", "carrera": "Ingeniería de Software",
    "correo": "ana.mora@ucenfotec.ac.cr",
    "competencias": [ { "id": "comp-javascript", "nivel": "avanzado" }, { "id": "comp-gestion", "nivel": "intermedio" } ],
    "intereses": ["comunidad", "educación"],
    "disponibilidad": { "horasSemana": 6, "detalle": "tardes y fines de semana" },
    "portafolio": "https://github.com/anamora",
    "bio": "Tercer año. Me interesa el impacto social de la tecnología." },

  { "id": "u2", "nombre": "Luis Pérez", "tipoMiembro": "docente", "carrera": "Ciberseguridad",
    "correo": "luis.perez@ucenfotec.ac.cr",
    "competencias": [ { "id": "comp-analisis-datos", "nivel": "avanzado" }, { "id": "comp-redes", "nivel": "avanzado" } ],
    "intereses": ["energía", "datos"],
    "disponibilidad": { "horasSemana": 4, "detalle": "martes y jueves" },
    "portafolio": null,
    "bio": "Docente de redes. Busco proyectos aplicados para mis cursos." },

  { "id": "u3", "nombre": "María Solís", "tipoMiembro": "estudiante", "carrera": "Agronomía",
    "correo": "maria.solis@ucenfotec.ac.cr",
    "competencias": [ { "id": "comp-gestion", "nivel": "intermedio" }, { "id": "comp-comunicacion", "nivel": "avanzado" } ],
    "intereses": ["ambiente", "voluntariado"],
    "disponibilidad": { "horasSemana": 3, "detalle": "mañanas" },
    "portafolio": null,
    "bio": "Coordino el huerto del campus." },

  { "id": "u4", "nombre": "Diego Vargas", "tipoMiembro": "administrativo", "carrera": "Administración",
    "correo": "diego.vargas@ucenfotec.ac.cr",
    "competencias": [ { "id": "comp-marketing", "nivel": "avanzado" }, { "id": "comp-diseno-ux", "nivel": "basico" } ],
    "intereses": ["emprendimiento"],
    "disponibilidad": { "horasSemana": 0, "detalle": "" },
    "portafolio": "https://www.behance.net/diegovargas",
    "bio": "" }
]
```

### `iniciativas.json` (official states, official categories, dates)

```json
[
  { "id": "ini-01", "titulo": "Plataforma de voluntariado universitario", "tipo": "idea",
    "resumen": "Coordinar voluntarios de distintas carreras en una sola aplicación.",
    "descripcion": "Hoy cada facultad organiza su voluntariado por chat. Proponemos centralizar convocatorias, inscripciones y horas certificadas.",
    "problema": "Los estudiantes no encuentran oportunidades fuera de su carrera.",
    "beneficiarios": "Estudiantes y organizaciones comunitarias aliadas.",
    "categoria": "cat-impacto-social", "competencias": ["comp-javascript", "comp-diseno-ux"],
    "participantesEstimados": 4, "visibilidad": "pública", "etiquetas": ["voluntariado", "comunidad"],
    "propietario": "u1", "estado": "en_formacion", "equipo": ["u2"],
    "fechaCreacion": "2026-09-08", "fechaModificacion": "2026-09-20" },

  { "id": "ini-02", "titulo": "Monitoreo de energía en laboratorios", "tipo": "reto",
    "resumen": "Sensores de bajo costo para medir consumo eléctrico en tiempo real.",
    "descripcion": "Los laboratorios no saben cuánta energía consume cada equipo. Se busca un sistema de sensores accesible.",
    "problema": "Alto consumo eléctrico sin datos para optimizarlo.",
    "beneficiarios": "Escuela de Ingeniería y administración de planta física.",
    "categoria": "cat-sostenibilidad", "competencias": ["comp-analisis-datos", "comp-redes"],
    "participantesEstimados": 3, "visibilidad": "pública", "etiquetas": ["energía", "sensores"],
    "propietario": "u2", "estado": "proyecto", "equipo": ["u1", "u3"],
    "fechaCreacion": "2026-08-30", "fechaModificacion": "2026-09-15" },

  { "id": "ini-03", "titulo": "Tutorías entre pares para cálculo", "tipo": "necesidad",
    "resumen": "Apoyo académico entre estudiantes en cursos de matemática.",
    "descripcion": "Muchos estudiantes reprueban cálculo por falta de acompañamiento fuera de clase.",
    "problema": "Alto índice de repitencia en cursos de cálculo.",
    "beneficiarios": "Estudiantes de primer ingreso.",
    "categoria": "cat-educacion", "competencias": ["comp-gestion", "comp-comunicacion"],
    "participantesEstimados": 5, "visibilidad": "institucional", "etiquetas": ["educación", "tutorías"],
    "propietario": "u3", "estado": "publicada", "equipo": [],
    "fechaCreacion": "2026-09-10", "fechaModificacion": "2026-09-10" },

  { "id": "ini-04", "titulo": "App de reciclaje en el campus", "tipo": "idea",
    "resumen": "Mapa de puntos de reciclaje y guía de separación de residuos.",
    "descripcion": "No existe un mapa claro de dónde reciclar cada tipo de material dentro del campus.",
    "problema": "Bajo aprovechamiento de los puntos de reciclaje existentes.",
    "beneficiarios": "Toda la comunidad universitaria.",
    "categoria": "cat-sostenibilidad", "competencias": ["comp-javascript", "comp-diseno-grafico"],
    "participantesEstimados": 3, "visibilidad": "pública", "etiquetas": ["ambiente", "reciclaje"],
    "propietario": "u1", "estado": "publicada", "equipo": [],
    "fechaCreacion": "2026-09-14", "fechaModificacion": "2026-09-14" },

  { "id": "ini-05", "titulo": "Reducir tiempo de atención en soporte TI", "tipo": "reto",
    "resumen": "Optimizar el proceso de tickets del área de soporte técnico.",
    "descripcion": "El tiempo promedio de resolución de tickets supera las 48 horas.",
    "problema": "Insatisfacción de usuarios por demoras en soporte técnico.",
    "beneficiarios": "Personal administrativo y docente.",
    "categoria": "cat-tecnologia", "competencias": ["comp-analisis-datos"],
    "participantesEstimados": 2, "visibilidad": "restringida", "etiquetas": ["soporte", "procesos"],
    "propietario": "u4", "estado": "publicada", "equipo": [],
    "fechaCreacion": "2026-09-12", "fechaModificacion": "2026-09-18" },

  { "id": "ini-06", "titulo": "Espacio de coworking para emprendedores", "tipo": "necesidad",
    "resumen": "Un lugar físico y una comunidad para estudiantes emprendedores.",
    "descripcion": "Los emprendedores estudiantiles no tienen un espacio de trabajo colaborativo dentro del campus.",
    "problema": "Falta de infraestructura para el emprendimiento estudiantil.",
    "beneficiarios": "Estudiantes con proyectos propios.",
    "categoria": "cat-negocios", "competencias": ["comp-gestion", "comp-marketing"],
    "participantesEstimados": 6, "visibilidad": "pública", "etiquetas": ["emprendimiento", "comunidad"],
    "propietario": "u2", "estado": "en_formacion", "equipo": ["u4"],
    "fechaCreacion": "2026-09-05", "fechaModificacion": "2026-09-12" },

  { "id": "ini-07", "titulo": "Diario de bienestar estudiantil", "tipo": "idea",
    "resumen": "Aplicación privada para registrar estado de ánimo y hábitos de estudio.",
    "descripcion": "Herramienta de autorregistro para detectar señales tempranas de estrés académico.",
    "problema": "Poca información temprana sobre el bienestar emocional del estudiantado.",
    "beneficiarios": "Estudiantes que quieran monitorear su bienestar.",
    "categoria": "cat-salud", "competencias": ["comp-diseno-ux", "comp-javascript"],
    "participantesEstimados": 3, "visibilidad": "privada", "etiquetas": ["salud", "bienestar"],
    "propietario": "u3", "estado": "en_formacion", "equipo": ["u1"],
    "fechaCreacion": "2026-09-16", "fechaModificacion": "2026-09-19" },

  { "id": "ini-08", "titulo": "Festival de cortos universitarios", "tipo": "reto",
    "resumen": "Organizar una muestra anual de cortometrajes hechos por estudiantes.",
    "descripcion": "Hay producción audiovisual estudiantil dispersa y sin un espacio para exhibirla ni criterios para premiarla.",
    "problema": "Falta de visibilidad para el trabajo creativo del estudiantado.",
    "beneficiarios": "Estudiantes de carreras creativas y comunidad universitaria.",
    "categoria": "cat-cultura", "competencias": ["comp-comunicacion", "comp-diseno-grafico", "comp-gestion"],
    "participantesEstimados": 4, "visibilidad": "pública", "etiquetas": ["cultura", "cine", "evento"],
    "propietario": "u4", "estado": "publicada", "equipo": [],
    "fechaCreacion": "2026-09-21", "fechaModificacion": "2026-09-21" }
]
```

### `solicitudes.json` (unchanged content, kept for reference)

```json
[
  { "id": "sol-01", "iniciativaId": "ini-02", "usuarioId": "u3", "mensaje": "Puedo aportar análisis de datos del consumo eléctrico.",
    "competencia": "comp-analisis-datos", "rol": "Analista de datos", "disponibilidad": "6 h/semana", "estado": "aceptada", "fecha": "2026-09-10" },
  { "id": "sol-02", "iniciativaId": "ini-06", "usuarioId": "u4", "mensaje": "Tengo experiencia en marketing para emprendimientos estudiantiles.",
    "competencia": "comp-marketing", "rol": "Encargado de difusión", "disponibilidad": "4 h/semana", "estado": "aceptada", "fecha": "2026-09-12" },
  { "id": "sol-03", "iniciativaId": "ini-01", "usuarioId": "u3", "mensaje": "Me gustaría ayudar con la gestión de voluntarios.",
    "competencia": "comp-gestion", "rol": "Coordinadora", "disponibilidad": "3 h/semana", "estado": "pendiente", "fecha": "2026-09-16" }
]
```

Commit: the four changed JSON files.
Trailers: `Seccion: Datos` · `Cambio: Catálogos oficiales, estados del dominio, fechas y perfil completo de usuarios`

---

## PASO 7 · Herramientas: tabla de commits con 6 columnas

### `herramientas/tabla-commits.sh` (full replacement)

```bash
#!/bin/bash
# Regenera la tabla de commits dentro del README, entre las marcas.
# Columnas: número, fecha, hash corto, mensaje, sección del sistema, cambio principal.
# La sección y el cambio se leen de los trailers "Seccion:" y "Cambio:" del commit.
# Para commits antiguos sin trailers se usa herramientas/commits-meta.tsv (hash<TAB>seccion<TAB>cambio).

set -euo pipefail

cd "$(git rev-parse --show-toplevel)"

README="README.md"
META="herramientas/commits-meta.tsv"
INICIO="<!-- INICIO TABLA COMMITS -->"
FIN="<!-- FIN TABLA COMMITS -->"
SEP=$'\037'
REG=$'\036'

TABLA="$(mktemp)"
trap 'rm -f "$TABLA"' EXIT

{
  echo "| # | Fecha | Hash | Mensaje | Sección del sistema | Cambio principal |"
  echo "|---|-------|------|---------|---------------------|------------------|"
  if git rev-parse HEAD >/dev/null 2>&1; then
    git log --reverse --date=short \
      --pretty=format:"%h%x1f%ad%x1f%s%x1f%(trailers:key=Seccion,valueonly,separator=%x2c%x20)%x1f%(trailers:key=Cambio,valueonly,separator=%x2c%x20)%x1e" \
      | tr -d '\n' | tr "$REG" '\n' \
      | awk -F "$SEP" -v meta="$META" '
          BEGIN {
            while ((getline linea < meta) > 0) {
              if (linea ~ /^#/ || linea == "") continue
              split(linea, col, "\t")
              sec[col[1]] = col[2]; cam[col[1]] = col[3]
            }
          }
          NF >= 3 {
            n++; h = $1; f = $2; m = $3; s = $4; c = $5
            if (s == "" && (h in sec)) s = sec[h]
            if (c == "" && (h in cam)) c = cam[h]
            if (s == "") s = "General"
            if (c == "") c = m
            gsub(/\|/, "\\|", m); gsub(/\|/, "\\|", s); gsub(/\|/, "\\|", c)
            print "| " n " | " f " | " h " | " m " | " s " | " c " |"
          }'
  fi
  echo
} > "$TABLA"

if ! grep -qF "$INICIO" "$README" || ! grep -qF "$FIN" "$README"; then
  echo "Error: no encontré las marcas de la tabla en $README" >&2
  exit 1
fi

awk -v inicio="$INICIO" -v fin="$FIN" -v tabla="$TABLA" '
  $0 == inicio { print; while ((getline linea < tabla) > 0) print linea; dentro = 1; next }
  $0 == fin    { dentro = 0 }
  !dentro      { print }
' "$README" > "$README.nuevo"

mv "$README.nuevo" "$README"
echo "Tabla actualizada."
```

### `herramientas/commits-meta.tsv` (new; TAB-separated, one row per existing commit)

```
# hash	seccion	cambio
9726a0b	Repositorio	Estructura inicial de carpetas y README
f9593e9	Herramientas	Script que genera la tabla de commits
f75aa83	Herramientas	Permisos de ejecución de los scripts
4ed592b	Repositorio	Regeneración de la tabla de commits
4526bff	Página principal	HTML semántico base y sección de catálogo
308f2ee	Página principal	Adopción de la grilla de Bootstrap
e045ca8	Datos	Archivos JSON de iniciativas, categorías, competencias, usuarios y solicitudes
0a6e481	Catálogo	Esqueleto de la página de catálogo
9cea2c6	Detalle	Esqueleto de la página de detalle
dad34ec	Formulario	Esqueleto del formulario de iniciativa
c944b09	Interfaz	Navbar compartido generado desde JavaScript
7bfb074	Carga de datos	Módulo de carga asincrónica de JSON con fetch
7c8b13b	Reglas de negocio	Niveles de acceso según visibilidad
c2357a1	Formulario	Lógica del formulario con validación por campo
65151dd	Validación	Módulo de reglas de validación reutilizables
947a13d	Solicitud	Página de solicitud de participación
3be9179	Perfil	Lógica de la página de perfil
d52bd44	Solicitud	Lógica de envío simulado de solicitud
7004692	Perfil	Página de perfil y lógica del catálogo
1a2bcd7	Interfaz	Bootstrap por CDN en todas las páginas
```

Regenerate and check:

```bash
bash herramientas/tabla-commits.sh && grep -c "^| " README.md
```

The hook in `herramientas/pre-commit` needs no change.

Commit: `herramientas/tabla-commits.sh`, `herramientas/commits-meta.tsv`, `README.md`.
Trailers: `Seccion: Herramientas` · `Cambio: Tabla de commits con sección y cambio principal`

---

## PASO 8 · README.md (full replacement)

```markdown
# Innovation Hub

Proyecto del curso SOFT-12 — Programación web avanzada.

**Estudiante:** Jose Ricardo Barrantes Saenz
**Sección:** SCV2    **Periodo:** III cuatrimestre 2026
**Docente:** Álvaro Cordero Peña

## Descripción

Aplicación web que permite publicar ideas, necesidades y retos, declarar las
competencias que cada iniciativa requiere, solicitar participación y conformar
equipos interdisciplinarios dentro de la comunidad universitaria. Cuando una
iniciativa reúne equipo, su propietario la convierte en proyecto.

El sistema se construye una sola vez y evoluciona en tres avances:

| Avance | Tecnología | Estado |
|---|---|---|
| 1 — Prototipo navegable | HTML, Sass + Bootstrap 5, JavaScript, JSON | `avance1/` (esta entrega) |
| 2 — Cliente-servidor | React, Node.js + Express, MongoDB | pendiente |
| Final | React + ASP.NET Core + EF Core | pendiente |

## Estructura del repositorio

```
innovation-hub/
├── README.md
├── .gitignore
├── package.json            scripts de compilación de Sass
├── herramientas/           scripts de apoyo (tabla de commits, hook)
└── avance1/
    ├── index.html          página principal
    ├── paginas/            catálogo, detalle, formulario, perfil, solicitud
    ├── datos/              archivos JSON con datos de ejemplo
    ├── js/                 módulos de JavaScript (un archivo por responsabilidad)
    ├── scss/               variables y parciales de Sass
    ├── css/                hoja de estilos compilada (se versiona)
    └── img/                logotipo e ilustraciones
```

### Módulos de JavaScript

| Archivo | Responsabilidad |
|---|---|
| `ui.js` | Escapado de HTML, badges, chips, tarjetas, navbar, toasts, carga con estados |
| `sesion.js` | Simulación de sesión ("Ver como") guardada en `localStorage` |
| `datos.js` | Carga asincrónica de JSON con `fetch` y `async/await` |
| `almacen.js` | Estado en memoria, persistencia opcional en `localStorage`, mutaciones |
| `reglas.js` | Reglas de negocio: visibilidad, solicitudes, conversión a proyecto |
| `validacion.js` | Reglas de validación y presentación de errores por campo |
| `inicio.js`, `catalogo.js`, `detalle.js`, `formulario.js`, `perfil.js`, `solicitud.js` | Lógica de cada página |

## Cómo ejecutar

Requisitos: Visual Studio Code con la extensión **Live Server** (`ritwickdey.LiveServer`)
y, solo si se van a modificar los estilos, Node.js 18 o superior.

1. Abrir la carpeta del repositorio en VS Code.
2. Clic derecho sobre `avance1/index.html` → **Open with Live Server**.
3. Se abre `http://127.0.0.1:5500/avance1/index.html` y la página se recarga con cada cambio.

**¿Por qué un servidor local?** Las páginas cargan los archivos JSON con `fetch`.
Chrome bloquea esas peticiones cuando el HTML se abre directamente con doble clic
(protocolo `file://`). La página principal sí funciona desde el archivo; el resto
muestra un mensaje que indica cómo abrirla con Live Server.

### Compilar los estilos de Sass

El CSS compilado (`avance1/css/main.css`) se incluye en el repositorio, así que el
prototipo funciona sin ejecutar Sass. Para modificar los estilos:

```bash
npm install          # instala Sass y el código fuente de Bootstrap (node_modules/ no se versiona)
npm run css          # compila avance1/scss/main.scss → avance1/css/main.css
npm run css:watch    # recompila en cada cambio; Live Server recarga el navegador
```

## Simulación de sesión

En el Avance 1 no hay autenticación. La barra superior tiene un selector
**Ver como** que permite navegar como visitante o como cualquiera de los cuatro
usuarios de ejemplo. Con eso se pueden verificar los cuatro niveles de
visibilidad, las acciones exclusivas del propietario y las reglas de solicitud.

## Decisiones de diseño

1. **Sass sobre el código fuente de Bootstrap.** `scss/_variables.scss` sobrescribe
   variables de origen (colores de tema, tipografía, radios, espaciado, formularios,
   navbar, modal, toast) antes de importar Bootstrap. Los parciales `_base`,
   `_navbar`, `_hero`, `_tarjetas` y `_formularios` agregan solo lo que Bootstrap
   no ofrece.
2. **Identidad visual.** Paleta azul: primario `#2563EB`, degradado marino → azul →
   petróleo en el hero, verde menta y ámbar para necesidad y reto. Tipografía Sora
   para títulos y DM Sans para texto. Botones y badges en forma de píldora,
   tarjetas sin borde con sombra suave. Iconografía de Bootstrap Icons.
3. **Contenido generado desde datos.** Tarjetas, detalle, perfil, selects y badges
   se construyen en JavaScript a partir de los JSON. El HTML solo contiene la
   estructura y el contenido fijo de la portada.
4. **Estados del dominio.** Se usan los valores controlados del documento general:
   `borrador`, `publicada`, `en_formacion`, `proyecto`, `archivada`; las ocho
   categorías obligatorias; niveles `basico`, `intermedio`, `avanzado`.
5. **Persistencia.** Los JSON son el modelo principal. Las altas, ediciones,
   eliminaciones y solicitudes se guardan además en `localStorage` para que el
   catálogo refleje los cambios entre páginas. El botón **Restablecer datos de
   ejemplo** del perfil vuelve a los JSON originales.
6. **Eliminar o archivar.** Si la iniciativa tiene equipo, solicitudes o proyecto,
   se archiva en lugar de eliminarse (RN-08 y RN-09). La confirmación se hace con
   un modal de Bootstrap que muestra el título y la acción que se ejecutará.
7. **Validación propia.** Obligatoriedad, longitud mínima y máxima, rango,
   selección válida, formato (etiquetas y disponibilidad) y relación entre campos
   (los integrantes estimados no pueden ser menos que el equipo actual). Los
   mensajes son específicos por campo y se anuncian con `aria-invalid` y
   `aria-describedby`.
8. **Accesibilidad.** HTML semántico, enlace para saltar al contenido, jerarquía
   de encabezados, etiquetas asociadas, pestañas con roles ARIA y navegación por
   teclado, textos alternativos, estados que combinan icono, texto y color, y
   respeto a `prefers-reduced-motion`.
9. **Preparación para el Avance 2.** Cada página es un módulo con estado propio y
   funciones de render puras sobre los datos, lo que facilita convertirlas en
   componentes de React. `datos.js` es el único punto que hablará con la API.

## Resumen de commits

<!-- INICIO TABLA COMMITS -->
<!-- FIN TABLA COMMITS -->

## Automatización de la tabla de commits

La tabla se genera con `herramientas/tabla-commits.sh`, que lee el historial real
del repositorio. La sección y el cambio principal se toman de los trailers
`Seccion:` y `Cambio:` de cada commit; para los commits anteriores a esa
convención se usa `herramientas/commits-meta.tsv`.

Convención de commit:

    git commit -m "Mensaje corto" --trailer "Seccion: Catálogo" --trailer "Cambio: Búsqueda por etiquetas"

Regenerar manualmente:

    bash herramientas/tabla-commits.sh

Para que se actualice sola en cada commit, instalar el hook una vez por clon:

    cp herramientas/pre-commit .git/hooks/pre-commit
    chmod +x .git/hooks/pre-commit

El hook corre antes de que exista el commit, así que la tabla refleja el historial
hasta el commit anterior. Si llegara a molestar, se desactiva con
`rm .git/hooks/pre-commit`.
```

Note: the table between the markers is regenerated by the script, so leave the two markers with nothing between them and run the script once.

Commit: `README.md`.
Trailers: `Seccion: Documentación` · `Cambio: Instrucciones de Sass y ejecución, decisiones de diseño y módulos`

---

## PASO 9 · Verificación final y etiqueta

1. `npm run css` once more so `css/main.css` matches the last SCSS.
2. In VS Code, right-click `avance1/index.html` → **Open with Live Server**, then run through this list:

| Check | Where | Expected |
|---|---|---|
| Widths 375, 768, 1280 | every page | no horizontal scroll, hero image hidden below lg, filters stack on phone |
| Fonts and icons | any page | Sora headings, DM Sans body, icons render |
| Visitante → detalle ini-03 | Ver como: Visitante | blocked with "requiere iniciar sesión" |
| Visitante → detalle ini-05 | Ver como: Visitante | summary only, amber notice |
| Diego Vargas → detalle ini-05 | Ver como: Diego | full content, owner buttons |
| Visitante → catálogo | Ver como: Visitante | ini-07 (privada) does not appear |
| María Solís → catálogo | Ver como: María | ini-07 appears (owner) |
| Search "cine" | catálogo | finds ini-08 by tag only |
| Empty search | catálogo | empty state with illustration and count "0 iniciativas" |
| Publicar as Ana | formulario | validation messages per field, counters, add and remove competencias, toast "Iniciativa publicada" |
| Edit ini-01 as Ana, set integrantes 1 | formulario | relationship error: team already has 2 |
| Delete ini-04 as Ana | detalle | modal says "se eliminará"; toast on catalog |
| Delete ini-01 as Ana | detalle | modal says "se archivará"; badge Archivada |
| Reload after edit | catálogo | changes persist |
| Restablecer datos | perfil | returns to JSON state |
| Solicitar as María on ini-06 | solicitud | disponibilidad format rule, toast on detail |
| Solicitar as Luis on ini-06 | solicitud | blocked: own initiative |
| Tabs with arrow keys | perfil | focus moves and panel updates |
| Open index.html by double-click | file:// | home renders; catalog shows the Live Server hint |

3. Tag and push:

```bash
git tag -a entrega-1 -m "Avance 1: prototipo funcional con HTML, Sass, Bootstrap y JavaScript"
```

```bash
git push origin main --tags
```

4. Create `entrega-1.txt` containing the repository URL and upload it to Moodle before Sunday 27 September 2026, 11:55 p.m.

---

## Orden de commits sugerido

| # | Files | Seccion | Cambio |
|---|---|---|---|
| 1 | package.json, package-lock.json, .gitignore | Herramientas | Sass y Bootstrap como dependencias de desarrollo |
| 2 | scss/*, css/main.css | Estilos | Sistema de diseño en Sass que sobrescribe variables de Bootstrap |
| 3 | img/* | Diseño visual | Logotipo e ilustraciones |
| 4 | *.html | Interfaz | Páginas con CSS compilado, fuentes, iconos, estados de carga y accesibilidad |
| 5 | js/ui.js, js/sesion.js, delete js/app.js | Interfaz | Componentes compartidos, toasts, simulación de sesión |
| 6 | js/datos.js, js/almacen.js | Datos | Persistencia local y relaciones de la iniciativa |
| 7 | js/reglas.js | Reglas de negocio | Visibilidad corregida, espacios disponibles, reglas de solicitud |
| 8 | js/validacion.js | Validación | Longitud máxima, formato, selección válida y foco en el primer error |
| 9 | js/inicio.js, js/catalogo.js | Catálogo | Búsqueda por etiquetas, conteo y cifras en portada |
| 10 | js/detalle.js | Detalle | Campos completos, modal de confirmación y acciones del propietario |
| 11 | js/formulario.js | Formulario | Sesión, edición segura, contadores y regla de integrantes |
| 12 | js/perfil.js, js/solicitud.js | Perfil y solicitud | Perfil completo con pestañas accesibles; solicitud con contexto |
| 13 | datos/*.json | Datos | Catálogos oficiales, estados del dominio, fechas y perfil completo |
| 14 | herramientas/*, README.md | Herramientas | Tabla de commits con sección y cambio principal |
| 15 | README.md | Documentación | Instrucciones de Sass y ejecución, decisiones de diseño |

Fifteen commits on top of the existing twenty, spread over the remaining days, comfortably clears the minimum history requirement.
