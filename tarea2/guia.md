# Guía de commits: construir la Tarea 2 paso a paso

Esta guía es para que **escribas tú** el proyecto, un commit a la vez, del martes 20 al domingo 25 de octubre. En cada paso te digo exactamente qué archivo crear o qué parte cambiar, qué código escribir, cómo probarlo y con qué mensaje hacer el commit.

La carpeta `Tarea2` (donde está esta guía) queda como **solución de referencia**. No la subas. Vas a construir el repositorio en una carpeta nueva, `tarea-2`, y si algo no te funciona puedes comparar tu archivo con el de `Tarea2`.

Archivo que acompaña a este: `GUIA_DEFENSA.md`, con la explicación de cada decisión para la defensa.

---

## Cómo leer esta guía

Cada paso tiene estas partes:

- **Qué vas a hacer**: el objetivo del commit en una o dos frases.
- **Archivos**: qué archivos se crean [NUEVO] o se modifican [MODIFICAR].
- **Código**: el contenido exacto. Hay dos formatos:
  - *Archivo completo*: copia todo el bloque en el archivo nuevo.
  - *Cambio*: te muestro **Busca esto** (el texto que ya tienes) y **Cámbialo por** o **Agrega debajo** (lo nuevo). Usa Ctrl+F en VS Code para encontrar el texto.
- **Por qué**: lo que tienes que entender de ese código. Esto es lo que te van a preguntar.
- **Prueba**: cómo confirmar que funciona antes del commit.
- **Commit**: el mensaje exacto.

### Consejo para aprender de verdad

No copies y pegues bloques enteros sin leerlos. Lo mejor es escribirlos a mano (o al menos pegarlos línea por línea leyendo cada una). Cuando termines un archivo, cierra la guía y trata de explicarte en voz alta qué hace cada función. Si no puedes, vuelve a leer el **Por qué** de ese paso. En la defensa no puedes usar IA y el código que no sepas explicar cuenta como no hecho por ti.

### Cómo hacer cada commit

Siempre igual, desde la carpeta `tarea-2`:

```powershell
git add .
git status
git commit -m "mensaje del paso"
git push
```

Usar `git add .` es seguro porque el `.gitignore` (paso 1) deja fuera `node_modules` y el CSS generado. Igual revisa con `git status` que solo aparezcan los archivos del paso. Si aparece `node_modules`, **para** y revisa el `.gitignore`.

No cambies las fechas de los commits a mano. El profe pide commits distribuidos en el periodo, y si los haces cada día eso queda solo.

### Por qué los mensajes están escritos así

- Empiezan con **"Parte A:"** o **"Parte B:"** para que el historial se lea rápido.
- Usan **verbo en presente**: "agrega", "configura", "corrige". Describen lo que hace el commit.
- Dicen **qué y para qué**, nunca "avance" o "cambios". La rúbrica pide un historial descriptivo (criterio 13).
- Cada commit es **una idea que puedes explicar en una frase**. Si el profe señala un commit y pregunta "¿qué hiciste aquí?", la respuesta es el mensaje.

### Resumen del plan

| Día | Commits | Qué queda listo |
|---|---|---|
| Mar 20 | 1 a 3 | Repositorio, npm, Sass con Bootstrap |
| Mié 21 | 4 a 6 | HTML completo, componente propio, módulos de API y render |
| Jue 22 | 7 a 10 | Búsqueda, modal, lista de lecturas y corrección de un error real |
| Vie 23 | 11 a 14 | Animaciones, validación, autores dinámicos y envío |
| Sáb 24 | 15 a 18 | Optimización de Bootstrap y Parte B hasta filtros y resumen |
| Dom 25 | 19 a 21 + etiqueta | Formulario de React, documentación y entrega |

Son 21 commits (la consigna pide al menos 15). Si un día no puedes, junta ese día con el siguiente, pero trata de no hacer todo el domingo.

---

## Antes de empezar (una sola vez, en Windows)

Esta guía está escrita para **Windows con PowerShell**, que es la terminal que abre VS Code por defecto (menú **Terminal → New Terminal**, o Ctrl+ñ en teclado en español / Ctrl+` en teclado en inglés). Todos los comandos de la guía son para PowerShell.

1. **Instala Node.js** (versión LTS, 20 o superior) desde https://nodejs.org con el instalador `.msi`, dejando las opciones por defecto.
2. **Instala Git para Windows** desde https://git-scm.com/download/win, también con las opciones por defecto.
3. **Cierra y vuelve a abrir VS Code** (para que la terminal reconozca `node` y `git`) y comprueba:
   ```powershell
   node -v
   npm -v
   git --version
   ```
4. **Configura tu nombre y correo en git** (una sola vez en la computadora; usa el mismo correo de tu cuenta de GitHub):
   ```powershell
   git config --global user.name "José Barrantes"
   git config --global user.email "tu-correo@ejemplo.com"
   ```
5. En GitHub crea un repositorio **vacío** llamado `tarea-2` (sin README, sin .gitignore, sin licencia).
6. Usa VS Code **con Copilot y cualquier autocompletado con IA apagado** desde ya. Así practicas como en la defensa.
7. **Copia la carpeta `Tarea2` (la de referencia) a la computadora con Windows**, por ejemplo con una memoria USB, OneDrive o Google Drive. No copies las carpetas `node_modules` (pesan mucho y no hacen falta). La vas a usar para comparar si algo no te funciona.

### Si PowerShell no deja ejecutar `npm`

Si al correr `npm` aparece un error rojo que dice algo como *"npm.ps1 cannot be loaded because running scripts is disabled on this system"*, Windows está bloqueando los scripts de PowerShell. Tienes dos salidas:

- **Opción A (sin cambiar nada de Windows):** en la terminal de VS Code, en la flechita al lado del **+**, elige **Command Prompt** (cmd). Ahí `npm` funciona. Los comandos de esta guía sirven igual en cmd, salvo `New-Item` y `Remove-Item` (en esos pasos te doy la alternativa) y `$HOME`, que en cmd se escribe `%USERPROFILE%`.
- **Opción B:** permitir scripts solo para tu usuario. Es un cambio de seguridad de Windows, así que decídelo tú: abre PowerShell y ejecuta `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, responde `S` (o `Y`) y vuelve a abrir VS Code.

### Diferencias con otras guías que veas en internet

Muchos tutoriales usan comandos de Mac o Linux. Sus equivalentes en PowerShell:

| Mac / Linux | PowerShell (Windows) | Para qué |
|---|---|---|
| `touch archivo` | `New-Item archivo -ItemType File` | Crear un archivo vacío |
| `mkdir -p a/b` | `mkdir a\b` | Crear carpetas (PowerShell crea las intermedias solo) |
| `rm archivo` | `Remove-Item archivo` | Borrar un archivo |
| `rm -rf carpeta` | `Remove-Item carpeta -Recurse` | Borrar una carpeta con su contenido |
| `ls` | `ls` o `dir` | Listar archivos |
| `cmd1 && cmd2` | dos líneas separadas | En PowerShell 5 (el que trae Windows) `&&` no existe |
| `~/Documents` | `$HOME\Documents` | Tu carpeta de usuario |

En las rutas de Windows se usa `\`, pero PowerShell también acepta `/`, así que si ves rutas con `/` dentro de git o npm, funcionan igual.

**Tip:** también puedes crear archivos y carpetas desde el explorador de VS Code (los íconos de "nuevo archivo" y "nueva carpeta" arriba de la lista de archivos). Es lo mismo que hacerlo con comandos.

---

# Martes 20 de octubre

## Commit 1. Estructura inicial del repositorio

**Qué vas a hacer:** crear la carpeta del proyecto, conectar git con GitHub y definir qué archivos nunca se suben.

**Archivos:**
- [NUEVO] `.gitignore`
- [NUEVO] `README.md` (versión corta, se completa en el commit 21)
- [NUEVO] `docs/capturas/.gitkeep` (archivo vacío)

### Pasos en la terminal

Crea la carpeta del proyecto donde tengas tus cosas de la universidad. El ejemplo usa `Documentos\Universidad\PWA`; cambia la ruta por la tuya. (Si la carpeta Documentos está dentro de OneDrive, la ruta es `$HOME\OneDrive\Documents\...`; lo más fácil es abrir la carpeta en el explorador de Windows, clic en la barra de dirección y copiar la ruta.)

Abre una terminal PowerShell (puede ser la de VS Code) y escribe una línea a la vez:

```powershell
cd $HOME\Documents\Universidad\PWA
mkdir tarea-2
cd tarea-2
git init
git branch -M main
git remote add origin https://github.com/TU_USUARIO/tarea-2.git
mkdir docs\capturas
New-Item docs\capturas\.gitkeep -ItemType File
```

- `New-Item ... -ItemType File` es el equivalente de `touch` en PowerShell: crea un archivo vacío.
- Si usas **cmd** en vez de PowerShell, la última línea es: `type nul > docs\capturas\.gitkeep`
- O créalo desde VS Code: clic derecho en la carpeta `capturas` → **New File** → escribe `.gitkeep`.

Después, en VS Code: **File → Open Folder** y elige la carpeta `tarea-2`. Abre la terminal integrada (**Terminal → New Terminal**): ya queda ubicada en `tarea-2`.

### `.gitignore` (archivo completo)

```gitignore
# Dependencias
node_modules/

# Archivos generados
buscador-js/css/
dist/

# Sistema y editor
.DS_Store
.vscode/
```

### `README.md` (archivo completo, versión inicial)

```markdown
# Tarea 2 · Estantería Común

Programación Web Avanzada (SOFT-12), Universidad CENFOTEC, III cuatrimestre 2026.

Proyecto en construcción: buscador de libros con JavaScript, Bootstrap y Sass (Parte A) y tablero de lecturas con React (Parte B).
```

### Por qué

- **`.gitignore` va primero** porque si haces `npm install` antes, git ve miles de archivos en `node_modules` y es fácil subirlos por error. La consigna dice explícitamente que `node_modules` no se sube.
- **`node_modules/`**: se puede regenerar con `npm install` a partir de `package.json` y `package-lock.json`. Subirlo pesa cientos de MB y no aporta nada.
  - **Ojo, aunque no diga `buscador-js/`, sí ignora `buscador-js/node_modules`.** En `.gitignore`, un patrón que **no tiene `/` al inicio ni en el medio** (solo al final, como `node_modules/`) se aplica a **cualquier carpeta con ese nombre, en cualquier nivel**. Por eso esta sola línea ignora `buscador-js/node_modules/` y también `tablero-react/node_modules/` (que se crea el sábado).
  - En cambio, `buscador-js/css/` **sí** tiene una `/` en el medio, y eso hace que se lea desde la raíz del repositorio: ignora solo esa carpeta exacta. Si pusieras solo `css/`, ignoraría cualquier carpeta `css` del proyecto, y eso podría esconder algo que sí quieres subir.
  - Lo compruebas en el commit 2: después de `npm install`, `git status` no muestra `node_modules`. También puedes preguntarle a git directamente con `git check-ignore -v buscador-js/node_modules`, que responde qué línea del `.gitignore` la está ignorando.
- **`buscador-js/css/`**: es el CSS que genera Sass. Es un archivo *derivado*: si subes el `.scss` y el script que lo compila, cualquiera lo puede generar. Subir archivos generados causa conflictos y confusión ("¿cuál edito, el .scss o el .css?").
- **`dist/`**: lo genera `npm run build` en la Parte B. Misma razón.
- **`.gitkeep`**: git no guarda carpetas vacías, solo archivos. La consigna pide la carpeta `docs/capturas/`, así que se pone un archivo vacío para que exista desde el inicio. El nombre `.gitkeep` es una convención, no tiene nada especial.

### Prueba

```powershell
git status
```
Deben aparecer `.gitignore`, `README.md` y `docs/`.

### Commit

```powershell
git add .
git commit -m "Crea la estructura inicial del repositorio con README y .gitignore"
git push -u origin main
```

El `-u` solo se usa la primera vez; deja conectada la rama `main` con GitHub. Desde aquí, solo `git push`.

La primera vez que hagas `push`, Git para Windows abre una ventana para iniciar sesión en GitHub (Git Credential Manager). Elige **Sign in with your browser**, autoriza, y queda guardado para los siguientes push.

---

## Commit 2. Proyecto npm de la Parte A

**Qué vas a hacer:** crear `buscador-js/`, instalar Bootstrap y Sass con npm y definir los scripts de compilación.

**Archivos:**
- [NUEVO] `buscador-js/package.json`
- [NUEVO] `buscador-js/package-lock.json` (lo crea npm solo)

### Pasos en la terminal

```powershell
mkdir buscador-js
cd buscador-js
```

### `buscador-js/package.json` (archivo completo, antes de instalar)

```json
{
  "name": "buscador-js",
  "version": "1.0.0",
  "description": "Parte A de la Tarea 2: buscador de libros con JavaScript, Bootstrap y Sass",
  "private": true,
  "scripts": {
    "sass": "sass --load-path=node_modules --quiet-deps --silence-deprecation=import scss/main.scss css/main.css",
    "sass:watch": "sass --watch --load-path=node_modules --quiet-deps --silence-deprecation=import scss/main.scss css/main.css",
    "prestart": "npm run sass",
    "start": "serve . -l 3000"
  }
}
```

Ahora instala las dependencias (desde `buscador-js`):

```powershell
npm install bootstrap@5
npm install -D sass serve
```

npm agrega solo las secciones `dependencies` y `devDependencies` al `package.json` y crea `package-lock.json` y `node_modules/`. Abre el `package.json` y comprueba que quedó parecido a esto (los números de versión pueden variar un poco):

```json
  "dependencies": {
    "bootstrap": "^5.3.8"
  },
  "devDependencies": {
    "sass": "^1.105.2",
    "serve": "^14.2.6"
  }
```

**Si npm dice "X vulnerabilities" al terminar:** es un aviso, no un error. La instalación funcionó. Las vulnerabilidades vienen de paquetes que usa `serve` por dentro (por ejemplo `compression`), es decir, de una herramienta que solo corre en tu computadora mientras desarrollas. No llegan a la página que ve el usuario. **No ejecutes `npm audit fix --force`**: para "arreglarlo" bajaría `serve` a una versión vieja que no es compatible, y se puede romper el script `start`. Si quieres ver el detalle, corre `npm audit`.

### Por qué

- **Bootstrap con npm y no con CDN**: la consigna lo pide, y además es necesario para personalizarlo con Sass. El CDN solo da el CSS ya compilado; con npm tienes los archivos `.scss` originales en `node_modules/bootstrap/scss/`.
- **`dependencies` vs `devDependencies`**: Bootstrap se usa en la página que ve el usuario (su JavaScript del modal y la navbar se carga en el navegador), por eso va en `dependencies`. Sass y serve solo se usan mientras desarrollas, por eso van con `-D` en `devDependencies`.
- **`package-lock.json`** guarda la versión exacta de cada paquete instalado. Se sube para que el profe instale exactamente lo mismo que tú.
- **Los scripts**:
  - `sass`: compila `scss/main.scss` y escribe `css/main.css`.
    - `--load-path=node_modules` le dice a Sass dónde buscar cuando escribes `@import "bootstrap/scss/..."`. Sin esto no encuentra Bootstrap.
    - `--quiet-deps` oculta los avisos que salen de los archivos de Bootstrap (no son tuyos y no los puedes arreglar).
    - `--silence-deprecation=import` oculta el aviso de que `@import` va a desaparecer en Sass 3. Bootstrap 5 todavía está hecho con `@import`, así que tu código también lo usa para poder compartir variables con Bootstrap.
  - `sass:watch`: igual, pero se queda corriendo y recompila cada vez que guardas un `.scss`.
  - `prestart`: npm ejecuta automáticamente cualquier script `preX` antes de `X`. Así `npm start` siempre compila Sass antes de levantar el servidor.
  - `start`: levanta un servidor local en el puerto 3000. **Hace falta un servidor** porque los módulos de JavaScript (`type="module"`) no funcionan si abres el HTML con doble clic (`file://`); el navegador los bloquea por seguridad (política CORS).

### Prueba

```powershell
ls node_modules\bootstrap\scss
```
Tienen que aparecer archivos como `_variables.scss`, `_buttons.scss`, etc. (En cmd usa `dir` en lugar de `ls`.)

```powershell
cd ..
git status
```
Deben aparecer solo `buscador-js/package.json` y `buscador-js/package-lock.json`. **No** debe aparecer `node_modules`.

### Commit

```powershell
git add .
git commit -m "Parte A: configura npm con Bootstrap, Sass y los scripts de compilación"
git push
```

---

## Commit 3. Personalizar Bootstrap con Sass

**Qué vas a hacer:** crear los primeros parciales de Sass, cambiar los colores y la tipografía de Bootstrap con sus variables de origen y compilar.

**Archivos:**
- [NUEVO] `buscador-js/scss/_variables.scss`
- [NUEVO] `buscador-js/scss/_mixins.scss`
- [NUEVO] `buscador-js/scss/main.scss`

### `buscador-js/scss/_variables.scss` (archivo completo)

```scss
// Variables de origen de Bootstrap.
// Tienen que ir antes de importar Bootstrap para que sus valores por defecto (!default) no se apliquen.
$primary: #2f5d50;
$secondary: #b5838d;
$success: #3a7d44;
$danger: #b23a48;
$warning: #e0a458;
$light: #f7f3ee;

$body-bg: #fbf9f6;
$font-family-sans-serif: "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
$headings-font-family: Georgia, "Times New Roman", serif;
$border-radius: .6rem;
$border-radius-lg: .9rem;
$enable-shadows: true;

// Variables propias del proyecto
$espacio-base: 16px;
$alto-portada: 220px;
$duracion-animacion: .35s;
```

### `buscador-js/scss/_mixins.scss` (archivo completo)

```scss
@use "sass:math";
@use "sass:map";

// Convierte píxeles a rem tomando 16px como base. Ej: a-rem(24px) => 1.5rem
@function a-rem($px) {
  @return math.div($px, 16px) * 1rem;
}

// Media query mobile-first: los estilos aplican desde ese punto de corte hacia arriba.
// Usa los mismos puntos de corte de Bootstrap ($grid-breakpoints).
@mixin desde($punto) {
  @media (min-width: map.get($grid-breakpoints, $punto)) {
    @content;
  }
}

@mixin sombra-suave {
  box-shadow: 0 a-rem(4px) a-rem(12px) rgba(0, 0, 0, .08);
}
```

### `buscador-js/scss/main.scss` (archivo completo, versión 1)

```scss
// 1. Mis variables primero, para que Bootstrap tome estos valores en lugar de los suyos
@import "variables";

// 2. Bootstrap completo (en el commit 15 se cambia por solo las partes que se usan)
@import "bootstrap/scss/bootstrap";

// 3. Estilos propios
@import "mixins";
```

### Por qué

- **Parciales**: los archivos que empiezan con `_` (guion bajo) son *parciales*. Sass no los compila solos; solo existen para ser importados. El único archivo que se compila es `main.scss`, que los junta. Al importar se escribe sin `_` y sin `.scss`: `@import "variables"`.
- **Variables de origen y `!default`**: dentro de Bootstrap, las variables están declaradas así: `$primary: $blue !default;`. El `!default` significa "asigna este valor *solo si la variable no tiene uno todavía*". Como tu `_variables.scss` se importa **antes**, `$primary` ya existe con tu verde y Bootstrap respeta tu valor. Luego Bootstrap genera todo (botones, navbar, alertas, links) con ese color.
- **Por qué no `!important`**: la consigna lo prohíbe y la rúbrica lo califica como deficiente. Con `!important` el CSS de Bootstrap seguiría teniendo sus colores azules y tú pondrías parches encima. Con variables de origen el CSS se genera correcto desde el principio, sin pelear con la especificidad.
- **Variables propias** (`$espacio-base`, `$alto-portada`, `$duracion-animacion`): son el requisito de "al menos una variable". Centralizan valores que se usan en varios lugares; si quieres que las animaciones duren más, cambias una línea.
- **La función `a-rem()`**: recibe un valor en píxeles y **devuelve** un valor en rem. `math.div` es la división de Sass (el `/` ya no se usa para dividir). Se usa rem porque es una unidad relativa al tamaño de letra del navegador: si una persona aumenta la letra por accesibilidad, todo crece en proporción.
- **El mixin `desde()`**: recibe un nombre de punto de corte (`sm`, `md`, `lg`...) y **genera** una media query `min-width` con el valor que Bootstrap tiene en `$grid-breakpoints`. `@content` es el lugar donde se inserta lo que escribas dentro del `@include`. Usar los puntos de corte de Bootstrap hace que tu componente cambie en los mismos tamaños que la grilla.
- **Diferencia función vs mixin** (pregunta típica): la función devuelve *un valor* que usas dentro de una propiedad (`height: a-rem(220px)`); el mixin inserta *un bloque de reglas* (`@include desde(lg) { ... }`).
- **`@use "sass:math"` y `@use "sass:map"`**: cargan módulos que trae Sass. `map.get` saca un valor de un mapa (como un objeto en JS). Se usan así en vez de las funciones globales viejas (`map-get`) para que Sass no muestre avisos de que están obsoletas.
- **El orden de `main.scss` importa**: variables → Bootstrap → mixins. Los mixins van después de Bootstrap porque usan `$grid-breakpoints`, que se define dentro de Bootstrap.

### Prueba

```powershell
cd buscador-js
npm run sass
```
No debe salir ningún error y se crea `css/main.css`. Ábrelo y busca (Ctrl+F) `#2f5d50`: tiene que aparecer, porque Bootstrap ya usa tu color primario. Por ejemplo `--bs-primary: #2f5d50;`.

En `git status` **no** debe aparecer la carpeta `css/` (está en `.gitignore`).

### Commit

```powershell
cd ..
git add .
git commit -m "Parte A: personaliza Bootstrap con variables de origen y agrega mixins y una función de Sass"
git push
```

---

# Miércoles 21 de octubre

## Commit 4. HTML de la página

**Qué vas a hacer:** escribir toda la estructura fija de la página: navbar, buscador, lista de lecturas (tabla), formulario de propuesta y modal. Lo dinámico (las tarjetas y las filas) lo crea JavaScript más adelante.

**Archivos:**
- [NUEVO] `buscador-js/index.html`
- [NUEVO] `buscador-js/img/sin-portada.svg`

### `buscador-js/index.html` (archivo completo)

Fíjate que al final **solo** está el script de Bootstrap. El de `js/main.js` se agrega en el commit 7, cuando ese archivo exista (si lo pones ahora, la consola muestra un error 404).

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Estantería Común | Buscador de libros</title>
  <link rel="stylesheet" href="css/main.css">
</head>
<body>
  <!-- Barra de navegación -->
  <nav class="navbar navbar-expand-md navbar-dark bg-primary sticky-top">
    <div class="container">
      <a class="navbar-brand fw-bold" href="#">Estantería Común</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#menu"
        aria-controls="menu" aria-expanded="false" aria-label="Abrir menú">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="menu">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item"><a class="nav-link" href="#buscar">Buscar</a></li>
          <li class="nav-item"><a class="nav-link" href="#mi-lista">Mi lista</a></li>
          <li class="nav-item"><a class="nav-link" href="#proponer">Proponer lectura</a></li>
        </ul>
      </div>
    </div>
  </nav>

  <main class="container py-4">
    <div class="row g-4">
      <!-- Columna principal: buscador y resultados -->
      <section id="buscar" class="col-12 col-lg-8">
        <h1 class="h3 mb-3">Buscar libros</h1>

        <form id="form-busqueda" class="row g-2 mb-3" role="search">
          <div class="col-12 col-sm-9">
            <label for="texto-busqueda" class="visually-hidden">Título o autor</label>
            <input type="search" id="texto-busqueda" class="form-control" placeholder="Título o autor"
              required minlength="2">
          </div>
          <div class="col-12 col-sm-3 d-grid">
            <button type="submit" class="btn btn-primary">Buscar</button>
          </div>
        </form>

        <!-- Aquí se muestran los estados de carga, error y sin resultados -->
        <div id="estado-busqueda" aria-live="polite"></div>

        <div id="resultados" class="estante"></div>
      </section>

      <!-- Columna lateral: lista de lecturas -->
      <aside id="mi-lista" class="col-12 col-lg-4">
        <div class="card">
          <div class="card-header bg-white">
            <h2 class="h5 mb-0">Mi lista de lecturas</h2>
          </div>
          <div class="card-body p-0">
            <div id="aviso-lista" class="px-3 pt-3" aria-live="polite"></div>
            <div class="table-responsive">
              <table class="table table-hover align-middle mb-0">
                <thead>
                  <tr>
                    <th scope="col">Libro</th>
                    <th scope="col">Estado</th>
                    <th scope="col" class="text-end">Acciones</th>
                  </tr>
                </thead>
                <tbody id="tabla-lecturas"></tbody>
              </table>
            </div>
            <p id="lista-vacia" class="text-muted small p-3 mb-0">Todavía no agregaste libros.</p>
          </div>
        </div>
      </aside>
    </div>

    <!-- Formulario de propuesta -->
    <section id="proponer" class="mt-5">
      <div class="row justify-content-center">
        <div class="col-12 col-md-10 col-xl-8">
          <div class="card">
            <div class="card-body p-4">
              <h2 class="h4 mb-3">Proponer un libro para la próxima sesión</h2>

              <div id="alerta-propuesta" aria-live="polite"></div>

              <form id="form-propuesta" novalidate>
                <div class="row g-3">
                  <div class="col-12">
                    <label for="titulo" class="form-label">Título</label>
                    <input type="text" id="titulo" name="titulo" class="form-control" required minlength="2"
                      maxlength="120">
                    <div class="invalid-feedback"></div>
                  </div>

                  <div class="col-12">
                    <label class="form-label">Autores</label>
                    <div id="lista-autores">
                      <div class="input-group has-validation mb-2 campo-autor">
                        <input type="text" name="autor" class="form-control" placeholder="Nombre del autor"
                          aria-label="Autor" required minlength="3" maxlength="80"
                          pattern="[A-Za-zÁÉÍÓÚáéíóúÑñÜü.' \-]+">
                        <button type="button" class="btn btn-outline-danger" data-accion="quitar-autor"
                          aria-label="Quitar autor">&times;</button>
                        <div class="invalid-feedback"></div>
                      </div>
                    </div>
                    <button type="button" id="agregar-autor" class="btn btn-sm btn-outline-primary">+ Agregar
                      autor</button>
                    <small class="text-muted ms-2">Mínimo 1, máximo 5.</small>
                  </div>

                  <div class="col-12 col-sm-6 col-md-4">
                    <label for="anio" class="form-label">Año de publicación</label>
                    <input type="number" id="anio" name="anio" class="form-control" required min="1450" step="1">
                    <div class="invalid-feedback"></div>
                  </div>

                  <div class="col-12 col-sm-6 col-md-4">
                    <label for="paginas" class="form-label">Cantidad de páginas</label>
                    <input type="number" id="paginas" name="paginas" class="form-control" required min="10"
                      max="3000" step="1">
                    <div class="invalid-feedback"></div>
                  </div>

                  <div class="col-12 col-md-4">
                    <label for="genero" class="form-label">Género</label>
                    <select id="genero" name="genero" class="form-select" required>
                      <option value="">Elija uno...</option>
                      <option>Novela</option>
                      <option>Cuento</option>
                      <option>Poesía</option>
                      <option>Ensayo</option>
                      <option>Ciencia ficción</option>
                      <option>Fantasía</option>
                      <option>Historia</option>
                    </select>
                    <div class="invalid-feedback"></div>
                  </div>

                  <div class="col-12 col-md-6">
                    <label for="correo" class="form-label">Correo de quien propone</label>
                    <input type="email" id="correo" name="correo" class="form-control" required
                      placeholder="nombre@correo.com">
                    <div class="invalid-feedback"></div>
                  </div>

                  <div class="col-12 col-md-6">
                    <label for="fecha" class="form-label">Fecha propuesta para la sesión</label>
                    <input type="date" id="fecha" name="fecha" class="form-control" required>
                    <div class="invalid-feedback"></div>
                  </div>

                  <div class="col-12">
                    <label for="motivo" class="form-label">Motivo de la propuesta</label>
                    <textarea id="motivo" name="motivo" class="form-control" rows="3" required minlength="20"
                      maxlength="500"></textarea>
                    <div class="invalid-feedback"></div>
                  </div>

                  <div class="col-12 d-grid d-sm-flex justify-content-sm-end">
                    <button type="submit" id="boton-enviar" class="btn btn-primary px-4">Enviar propuesta</button>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>

  <!-- Modal de detalle del libro -->
  <div class="modal fade" id="modal-detalle" tabindex="-1" aria-labelledby="detalle-titulo" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h2 class="modal-title h5" id="detalle-titulo"></h2>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
        </div>
        <div class="modal-body">
          <div class="row g-3">
            <div class="col-12 col-sm-5 text-center">
              <img id="detalle-portada" class="img-fluid detalle-portada" src="img/sin-portada.svg" alt="">
            </div>
            <div class="col-12 col-sm-7">
              <dl class="mb-0" id="detalle-datos"></dl>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
        </div>
      </div>
    </div>
  </div>

  <script src="node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### `buscador-js/img/sin-portada.svg` (archivo completo)

Crea la carpeta `img` dentro de `buscador-js` y este archivo:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="180" height="270" viewBox="0 0 180 270">
  <rect width="180" height="270" rx="8" fill="#e9e4dc"/>
  <rect x="30" y="60" width="120" height="150" rx="4" fill="none" stroke="#2f5d50" stroke-width="4"/>
  <line x1="50" y1="100" x2="130" y2="100" stroke="#2f5d50" stroke-width="4"/>
  <line x1="50" y1="125" x2="130" y2="125" stroke="#2f5d50" stroke-width="4"/>
  <line x1="50" y1="150" x2="100" y2="150" stroke="#2f5d50" stroke-width="4"/>
  <text x="90" y="245" font-family="Arial, sans-serif" font-size="16" fill="#2f5d50" text-anchor="middle">Sin portada</text>
</svg>
```

### Por qué

**Grilla de Bootstrap (criterio 8):**
- `container` centra el contenido y le pone un ancho máximo según el tamaño de pantalla.
- `row` es la fila; `g-4` es el espacio (gutter) entre columnas.
- `col-12 col-lg-8` y `col-12 col-lg-4`: en pantallas pequeñas cada columna ocupa las 12 partes (todo el ancho, una debajo de otra). Desde `lg` (992 px) los resultados ocupan 8 de 12 y la lista 4 de 12, lado a lado. Así se lee: "12 columnas siempre, y desde lg, 8".
- En el formulario, `col-12 col-sm-6 col-md-4` hace que año y páginas estén uno debajo del otro en celular, lado a lado desde `sm` y en tres columnas (con género) desde `md`.

**Componentes de Bootstrap pedidos:**
- **Navbar**: `navbar-expand-md` hace que el menú se vea completo desde `md` y se convierta en botón hamburguesa en celular. El botón usa `data-bs-toggle="collapse"`, y el JavaScript de Bootstrap se encarga de abrir y cerrar sin que escribas código.
- **Tarjeta**: `card`, `card-header`, `card-body` en la lista y en el formulario. Las tarjetas de libros se crean desde JS.
- **Tabla**: `table table-hover align-middle` dentro de `table-responsive`, que agrega scroll horizontal si la tabla no cabe en celular.
- **Formulario**: `form-control`, `form-select`, `form-label` e `invalid-feedback`.
- **Modal**: la estructura `modal > modal-dialog > modal-content > header/body/footer`. `fade` le da animación. Los botones con `data-bs-dismiss="modal"` lo cierran sin código tuyo.
- **Alerta**: se crean desde JavaScript (commit 6).

**Clases utilitarias:** `py-4`, `mb-3`, `mt-5`, `p-0`, `fw-bold`, `text-muted`, `small`, `ms-auto`, `d-grid`, `d-sm-flex`, `justify-content-sm-end`, `visually-hidden`. Son clases de una sola propiedad que trae Bootstrap; usarlas evita escribir CSS propio para cosas como márgenes.

**Atributos del formulario (criterio 3):**
- `required`: obligatorio.
- `minlength` / `maxlength`: largo mínimo y máximo del texto.
- `type="number"` con `min`, `max` y `step="1"`: solo enteros dentro de un rango. El `max` del año no está porque depende del año actual; lo pone JavaScript en el commit 12.
- `type="email"`: el navegador revisa que tenga formato de correo.
- `type="date"`: selector de fecha. El `min` (hoy) también lo pone JavaScript.
- `pattern` en autor: expresión regular que solo deja letras (incluidas tildes y ñ), espacios, puntos, apóstrofos y guiones.
- `novalidate` en el `<form>`: **apaga los globitos** del navegador, pero las reglas siguen funcionando. Lo hacemos para poner mensajes propios al lado de cada campo.
- Cada campo tiene un `<div class="invalid-feedback">` vacío al lado, donde JavaScript escribe el error. Bootstrap solo lo muestra cuando el input tiene la clase `is-invalid`.
- El autor está dentro de `input-group has-validation` para que el botón de quitar quede pegado al campo y el mensaje de error se vea bien dentro del grupo.
- Todos los inputs de autor se llaman `name="autor"` (el mismo nombre). Eso es clave para el envío (commit 14).

**Accesibilidad:**
- `aria-live="polite"` en los contenedores de mensajes: los lectores de pantalla anuncian el texto cuando cambia.
- `visually-hidden` en la etiqueta del buscador: no se ve, pero el lector de pantalla la lee.
- `lang="es"` para que el navegador sepa el idioma.

**Los ids** (`form-busqueda`, `resultados`, `tabla-lecturas`, etc.) son los puntos donde JavaScript se va a conectar. Por eso el HTML va antes que el JS.

### Prueba

```powershell
cd buscador-js
npm start
```
Si Windows muestra una ventana del Firewall preguntando si Node.js puede usar la red, permite el acceso en redes privadas (es el servidor local).

Abre http://localhost:3000. Tiene que verse la página completa con los colores verdes. Achica la ventana: la lista debe bajar debajo del buscador y el menú convertirse en botón hamburguesa. Abre la consola (F12): no debe haber errores. Para detener el servidor usa Ctrl+C.

### Commit

```powershell
cd ..
git add .
git commit -m "Parte A: maqueta la página con la grilla de Bootstrap, navbar, tabla, modal y formulario"
git push
```

---

## Commit 5. Componente propio con CSS Grid

**Qué vas a hacer:** crear el componente `.estante`, donde se acomodan las tarjetas de resultados, y los estilos de las tarjetas y de las etiquetas de estado.

**Archivos:**
- [NUEVO] `buscador-js/scss/_componentes.scss`
- [MODIFICAR] `buscador-js/scss/main.scss`

### `buscador-js/scss/_componentes.scss` (archivo completo)

```scss
// Componente propio: el "estante" donde se acomodan los resultados.
// Mobile-first: arranca en una columna y va sumando columnas con min-width.
.estante {
  --columnas: 1;
  --separacion: #{a-rem($espacio-base)};

  display: grid;
  grid-template-columns: repeat(var(--columnas), minmax(0, 1fr));
  gap: var(--separacion);

  @include desde(sm) {
    --columnas: 2;
  }

  @include desde(lg) {
    --columnas: 3;
    --separacion: #{a-rem($espacio-base * 1.5)};
  }

  @include desde(xxl) {
    --columnas: 4;
  }
}

// Tarjeta de cada libro dentro del estante
.libro {
  transition: transform $duracion-animacion ease;

  &:hover {
    transform: translateY(-0.25rem);
    @include sombra-suave;
  }

  &__portada {
    height: a-rem($alto-portada);
    object-fit: contain;
    background-color: $light;
    padding: .5rem;
  }

  &__titulo {
    font-size: 1rem;
    margin-bottom: .25rem;
  }

  &__autores {
    font-size: .875rem;
    color: $secondary;
  }
}

// Colores de cada estado de lectura
.estado {
  &--pendiente {
    background-color: $warning;
    color: $dark;
  }

  &--leyendo {
    background-color: $primary;
  }

  &--leido {
    background-color: $success;
  }
}

.detalle-portada {
  max-height: a-rem(320px);
  object-fit: contain;
}
```

### `buscador-js/scss/main.scss` (cambio)

**Busca esto:**
```scss
// 3. Estilos propios
@import "mixins";
```

**Agrega debajo:**
```scss
@import "componentes";
```

### Por qué

Este componente cubre el criterio 9: CSS Grid, variables CSS, unidades relativas y mobile-first.

- **CSS Grid**: `display: grid` con `grid-template-columns: repeat(var(--columnas), minmax(0, 1fr))` crea N columnas iguales. `1fr` es "una fracción del espacio libre". `minmax(0, 1fr)` evita que un título muy largo estire su columna más que las otras.
- **Variables CSS** (`--columnas`, `--separacion`): son distintas a las de Sass. Las de Sass (`$primary`) **desaparecen** al compilar, se reemplazan por su valor. Las de CSS **existen en el navegador** y se pueden cambiar en tiempo real. Por eso aquí solo se cambia `--columnas` dentro de cada media query, y la regla `grid-template-columns` se escribe una sola vez.
- **`#{...}` (interpolación)**: para guardar el resultado de una función de Sass dentro de una variable CSS hay que escribirlo así; si no, Sass deja el texto `a-rem($espacio-base)` sin calcular.
- **Mobile-first**: el estilo base (sin media query) es para celular: 1 columna. Luego con `min-width` (el mixin `desde`) se van **sumando** columnas a medida que hay más espacio: 2 desde `sm` (576 px), 3 desde `lg` (992 px), 4 desde `xxl` (1400 px). Lo contrario sería desktop-first con `max-width`, que obliga a deshacer estilos en pantallas chicas.
- **Unidades relativas**: `rem` en todo (la separación, la altura de la portada, el padding). Nada en px salvo como entrada de `a-rem()`.
- **Anidamiento con propósito**: dentro de `.libro` están `&:hover`, `&__portada`, `&__titulo` y `&__autores`. El `&` se reemplaza por el selector padre, así que `&__portada` compila a `.libro__portada`. Esto es la convención **BEM** (bloque__elemento--modificador): todo lo que pertenece a la tarjeta queda agrupado. `.estado { &--pendiente }` compila a `.estado--pendiente`. No se anida por anidar: cada nivel corresponde a una parte del mismo componente.
- **`@include sombra-suave`** dentro del hover y **`@include desde(...)`** dentro del estante: aquí se usan los mixins del commit 3.
- **`$warning`, `$primary`, `$success`, `$light`, `$dark`**: son las variables de Bootstrap (las tuyas o las de Bootstrap). Así los colores de los estados combinan con el resto.
- **Por qué no usé solo la grilla de Bootstrap para los resultados**: la consigna pide un componente propio con Grid o Flexbox. La grilla de Bootstrap se usa para el diseño general de la página; el estante es el componente propio.

### Prueba

Corre `npm start` (desde `buscador-js`). La página se ve igual porque todavía no hay tarjetas. Abre `css/main.css` y busca `.estante`: tienen que aparecer la regla base y tres `@media (min-width: ...)`. Busca `.libro__portada`: tiene que estar con `height: 13.75rem` (220 / 16).

### Commit

```powershell
git add .
git commit -m "Parte A: crea el componente propio .estante con CSS Grid, variables CSS y enfoque mobile-first"
git push
```

---

## Commit 6. Módulo de la API y módulo de renderizado

**Qué vas a hacer:** escribir `api.js`, que habla con Open Library, y la primera versión de `render.js`, que crea las tarjetas y los mensajes con métodos del DOM.

**Archivos:**
- [NUEVO] `buscador-js/js/api.js`
- [NUEVO] `buscador-js/js/render.js`

### `buscador-js/js/api.js` (archivo completo, versión 1)

```js
// Acceso a servicios externos. Este módulo solo hace peticiones y devuelve datos:
// no sabe nada del DOM.

const URL_BUSQUEDA = 'https://openlibrary.org/search.json';
const URL_PORTADAS = 'https://covers.openlibrary.org/b/id';

export async function buscarLibros(texto) {
  const url = `${URL_BUSQUEDA}?q=${encodeURIComponent(texto)}&limit=12`;
  const respuesta = await fetch(url);

  // fetch solo lanza error si falla la red; un 404 o un 500 hay que revisarlo a mano
  if (!respuesta.ok) {
    throw new Error(`Open Library respondió con estado ${respuesta.status}`);
  }

  const datos = await respuesta.json();
  return datos.docs.map(convertirLibro);
}

// Me quedo solo con los campos que uso, con nombres en español
function convertirLibro(doc) {
  return {
    id: doc.key,
    titulo: doc.title,
    autores: doc.author_name ?? ['Autor desconocido'],
    anio: doc.first_publish_year ?? 'Sin dato',
    portada: doc.cover_i ? `${URL_PORTADAS}/${doc.cover_i}-M.jpg` : null,
    ediciones: doc.edition_count ?? 0,
    idiomas: doc.language ?? []
  };
}
```

### `buscador-js/js/render.js` (archivo completo, versión 1)

```js
// Funciones que crean o actualizan elementos en pantalla.
// Todo se arma con createElement y textContent; nunca se mete texto de la API como HTML.

const PORTADA_ALTERNATIVA = 'img/sin-portada.svg';

// Helper para no repetir createElement + className + textContent en cada elemento
function crear(etiqueta, clases = '', texto = '') {
  const elemento = document.createElement(etiqueta);
  if (clases) elemento.className = clases;
  if (texto) elemento.textContent = texto;
  return elemento;
}

function crearPortada(libro, clases) {
  const imagen = crear('img', clases);
  imagen.src = libro.portada ?? PORTADA_ALTERNATIVA;
  imagen.alt = `Portada de ${libro.titulo}`;
  imagen.loading = 'lazy';
  // Si la URL existe pero la imagen no carga, también uso la alternativa
  imagen.addEventListener('error', () => {
    imagen.src = PORTADA_ALTERNATIVA;
  }, { once: true });
  return imagen;
}

// ---------- Resultados de búsqueda ----------

function crearTarjetaLibro(libro) {
  const tarjeta = crear('article', 'card h-100 libro');
  tarjeta.dataset.id = libro.id;

  const cuerpo = crear('div', 'card-body d-flex flex-column');
  const titulo = crear('h3', 'card-title libro__titulo', libro.titulo);
  const autores = crear('p', 'card-text libro__autores mb-1', libro.autores.join(', '));
  const anio = crear('p', 'card-text small text-muted', `Primera publicación: ${libro.anio}`);

  const boton = crear('button', 'btn btn-sm btn-outline-primary mt-auto', 'Agregar a mi lista');
  boton.type = 'button';
  boton.dataset.accion = 'agregar';

  cuerpo.append(titulo, autores, anio, boton);
  tarjeta.append(crearPortada(libro, 'card-img-top libro__portada'), cuerpo);
  return tarjeta;
}

export function mostrarResultados(contenedor, libros) {
  // replaceChildren vacía el contenedor y pone las tarjetas nuevas de una vez
  contenedor.replaceChildren(...libros.map(crearTarjetaLibro));
}

// tipo: 'cargando' | 'error' | 'vacio' | 'ninguno'
export function mostrarEstado(contenedor, tipo, mensaje = '') {
  if (tipo === 'ninguno') {
    contenedor.replaceChildren();
    return;
  }

  if (tipo === 'cargando') {
    const caja = crear('div', 'd-flex align-items-center gap-2 text-primary mb-3');
    const spinner = crear('div', 'spinner-border spinner-border-sm');
    spinner.setAttribute('role', 'status');
    caja.append(spinner, crear('span', '', 'Buscando libros...'));
    contenedor.replaceChildren(caja);
    return;
  }

  const color = tipo === 'error' ? 'alert-danger' : 'alert-info';
  contenedor.replaceChildren(crearAlerta(color, mensaje));
}

export function crearAlerta(color, mensaje) {
  const alerta = crear('div', `alert ${color} mb-3`, mensaje);
  alerta.setAttribute('role', 'alert');
  return alerta;
}
```

### Por qué

**`api.js` (criterio 5 y 7):**
- **Separar el acceso a la API**: este módulo no toca el DOM. Recibe texto y devuelve un arreglo de libros. Si mañana cambian de Open Library a Google Books, solo se cambia este archivo.
- **`async`/`await`**: `fetch` devuelve una promesa (un valor que llega después). `await` pausa *esa función* hasta que llegue la respuesta, sin congelar la página. El código se lee de arriba abajo, en vez de encadenar `.then().then()`.
- **`encodeURIComponent(texto)`**: convierte caracteres especiales para que sean válidos en una URL. Si alguien busca `harry & potter`, el `&` partiría la URL en otro parámetro. Con esto se envía como `harry%20%26%20potter`.
- **`respuesta.ok`**: es `true` cuando el código HTTP está entre 200 y 299. **`fetch` solo falla (rechaza la promesa) cuando no hay conexión.** Si el servidor responde 404 o 500, `fetch` lo considera "éxito" porque sí hubo respuesta. Por eso hay que revisar `ok` y lanzar el error a mano con `throw`.
- **No hay `try/catch` aquí a propósito**: el error se lanza y lo atrapa quien llama (`main.js`, commit 7). La razón es que quien llama es el que sabe qué mostrar en pantalla. `api.js` no debería saber que existe una alerta.
- **`convertirLibro`**: Open Library devuelve muchos campos con nombres en inglés (`author_name`, `first_publish_year`, `cover_i`). Esta función arma un objeto más simple con solo lo que uso. Así el resto del código no depende del formato de la API.
- **`??` (nullish coalescing)**: si el valor de la izquierda es `null` o `undefined`, usa el de la derecha. Algunos libros no traen autor ni año, y así no aparece `undefined` en pantalla.
- **La portada**: la consigna da la URL `covers.openlibrary.org/b/id/{cover_i}-M.jpg`. Si el libro no tiene `cover_i`, se deja `null` y `render.js` pone la imagen alternativa.
- **`id: doc.key`**: Open Library da a cada obra una clave única como `/works/OL27448W`. Se usa como id para no agregar dos veces el mismo libro.

**`render.js` (criterio 1):**
- **Por qué no `innerHTML`**: si armara la tarjeta como texto, por ejemplo `` `<h3>${libro.titulo}</h3>` ``, y un título trajera `<img src=x onerror="alert('hackeado')">`, el navegador lo interpretaría como HTML y ejecutaría ese código. Eso se llama **XSS** (cross-site scripting). Con `textContent` el texto siempre se trata como texto, nunca como HTML. La consigna lo prohíbe y la rúbrica lo califica como deficiente.
- **`crear(etiqueta, clases, texto)`**: crear un elemento siempre son tres pasos (crear, poner clases, poner texto). Esta función evita repetirlos en cada línea. Es lo que la rúbrica llama "funciones de renderizado reutilizables".
- **`crearTarjetaLibro(libro)`** recibe *un* libro y devuelve *un* elemento `<article>`. No lo inserta en la página; eso lo hace `mostrarResultados`. Separarlo así permite usar `map`.
- **`libros.map(crearTarjetaLibro)`**: convierte el arreglo de datos en un arreglo de elementos. Es "generar la interfaz a partir del arreglo recibido", como pide la consigna.
- **`replaceChildren(...elementos)`**: vacía el contenedor y pone los nuevos de una vez. El `...` (spread) pasa cada elemento del arreglo como un argumento separado. Es la forma moderna de limpiar un contenedor sin usar `innerHTML = ''`.
- **`dataset.id` y `dataset.accion`**: escriben atributos `data-id="..."` y `data-accion="agregar"` en el HTML. Sirven para que, más adelante, cuando se haga clic, JavaScript sepa de qué libro y de qué acción se trata (commit 9).
- **`crearPortada`**: si `libro.portada` es `null` usa la imagen alternativa. Además, si la URL existe pero la imagen falla (pasa con algunas portadas de Open Library), el evento `error` cambia a la alternativa. `{ once: true }` borra la escucha después de usarse una vez; si la alternativa también fallara, no se queda en un ciclo infinito.
- **`loading = 'lazy'`**: el navegador no descarga la imagen hasta que está cerca de verse. Las portadas son lo más pesado de la página, así que esto mejora el rendimiento.
- **`mostrarEstado`**: una sola función para los estados de la búsqueda. `'cargando'` pone un spinner de Bootstrap, `'error'` una alerta roja, `'vacio'` una alerta azul y `'ninguno'` limpia. La rúbrica pide distinguir *explícitamente* carga, error, vacío y resultados.
- **`role="alert"` y `role="status"`**: atributos de accesibilidad para que los lectores de pantalla anuncien el mensaje.
- **`export`**: solo se exporta lo que otros módulos necesitan. `crear`, `crearPortada` y `crearTarjetaLibro` son internos.

### Prueba

Todavía no hay `main.js`, pero puedes probar los módulos desde la consola. Corre `npm start`, abre http://localhost:3000, abre la consola (F12) y escribe:

```js
const api = await import('./js/api.js');
const libros = await api.buscarLibros('tolkien');
console.table(libros);
```
Tiene que salir una tabla con 12 libros. Luego:

```js
const render = await import('./js/render.js');
render.mostrarResultados(document.getElementById('resultados'), libros);
```
Tienen que aparecer las tarjetas en el estante. Achica la ventana para ver cómo cambian las columnas. Recarga la página para dejarla limpia.

### Commit

```powershell
git add .
git commit -m "Parte A: agrega api.js para buscar en Open Library y render.js para crear tarjetas con métodos del DOM"
git push
```

---

# Jueves 22 de octubre

Hoy la Parte A empieza a funcionar. Además vas a encontrar un error **a propósito** en el commit 9 y lo vas a depurar con puntos de interrupción. Ese error real es el que se documenta en `docs/depuracion.md`, así que hoy también tomas tus primeras capturas.

## Commit 7. Búsqueda con los cuatro estados

**Qué vas a hacer:** crear `main.js`, que conecta el formulario de búsqueda con `api.js` y `render.js`, y cargarlo desde el HTML.

**Archivos:**
- [NUEVO] `buscador-js/js/main.js`
- [MODIFICAR] `buscador-js/index.html`

### `buscador-js/js/main.js` (archivo completo, versión 1)

```js
// Punto de entrada: conecta los eventos de la página con los demás módulos.
import { buscarLibros } from './api.js';
import { mostrarResultados, mostrarEstado } from './render.js';

const formBusqueda = document.getElementById('form-busqueda');
const textoBusqueda = document.getElementById('texto-busqueda');
const estadoBusqueda = document.getElementById('estado-busqueda');
const resultados = document.getElementById('resultados');

// Libros de la última búsqueda, para encontrarlos por id cuando se hace clic en una tarjeta
let librosEncontrados = [];

// ---------- Búsqueda ----------

// Se busca solo cuando se envía el formulario (Enter o botón), no con cada tecla
formBusqueda.addEventListener('submit', async (evento) => {
  evento.preventDefault();
  const texto = textoBusqueda.value.trim();
  if (texto.length < 2) {
    mostrarEstado(estadoBusqueda, 'error', 'Escriba al menos 2 caracteres para buscar.');
    return;
  }

  mostrarEstado(estadoBusqueda, 'cargando');
  resultados.replaceChildren();

  try {
    librosEncontrados = await buscarLibros(texto);

    if (librosEncontrados.length === 0) {
      mostrarEstado(estadoBusqueda, 'vacio', `No se encontraron libros para "${texto}".`);
      return;
    }

    mostrarEstado(estadoBusqueda, 'ninguno');
    mostrarResultados(resultados, librosEncontrados);
  } catch (error) {
    librosEncontrados = [];
    mostrarEstado(estadoBusqueda, 'error', 'No se pudo conectar con Open Library. Intente de nuevo en un momento.');
    console.warn('Error en la búsqueda:', error.message);
  }
});
```

### `buscador-js/index.html` (cambio)

**Busca esto** (casi al final):
```html
  <script src="node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
```

**Agrega debajo:**
```html
  <script type="module" src="js/main.js"></script>
```

### Por qué

- **Un solo `<script type="module">`**: el HTML solo carga `main.js`. Ese archivo importa `api.js` y `render.js`, y el navegador descarga lo que haga falta. Ventajas de `type="module"`:
  - Permite `import` y `export`.
  - Cada archivo tiene su propio alcance: una variable de `main.js` no se ve en `render.js` (no hay variables globales sueltas que choquen).
  - Se ejecuta **después** de que el HTML terminó de cargarse (como `defer`), por eso `document.getElementById` encuentra los elementos aunque el script esté arriba o abajo.
- **Bootstrap se carga con un `<script>` normal antes**: el archivo `bootstrap.bundle.min.js` no es un módulo; crea una variable global `bootstrap`. Va antes para que ya exista cuando corre `main.js`.
- **El evento `submit` y no `click` ni `input`**: la consigna dice que la búsqueda se lanza al enviar el formulario, no con cada tecla. `submit` se dispara con el botón **y** con Enter. Con `input` se haría una petición por cada letra escrita.
- **`evento.preventDefault()`**: lo que hace un formulario por defecto es recargar la página enviando los datos. Esto lo cancela. Sin esto la rúbrica te pone deficiente ("la página se recarga al buscar").
- **`trim()`**: quita espacios al inicio y al final. Así "   " no cuenta como búsqueda.
- **El flujo de estados** (pregunta segura en la defensa):
  1. `mostrarEstado(..., 'cargando')`: aparece el spinner **antes** de esperar. Así la pantalla no queda en blanco mientras llega la respuesta (la rúbrica lo menciona).
  2. `resultados.replaceChildren()`: borra las tarjetas de la búsqueda anterior.
  3. `await buscarLibros(texto)`: espera la respuesta.
  4. Si el arreglo viene vacío: estado `'vacio'` con mensaje.
  5. Si hay libros: se quita el mensaje (`'ninguno'`) y se dibujan las tarjetas.
  6. Si hubo error de red **o** `api.js` lanzó el error por `!respuesta.ok`, se va al `catch` y se muestra el estado `'error'`.
- **`try/catch` aquí**: `api.js` lanza el error y `main.js` lo atrapa porque es el que decide qué se ve. El `console.warn` deja registro para depurar pero **no** es un error rojo en la consola (la rúbrica pide consola sin errores en uso normal; un fallo de red no es uso normal, pero igual queda controlado).
- **`librosEncontrados`**: se guarda el arreglo de la última búsqueda. Se usa en el commit 8 para encontrar el libro cuando se hace clic en una tarjeta. Es `let` porque se reasigna en cada búsqueda.
- **Template literal** (`` `No se encontraron libros para "${texto}".` ``): aunque `texto` lo escribió el usuario, es seguro porque `mostrarEstado` lo mete con `textContent` dentro de `crearAlerta`.

### Prueba

`npm start`, abre http://localhost:3000 y prueba los cuatro estados:
1. **Resultados**: busca `tolkien`. Primero se ve "Buscando libros..." y luego 12 tarjetas.
2. **Vacío**: busca `zzqxwvlkjh`. Debe salir la alerta azul.
3. **Error**: en la pestaña Network de DevTools cambia "No throttling" por **Offline** y busca algo. Debe salir la alerta roja. Vuelve a "No throttling".
4. **Sin recarga**: mira que la URL no cambia y que la página no parpadea.

Revisa que la consola no tenga errores rojos.

### Commit

```powershell
git add .
git commit -m "Parte A: implementa la búsqueda con fetch y los estados de carga, error, vacío y resultados"
git push
```

---

## Commit 8. Detalle del libro en el modal

**Qué vas a hacer:** al hacer clic en una tarjeta se abre el modal de Bootstrap con los datos del libro. En esta versión el modal se abre con los atributos `data-bs-*` de Bootstrap. (Spoiler: en el commit 9 esto causa un error y en el 10 se cambia.)

**Archivos:**
- [MODIFICAR] `buscador-js/js/render.js`
- [MODIFICAR] `buscador-js/js/main.js`

### `render.js` (cambio 1 de 2)

**Busca esto** (dentro de `crearTarjetaLibro`):
```js
  const tarjeta = crear('article', 'card h-100 libro');
  tarjeta.dataset.id = libro.id;
```

**Agrega debajo:**
```js
  // Bootstrap abre el modal solo con estos atributos
  tarjeta.dataset.bsToggle = 'modal';
  tarjeta.dataset.bsTarget = '#modal-detalle';
```

### `render.js` (cambio 2 de 2)

**Agrega al final del archivo** (después de la función `crearAlerta`):
```js

// ---------- Modal de detalle ----------

// Rellena el modal; Bootstrap se encarga de abrirlo
export function rellenarDetalle(libro) {
  document.getElementById('detalle-titulo').textContent = libro.titulo;

  const portada = document.getElementById('detalle-portada');
  portada.src = libro.portada ? libro.portada.replace('-M.jpg', '-L.jpg') : PORTADA_ALTERNATIVA;
  portada.alt = `Portada de ${libro.titulo}`;

  const datos = [
    ['Autores', libro.autores.join(', ')],
    ['Primera publicación', libro.anio],
    ['Ediciones', libro.ediciones],
    ['Idiomas', libro.idiomas.length ? libro.idiomas.slice(0, 5).join(', ') : 'Sin dato']
  ];

  const lista = document.getElementById('detalle-datos');
  lista.replaceChildren();
  datos.forEach(([nombre, valor]) => {
    lista.append(crear('dt', '', nombre), crear('dd', '', String(valor)));
  });
}
```

### `main.js` (cambio 1 de 2)

**Busca esto:**
```js
import { mostrarResultados, mostrarEstado } from './render.js';
```

**Cámbialo por:**
```js
import { mostrarResultados, mostrarEstado, rellenarDetalle } from './render.js';
```

### `main.js` (cambio 2 de 2)

**Agrega al final del archivo:**
```js

// ---------- Detalle ----------

function buscarPorId(id) {
  return librosEncontrados.find((libro) => libro.id === id);
}

// Bootstrap avisa con show.bs.modal antes de abrir; relatedTarget es la tarjeta que se tocó
document.getElementById('modal-detalle').addEventListener('show.bs.modal', (evento) => {
  rellenarDetalle(buscarPorId(evento.relatedTarget.dataset.id));
});

// Al cerrar, Bootstrap pone aria-hidden en el modal. Si el botón de cerrar sigue con el foco
// la consola muestra un aviso de accesibilidad, así que se quita el foco antes.
document.getElementById('modal-detalle').addEventListener('hide.bs.modal', () => {
  document.activeElement.blur();
});
```

### Por qué

- **`data-bs-toggle="modal"` y `data-bs-target`**: `dataset.bsToggle` se escribe en el HTML como `data-bs-toggle` (el camelCase se convierte en guiones). Bootstrap busca esos atributos y abre el modal indicado sin que escribas código para abrirlo.
- **`show.bs.modal`**: es un evento que Bootstrap dispara justo **antes** de mostrar el modal. `evento.relatedTarget` es el elemento que lo abrió (la tarjeta). De ahí se saca el `data-id`.
- **`buscarPorId`**: con el id se busca el objeto completo en `librosEncontrados`. Así el modal muestra datos que no están en la tarjeta (ediciones, idiomas) sin hacer otra petición.
- **La portada grande**: la URL de la tarjeta termina en `-M.jpg` (mediana). Para el modal se cambia a `-L.jpg` (grande).
- **`<dl>`, `<dt>`, `<dd>`**: lista de definiciones de HTML, pensada justo para pares "nombre: valor".
- **El arreglo `datos`** de pares `[nombre, valor]` y el `forEach` con desestructuración `([nombre, valor])`: en vez de escribir cuatro veces "crear dt, crear dd", se recorre el arreglo. Si te piden mostrar un dato más (modificación en caliente típica), solo agregas un par al arreglo.
- **`String(valor)`**: `crear` solo pone el texto si no es vacío; `ediciones` puede ser el número 0, y `String(0)` es `"0"`, que sí se muestra.
- **`hide.bs.modal` y `blur()`**: al cerrar, Bootstrap marca el modal con `aria-hidden="true"`. Si el foco sigue dentro (en el botón de cerrar), Chrome muestra un aviso en la consola porque un lector de pantalla quedaría en un elemento oculto. Quitar el foco antes lo evita. Es el segundo problema que se documenta en `depuracion.md`.

### Prueba

Busca algo y haz clic en una tarjeta: se abre el modal con título, portada grande y datos. Ciérralo con la X, con el botón Cerrar y con Escape. La consola no debe mostrar avisos.

### Commit

```powershell
git add .
git commit -m "Parte A: muestra el detalle del libro en el modal de Bootstrap"
git push
```

---

## Commit 9. Lista de lecturas con delegación de eventos

**Qué vas a hacer:** agregar libros a la lista desde las tarjetas, cambiarles el estado y quitarlos. Todo con delegación: una escucha por contenedor.

**Archivos:**
- [NUEVO] `buscador-js/js/lista.js`
- [MODIFICAR] `buscador-js/js/render.js`
- [MODIFICAR] `buscador-js/js/main.js`

### `buscador-js/js/lista.js` (archivo completo)

```js
// Estado de la lista de lecturas. Vive en memoria mientras la página esté abierta.
// Este módulo solo maneja datos; quien lo usa decide cómo se ve en pantalla.

export const ESTADOS = ['Pendiente', 'Leyendo', 'Leído'];

const lecturas = [];

export function obtenerLectura(id) {
  return lecturas.find((lectura) => lectura.id === id);
}

// Devuelve la lectura creada, o null si el libro ya estaba en la lista
export function agregarLectura(libro) {
  if (obtenerLectura(libro.id)) {
    return null;
  }

  // Copio todos los datos del libro para poder mostrar su detalle aunque se haga otra búsqueda
  const lectura = { ...libro, estado: ESTADOS[0] };
  lecturas.push(lectura);
  return lectura;
}

// Pasa al siguiente estado del arreglo; después del último vuelve al primero
export function siguienteEstado(id) {
  const lectura = obtenerLectura(id);
  const posicion = ESTADOS.indexOf(lectura.estado);
  lectura.estado = ESTADOS[(posicion + 1) % ESTADOS.length];
  return lectura;
}

export function quitarLectura(id) {
  const posicion = lecturas.findIndex((lectura) => lectura.id === id);
  if (posicion !== -1) {
    lecturas.splice(posicion, 1);
  }
}

export function cantidadLecturas() {
  return lecturas.length;
}
```

### `render.js` (cambio)

**Agrega al final del archivo** (después de `rellenarDetalle`):
```js

// ---------- Lista de lecturas ----------

function claseEstado(estado) {
  // 'Leído' -> 'estado--leido' (se quitan las tildes para usarlo como clase)
  const nombre = estado.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase();
  return `badge estado estado--${nombre}`;
}

export function crearFilaLectura(lectura) {
  const fila = crear('tr', 'fila-lectura');
  fila.dataset.id = lectura.id;

  const celdaLibro = crear('td');
  celdaLibro.append(
    crear('div', 'fw-semibold', lectura.titulo),
    crear('div', 'small text-muted', lectura.autores[0])
  );

  const celdaEstado = crear('td');
  celdaEstado.append(crear('span', claseEstado(lectura.estado), lectura.estado));

  const celdaAcciones = crear('td', 'text-end text-nowrap');
  const cambiar = crear('button', 'btn btn-sm btn-outline-primary me-1', 'Cambiar estado');
  cambiar.type = 'button';
  cambiar.dataset.accion = 'cambiar-estado';
  const quitar = crear('button', 'btn btn-sm btn-outline-danger', 'Quitar');
  quitar.type = 'button';
  quitar.dataset.accion = 'quitar';
  celdaAcciones.append(cambiar, quitar);

  fila.append(celdaLibro, celdaEstado, celdaAcciones);
  return fila;
}

// Solo se toca la etiqueta de esa fila, no se vuelve a dibujar la tabla
export function actualizarEstadoFila(fila, lectura) {
  const etiqueta = fila.querySelector('.estado');
  etiqueta.className = claseEstado(lectura.estado);
  etiqueta.textContent = lectura.estado;
}
```

### `main.js` (cambio 1 de 3): los imports

**Busca esto** (las primeras líneas):
```js
import { buscarLibros } from './api.js';
import { mostrarResultados, mostrarEstado, rellenarDetalle } from './render.js';
```

**Cámbialo por:**
```js
import { buscarLibros } from './api.js';
import { agregarLectura, siguienteEstado, quitarLectura, cantidadLecturas } from './lista.js';
import {
  mostrarResultados, mostrarEstado, rellenarDetalle, crearAlerta,
  crearFilaLectura, actualizarEstadoFila
} from './render.js';
```

### `main.js` (cambio 2 de 3): referencias a elementos

**Busca esto:**
```js
const resultados = document.getElementById('resultados');
```

**Agrega debajo:**
```js
const tablaLecturas = document.getElementById('tabla-lecturas');
const listaVacia = document.getElementById('lista-vacia');
const avisoLista = document.getElementById('aviso-lista');
```

### `main.js` (cambio 3 de 3): las escuchas

**Agrega al final del archivo:**
```js

// ---------- Resultados (delegación) ----------

// Una sola escucha para los botones "Agregar" de todas las tarjetas, aunque se creen después
resultados.addEventListener('click', (evento) => {
  const boton = evento.target.closest('[data-accion="agregar"]');
  if (!boton) return;

  // El botón está dentro de la tarjeta, que abre el modal.
  // Se corta la propagación para que agregar no abra también el detalle.
  evento.stopPropagation();

  const tarjeta = boton.closest('.libro');
  agregarALista(buscarPorId(tarjeta.dataset.id));
});

// ---------- Lista de lecturas ----------

function actualizarMensajeListaVacia() {
  listaVacia.classList.toggle('d-none', cantidadLecturas() > 0);
}

function agregarALista(libro) {
  const lectura = agregarLectura(libro);

  if (!lectura) {
    avisoLista.replaceChildren(crearAlerta('alert-warning py-2', `"${libro.titulo}" ya está en su lista.`));
    return;
  }

  avisoLista.replaceChildren();
  tablaLecturas.append(crearFilaLectura(lectura));
  actualizarMensajeListaVacia();
}

// Delegación: una sola escucha en el tbody maneja los botones de todas las filas
tablaLecturas.addEventListener('click', (evento) => {
  const boton = evento.target.closest('button[data-accion]');
  if (!boton) return;

  const fila = boton.closest('tr');
  const id = fila.dataset.id;

  if (boton.dataset.accion === 'cambiar-estado') {
    const lectura = siguienteEstado(id);
    actualizarEstadoFila(fila, lectura);
  }

  if (boton.dataset.accion === 'quitar') {
    quitarLectura(id);
    fila.remove();
    actualizarMensajeListaVacia();
  }
});
```

### Por qué

**`lista.js`, el estado (criterio 7):**
- **El estado vive en un módulo, no en el DOM**: la tabla es solo *cómo se ve*. La verdad está en el arreglo `lecturas`. Si quisiera saber el estado de un libro leyendo el texto de la etiqueta, dependería de cómo está escrito en pantalla, y eso se rompe fácil.
- **`lecturas` no se exporta**: nadie de afuera puede modificar el arreglo directamente; solo a través de las funciones. Eso se llama *encapsular*. `ESTADOS` sí se exporta porque es una constante que otros pueden necesitar leer.
- **`const lecturas = []`**: `const` impide *reasignar* la variable, pero el arreglo sí se puede modificar por dentro con `push` y `splice`. (Pregunta trampa típica.)
- **No duplicar** (requisito de la consigna): `agregarLectura` primero busca si ya existe ese id. Si existe devuelve `null`, y `main.js` muestra el aviso.
- **`{ ...libro, estado: ESTADOS[0] }`**: copia todas las propiedades del libro y agrega `estado: 'Pendiente'`. Se copia todo para poder abrir el detalle desde la lista aunque después se haga otra búsqueda (commit 10).
- **`siguienteEstado` con `%`**: `(posicion + 1) % ESTADOS.length`. Si está en la posición 2 ("Leído"), `(2+1) % 3 = 0` y vuelve a "Pendiente". Así no hay un `if` por cada estado, y **agregar un estado nuevo es agregar una palabra al arreglo** (modificación en caliente probable).
- **`push` y `splice` aquí sí están bien**: en la Parte A nadie depende de detectar el cambio; yo actualizo la pantalla a mano. En React (Parte B) está prohibido porque React compara referencias.
- **`findIndex` + `if (posicion !== -1)`**: si el id no existe, `findIndex` da -1 y `splice(-1, 1)` borraría el **último** elemento. El `if` lo evita.

**`render.js`, las filas (criterio 1):**
- **`claseEstado`**: convierte "Leído" en `badge estado estado--leido`. `normalize('NFD')` separa la letra de su tilde (í → i + ´) y el `replace` con ese rango Unicode borra las tildes. Se hace porque no conviene tener tildes en nombres de clases CSS. Las clases `.estado--leido` etc. las definiste en el commit 5.
- **`data-id` en la fila y `data-accion` en cada botón**: así la escucha delegada sabe qué libro y qué acción.
- **`actualizarEstadoFila`**: no vuelve a dibujar la tabla ni la fila. Solo cambia la clase y el texto de la etiqueta de esa fila. Eso es lo que la rúbrica llama "actualizaciones precisas". Redibujar todo ante cualquier cambio está en el nivel Regular.

**`main.js`, la delegación (criterio 2):**
- **Delegación de eventos**: en vez de poner `addEventListener` a cada botón de cada fila, se pone **una sola** escucha en el `tbody`. Funciona porque los eventos **burbujean**: el clic sale del botón y sube por `td`, `tr`, `tbody`, `table`... hasta `document`. Al pasar por el `tbody`, la escucha lo atrapa.
- **Por qué delegación**:
  1. Las filas se crean después de cargar la página. Una escucha puesta al inicio en cada botón no funcionaría con botones que todavía no existen.
  2. Con una escucha por fila, cada vez que agregas una tienes que acordarte de ponerle la escucha, y al quitarla, de limpiarla.
  3. Una escucha gasta menos memoria que cientos.
- **`evento.target.closest('button[data-accion]')`**: `evento.target` es el elemento exacto donde se hizo clic. Podría ser el botón, pero también un texto o un ícono dentro de él. `closest()` sube desde ahí hasta encontrar el primer ancestro (o él mismo) que cumpla el selector. Si se hizo clic en otra parte de la fila, devuelve `null` y el `return` corta. Eso es "identificar de forma robusta el elemento de origen".
- **La acción sale de `data-accion`, no del texto del botón**: si mañana cambias "Quitar" por "Eliminar", el código sigue funcionando.
- **`classList.toggle('d-none', condicion)`**: si la condición es verdadera agrega la clase, si no, la quita. `d-none` es la utilidad de Bootstrap para `display: none`. Así se oculta "Todavía no agregaste libros" cuando hay al menos uno.
- **`stopPropagation()` en el botón Agregar**: la intención es que el clic no siga hacia la tarjeta, que abre el modal. **Pero no funciona.** Ese es el error que vas a depurar ahora.

### Prueba y depuración del error (importante: aquí tomas capturas)

`npm start` y busca `tolkien`.

1. Haz clic en **Agregar a mi lista** de una tarjeta. El libro aparece en la tabla... **y además se abre el modal.** Eso no debería pasar.
2. Cierra el modal. Prueba **Cambiar estado** (Pendiente → Leyendo → Leído → Pendiente) y **Quitar**. Esas funcionan.
3. Agrega el mismo libro dos veces: debe salir el aviso amarillo.

**Ahora depura con puntos de interrupción** (esto es lo que pide `depuracion.md`):

1. Abre DevTools (F12) → pestaña **Sources**. A la izquierda, en `localhost:3000` → `js` → abre `main.js`.
2. Busca la línea `evento.stopPropagation();` dentro de `resultados.addEventListener`. Haz clic en el **número de línea**: aparece un marcador azul. Ese es el punto de interrupción (breakpoint).
3. Cierra el modal si está abierto y haz clic en **Agregar** de otra tarjeta.
4. La ejecución se detiene en tu línea y aparece el mensaje "Paused in debugger". Tu `stopPropagation()` todavía **no** se ejecutó.
5. Con la ejecución detenida, abre la consola (tecla Esc dentro de DevTools abre una consola abajo) y escribe:
   ```js
   document.body.classList.contains('modal-open')
   ```
   Da `true`. Bootstrap pone esa clase en el `<body>` cuando empieza a abrir un modal. O sea, **Bootstrap ya abrió el modal antes de que tu código corriera**. (Puede que en pantalla todavía no se vea, porque el navegador no dibuja mientras está pausado.)
6. Mira también el panel derecho: en **Call Stack** estás en la función anónima de `main.js`, y en **Scope** puedes ver los valores de `boton` y `evento`. Escribe `evento.eventPhase` en la consola: da `3`, que significa fase de burbuja.
7. **Captura de pantalla 1**: toma una captura con el código detenido en el breakpoint y la consola mostrando `true`. Guárdala como `docs/capturas/01-breakpoint-main.png`. (En Windows: **Win+Shift+S**, eliges el área, y la captura queda en el portapapeles; haz clic en la notificación que aparece para abrir la Herramienta Recortes y guardarla con Ctrl+S.)
8. Presiona F8 (o el botón "Resume") para continuar.
9. Ahora averigua por qué Bootstrap va primero: pestaña **Elements**, selecciona una tarjeta (`<article class="card h-100 libro" ...>`). En el panel de la derecha abre la pestaña **Event Listeners**. Marca la casilla **Ancestors** y despliega **click**. Vas a ver una escucha en `document` que viene de `bootstrap.bundle.min.js`. Despliégala: dice **`useCapture: true`**.
10. **Captura de pantalla 2**: guárdala como `docs/capturas/02-event-listeners.png`.
11. Quita el breakpoint haciendo clic otra vez en el número de línea.

**La explicación del error** (apréndetela, es pregunta segura):

Un evento de clic recorre el documento en tres fases:
1. **Captura**: baja desde `window` → `document` → `html` → `body` → ... hasta el elemento.
2. **Objetivo**: llega al elemento donde se hizo clic.
3. **Burbuja**: sube de vuelta desde el elemento hasta `document` y `window`.

`addEventListener('click', fn)` escucha por defecto en la fase de **burbuja**. Bootstrap registró su escucha en `document` con `useCapture: true`, o sea en la fase de **captura**, que ocurre **antes**. Entonces el orden fue:

1. Captura en `document`: Bootstrap ve que el clic fue dentro de algo con `data-bs-toggle="modal"` y abre el modal.
2. Burbuja en `#resultados`: tu escucha llama a `stopPropagation()`, pero ya es tarde.

`stopPropagation()` solo detiene lo que **todavía no ha pasado**. Ya no puede deshacer la fase de captura.

### Commit

Sí, el commit lleva el error. Es parte de un historial real: en el siguiente se corrige.

```powershell
git add .
git commit -m "Parte A: agrega la lista de lecturas con delegación de eventos para cambiar estado y quitar"
git push
```

---

## Commit 10. Corrección: el botón Agregar abría el modal

**Qué vas a hacer:** dejar de usar los atributos `data-bs-*` y abrir el modal desde una escucha propia en `<main>`, que trabaja en fase de burbuja. Así el `stopPropagation()` sí funciona. De paso, las filas de la lista también abren el detalle.

**Archivos:**
- [MODIFICAR] `buscador-js/js/render.js`
- [MODIFICAR] `buscador-js/js/main.js`
- [MODIFICAR] `buscador-js/scss/_componentes.scss`

### `render.js` (cambio 1 de 4): la tarjeta

**Busca esto:**
```js
  // Bootstrap abre el modal solo con estos atributos
  tarjeta.dataset.bsToggle = 'modal';
  tarjeta.dataset.bsTarget = '#modal-detalle';
```

**Cámbialo por:**
```js
  tarjeta.dataset.detalle = ''; // marca que al hacer clic se abre el detalle
```

### `render.js` (cambio 2 de 4): el nombre de la función

**Busca esto:**
```js
// Rellena el modal; Bootstrap se encarga de abrirlo
export function rellenarDetalle(libro) {
```

**Cámbialo por:**
```js
export function mostrarDetalle(libro) {
```

### `render.js` (cambio 3 de 4): abrir el modal

**Busca esto** (al final de la misma función):
```js
  datos.forEach(([nombre, valor]) => {
    lista.append(crear('dt', '', nombre), crear('dd', '', String(valor)));
  });
}
```

**Cámbialo por:**
```js
  datos.forEach(([nombre, valor]) => {
    lista.append(crear('dt', '', nombre), crear('dd', '', String(valor)));
  });

  bootstrap.Modal.getOrCreateInstance(document.getElementById('modal-detalle')).show();
}
```

### `render.js` (cambio 4 de 4): la fila

**Busca esto** (dentro de `crearFilaLectura`):
```js
  const fila = crear('tr', 'fila-lectura');
  fila.dataset.id = lectura.id;
```

**Cámbialo por:**
```js
  const fila = crear('tr', 'fila-lectura');
  fila.dataset.id = lectura.id;
  fila.dataset.detalle = '';
```

### `main.js` (cambio 1 de 5): los imports

**Busca esto:**
```js
import { agregarLectura, siguienteEstado, quitarLectura, cantidadLecturas } from './lista.js';
import {
  mostrarResultados, mostrarEstado, rellenarDetalle, crearAlerta,
  crearFilaLectura, actualizarEstadoFila
} from './render.js';
```

**Cámbialo por:**
```js
import { agregarLectura, obtenerLectura, siguienteEstado, quitarLectura, cantidadLecturas } from './lista.js';
import {
  mostrarResultados, mostrarEstado, mostrarDetalle, crearAlerta,
  crearFilaLectura, actualizarEstadoFila
} from './render.js';
```

### `main.js` (cambio 2 de 5): referencia a `<main>`

**Busca esto:**
```js
const avisoLista = document.getElementById('aviso-lista');
```

**Agrega debajo:**
```js
const contenido = document.querySelector('main');
```

### `main.js` (cambio 3 de 5): la sección del detalle

**Busca esto:**
```js
// ---------- Detalle ----------

function buscarPorId(id) {
  return librosEncontrados.find((libro) => libro.id === id);
}

// Bootstrap avisa con show.bs.modal antes de abrir; relatedTarget es la tarjeta que se tocó
document.getElementById('modal-detalle').addEventListener('show.bs.modal', (evento) => {
  rellenarDetalle(buscarPorId(evento.relatedTarget.dataset.id));
});
```

**Cámbialo por:**
```js
// ---------- Detalle (delegación en <main>) ----------

function buscarPorId(id) {
  return librosEncontrados.find((libro) => libro.id === id);
}

// Una sola escucha abre el detalle de cualquier elemento marcado con data-detalle:
// las tarjetas de resultados y las filas de la lista de lecturas.
contenido.addEventListener('click', (evento) => {
  const elemento = evento.target.closest('[data-detalle]');
  if (!elemento) return;

  const id = elemento.dataset.id;
  mostrarDetalle(obtenerLectura(id) ?? buscarPorId(id));
});
```

(La escucha de `hide.bs.modal` que está debajo **se queda igual**.)

### `main.js` (cambio 4 de 5): el comentario del botón Agregar

**Busca esto:**
```js
  // El botón está dentro de la tarjeta, que abre el modal.
  // Se corta la propagación para que agregar no abra también el detalle.
```

**Cámbialo por:**
```js
  // El botón está dentro de la tarjeta. Sin esto el clic sigue subiendo hasta <main>
  // y además de agregar el libro se abriría el modal de detalle.
```

### `main.js` (cambio 5 de 5): propagación en la tabla

**Busca esto** (dentro de `tablaLecturas.addEventListener`):
```js
  const boton = evento.target.closest('button[data-accion]');
  if (!boton) return;

  const fila = boton.closest('tr');
```

**Cámbialo por:**
```js
  const boton = evento.target.closest('button[data-accion]');
  if (!boton) return;

  // Igual que en las tarjetas: la fila abre el detalle, pero los botones no deben hacerlo
  evento.stopPropagation();

  const fila = boton.closest('tr');
```

### `_componentes.scss` (cambio)

**Busca esto:**
```scss
// Tarjeta de cada libro dentro del estante
.libro {
```

**Agrega ARRIBA** (antes de esas líneas):
```scss
// Cualquier elemento que abre el detalle del libro
[data-detalle] {
  cursor: pointer;
}

```

### Por qué

- **La solución**: ahora el modal lo abre **mi** escucha en `<main>`, que es un ancestro de las tarjetas y de la tabla y trabaja en fase de **burbuja**. El recorrido de un clic en "Agregar" queda así:
  1. Sale del botón y sube hasta `#resultados`.
  2. La escucha de `#resultados` agrega el libro y llama a `stopPropagation()`.
  3. El evento **ya no sube** a `<main>`, así que el modal no se abre.

  Y un clic en cualquier otra parte de la tarjeta:
  1. Sube hasta `#resultados`. Ahí `closest('[data-accion="agregar"]')` da `null` y la escucha no hace nada.
  2. Sigue subiendo hasta `<main>`. `closest('[data-detalle]')` encuentra la tarjeta y se abre el detalle.
- **Esto es "controlar la propagación donde corresponde"** (criterio 2): los botones están dentro de elementos que tienen su propia acción, y se corta la propagación justo ahí, en ningún otro lado.
- **`data-detalle` como marca**: es un atributo vacío que solo dice "este elemento abre el detalle". Así la misma escucha sirve para tarjetas y filas, y si mañana quieres que otra cosa abra el detalle, solo le pones el atributo.
- **`[data-detalle] { cursor: pointer; }`**: selector de atributo en CSS. Muestra la manito en todo lo que se puede tocar.
- **`obtenerLectura(id) ?? buscarPorId(id)`**: si el libro está en la lista, se usan esos datos (que se copiaron completos en `agregarLectura`); si no, se busca en la última búsqueda. Así el detalle de la lista funciona aunque hayas hecho otra búsqueda después.
- **`bootstrap.Modal.getOrCreateInstance(...).show()`**: API de JavaScript de Bootstrap. `getOrCreateInstance` devuelve el objeto Modal de ese elemento (o lo crea la primera vez) y `.show()` lo abre. Así no se crea un objeto nuevo en cada clic.
- **`stopPropagation` en la tabla**: como ahora la fila también abre el detalle, los botones "Cambiar estado" y "Quitar" necesitan lo mismo que "Agregar".
- **Alternativas que descarté** (por si te preguntan):
  - *Usar `stopImmediatePropagation` o registrar mi escucha también en captura*: no sirve, porque la de Bootstrap en `document` igual va antes que cualquier cosa más abajo.
  - *Poner el botón Agregar fuera de la tarjeta*: arregla el síntoma pero cambia el diseño.
  - *Revisar en `show.bs.modal` si el clic vino del botón y cancelar con `preventDefault()`*: funciona, pero mezcla la lógica de agregar con la del modal.

### Prueba

1. Busca algo y haz clic en **Agregar**: el libro va a la tabla y el modal **no** se abre.
2. Haz clic en el resto de la tarjeta: se abre el detalle.
3. Haz clic en una fila de la tabla (no en los botones): se abre el detalle de ese libro.
4. **Cambiar estado** y **Quitar** no abren el modal.
5. Haz otra búsqueda diferente y haz clic en el libro de la lista: el detalle sigue funcionando.
6. **Captura de pantalla 3**: el libro agregado en la tabla y el modal cerrado. Guárdala como `docs/capturas/03-arreglado.png`.

### Commit

```powershell
git add .
git commit -m "Corrige que el botón Agregar abriera el modal: el detalle se abre desde una escucha delegada en main"
git push
```

---

# Viernes 23 de octubre

## Commit 11. Animaciones de la lista

**Qué vas a hacer:** animar las tres acciones de la lista (agregar, cambiar de estado y quitar) con CSS. JavaScript solo pone y quita clases. Al quitar, la fila se borra del DOM cuando termina la transición.

**Archivos:**
- [NUEVO] `buscador-js/scss/_animaciones.scss`
- [MODIFICAR] `buscador-js/scss/main.scss`
- [MODIFICAR] `buscador-js/js/render.js`
- [MODIFICAR] `buscador-js/js/main.js`

### `buscador-js/scss/_animaciones.scss` (archivo completo)

```scss
// Las animaciones viven aquí en CSS. JavaScript solo pone o quita estas clases.

// Al agregar un libro a la lista
@keyframes aparecer {
  from {
    opacity: 0;
    transform: translateX(-1rem);
  }

  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.fila-entrando {
  animation: aparecer $duracion-animacion ease-out;
}

// Al quitar un libro: se desvanece y cuando termina la transición JS lo saca del DOM
.fila-lectura {
  transition: opacity $duracion-animacion ease, transform $duracion-animacion ease;
}

.fila-saliendo {
  opacity: 0;
  transform: translateX(1rem);
}

// Al cambiar de estado la etiqueta hace un pequeño "salto"
@keyframes resaltar {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.25);
  }

  100% {
    transform: scale(1);
  }
}

.estado-cambiado {
  display: inline-block;
  animation: resaltar $duracion-animacion ease;
}
```

### `main.scss` (cambio)

**Busca esto:**
```scss
@import "componentes";
```

**Agrega debajo:**
```scss
@import "animaciones";
```

### `render.js` (cambio 1 de 4): clase al crear la fila

**Busca esto:**
```js
  const fila = crear('tr', 'fila-lectura');
```

**Cámbialo por:**
```js
  const fila = crear('tr', 'fila-lectura fila-entrando');
```

### `render.js` (cambio 2 de 4): quitar la clase al terminar

**Busca esto** (al final de `crearFilaLectura`):
```js
  fila.append(celdaLibro, celdaEstado, celdaAcciones);
  return fila;
}
```

**Cámbialo por:**
```js
  fila.append(celdaLibro, celdaEstado, celdaAcciones);

  // Se quita la clase al terminar para que la animación pueda repetirse si hiciera falta
  fila.addEventListener('animationend', () => fila.classList.remove('fila-entrando'), { once: true });
  return fila;
}
```

### `render.js` (cambio 3 de 4): animar el cambio de estado

**Busca esto:**
```js
  etiqueta.className = claseEstado(lectura.estado);
  etiqueta.textContent = lectura.estado;
}
```

**Cámbialo por:**
```js
  etiqueta.className = claseEstado(lectura.estado);
  etiqueta.textContent = lectura.estado;

  etiqueta.classList.add('estado-cambiado');
  etiqueta.addEventListener('animationend', () => {
    etiqueta.classList.remove('estado-cambiado');
  }, { once: true });
}
```

### `render.js` (cambio 4 de 4): quitar con transición

**Agrega al final del archivo:**
```js

// Primero corre la transición y cuando termina se saca la fila del DOM
export function quitarFilaConTransicion(fila, alTerminar) {
  fila.addEventListener('transitionend', () => {
    fila.remove();
    alTerminar();
  }, { once: true });
  fila.classList.add('fila-saliendo');
}
```

### `main.js` (cambio 1 de 2): import

**Busca esto:**
```js
  crearFilaLectura, actualizarEstadoFila
} from './render.js';
```

**Cámbialo por:**
```js
  crearFilaLectura, actualizarEstadoFila, quitarFilaConTransicion
} from './render.js';
```

### `main.js` (cambio 2 de 2): usar la transición al quitar

**Busca esto:**
```js
  if (boton.dataset.accion === 'quitar') {
    quitarLectura(id);
    fila.remove();
    actualizarMensajeListaVacia();
  }
```

**Cámbialo por:**
```js
  if (boton.dataset.accion === 'quitar') {
    boton.disabled = true; // evita un doble clic mientras corre la transición
    quitarLectura(id);
    quitarFilaConTransicion(fila, actualizarMensajeListaVacia);
  }
```

### Por qué

- **Animación vs transición** (pregunta típica):
  - Una **transición** (`transition`) anima el paso de un valor a otro cuando una propiedad **cambia**. Necesita un "antes" y un "después": la fila tiene `opacity: 1` y al agregarle `.fila-saliendo` pasa a `opacity: 0`. La `transition` de `.fila-lectura` hace que ese cambio dure 0.35 s en lugar de ser instantáneo.
  - Una **animación** (`@keyframes` + `animation`) corre una secuencia definida apenas se aplica la clase, sin necesidad de un cambio previo. Por eso para *aparecer* se usa animación: el elemento recién creado no tiene un "antes".
- **Las tres acciones animadas** (lo que pide la consigna):
  1. *Agregar*: la fila nace con `fila-entrando` → animación `aparecer` (entra desde la izquierda).
  2. *Cambiar estado*: se agrega `estado-cambiado` a la etiqueta → animación `resaltar` (crece y vuelve).
  3. *Quitar*: se agrega `fila-saliendo` → transición de `opacity` y `transform`.
- **JavaScript solo pone y quita clases** (criterio 6): la rúbrica pone en Regular las animaciones con estilos en línea (`elemento.style.opacity = 0`). Con clases, el "cómo se ve" queda en CSS y el "cuándo" en JS. Si quieres cambiar la duración, cambias `$duracion-animacion` en un solo lugar.
- **`transitionend`**: el navegador dispara este evento cuando la transición termina. Recién ahí se hace `fila.remove()`. Si se quitara el elemento de inmediato, no se vería la animación. Esto es lo que pide la consigna: "el elemento se retira del DOM cuando la transición termina".
- **`{ once: true }`**: la escucha se borra sola después de ejecutarse. Es importante en `transitionend` porque se dispara **una vez por cada propiedad** que transiciona (aquí `opacity` y `transform`, o sea dos veces). Sin `once`, se intentaría quitar la fila dos veces.
- **`animationend` y quitar la clase**: si la clase `estado-cambiado` se quedara puesta, la próxima vez que cambies el estado la animación no se repetiría (agregar una clase que ya está no hace nada).
- **`display: inline-block` en `.estado-cambiado`**: `transform` no funciona en elementos `inline`. El badge de Bootstrap ya es inline-block, pero se deja explícito.
- **`alTerminar` como parámetro**: `quitarFilaConTransicion` no sabe qué hay que hacer después (eso es cosa de `main.js`), así que recibe una función y la llama al final. Aquí se le pasa `actualizarMensajeListaVacia` para que el mensaje "Todavía no agregaste libros" aparezca cuando la fila ya se fue, y no antes.
- **`boton.disabled = true`**: si se hace doble clic en Quitar durante los 0.35 s de la transición, se intentaría quitar dos veces.
- **El dato se quita de inmediato, la fila después**: `quitarLectura(id)` se llama antes de la transición. Así, si durante esos 0.35 s se vuelve a agregar el mismo libro, el estado ya está al día.

### Prueba

Agrega un libro (entra deslizándose), cámbiale el estado (la etiqueta "salta") y quítalo (se desvanece hacia la derecha y luego desaparece). En **Elements** mira el `<tr>`: mientras se va tiene la clase `fila-saliendo` y después ya no existe. Prueba quitar el último libro: el mensaje de lista vacía aparece después de la animación.

### Commit

```powershell
git add .
git commit -m "Parte A: anima agregar, cambiar de estado y quitar libros con clases de CSS"
git push
```

---

## Commit 12. Validación del formulario de propuesta

**Qué vas a hacer:** crear `formulario.js` con la validación: atributos de HTML5 más la API de validación de restricciones, mensajes propios junto a cada campo y las reglas de año y fecha. Todavía no se envía nada.

**Archivos:**
- [NUEVO] `buscador-js/js/formulario.js`
- [MODIFICAR] `buscador-js/js/main.js`

### `buscador-js/js/formulario.js` (archivo completo, versión 1)

```js
// Formulario de propuesta de lectura: validación, campos de autor dinámicos y envío.

const formulario = document.getElementById('form-propuesta');

// Fecha de hoy en formato yyyy-mm-dd usando la hora local (toISOString usa UTC y puede dar el día siguiente)
function hoyComoTexto() {
  const hoy = new Date();
  const mes = String(hoy.getMonth() + 1).padStart(2, '0');
  const dia = String(hoy.getDate()).padStart(2, '0');
  return `${hoy.getFullYear()}-${mes}-${dia}`;
}

// ---------- Validación ----------

// Reglas que el HTML no puede expresar solo; se agregan con setCustomValidity
function reglasExtra(campo) {
  if (campo.name === 'anio' && Number(campo.value) > new Date().getFullYear()) {
    return 'El año de publicación no puede ser futuro.';
  }
  if (campo.name === 'fecha' && campo.value && campo.value < hoyComoTexto()) {
    return 'La fecha de la sesión no puede ser anterior a hoy.';
  }
  return '';
}

// Traduce lo que reporta la API de validación (campo.validity) a un mensaje propio
function mensajeDeError(campo) {
  const v = campo.validity;
  // Primero las reglas propias, para que su mensaje gane sobre el de min/max
  if (v.customError) return campo.validationMessage;
  if (v.valueMissing) return 'Este campo es obligatorio.';
  if (v.typeMismatch && campo.type === 'email') return 'Escriba un correo válido, por ejemplo nombre@correo.com.';
  if (v.tooShort) return `Debe tener al menos ${campo.minLength} caracteres.`;
  if (v.tooLong) return `No puede pasar de ${campo.maxLength} caracteres.`;
  if (v.patternMismatch) return 'Solo se permiten letras, espacios, puntos y guiones.';
  if (v.rangeUnderflow) return `El valor mínimo es ${campo.min}.`;
  if (v.rangeOverflow) return `El valor máximo es ${campo.max}.`;
  if (v.stepMismatch || v.badInput) return 'Escriba un número entero.';
  return '';
}

function validarCampo(campo) {
  campo.setCustomValidity(reglasExtra(campo));

  const valido = campo.checkValidity();
  // El mensaje va en el .invalid-feedback que está junto al campo
  const feedback = campo.parentElement.querySelector('.invalid-feedback');
  feedback.textContent = valido ? '' : mensajeDeError(campo);
  campo.classList.toggle('is-invalid', !valido);
  campo.classList.toggle('is-valid', valido);
  return valido;
}

function validarFormulario() {
  const campos = formulario.querySelectorAll('input, select, textarea');
  // map en vez de every para que se marquen todos los campos y no solo el primero con error
  const resultados = [...campos].map(validarCampo);
  return resultados.every(Boolean);
}

// ---------- Envío ----------

function manejarEnvio(evento) {
  evento.preventDefault();

  if (!validarFormulario()) {
    formulario.querySelector('.is-invalid').focus();
    return;
  }

  // Por ahora solo se valida; el envío con fetch se agrega en el commit 14
  console.log('Formulario válido');
}

export function iniciarFormulario() {
  // Límites que dependen del día actual
  formulario.anio.max = new Date().getFullYear();
  formulario.fecha.min = hoyComoTexto();

  // Una sola escucha de 'input' en el formulario: sirve también para los autores que se agreguen luego
  formulario.addEventListener('input', (evento) => {
    if (evento.target.matches('input, select, textarea')) validarCampo(evento.target);
  });

  formulario.addEventListener('submit', manejarEnvio);
}
```

### `main.js` (cambio 1 de 2)

**Busca esto:**
```js
} from './render.js';
```

**Agrega debajo:**
```js
import { iniciarFormulario } from './formulario.js';
```

### `main.js` (cambio 2 de 2)

**Agrega al final del archivo:**
```js

iniciarFormulario();
```

### Por qué

**Dos capas de validación (criterio 3):**
1. **HTML5 nativa**: los atributos que pusiste en el commit 4 (`required`, `minlength`, `type="email"`, `min`, `max`, `pattern`...). El navegador ya sabe revisar eso.
2. **JavaScript con la API de validación de restricciones**: se usa para (a) poner mensajes propios en español junto a cada campo y (b) agregar reglas que el HTML no puede expresar.

**La API de validación de restricciones** (apréndete estos nombres):
- `campo.validity`: un objeto con banderas `true`/`false` que dice **qué regla falla**:
  - `valueMissing` → falta un `required`
  - `typeMismatch` → no tiene formato de email
  - `tooShort` / `tooLong` → `minlength` / `maxlength`
  - `patternMismatch` → no cumple el `pattern`
  - `rangeUnderflow` / `rangeOverflow` → menor que `min` / mayor que `max`
  - `stepMismatch` → no es múltiplo del `step` (por ejemplo 3.5 con `step="1"`)
  - `badInput` → el navegador no pudo convertir lo escrito (letras en un `type="number"`)
  - `customError` → se puso un error con `setCustomValidity`
- `campo.setCustomValidity('mensaje')`: marca el campo como inválido con una regla propia. Con `''` lo vuelve a marcar válido. **Por eso se llama siempre**, con mensaje o vacío: si no se limpiara, el campo quedaría inválido para siempre.
- `campo.checkValidity()`: devuelve `true` o `false` tomando en cuenta todas las reglas, las de HTML y la personalizada.
- `campo.validationMessage`: el mensaje actual del campo; si usaste `setCustomValidity`, es tu mensaje.

**Funciones:**
- **`reglasExtra(campo)`**: las dos reglas de la consigna.
  - *Año no futuro*: compara con `new Date().getFullYear()`.
  - *Fecha no anterior a hoy*: compara textos `"2026-10-23" < "2026-10-25"`. Funciona porque el formato año-mes-día con ceros (`yyyy-mm-dd`) ordena igual alfabéticamente que cronológicamente.
  - Devuelve el mensaje o `''`.
- **`hoyComoTexto()`**: arma la fecha de hoy en ese formato. `padStart(2, '0')` convierte `5` en `"05"`. No se usa `new Date().toISOString().slice(0, 10)` porque `toISOString` trabaja en hora UTC. Costa Rica está en UTC-6, así que después de las 6 p. m. ese método ya da la fecha de mañana y la validación fallaría.
- **`mensajeDeError(campo)`**: traduce las banderas de `validity` a mensajes. `customError` va **primero** porque si el año es 2030, falla tanto la regla propia como el `max`, y queremos mostrar el mensaje propio ("no puede ser futuro") y no el genérico ("el valor máximo es 2026").
- **`validarCampo(campo)`**: el centro de todo. Aplica la regla extra, pregunta si es válido, escribe el mensaje en el `.invalid-feedback` vecino y pone `is-invalid` o `is-valid` (clases de Bootstrap que pintan el borde rojo o verde y muestran u ocultan el feedback). Devuelve `true`/`false`.
- **`campo.parentElement.querySelector('.invalid-feedback')`**: busca el mensaje *dentro del mismo contenedor que el campo*. Así funciona para cualquier campo sin ids especiales, incluidos los autores que se clonan después.
- **`validarFormulario()`**: valida todos los campos. Usa `map` y no `every` a propósito: `every` se detiene en el primer `false` y dejaría los demás campos sin marcar. Con `map` se valida y se marca cada uno, y después se revisa si todos dieron `true`.
- **`[...campos]`**: `querySelectorAll` devuelve un `NodeList`, que no tiene `map`. El spread lo convierte en arreglo.
- **`formulario.querySelector('.is-invalid').focus()`**: lleva el cursor al primer campo con error. Es un detalle de usabilidad.
- **`iniciarFormulario()`**:
  - Pone `max` al año y `min` a la fecha desde JS, porque dependen del día de hoy y en el HTML quedarían fijos. Así además el selector de fecha del navegador ni siquiera deja elegir días pasados.
  - `formulario.anio` y `formulario.fecha`: un `<form>` permite acceder a sus campos por el `name`.
  - **Una sola escucha de `input` en el formulario** (delegación otra vez): el evento `input` burbujea desde cada campo hasta el `<form>`. Así se valida mientras se escribe, y **funciona también con los campos de autor que se agreguen en el commit 13** sin tocar nada.
  - `evento.target.matches(...)`: revisa que el evento venga de un campo y no de otra cosa.
- **`export` solo de `iniciarFormulario`**: el resto es interno del módulo. `main.js` solo necesita arrancarlo.

### Prueba

1. Haz clic en **Enviar propuesta** con todo vacío: todos los campos se ponen rojos con "Este campo es obligatorio." y el cursor va al título.
2. Escribe en el año `2030`: "El año de publicación no puede ser futuro."
3. Elige una fecha de ayer (escríbela a mano si el selector no te deja): "La fecha de la sesión no puede ser anterior a hoy."
4. Escribe `correo` en el email, `J4` en el autor, `Hola` en el motivo y `5` en páginas, y mira cada mensaje.
5. Llena todo bien y envía: la consola dice "Formulario válido".

### Commit

```powershell
git add .
git commit -m "Parte A: valida el formulario de propuesta con la API de validación de restricciones y mensajes propios"
git push
```

---

## Commit 13. Campos de autor dinámicos

**Qué vas a hacer:** permitir agregar y quitar campos de autor en tiempo de ejecución, mínimo uno y máximo cinco.

**Archivos:**
- [MODIFICAR] `buscador-js/js/formulario.js`

### `formulario.js` (cambio 1 de 3): constantes y referencias

**Busca esto:**
```js
const formulario = document.getElementById('form-propuesta');
```

**Cámbialo por:**
```js
const MIN_AUTORES = 1;
const MAX_AUTORES = 5;

const formulario = document.getElementById('form-propuesta');
const listaAutores = document.getElementById('lista-autores');
const botonAgregarAutor = document.getElementById('agregar-autor');
```

### `formulario.js` (cambio 2 de 3): las funciones

**Busca esto:**
```js
// ---------- Envío ----------
```

**Agrega ARRIBA** (antes de esa línea):
```js
// ---------- Autores dinámicos ----------

function camposAutor() {
  return listaAutores.querySelectorAll('.campo-autor');
}

function actualizarBotonesAutor() {
  const cantidad = camposAutor().length;
  botonAgregarAutor.disabled = cantidad >= MAX_AUTORES;
  listaAutores.querySelectorAll('[data-accion="quitar-autor"]').forEach((boton) => {
    boton.disabled = cantidad <= MIN_AUTORES;
  });
}

function agregarCampoAutor() {
  if (camposAutor().length >= MAX_AUTORES) return;

  // Clono el primer campo para que el nuevo tenga los mismos atributos de validación
  const nuevo = camposAutor()[0].cloneNode(true);
  const input = nuevo.querySelector('input');
  input.value = '';
  input.classList.remove('is-valid', 'is-invalid');
  nuevo.querySelector('.invalid-feedback').textContent = '';

  listaAutores.append(nuevo);
  actualizarBotonesAutor();
  input.focus();
}

function quitarCampoAutor(boton) {
  if (camposAutor().length <= MIN_AUTORES) return;
  boton.closest('.campo-autor').remove();
  actualizarBotonesAutor();
}

```

### `formulario.js` (cambio 3 de 3): las escuchas

**Busca esto** (al final de `iniciarFormulario`):
```js
  formulario.addEventListener('submit', manejarEnvio);
}
```

**Cámbialo por:**
```js
  listaAutores.addEventListener('click', (evento) => {
    const boton = evento.target.closest('[data-accion="quitar-autor"]');
    if (boton) quitarCampoAutor(boton);
  });

  botonAgregarAutor.addEventListener('click', agregarCampoAutor);
  formulario.addEventListener('submit', manejarEnvio);
  actualizarBotonesAutor();
}
```

### Por qué

- **`cloneNode(true)`**: copia el primer grupo de autor con todos sus hijos (`true` = copia profunda). Así el nuevo input tiene exactamente los mismos atributos: `name="autor"`, `required`, `minlength`, `maxlength`, `pattern`. Por eso **se valida igual que el original** sin escribir nada más (criterio 4). La copia no trae las escuchas de eventos, pero no hace falta porque todo se maneja con delegación.
- **Limpiar la copia**: si el primer autor ya tenía texto o estaba en rojo, la copia lo heredaría. Por eso se borra el valor, las clases y el mensaje.
- **Mínimo y máximo, dos veces**:
  1. `actualizarBotonesAutor()` deshabilita "+ Agregar autor" al llegar a 5 y los botones × cuando queda 1. Eso es lo que ve el usuario.
  2. `agregarCampoAutor` y `quitarCampoAutor` también revisan el límite. Es una segunda protección por si alguien habilita el botón desde DevTools.
- **`MIN_AUTORES` y `MAX_AUTORES` como constantes**: si te piden cambiar el máximo a 3, cambias una línea.
- **Delegación para quitar**: los botones × se crean al clonar, así que no tendría sentido ponerles una escucha a cada uno. Hay una sola en `#lista-autores`, y `closest('[data-accion="quitar-autor"]')` encuentra el botón aunque se haga clic en la ×.
- **`boton.closest('.campo-autor').remove()`**: desde el botón sube hasta su grupo y lo quita completo (input, botón y mensaje).
- **Los nuevos se validan mientras escribes**: la escucha de `input` del commit 12 está en el `<form>`, y el evento de los inputs nuevos también burbujea hasta ahí.
- **Los nuevos se validan al enviar**: `validarFormulario` usa `querySelectorAll` en el momento del envío, así que encuentra todos los autores que haya en ese momento.
- **`actualizarBotonesAutor()` al final de `iniciarFormulario`**: al cargar la página hay un solo autor, y su botón × debe empezar deshabilitado.
- **`input.focus()`**: deja el cursor en el autor nuevo para escribir de una vez.

### Prueba

1. Al cargar, el × del único autor está deshabilitado.
2. Haz clic en "+ Agregar autor" cuatro veces: hay 5 campos y el botón se deshabilita.
3. Escribe `J4` en el tercer autor: se pone rojo con su mensaje.
4. Quita autores con ×: al quedar uno, su × se deshabilita otra vez.
5. Envía con un autor nuevo vacío: también se marca como obligatorio.

### Commit

```powershell
git add .
git commit -m "Parte A: permite agregar y quitar campos de autor entre 1 y 5 con la misma validación"
git push
```

---

## Commit 14. Envío asincrónico de la propuesta

**Qué vas a hacer:** enviar la propuesta con `fetch` a JSONPlaceholder y mostrar una alerta de éxito o de error sin recargar la página.

**Archivos:**
- [MODIFICAR] `buscador-js/js/api.js`
- [MODIFICAR] `buscador-js/js/formulario.js`

### `api.js` (cambio 1 de 2)

**Busca esto:**
```js
const URL_PORTADAS = 'https://covers.openlibrary.org/b/id';
```

**Agrega debajo:**
```js
const URL_PROPUESTAS = 'https://jsonplaceholder.typicode.com/posts';
```

### `api.js` (cambio 2 de 2)

**Agrega al final del archivo:**
```js

export async function enviarPropuesta(propuesta) {
  const respuesta = await fetch(URL_PROPUESTAS, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(propuesta)
  });

  if (!respuesta.ok) {
    throw new Error(`El servidor respondió con estado ${respuesta.status}`);
  }

  return respuesta.json();
}
```

### `formulario.js` (cambio 1 de 3): imports

**Busca esto** (la primera línea):
```js
// Formulario de propuesta de lectura: validación, campos de autor dinámicos y envío.
```

**Agrega debajo:**
```js
import { enviarPropuesta } from './api.js';
import { crearAlerta } from './render.js';
```

### `formulario.js` (cambio 2 de 3): más referencias

**Busca esto:**
```js
const botonAgregarAutor = document.getElementById('agregar-autor');
```

**Agrega debajo:**
```js
const botonEnviar = document.getElementById('boton-enviar');
const alerta = document.getElementById('alerta-propuesta');
```

### `formulario.js` (cambio 3 de 3): la sección de envío

**Busca toda esta función:**
```js
function manejarEnvio(evento) {
  evento.preventDefault();

  if (!validarFormulario()) {
    formulario.querySelector('.is-invalid').focus();
    return;
  }

  // Por ahora solo se valida; el envío con fetch se agrega en el commit 14
  console.log('Formulario válido');
}
```

**Cámbiala por:**
```js
function leerDatos() {
  const datos = new FormData(formulario);
  return {
    titulo: datos.get('titulo').trim(),
    // getAll trae todos los inputs name="autor", incluidos los que se agregaron después
    autores: datos.getAll('autor').map((autor) => autor.trim()),
    anio: Number(datos.get('anio')),
    paginas: Number(datos.get('paginas')),
    genero: datos.get('genero'),
    correo: datos.get('correo').trim(),
    fecha: datos.get('fecha'),
    motivo: datos.get('motivo').trim()
  };
}

function reiniciarFormulario() {
  formulario.reset();
  // Dejo un solo campo de autor y limpio las marcas de validación
  camposAutor().forEach((campo, i) => {
    if (i > 0) campo.remove();
  });
  formulario.querySelectorAll('.is-valid, .is-invalid').forEach((campo) => {
    campo.classList.remove('is-valid', 'is-invalid');
  });
  actualizarBotonesAutor();
}

async function manejarEnvio(evento) {
  evento.preventDefault();
  alerta.replaceChildren();

  if (!validarFormulario()) {
    formulario.querySelector('.is-invalid').focus();
    return;
  }

  botonEnviar.disabled = true;
  botonEnviar.textContent = 'Enviando...';

  try {
    const respuesta = await enviarPropuesta(leerDatos());
    alerta.replaceChildren(crearAlerta('alert-success', `¡Gracias! Su propuesta quedó registrada con el número ${respuesta.id}.`));
    reiniciarFormulario();
  } catch (error) {
    alerta.replaceChildren(crearAlerta('alert-danger', 'No se pudo enviar la propuesta. Revise su conexión e intente de nuevo.'));
    console.warn('Error al enviar la propuesta:', error.message);
  } finally {
    botonEnviar.disabled = false;
    botonEnviar.textContent = 'Enviar propuesta';
  }
}
```

### Por qué

- **`fetch` con opciones**: por defecto `fetch` hace GET. Para enviar datos se le pasa un objeto:
  - `method: 'POST'`: crear un recurso nuevo.
  - `headers: { 'Content-Type': 'application/json' }`: le avisa al servidor que el cuerpo es JSON.
  - `body: JSON.stringify(propuesta)`: `fetch` envía texto, así que el objeto se convierte a texto JSON.
- **JSONPlaceholder**: es un servicio de pruebas. No guarda nada de verdad, pero responde como si lo hiciera: devuelve lo que mandaste más un `id` (siempre 101). Por eso la alerta muestra ese número.
- **Mismo patrón que la búsqueda**: `api.js` revisa `respuesta.ok` y lanza el error; quien llama tiene el `try/catch`. La rúbrica pide eso en **la búsqueda y en el envío**.
- **`FormData(formulario)`**: lee todos los campos del form por su `name`. `datos.get('titulo')` devuelve un valor.
- **`datos.getAll('autor')`**: como todos los inputs de autor se llaman igual, `getAll` devuelve **un arreglo con todos**, incluidos los que se agregaron después. Eso cubre "todos se incluyen en el envío" del criterio 4.
- **`Number(...)`**: los valores de un formulario siempre llegan como texto; se convierten a número para enviar el tipo correcto.
- **`alerta.replaceChildren()` al inicio**: borra la alerta del envío anterior.
- **Botón "Enviando..." deshabilitado**: es el estado de carga del envío. Evita que se mande dos veces si se hace doble clic, y le muestra al usuario que algo está pasando.
- **`finally`**: se ejecuta **siempre**, haya salido bien o mal. Ahí se vuelve a habilitar el botón; si se pusiera solo en el `try`, después de un error el botón quedaría deshabilitado para siempre.
- **`reiniciarFormulario()`**: `formulario.reset()` vacía los campos, pero no quita los autores agregados ni las clases `is-valid`, así que eso se hace a mano. Al final se llama `actualizarBotonesAutor()` porque al quedar un solo autor su × debe deshabilitarse.
- **Sin recargar**: `preventDefault()` al inicio, igual que en la búsqueda.

### Prueba

1. Llena el formulario con datos válidos y dos autores. En Network, filtra por `posts`: verás una petición POST. Haz clic en ella → **Payload**: deben salir los dos autores en el arreglo `autores`.
2. Debe aparecer la alerta verde con el número 101 y el formulario queda limpio con un solo autor.
3. Pon Network en **Offline** y envía de nuevo: alerta roja y el botón vuelve a habilitarse. Vuelve a "No throttling".
4. Consola sin errores rojos.

### Commit

```powershell
git add .
git commit -m "Parte A: envía la propuesta con fetch a JSONPlaceholder y muestra una alerta de éxito o error"
git push
```

---

# Sábado 24 de octubre

## Commit 15. Optimización: solo los parciales de Bootstrap que se usan

**Qué vas a hacer:** medir cuánto pesa el CSS con Bootstrap completo, cambiar `main.scss` para importar solo las partes que la página usa y volver a medir. También haces el análisis del panel de red para `depuracion.md`.

**Archivos:**
- [MODIFICAR] `buscador-js/scss/main.scss`
- [NUEVO] capturas en `docs/capturas/`

### Paso 1: medir ANTES

1. `npm start` (compila con Bootstrap completo).
2. DevTools → **Network**. Marca **Disable cache**.
3. Recarga con Ctrl+Shift+R (recarga forzada, sin caché).
4. Busca la fila `main.css` y anota el valor de la columna **Size** (o "Transferred"). Anota también, en la barra de abajo, la cantidad de **requests**, los **transferred** y los **resources**.
5. **Captura 4**: la pestaña Network completa tras la carga inicial → `docs/capturas/04-red-inicial.png`.
6. Busca `tolkien` y espera que carguen las portadas. Anota otra vez requests y transferred. **Captura 5** → `docs/capturas/05-red-busqueda.png`.
7. Recarga sin buscar y toma una captura donde se vea la fila `main.css` → `docs/capturas/06-css-antes.png`.

Escribe los números en un papel o en un archivo aparte; los vas a pasar a `depuracion.md` en el commit 20.

### Paso 2: el cambio

### `main.scss` (archivo completo, versión final)

Reemplaza **todo** el contenido por:

```scss
// 1. Mis variables primero, para que Bootstrap tome estos valores en lugar de los suyos
@import "variables";

// 2. Configuración de Bootstrap (no genera CSS, solo variables, mapas y mixins)
@import "bootstrap/scss/functions";
@import "bootstrap/scss/variables";
@import "bootstrap/scss/variables-dark";
@import "bootstrap/scss/maps";
@import "bootstrap/scss/mixins";
@import "bootstrap/scss/utilities";

// 3. Solo las partes de Bootstrap que la página usa (antes se importaba "bootstrap/scss/bootstrap" completo)
@import "bootstrap/scss/root";
@import "bootstrap/scss/reboot";
@import "bootstrap/scss/type";
@import "bootstrap/scss/images";
@import "bootstrap/scss/containers";
@import "bootstrap/scss/grid";
@import "bootstrap/scss/tables";
@import "bootstrap/scss/forms";
@import "bootstrap/scss/buttons";
@import "bootstrap/scss/transitions";
@import "bootstrap/scss/nav";
@import "bootstrap/scss/navbar";
@import "bootstrap/scss/card";
@import "bootstrap/scss/alert";
@import "bootstrap/scss/badge";
@import "bootstrap/scss/close";
@import "bootstrap/scss/modal";
@import "bootstrap/scss/spinners";
@import "bootstrap/scss/helpers";
@import "bootstrap/scss/utilities/api";

// 4. Estilos propios
@import "mixins";
@import "componentes";
@import "animaciones";
```

### Paso 3: medir DESPUÉS

1. Detén el servidor (Ctrl+C) y corre `npm start` de nuevo (recompila).
2. Repite la recarga con caché desactivada y anota el nuevo tamaño de `main.css`.
3. **Captura 7** → `docs/capturas/07-css-despues.png`.
4. Recorre toda la página (navbar en celular, búsqueda, modal, lista, formulario con errores, alertas) para confirmar que **nada se ve roto**. Si algo perdió estilos, falta el parcial correspondiente.

Como referencia, en mi prueba pasó de unos 274 KB a 207 KB (24 % menos). Tus números pueden variar un poco.

### Por qué

- **`@import "bootstrap/scss/bootstrap"`** es un archivo que importa **todo**: carrusel, acordeón, offcanvas, dropdowns, tooltips, popovers, toasts, paginación, breadcrumb, list group, progress, placeholders... La página no usa nada de eso, pero el CSS igual se descargaba.
- **El orden obligatorio** (está en la documentación de Bootstrap, "Importing" en la sección Sass):
  1. `functions` primero, porque las variables de Bootstrap usan funciones para calcular colores.
  2. Tus variables (ya van antes de todo en este proyecto; como no usan funciones de Bootstrap, funciona).
  3. `variables`, `variables-dark`, `maps`, `mixins` y `utilities`: **no generan CSS**, solo definen cosas.
  4. Los componentes que **sí** generan CSS, cada uno por separado.
  5. `utilities/api` al final, que genera las clases utilitarias (`mb-3`, `d-flex`, etc.).
- **Cada parcial y por qué está**:
  - `root`: las variables CSS `--bs-*`.
  - `reboot`: normaliza estilos base entre navegadores.
  - `type`: títulos y texto.
  - `images`: `img-fluid`.
  - `containers` y `grid`: `container`, `row`, `col-*`.
  - `tables`, `forms`, `buttons`, `nav`, `navbar`, `card`, `alert`, `badge`, `modal`, `spinners`: los componentes de la página.
  - `close`: la X del modal (`btn-close`).
  - `transitions`: las clases `fade` y `collapse` que usan el modal y la navbar.
  - `helpers`: `visually-hidden` y otros.
- **El JavaScript de Bootstrap no cambia**: `bootstrap.bundle.min.js` sigue completo. Se podría importar solo `Modal` y `Collapse` con un empaquetador, pero eso requiere herramientas que la Parte A no usa (no hay Vite ni webpack aquí). Es una limitación que puedes mencionar.
- **Medir antes y después** es lo que pide la rúbrica (criterio 7): no basta con decir "optimicé", hay que demostrarlo con números.

### Commit

```powershell
git add .
git commit -m "Parte A: importa solo los parciales de Bootstrap que usa la página para reducir el CSS"
git push
```

---

## Commit 16. Proyecto de Vite para la Parte B

**Qué vas a hacer:** crear el proyecto de React con Vite, limpiar la plantilla, instalar Bootstrap y Sass y reutilizar los estilos de la Parte A.

**Archivos:**
- [NUEVO] `tablero-react/` (lo crea Vite)
- [MODIFICAR] `tablero-react/index.html`, `vite.config.js`, `src/main.jsx`, `src/App.jsx`
- [NUEVO] `tablero-react/src/styles/main.scss`
- Borrar archivos de la plantilla

### Paso 1: crear el proyecto

Desde la raíz `tarea-2` (no desde `buscador-js`):

```powershell
npm create vite@latest tablero-react -- --template react
```

Si te hace preguntas: a cualquier opción "experimental" responde **No**, y a "Install with npm and start now?" responde **No** (lo haces tú). Luego:

```powershell
cd tablero-react
npm install
npm install bootstrap@5
npm install -D sass
```

### Paso 2: limpiar la plantilla

Borra estos archivos y carpetas (algunos pueden no existir según la versión de Vite; si no está, sigue):

- `tablero-react/README.md`
- `tablero-react/src/App.css`
- `tablero-react/src/index.css`
- `tablero-react/src/assets/` (la carpeta completa)
- `tablero-react/public/icons.svg`

Lo más fácil es borrarlos desde el explorador de VS Code (clic derecho → **Delete**). Con PowerShell, desde la carpeta `tablero-react`:

```powershell
Remove-Item README.md
Remove-Item src\App.css
Remove-Item src\index.css
Remove-Item src\assets -Recurse
Remove-Item public\icons.svg
```

Si alguno dice que no existe, no pasa nada: sigue con el siguiente.

En **cmd** los comandos son `del README.md`, `del src\App.css`, `del src\index.css`, `rmdir /s /q src\assets` y `del public\icons.svg`.

**No borres** `public/favicon.svg` (o `public/vite.svg`), `.gitignore`, `package.json`, `package-lock.json`, `vite.config.js`, `index.html` ni el archivo de configuración del linter (`.oxlintrc.json` o `eslint.config.js`, el que haya traído).

### Paso 3: `tablero-react/index.html` (cambio)

Cambia solo dos cosas:

**Busca:** `<html lang="en">` **cámbialo por:** `<html lang="es">`

**Busca:** `<title>tablero-react</title>` **cámbialo por:** `<title>Estantería Común | Tablero del club</title>`

### Paso 4: `tablero-react/vite.config.js` (archivo completo)

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  css: {
    preprocessorOptions: {
      scss: {
        // Bootstrap 5 todavía usa @import; esto evita llenar la terminal de avisos
        quietDeps: true,
        silenceDeprecations: ['import']
      }
    }
  }
})
```

### Paso 5: `tablero-react/src/styles/main.scss` (archivo completo, versión 1)

Crea la carpeta `src/styles`:

```scss
// Se reutilizan las variables y estilos de la Parte A para que ambas se vean igual
@import "../../../buscador-js/scss/variables";
@import "bootstrap/scss/bootstrap";
@import "../../../buscador-js/scss/mixins";
@import "../../../buscador-js/scss/componentes";
```

### Paso 6: `tablero-react/src/main.jsx` (archivo completo)

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './styles/main.scss'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

### Paso 7: `tablero-react/src/App.jsx` (archivo completo, versión 0)

Borra todo lo que trae la plantilla y deja esto:

```jsx
// App es el ancestro común: aquí viven los libros y los filtros, y de aquí bajan por props.
function App() {
  return (
    <>
      <nav className="navbar navbar-dark bg-primary">
        <div className="container">
          <span className="navbar-brand fw-bold">Estantería Común · Tablero del club</span>
        </div>
      </nav>

      <main className="container py-4">
        <p>Tablero en construcción.</p>
      </main>
    </>
  );
}

export default App;
```

### Por qué

- **Vite** lo pide la consigna. Es una herramienta que levanta un servidor de desarrollo muy rápido, recarga la página al guardar (HMR) y empaqueta el proyecto para producción (`npm run build`). Entiende JSX y Sass sin configuración extra (solo hay que instalar el paquete `sass`).
- **`--template react`**: plantilla de React con JavaScript (no TypeScript).
- **Se limpia la plantilla**: trae un contador de ejemplo, logos y CSS que no se usan. Dejarlos hace que el proyecto parezca copiado y confunde.
- **`vite.config.js`**: `plugins: [react()]` activa JSX y la recarga rápida de React. Las opciones de `scss` son las mismas que `--quiet-deps` y `--silence-deprecation` del script de la Parte A, pero escritas como configuración.
- **Reutilizar los estilos de la Parte A** (la consigna lo permite): `main.scss` importa `_variables`, `_mixins` y `_componentes` directamente desde `buscador-js/scss/` con rutas relativas (`../../../` sube de `styles` a `src`, a `tablero-react` y a la raíz). Ventajas: **los mismos colores y las mismas clases de estado** (`.estado--leido`, etc.) en las dos partes, y si cambias `$primary` en la Parte A, la Parte B también cambia. Desventaja: la Parte B depende de que exista la carpeta de la Parte A.
- **Aquí sí se importa Bootstrap completo**: Vite podría optimizarlo igual que en la Parte A, pero la optimización que se pide y se mide es la de la Parte A. Por eso aquí se dejó simple.
- **`main.jsx`** es el punto de entrada. `createRoot(document.getElementById('root'))` le dice a React dónde dibujar. Es el **único** lugar donde se usa `getElementById`, y no es un componente. La consigna prohíbe manipular el DOM **dentro de los componentes**.
- **`StrictMode`**: en desarrollo, React ejecuta algunas cosas dos veces para detectar errores. No afecta en producción.
- **`<>...</>`** (fragmento): un componente debe devolver un solo elemento raíz. El fragmento agrupa `nav` y `main` sin agregar un `<div>` extra al HTML.
- **`className` en vez de `class`**: en JSX se escribe `className` porque `class` es palabra reservada en JavaScript.

### Prueba

```powershell
npm run dev
```
Abre http://localhost:5173. Debe verse la navbar verde (los colores de la Parte A) y el texto "Tablero en construcción.". Consola sin errores.

### Commit

```powershell
cd ..
git status
```
**No** deben aparecer `tablero-react/node_modules` ni `tablero-react/dist` (Vite trae su propio `.gitignore` que los ignora).

```powershell
git add .
git commit -m "Parte B: crea el proyecto de Vite con React y reutiliza Bootstrap y los estilos de la Parte A"
git push
```

---

## Commit 17. Lista de libros con cambio de estado y quitar

**Qué vas a hacer:** los datos iniciales, los componentes de la lista y la versión 1 de `App` con el estado de los libros. Es la misma interacción de la Parte A, pero en React.

**Archivos:**
- [NUEVO] `tablero-react/src/data/libros.js`
- [NUEVO] `tablero-react/src/constantes.js`
- [NUEVO] `tablero-react/src/components/Panel/Panel.jsx`
- [NUEVO] `tablero-react/src/components/ListaLibros/LibroItem.jsx`
- [NUEVO] `tablero-react/src/components/ListaLibros/ListaLibros.jsx`
- [MODIFICAR] `tablero-react/src/App.jsx`

### `src/data/libros.js` (archivo completo)

```js
// Datos iniciales del tablero. En esta parte no se consume ninguna API.

export const miembros = [
  { id: 'm1', nombre: 'Ana Solís' },
  { id: 'm2', nombre: 'Carlos Mora' },
  { id: 'm3', nombre: 'Lucía Vargas' },
  { id: 'm4', nombre: 'Diego Jiménez' }
];

export const librosIniciales = [
  { id: 'l1', titulo: 'Cien años de soledad', autor: 'Gabriel García Márquez', estado: 'Leído', miembroId: 'm1' },
  { id: 'l2', titulo: 'Única mirando al mar', autor: 'Fernando Contreras Castro', estado: 'Leyendo', miembroId: 'm2' },
  { id: 'l3', titulo: 'Ficciones', autor: 'Jorge Luis Borges', estado: 'Pendiente', miembroId: 'm3' },
  { id: 'l4', titulo: 'Mamita Yunai', autor: 'Carlos Luis Fallas', estado: 'Leído', miembroId: 'm4' },
  { id: 'l5', titulo: 'Rayuela', autor: 'Julio Cortázar', estado: 'Pendiente', miembroId: 'm1' },
  { id: 'l6', titulo: 'La casa de los espíritus', autor: 'Isabel Allende', estado: 'Leyendo', miembroId: 'm3' }
];
```

### `src/constantes.js` (archivo completo)

```js
// Los mismos tres estados de la Parte A. Agregar uno nuevo es sumarlo a este arreglo.
export const ESTADOS = ['Pendiente', 'Leyendo', 'Leído'];

// 'Leído' -> 'estado--leido', para reutilizar las clases de color de la Parte A
export function claseEstado(estado) {
  const nombre = estado.normalize('NFD').replace(/[\u0300-\u036f]/g, '').toLowerCase();
  return `badge estado estado--${nombre}`;
}
```

### `src/components/Panel/Panel.jsx` (archivo completo)

```jsx
// Envoltorio con estilo de tarjeta. Lo que va adentro llega por children.
function Panel({ titulo, children }) {
  return (
    <section className="card mb-4">
      <div className="card-header bg-white">
        <h2 className="h5 mb-0">{titulo}</h2>
      </div>
      <div className="card-body">{children}</div>
    </section>
  );
}

export default Panel;
```

### `src/components/ListaLibros/LibroItem.jsx` (archivo completo)

```jsx
import { claseEstado } from '../../constantes.js';

// Una fila de la lista. No cambia nada por su cuenta: llama a las funciones que le pasa el padre.
function LibroItem({ libro, nombreMiembro, onCambiarEstado, onQuitar }) {
  return (
    <li className="list-group-item d-flex flex-column flex-sm-row align-items-sm-center gap-2">
      <div className="flex-grow-1">
        <div className="fw-semibold">{libro.titulo}</div>
        <div className="small text-muted">
          {libro.autor} · Propuesto por {nombreMiembro}
        </div>
      </div>
      <span className={claseEstado(libro.estado)}>{libro.estado}</span>
      <div className="text-nowrap">
        <button type="button" className="btn btn-sm btn-outline-primary me-1"
          onClick={() => onCambiarEstado(libro.id)}>
          Cambiar estado
        </button>
        <button type="button" className="btn btn-sm btn-outline-danger"
          onClick={() => onQuitar(libro.id)}>
          Quitar
        </button>
      </div>
    </li>
  );
}

export default LibroItem;
```

### `src/components/ListaLibros/ListaLibros.jsx` (archivo completo)

```jsx
import LibroItem from './LibroItem.jsx';

function ListaLibros({ libros, miembros, onCambiarEstado, onQuitar }) {
  // Renderizado condicional: si el filtro no deja nada, se avisa en lugar de mostrar una lista vacía
  if (libros.length === 0) {
    return <p className="text-muted mb-0">Ningún libro coincide con los filtros elegidos.</p>;
  }

  function nombreDe(miembroId) {
    return miembros.find((miembro) => miembro.id === miembroId)?.nombre ?? 'Desconocido';
  }

  return (
    <ul className="list-group list-group-flush">
      {libros.map((libro) => (
        <LibroItem
          key={libro.id}
          libro={libro}
          nombreMiembro={nombreDe(libro.miembroId)}
          onCambiarEstado={onCambiarEstado}
          onQuitar={onQuitar}
        />
      ))}
    </ul>
  );
}

export default ListaLibros;
```

### `src/App.jsx` (archivo completo, versión 1)

```jsx
import { useState } from 'react';
import { miembros, librosIniciales } from './data/libros.js';
import { ESTADOS } from './constantes.js';
import Panel from './components/Panel/Panel.jsx';
import ListaLibros from './components/ListaLibros/ListaLibros.jsx';

// App es el ancestro común: aquí viven los libros y los filtros, y de aquí bajan por props.
function App() {
  const [libros, setLibros] = useState(librosIniciales);

  // ---- Actualizaciones inmutables: siempre se crea un arreglo nuevo ----
  function cambiarEstado(id) {
    setLibros(libros.map((libro) => {
      if (libro.id !== id) return libro;
      const siguiente = ESTADOS[(ESTADOS.indexOf(libro.estado) + 1) % ESTADOS.length];
      return { ...libro, estado: siguiente };
    }));
  }

  function quitarLibro(id) {
    setLibros(libros.filter((libro) => libro.id !== id));
  }

  return (
    <>
      <nav className="navbar navbar-dark bg-primary">
        <div className="container">
          <span className="navbar-brand fw-bold">Estantería Común · Tablero del club</span>
        </div>
      </nav>

      <main className="container py-4">
        <div className="row g-4">
          <div className="col-12 col-lg-8">
            <Panel titulo={`Libros del club (${libros.length})`}>
              <ListaLibros
                libros={libros}
                miembros={miembros}
                onCambiarEstado={cambiarEstado}
                onQuitar={quitarLibro}
              />
            </Panel>
          </div>
        </div>
      </main>
    </>
  );
}

export default App;
```

### Por qué

**Datos (`data/libros.js`):**
- La consigna dice que los datos iniciales vienen de un arreglo en un módulo, sin API.
- **Ids fijos** (`l1`, `m1`): sirven como `key` estable y para relacionar cada libro con su miembro (`miembroId`). Es como una llave foránea en una base de datos.
- Los miembros están aparte para no repetir el nombre en cada libro.

**`constantes.js`:**
- `ESTADOS` es el mismo arreglo que en la Parte A. Todos los componentes lo leen de aquí: si se agrega un estado, aparece en todos lados (modificación en caliente probable).
- `claseEstado` es la misma función de la Parte A, para usar las mismas clases de color.

**`Panel` y `children` (criterio 10):**
- `children` es una prop especial: es **lo que se escribe entre la etiqueta de apertura y la de cierre** del componente. En `<Panel titulo="X"><ListaLibros /></Panel>`, `children` es `<ListaLibros />`.
- `Panel` no sabe qué va adentro, solo pone el marco (la tarjeta con título). Por eso se reutiliza para la lista, el resumen, los filtros y el formulario.
- **Desestructuración de props** `function Panel({ titulo, children })`: es lo mismo que recibir `props` y usar `props.titulo`, pero más corto.

**`LibroItem` y las props de tipo función (criterio 10):**
- Recibe el libro y **dos funciones**: `onCambiarEstado` y `onQuitar`. Esas funciones son de `App`. `LibroItem` no modifica nada: cuando se hace clic, *avisa* llamando a la función con el id.
- **"Los datos bajan, los eventos suben"**: el libro baja de `App` → `ListaLibros` → `LibroItem` por props. El clic sube de `LibroItem` → `App` llamando a las funciones.
- `onClick={() => onCambiarEstado(libro.id)}`: se pasa una **función flecha**. Si se escribiera `onClick={onCambiarEstado(libro.id)}`, la función se ejecutaría **al dibujar**, no al hacer clic (error clásico).
- Aquí no hace falta `data-id` ni `closest()`: cada `LibroItem` ya sabe cuál es su libro porque lo recibió por props. Esta es una de las diferencias que explica el README.
- El nombre `on...` es una convención para las props que son funciones de eventos.

**`ListaLibros`, keys y renderizado condicional (criterio 11):**
- **`libros.map(...)`** convierte el arreglo de datos en un arreglo de componentes. Es lo mismo que `libros.map(crearTarjetaLibro)` de la Parte A, pero devolviendo JSX.
- **`key={libro.id}`**: React necesita una `key` única en cada elemento de una lista para saber cuál es cuál entre un render y otro. **Por qué no el índice**: si quitas el primer libro, el que era índice 1 pasa a ser 0, y React cree que el elemento 0 cambió de contenido en vez de que se borró. Con el id, React sabe exactamente cuál desapareció. La rúbrica lo exige.
- **Renderizado condicional**: si `libros.length === 0`, el componente devuelve un párrafo en lugar de la lista (un `return` temprano). En este commit no se ve porque no hay filtros todavía, pero si quitas todos los libros aparece.
- **`?.` (optional chaining)**: `miembros.find(...)?.nombre` no falla si `find` devuelve `undefined`. Y `?? 'Desconocido'` da un valor por defecto.
- **Carpetas por componente** (`components/ListaLibros/`): la consigna pide organizar en carpetas. `LibroItem` va en la misma carpeta que `ListaLibros` porque solo se usa ahí.

**`App`, estado e inmutabilidad (criterio 11):**
- **`useState(librosIniciales)`** devuelve dos cosas: el valor actual (`libros`) y una función para cambiarlo (`setLibros`). Cuando llamas a `setLibros(nuevo)`, React vuelve a ejecutar `App` con el valor nuevo y actualiza la pantalla.
- **Por qué inmutable**: React decide si algo cambió comparando la **referencia** (si es el mismo objeto en memoria), no el contenido. Si hicieras `libros[0].estado = 'Leído'; setLibros(libros)`, es el mismo arreglo, React no ve el cambio y puede no volver a dibujar. La consigna prohíbe `push`, `splice` y asignación directa.
- **`cambiarEstado`**: `map` crea un arreglo **nuevo**. Para los libros que no cambian devuelve el mismo objeto; para el que cambia crea uno **nuevo** con `{ ...libro, estado: siguiente }` (copia todas las propiedades y sobrescribe `estado`). El cálculo del siguiente estado es el mismo `%` de la Parte A.
- **`quitarLibro`**: `filter` crea un arreglo nuevo sin ese libro.
- **El título del panel** `` `Libros del club (${libros.length})` `` se calcula en cada render; no hace falta actualizarlo a mano como en la Parte A.

### Prueba

`npm run dev`: deben verse los 6 libros con su estado de color y quién lo propuso. Cambia estados (el color cambia) y quita libros (el contador del título baja). Quita los 6: debe aparecer "Ningún libro coincide con los filtros elegidos.". Recarga para volver a los datos iniciales. Consola sin errores (si React se queja de `key`, revisa que pusiste `key={libro.id}`).

### Commit

```powershell
git add .
git commit -m "Parte B: muestra la lista de libros y permite cambiar el estado y quitar de forma inmutable"
git push
```

---

## Commit 18. Filtros y resumen con estado elevado

**Qué vas a hacer:** agregar un componente de filtros (por estado y por miembro) y uno de resumen (cantidad por estado). Son hermanos y se sincronizan porque el estado de los filtros vive en `App`.

**Archivos:**
- [NUEVO] `tablero-react/src/components/Filtros/Filtros.jsx`
- [NUEVO] `tablero-react/src/components/Resumen/Resumen.jsx`
- [MODIFICAR] `tablero-react/src/styles/main.scss`
- [MODIFICAR] `tablero-react/src/App.jsx`

### `src/components/Filtros/Filtros.jsx` (archivo completo)

```jsx
import { ESTADOS } from '../../constantes.js';

// No guarda estado propio: muestra los filtros que recibe y avisa los cambios al padre.
function Filtros({ filtros, miembros, onCambiar }) {
  function manejarCambio(evento) {
    const { name, value } = evento.target;
    onCambiar({ ...filtros, [name]: value });
  }

  return (
    <div className="row g-2">
      <div className="col-12 col-sm-6">
        <label htmlFor="filtro-estado" className="form-label small">Estado</label>
        <select id="filtro-estado" name="estado" className="form-select"
          value={filtros.estado} onChange={manejarCambio}>
          <option value="todos">Todos</option>
          {ESTADOS.map((estado) => (
            <option key={estado} value={estado}>{estado}</option>
          ))}
        </select>
      </div>
      <div className="col-12 col-sm-6">
        <label htmlFor="filtro-miembro" className="form-label small">Miembro</label>
        <select id="filtro-miembro" name="miembro" className="form-select"
          value={filtros.miembro} onChange={manejarCambio}>
          <option value="todos">Todos</option>
          {miembros.map((miembro) => (
            <option key={miembro.id} value={miembro.id}>{miembro.nombre}</option>
          ))}
        </select>
      </div>
    </div>
  );
}

export default Filtros;
```

### `src/components/Resumen/Resumen.jsx` (archivo completo)

```jsx
import { ESTADOS, claseEstado } from '../../constantes.js';

// Muestra cuántos libros hay en cada estado. Al hacer clic en uno se filtra por ese estado,
// así este componente y Filtros (su hermano) cambian el mismo estado que vive en App.
function Resumen({ resumen, estadoActivo, onElegirEstado }) {
  return (
    <div className="d-flex flex-wrap gap-2">
      {ESTADOS.map((estado) => (
        <button
          key={estado}
          type="button"
          className={`btn btn-light resumen__boton ${estadoActivo === estado ? 'activo' : ''}`}
          onClick={() => onElegirEstado(estadoActivo === estado ? 'todos' : estado)}
        >
          <span className={claseEstado(estado)}>{resumen[estado]}</span>
          <span className="d-block small mt-1">{estado}</span>
        </button>
      ))}
    </div>
  );
}

export default Resumen;
```

### `src/styles/main.scss` (cambio)

**Agrega al final del archivo:**
```scss

// Propio del tablero
.resumen {
  &__boton {
    min-width: 6rem;

    &.activo {
      outline: .2rem solid $primary;
    }
  }
}
```

### `src/App.jsx` (cambio 1 de 4): imports

**Busca esto:**
```jsx
import Panel from './components/Panel/Panel.jsx';
import ListaLibros from './components/ListaLibros/ListaLibros.jsx';
```

**Cámbialo por:**
```jsx
import Panel from './components/Panel/Panel.jsx';
import Filtros from './components/Filtros/Filtros.jsx';
import Resumen from './components/Resumen/Resumen.jsx';
import ListaLibros from './components/ListaLibros/ListaLibros.jsx';
```

### `src/App.jsx` (cambio 2 de 4): estado de filtros y valores derivados

**Busca esto:**
```jsx
  const [libros, setLibros] = useState(librosIniciales);
```

**Cámbialo por:**
```jsx
  const [libros, setLibros] = useState(librosIniciales);
  const [filtros, setFiltros] = useState({ estado: 'todos', miembro: 'todos' });

  // ---- Valores derivados: se calculan en cada render, no se guardan en otro estado ----
  const librosDelMiembro = libros.filter(
    (libro) => filtros.miembro === 'todos' || libro.miembroId === filtros.miembro
  );

  const librosFiltrados = librosDelMiembro.filter(
    (libro) => filtros.estado === 'todos' || libro.estado === filtros.estado
  );

  // El resumen cuenta según el miembro elegido, para que cambie junto con los filtros
  const resumen = {};
  ESTADOS.forEach((estado) => {
    resumen[estado] = librosDelMiembro.filter((libro) => libro.estado === estado).length;
  });
```

### `src/App.jsx` (cambio 3 de 4): función para el resumen

**Busca esto:**
```jsx
  function quitarLibro(id) {
    setLibros(libros.filter((libro) => libro.id !== id));
  }
```

**Agrega debajo:**
```jsx

  function elegirEstado(estado) {
    setFiltros({ ...filtros, estado });
  }
```

### `src/App.jsx` (cambio 4 de 4): el JSX de la columna

**Busca esto:**
```jsx
          <div className="col-12 col-lg-8">
            <Panel titulo={`Libros del club (${libros.length})`}>
              <ListaLibros
                libros={libros}
```

**Cámbialo por:**
```jsx
          <div className="col-12 col-lg-8">
            <Panel titulo="Resumen">
              <Resumen resumen={resumen} estadoActivo={filtros.estado} onElegirEstado={elegirEstado} />
            </Panel>

            <Panel titulo="Filtros">
              <Filtros filtros={filtros} miembros={miembros} onCambiar={setFiltros} />
            </Panel>

            <Panel titulo={`Libros del club (${librosFiltrados.length})`}>
              <ListaLibros
                libros={librosFiltrados}
```

### Por qué

**Estado elevado (criterio 12, el más importante de la Parte B):**
- `Filtros` y `Resumen` son **hermanos**: los dos son hijos de `App`, ninguno contiene al otro.
- Los dos necesitan el mismo dato: el filtro de estado. `Filtros` lo muestra en el select; `Resumen` lo marca como activo y lo cambia al hacer clic.
- Si cada uno tuviera su propio `useState`, serían **dos copias** y no se enterarían de los cambios del otro (los hermanos no se pueden pasar props directamente).
- La solución de React: **subir ("elevar") el estado al ancestro común más cercano**, que es `App`. `App` lo guarda, se lo pasa a los dos por props y les da funciones para cambiarlo.
- **El recorrido**: haces clic en "Pendiente" en `Resumen` → se llama `onElegirEstado('Pendiente')` → es `elegirEstado` de `App` → `setFiltros(...)` → `App` se vuelve a dibujar → `Filtros` recibe `filtros.estado = 'Pendiente'` y su select cambia solo, y `ListaLibros` recibe solo los pendientes.
- Y al revés: cambias el miembro en `Filtros` → `onCambiar` (que es `setFiltros`) → `App` recalcula `resumen` con los libros de ese miembro → `Resumen` muestra los nuevos números.

**`Filtros`:**
- **No tiene `useState`**: es un componente controlado por el padre. Muestra `filtros` y avisa cambios con `onCambiar`.
- **`manejarCambio` con `[name]: value`**: `name` es el atributo del select (`"estado"` o `"miembro"`). Los corchetes en un objeto son una *propiedad calculada*: `{ [name]: value }` es lo mismo que `{ estado: value }` si `name` vale `"estado"`. Así una sola función sirve para los dos selects (y para cualquier filtro nuevo).
- **`{ ...filtros, [name]: value }`**: copia los filtros actuales y cambia solo uno. Inmutable.
- **`onCambiar={setFiltros}`**: se le pasa directamente la función de `useState`, porque `manejarCambio` ya arma el objeto completo.
- **`htmlFor`** en JSX equivale a `for` en HTML (`for` es palabra reservada).
- **`value` + `onChange`** en el select: es un campo **controlado**; lo que muestra viene del estado.

**`Resumen`:**
- Recorre `ESTADOS` y muestra un botón por estado con el número de `resumen[estado]`.
- **Clic para filtrar**: si el estado ya está activo, vuelve a `'todos'` (funciona como interruptor).
- **`key={estado}`**: aquí la key es el nombre del estado, que es único y no cambia de posición. Es estable.
- **La clase `activo`**: se agrega con un template literal y un ternario. Se define en `main.scss` con un `outline` del color primario. El anidamiento `&__boton { &.activo {} }` compila a `.resumen__boton.activo`.

**Valores derivados (criterio 11):**
- `librosDelMiembro`, `librosFiltrados` y `resumen` **no son estado**. Se calculan desde `libros` y `filtros` **en cada render**.
- **Por qué no guardarlos en `useState`**: serían copias de información que ya existe. Si quitas un libro y te olvidas de actualizar el resumen, quedarían distintos. Calculándolos siempre, es imposible que se desincronicen. La rúbrica pone en Bueno (no Excelente) "algún valor derivable guardado como estado aparte".
- **¿No es lento calcular en cada render?** Con unos cuantos libros es instantáneo. Si fueran miles se podría usar `useMemo`, pero aquí sería complicar sin necesidad.
- **El resumen cuenta sobre `librosDelMiembro`** (no sobre todos ni sobre los filtrados por estado). Así, al elegir un miembro, el resumen muestra cuántos tiene ese miembro en cada estado, y al elegir un estado los números no se vuelven cero en los otros estados.

### Prueba

1. El resumen dice 2 Pendiente, 2 Leyendo, 2 Leído.
2. Cambia el estado de un libro: el resumen se actualiza al instante.
3. Elige "Lucía Vargas" en Miembro: la lista muestra sus 2 libros y el resumen cambia a 1, 1, 0.
4. Haz clic en "Pendiente" del resumen: el select de Estado de **Filtros** cambia solo a "Pendiente" y la lista se filtra. Esa es la sincronización entre hermanos.
5. Con Lucía elegida, haz clic en "Leído": aparece "Ningún libro coincide con los filtros elegidos.".
6. Vuelve a hacer clic en "Leído": se desactiva y vuelve a "Todos".

### Commit

```powershell
git add .
git commit -m "Parte B: agrega filtros y resumen como componentes hermanos sincronizados con el estado elevado a App"
git push
```

---

# Domingo 25 de octubre (entrega a las 11:55 p. m.)

Trata de terminar en la mañana o a mediodía. Así, si algo falla, tienes tiempo.

## Commit 19. Formulario controlado en React

**Qué vas a hacer:** un formulario para agregar libros al tablero, controlado por React, con validación y mensajes junto a cada campo.

**Archivos:**
- [NUEVO] `tablero-react/src/components/FormularioLibro/FormularioLibro.jsx`
- [MODIFICAR] `tablero-react/src/App.jsx`

### `src/components/FormularioLibro/FormularioLibro.jsx` (archivo completo)

```jsx
import { useState } from 'react';
import { ESTADOS } from '../../constantes.js';

const VACIO = { titulo: '', autor: '', miembroId: '', estado: ESTADOS[0] };

// Devuelve un objeto con un mensaje por cada campo que tenga error
function validar(datos) {
  const errores = {};

  if (datos.titulo.trim().length < 2) {
    errores.titulo = 'El título debe tener al menos 2 caracteres.';
  }
  if (datos.autor.trim().length < 3) {
    errores.autor = 'El autor debe tener al menos 3 caracteres.';
  } else if (!/^[A-Za-zÁÉÍÓÚáéíóúÑñÜü.' -]+$/.test(datos.autor.trim())) {
    errores.autor = 'El autor solo puede tener letras, espacios y puntos.';
  }
  if (!datos.miembroId) {
    errores.miembroId = 'Elija quién propone el libro.';
  }

  return errores;
}

// Formulario controlado: cada input muestra lo que hay en el estado y cada cambio lo actualiza.
function FormularioLibro({ miembros, onAgregar }) {
  const [datos, setDatos] = useState(VACIO);
  const [errores, setErrores] = useState({});

  function manejarCambio(evento) {
    const { name, value } = evento.target;
    setDatos({ ...datos, [name]: value });
  }

  function manejarEnvio(evento) {
    evento.preventDefault();
    const nuevosErrores = validar(datos);
    setErrores(nuevosErrores);

    if (Object.keys(nuevosErrores).length > 0) return;

    onAgregar({ ...datos, titulo: datos.titulo.trim(), autor: datos.autor.trim() });
    setDatos(VACIO);
  }

  // Clase de Bootstrap para pintar en rojo el campo con error
  const clase = (campo, base = 'form-control') => `${base} ${errores[campo] ? 'is-invalid' : ''}`;

  return (
    <form onSubmit={manejarEnvio} noValidate>
      <div className="mb-3">
        <label htmlFor="titulo" className="form-label">Título</label>
        <input id="titulo" name="titulo" className={clase('titulo')}
          value={datos.titulo} onChange={manejarCambio} />
        <div className="invalid-feedback">{errores.titulo}</div>
      </div>

      <div className="mb-3">
        <label htmlFor="autor" className="form-label">Autor</label>
        <input id="autor" name="autor" className={clase('autor')}
          value={datos.autor} onChange={manejarCambio} />
        <div className="invalid-feedback">{errores.autor}</div>
      </div>

      <div className="mb-3">
        <label htmlFor="miembroId" className="form-label">Propuesto por</label>
        <select id="miembroId" name="miembroId" className={clase('miembroId', 'form-select')}
          value={datos.miembroId} onChange={manejarCambio}>
          <option value="">Elija un miembro...</option>
          {miembros.map((miembro) => (
            <option key={miembro.id} value={miembro.id}>{miembro.nombre}</option>
          ))}
        </select>
        <div className="invalid-feedback">{errores.miembroId}</div>
      </div>

      <div className="mb-3">
        <label htmlFor="estado" className="form-label">Estado inicial</label>
        <select id="estado" name="estado" className="form-select"
          value={datos.estado} onChange={manejarCambio}>
          {ESTADOS.map((estado) => (
            <option key={estado} value={estado}>{estado}</option>
          ))}
        </select>
      </div>

      <button type="submit" className="btn btn-primary w-100">Agregar libro</button>
    </form>
  );
}

export default FormularioLibro;
```

### `src/App.jsx` (cambio 1 de 3): import

**Busca esto:**
```jsx
import ListaLibros from './components/ListaLibros/ListaLibros.jsx';
```

**Agrega debajo:**
```jsx
import FormularioLibro from './components/FormularioLibro/FormularioLibro.jsx';
```

### `src/App.jsx` (cambio 2 de 3): la función de agregar

**Busca esto:**
```jsx
  function quitarLibro(id) {
    setLibros(libros.filter((libro) => libro.id !== id));
  }
```

**Agrega debajo:**
```jsx

  function agregarLibro(datos) {
    setLibros([...libros, { ...datos, id: crypto.randomUUID() }]);
  }
```

### `src/App.jsx` (cambio 3 de 3): la columna del formulario

**Busca esto** (cerca del final):
```jsx
            </Panel>
          </div>
        </div>
      </main>
```

**Cámbialo por:**
```jsx
            </Panel>
          </div>

          <div className="col-12 col-lg-4">
            <Panel titulo="Agregar un libro">
              <FormularioLibro miembros={miembros} onAgregar={agregarLibro} />
            </Panel>
          </div>
        </div>
      </main>
```

Con esto `App.jsx` queda en su versión final. Compáralo con `Tarea2/tablero-react/src/App.jsx`: deben ser iguales.

### Por qué

**Formulario controlado (criterio 12):**
- En un formulario **controlado**, React es la única fuente de verdad del valor de cada campo:
  - `value={datos.titulo}`: el input muestra lo que hay en el estado.
  - `onChange={manejarCambio}`: cada tecla actualiza el estado.
  - Si quitaras el `onChange`, no podrías escribir: el input siempre mostraría el valor del estado.
- Lo contrario es un formulario **no controlado**: el valor vive en el DOM y se lee al enviar (como hace la Parte A con `FormData`). En React se prefiere controlado porque el valor siempre está disponible en el estado (para validar mientras se escribe, limpiar, etc.).
- **`manejarCambio` con `[name]: value`**: igual que en `Filtros`. Los inputs tienen `name` igual a la propiedad del estado (`titulo`, `autor`, `miembroId`, `estado`), así una sola función sirve para todos.
- **Estado local del formulario**: `datos` y `errores` viven **dentro** de `FormularioLibro`, no en `App`, porque ningún otro componente los necesita. La regla es: el estado va en el componente más bajo que lo necesite. Solo cuando el libro está listo se le pasa a `App` con `onAgregar`.

**Validación:**
- **`validar(datos)`** es una función pura (no usa nada de afuera, solo su parámetro) que devuelve un objeto con un mensaje por cada campo con error: `{ titulo: '...', autor: '...' }`. Si el objeto está vacío, no hay errores.
- Está **fuera** del componente porque no necesita nada de React y así no se crea de nuevo en cada render.
- `Object.keys(nuevosErrores).length > 0`: cuenta cuántos errores hay.
- **Mensajes por campo**: cada campo muestra `errores.campo` en su `invalid-feedback`. La función `clase(campo)` agrega `is-invalid` si ese campo tiene error, y Bootstrap lo pinta de rojo y muestra el mensaje.
- **Regla del autor con expresión regular** `/^[A-Za-zÁÉÍÓÚáéíóúÑñÜü.' -]+$/`: `^` y `$` obligan a que *todo* el texto cumpla; `[...]` son los caracteres permitidos; `+` es "uno o más".
- **`noValidate`**: igual que en la Parte A, apaga los mensajes del navegador. Aquí además no se usan atributos HTML5; toda la validación es en JavaScript, que es lo que pide la consigna para la Parte B ("validación en el cliente").
- **`.trim()` al enviar**: se guardan los textos sin espacios sobrantes.
- **`setDatos(VACIO)`**: limpia el formulario después de agregar. `VACIO` está como constante afuera para no repetir el objeto.
- **`ESTADOS[0]`** como estado inicial: si cambia el orden de los estados, el formulario sigue empezando en el primero.

**Agregar en `App` (inmutable):**
- `[...libros, nuevo]`: arreglo nuevo con todos los libros y el nuevo al final. **No** `libros.push(nuevo)`.
- `{ ...datos, id: crypto.randomUUID() }`: copia los datos del formulario y agrega un id único. `crypto.randomUUID()` lo trae el navegador y genera algo como `"3b241101-e2bb-4255-8caf-4136c566a962"`. Ese id es la `key` estable del nuevo libro.
- **Por qué no `id: libros.length + 1`**: si quitas un libro y agregas otro, podrían repetirse ids.

**Layout final:** dos columnas desde `lg`: 8 para resumen, filtros y lista, 4 para el formulario. En celular todo va en una columna. Eso cubre "la interfaz es adaptable".

### Prueba

1. Haz clic en **Agregar libro** con todo vacío: tres mensajes (título, autor y miembro).
2. Escribe `A` en el título y `Juan 23` en el autor: mensajes de largo y de caracteres.
3. Llena bien (por ejemplo "El Principito", "Antoine de Saint-Exupéry", Carlos Mora) y agrega. El libro aparece al final de la lista, el resumen suma uno en Pendiente y el formulario queda vacío.
4. Con un filtro de miembro puesto, agrega un libro de otro miembro: no aparece en la lista filtrada (es lo correcto), pero sí si vuelves a "Todos".
5. Achica la ventana: el formulario baja debajo de la lista.
6. Corre `npm run build` dentro de `tablero-react`: debe terminar sin errores. (Crea `dist/`, que está ignorado.)

### Commit

```powershell
git add .
git commit -m "Parte B: agrega el formulario controlado para sumar libros con validación y mensajes por campo"
git push
```

---

## Commit 20. Documentación de depuración y rendimiento

**Qué vas a hacer:** escribir `docs/depuracion.md` con el error que depuraste el jueves, el análisis de red y la optimización del sábado, con tus capturas y tus números.

**Archivos:**
- [NUEVO] `docs/depuracion.md`
- Las 7 capturas en `docs/capturas/` (si no las subiste ya en commits anteriores)

### Revisa que tengas las capturas

```
docs/capturas/01-breakpoint-main.png     (commit 9)
docs/capturas/02-event-listeners.png     (commit 9)
docs/capturas/03-arreglado.png           (commit 10)
docs/capturas/04-red-inicial.png         (commit 15)
docs/capturas/05-red-busqueda.png        (commit 15)
docs/capturas/06-css-antes.png           (commit 15)
docs/capturas/07-css-despues.png         (commit 15)
```

Si te falta alguna de las de los commits 9 y 10, puedes reproducir el error en una rama temporal: `git switch -c captura`, vuelve a poner en `crearTarjetaLibro` las líneas `tarjeta.dataset.bsToggle = 'modal';` y `tarjeta.dataset.bsTarget = '#modal-detalle';` en lugar de `tarjeta.dataset.detalle = '';`, toma la captura, y luego `git switch main` y `git branch -D captura`.

### `docs/depuracion.md` (archivo completo)

Copia esto y **reemplaza cada `__` por tus números** (de las mediciones del commit 15). Puedes reescribir las frases con tus palabras; de hecho es mejor, porque el profe te puede preguntar por cualquier línea.

~~~~markdown
# Depuración y rendimiento (Parte A)

Todas las pruebas se hicieron en Google Chrome con las herramientas del desarrollador (F12), con la Parte A corriendo en http://localhost:3000.

## 1. Error encontrado: el botón "Agregar" también abría el modal

### Qué pasaba

En la primera versión las tarjetas de resultados abrían el modal de detalle con los atributos de Bootstrap `data-bs-toggle="modal"` y `data-bs-target="#modal-detalle"`. El botón "Agregar a mi lista" está dentro de la tarjeta, así que en la escucha delegada de `#resultados` le puse `event.stopPropagation()` para que el clic no llegara a la tarjeta.

Aun así, al tocar "Agregar" el libro se agregaba **y además** se abría el modal.

### Cómo lo localicé

1. En la pestaña **Sources** abrí `js/main.js` y puse un punto de interrupción en la línea de `evento.stopPropagation()` dentro de `resultados.addEventListener('click', ...)`.
2. Hice clic en "Agregar". La ejecución se detuvo en el breakpoint, antes de que corriera mi `stopPropagation()`.
3. Con la ejecución detenida, escribí en la consola `document.body.classList.contains('modal-open')` y dio `true`. Bootstrap pone esa clase cuando empieza a abrir un modal, así que el modal ya se estaba abriendo **antes** de que se ejecutara mi código. También revisé `evento.eventPhase`, que dio `3` (fase de burbuja).
4. Para entender por qué Bootstrap iba primero, en **Elements** seleccioné una tarjeta y abrí la pestaña **Event Listeners** con la casilla **Ancestors** marcada. Ahí se ve que Bootstrap registra su escucha de `click` en `document` con `useCapture: true`, o sea en la fase de captura.
5. Conclusión: el clic primero **baja** en fase de captura y pasa por `document`, donde Bootstrap abre el modal. Después **sube** en fase de burbuja hasta `#resultados`, donde estaba mi `stopPropagation()`. Para ese momento ya era tarde, porque `stopPropagation()` solo detiene lo que todavía no ha pasado.

![Breakpoint en main.js con la consola mostrando modal-open](capturas/01-breakpoint-main.png)

![Panel Event Listeners mostrando la escucha de Bootstrap en document con captura](capturas/02-event-listeners.png)

### Cómo lo arreglé

Quité los atributos `data-bs-*` de las tarjetas y abrí el modal desde mi propio código. Ahora hay una escucha delegada en `<main>` que abre el detalle de cualquier elemento con `data-detalle` (tarjetas y filas de la lista), y llama a `bootstrap.Modal.getOrCreateInstance(...).show()` en `mostrarDetalle()` (`js/render.js`). Como esa escucha está en un ancestro y usa la fase de burbuja, el `stopPropagation()` de los botones "Agregar", "Cambiar estado" y "Quitar" sí la detiene.

![Después del arreglo: se agrega el libro y el modal no se abre](capturas/03-arreglado.png)

### Un segundo aviso que salió en la consola

Al cerrar el modal, Chrome mostraba el aviso *"Blocked aria-hidden on an element because its descendant retained focus"*. Pasa porque Bootstrap pone `aria-hidden` en el modal mientras el botón de cerrar todavía tiene el foco. Lo resolví quitando el foco en el evento `hide.bs.modal` (`js/main.js`). Con eso la consola queda limpia durante el uso normal.

## 2. Análisis del panel de red

Pasos: pestaña **Network**, marcar **Disable cache**, recargar la página (Ctrl + Shift + R) y hacer una búsqueda de "tolkien".

| Momento | Peticiones | Transferido | Recursos | Tiempo de carga |
|---|---|---|---|---|
| Carga inicial (sin buscar) | __ | __ KB | __ KB | __ ms |
| Después de buscar "tolkien" | __ | __ KB | __ KB | __ ms |

Qué se carga al inicio: `index.html`, `css/main.css`, `bootstrap.bundle.min.js`, los cinco módulos de `js/` (cada `import` es una petición aparte) y `sin-portada.svg` solo si hace falta.

Al buscar se agrega una petición a `search.json` y hasta 12 a `covers.openlibrary.org`. Las portadas son lo que más pesa de la página. Por eso las imágenes llevan `loading="lazy"`: las tarjetas que no se ven en pantalla no descargan su portada hasta que el usuario baja.

![Panel de red en la carga inicial](capturas/04-red-inicial.png)

![Panel de red después de una búsqueda](capturas/05-red-busqueda.png)

## 3. Optimización: importar solo los parciales necesarios de Bootstrap

**Antes**, `scss/main.scss` importaba Bootstrap completo:

```scss
@import "variables";
@import "bootstrap/scss/bootstrap";
```

Eso incluye componentes que la página no usa (carrusel, acordeón, offcanvas, tooltips, popovers, toasts, paginación, breadcrumb, dropdowns, list group, progress, etc.).

**Después**, importé solo la configuración y los parciales que la página sí usa (grilla, tablas, formularios, botones, navbar, tarjetas, alertas, badges, modal, spinners, helpers y utilidades). Se puede ver la lista en `scss/main.scss`.

Medición de `css/main.css` (columna "Transferido" del panel de red, con caché desactivada):

| | Tamaño de `main.css` |
|---|---|
| Antes (Bootstrap completo) | __ KB |
| Después (solo parciales usados) | __ KB |
| Reducción | __ % |

Como referencia, compilando los dos archivos en mi equipo el CSS pasó de unos 274 KB a 207 KB sin comprimir (alrededor de un 24 % menos).

![main.css antes de la optimización](capturas/06-css-antes.png)

![main.css después de la optimización](capturas/07-css-despues.png)
~~~~

### Por qué

La rúbrica (criterio 7, nivel Excelente) pide tres cosas en este archivo, y cada sección cubre una:
1. **Un error real localizado con puntos de interrupción, con capturas**: sección 1. Es real porque te pasó en el commit 9 y está en el historial de git (commit 9 con el error, commit 10 con la corrección).
2. **El análisis del panel de red con cantidad de peticiones y peso total**: sección 2, la tabla.
3. **Una optimización con medición antes y después**: sección 3.

Los enlaces a imágenes `![texto](capturas/archivo.png)` usan rutas relativas: como `depuracion.md` está en `docs/`, `capturas/...` apunta a `docs/capturas/...`. GitHub las muestra dentro del documento.

### Prueba

Haz push y abre `docs/depuracion.md` en GitHub: deben verse las 7 imágenes. Si una sale rota, revisa que el nombre del archivo coincida exactamente (mayúsculas incluidas).

### Commit

```powershell
git add .
git commit -m "Documenta la depuración con puntos de interrupción, el análisis de red y la optimización medida"
git push
```

---

## Commit 21. README final

**Qué vas a hacer:** reemplazar el README corto del commit 1 por el completo: instalación, compilación de Sass, ejecución de las dos partes y la sección "Del DOM manual a React".

**Archivos:**
- [MODIFICAR] `README.md` (reemplaza todo el contenido)

### `README.md` (archivo completo, versión final)

~~~~markdown
# Tarea 2 · Estantería Común

Programación Web Avanzada (SOFT-12), Universidad CENFOTEC, III cuatrimestre 2026.
Autor: José Barrantes

Estantería Común es un club de lectura. El repositorio tiene dos partes:

- **buscador-js/** (Parte A): buscador público de libros contra la API de Open Library, con lista de lecturas y formulario para proponer libros. Hecho con JavaScript sin frameworks, Bootstrap y Sass.
- **tablero-react/** (Parte B): tablero interno donde el club lleva el registro de sus lecturas. Hecho con React y Vite.

Las dos partes resuelven la misma interacción (agregar un libro, cambiarle el estado y quitarlo) para poder comparar los dos enfoques.

## Requisitos

- Node.js 20 o superior (incluye npm)
- Un navegador actualizado

## Instalación

Cada parte tiene su propio `package.json`, así que las dependencias se instalan por separado:

```bash
cd buscador-js
npm install

cd ../tablero-react
npm install
```

## Parte A: buscador-js

### Compilar Sass

El CSS se genera desde `scss/main.scss` hacia `css/main.css` (la carpeta `css/` no se sube al repositorio porque es un archivo generado).

```bash
cd buscador-js
npm run sass          # compila una vez
npm run sass:watch    # recompila cada vez que se guarda un .scss
```

### Ejecutar

```bash
cd buscador-js
npm start
```

`npm start` compila Sass primero (script `prestart`) y luego levanta un servidor local en http://localhost:3000. Hace falta un servidor porque los módulos ES (`type="module"`) no cargan si se abre el `index.html` con doble clic.

### Organización

| Archivo | Qué hace |
|---|---|
| `js/api.js` | Peticiones con `fetch` a Open Library y a JSONPlaceholder |
| `js/render.js` | Funciones que crean y actualizan elementos del DOM |
| `js/lista.js` | Estado en memoria de la lista de lecturas |
| `js/formulario.js` | Validación, campos de autor dinámicos y envío de la propuesta |
| `js/main.js` | Punto de entrada, conecta los eventos con los demás módulos |
| `scss/_variables.scss` | Variables de origen de Bootstrap y variables propias |
| `scss/_mixins.scss` | Mixin `desde()`, mixin `sombra-suave` y función `a-rem()` |
| `scss/_componentes.scss` | Componente propio `.estante` (CSS Grid, mobile-first) y colores de estados |
| `scss/_animaciones.scss` | Animaciones y transiciones de la lista |

## Parte B: tablero-react

### Ejecutar

```bash
cd tablero-react
npm run dev
```

Vite levanta el proyecto en http://localhost:5173. Vite compila el Sass por su cuenta, no hace falta un paso aparte. Para una versión de producción se usa `npm run build`.

### Organización

```
src/
├── App.jsx                 estado de libros y filtros (ancestro común)
├── constantes.js           lista de estados y clase de color
├── data/libros.js          miembros y libros iniciales
├── styles/main.scss        Bootstrap + parciales de la Parte A
└── components/
    ├── Panel/              envoltorio que recibe children
    ├── Filtros/            filtros por estado y por miembro
    ├── Resumen/            cantidad de libros por estado
    ├── ListaLibros/        lista y cada fila (LibroItem)
    └── FormularioLibro/    formulario controlado para agregar libros
```

## Del DOM manual a React

Voy a usar como ejemplo **cambiar el estado de un libro** (Pendiente → Leyendo → Leído).

**En la Parte A** el dato y lo que se ve en pantalla son dos cosas separadas, y yo tengo que mantenerlas iguales a mano. Para eso hicieron falta cuatro piezas:

1. Guardar el dato: `siguienteEstado(id)` en `buscador-js/js/lista.js` busca la lectura y le cambia la propiedad `estado`.
2. Saber qué fila tocar: cada `<tr>` lleva un `data-id` que se le pone al crearla en `crearFilaLectura()` (`buscador-js/js/render.js`). La escucha delegada en el `tbody` (`buscador-js/js/main.js`, `tablaLecturas.addEventListener`) usa `closest('button[data-accion]')` y `closest('tr')` para saber qué botón se tocó y de qué libro es.
3. Actualizar la pantalla: `actualizarEstadoFila()` en `render.js` busca la etiqueta dentro de esa fila y le cambia el texto y la clase de color. Si me olvido de llamarla, el arreglo dice "Leído" y la pantalla sigue diciendo "Pendiente".
4. Encargarme de los elementos nuevos: como las filas se crean después de cargar la página, no puedo ponerles escuchas al inicio; por eso uso delegación. Además, al quitar un libro tengo que borrar el dato **y** sacar el `<tr>` del DOM cuando termina la transición (`quitarFilaConTransicion()`).

**En la Parte B** solo cambio el dato. `cambiarEstado(id)` en `tablero-react/src/App.jsx` crea un arreglo nuevo con `map` y se lo pasa a `setLibros`. React vuelve a ejecutar `App`, calcula otra vez `librosFiltrados` y `resumen`, y actualiza solo lo que cambió en el DOM. El botón de `LibroItem.jsx` llama a `onCambiarEstado(libro.id)` con un `onClick`.

Lo que deja de ser necesario y por qué:

- **Buscar elementos y modificarlos** (`querySelector`, `textContent`, `className`): la interfaz se escribe en JSX como una función del estado, así que no hay un "antes" que corregir.
- **Los `data-id` y `closest()`**: cada `LibroItem` ya sabe a qué libro pertenece porque lo recibe por props.
- **La delegación de eventos**: React registra los eventos por su cuenta y cada botón nuevo llega con su `onClick`.
- **Actualizar a mano el resumen**: en la Parte B el resumen se calcula desde `libros` en cada render, así que nunca queda desactualizado. En la Parte A habría tenido que acordarme de recalcularlo después de cada cambio.

Lo que no cambia es la regla de negocio: en los dos casos el siguiente estado sale de la posición en el arreglo `ESTADOS` (`lista.js` y `constantes.js`). Una diferencia que sí noté es que en React no puedo modificar el arreglo directamente como hago en `lista.js` con `push` y `splice`. Si lo hiciera, React no se entera del cambio y no vuelve a dibujar.

## Documentación de depuración

El análisis de depuración, red y rendimiento está en [docs/depuracion.md](docs/depuracion.md).
~~~~

### Por qué

- **Lo que exige la consigna** del README: cómo instalar dependencias, cómo compilar Sass, cómo ejecutar cada parte y la sección "Del DOM manual a React" de máximo una página, con referencias a archivos.
- **La sección "Del DOM manual a React"** es la pregunta central del criterio 16 ("explica con precisión qué cambia al pasar del DOM manual a React"). Léela hasta que puedas decirla sin mirar. La idea en una frase: *en la Parte A yo mantengo sincronizados los datos y la pantalla; en la Parte B solo cambio los datos y React actualiza la pantalla.*
- **Referencias a archivos y funciones**, no a números de línea: los números cambian con cualquier edición, los nombres de función no.

### Prueba

Abre el repositorio en GitHub y mira que el README se vea bien (tablas, bloques de código, enlace a `depuracion.md`). Sigue tú mismo las instrucciones desde cero para comprobar que funcionan:

```powershell
cd $HOME
git clone https://github.com/TU_USUARIO/tarea-2.git prueba-tarea-2
cd prueba-tarea-2\buscador-js
npm install
npm start
```

En otra terminal (el **+** de la terminal de VS Code abre una nueva):

```powershell
cd $HOME\prueba-tarea-2\tablero-react
npm install
npm run dev
```

Si las dos partes funcionan desde una copia limpia, al profe también le van a funcionar. Después detén los dos servidores con Ctrl+C y borra la carpeta `prueba-tarea-2` desde el explorador de Windows (está en `C:\Users\TU_USUARIO\prueba-tarea-2`).

### Commit

```powershell
git add .
git commit -m "Completa el README con instalación, ejecución y la sección Del DOM manual a React"
git push
```

---

## Etiqueta y entrega

### Comparar con la referencia (opcional, pero recomendado)

Windows no trae `diff`, pero git sí puede comparar carpetas que no son repositorios con `--no-index`. Desde la carpeta que contiene `Tarea2` y `tarea-2` (en PowerShell, una línea por comparación):

```powershell
git diff --no-index --stat Tarea2\buscador-js\js tarea-2\buscador-js\js
git diff --no-index --stat Tarea2\buscador-js\scss tarea-2\buscador-js\scss
git diff --no-index --stat Tarea2\buscador-js\index.html tarea-2\buscador-js\index.html
git diff --no-index --stat Tarea2\tablero-react\src tarea-2\tablero-react\src
```

Si un comando no imprime nada, esos archivos son iguales. Si imprime algo, quita `--stat` para ver exactamente qué línea cambia (sales de la vista con la tecla `q`). Si la única diferencia es al final de las líneas (`^M`), es por los saltos de línea de Windows y no importa.

Otra opción visual: en VS Code, clic derecho en un archivo → **Select for Compare**, y luego clic derecho en el otro → **Compare with Selected**.

### Crear la etiqueta

```powershell
cd tarea-2
git status                         # no debe quedar nada pendiente
git log --oneline                  # deben verse los 21 commits
git tag -a tarea-2 -m "Entrega de la Tarea 2"
git push origin tarea-2
```

En GitHub entra a la pestaña **Tags** (o "Releases" → "Tags") y comprueba que `tarea-2` apunta al último commit.

- **`git tag -a`** crea una etiqueta *anotada* (con autor, fecha y mensaje). Una etiqueta es un nombre fijo para un commit; a diferencia de una rama, no se mueve cuando haces más commits. Por eso la defensa se hace "sobre el commit con la etiqueta tarea-2".
- **`git push origin tarea-2`**: las etiquetas no se suben con `git push` normal; hay que subirlas aparte.

Si después de etiquetar necesitas corregir algo antes de la hora límite:

```powershell
git tag -d tarea-2                  # borra la etiqueta local
git push origin :refs/tags/tarea-2  # borra la etiqueta en GitHub
# haz la corrección y su commit
git tag -a tarea-2 -m "Entrega de la Tarea 2"
git push origin tarea-2
```

### Subir a Moodle

Crea un archivo de texto, por ejemplo `tarea2-jose-barrantes.txt`, con una sola línea:

```
https://github.com/TU_USUARIO/tarea-2
```

Súbelo a Moodle antes de las 11:55 p. m. Revisa que el repositorio sea **público** o que el profe tenga acceso.

### Para la defensa

Las modificaciones en caliente se hacen en una rama aparte, para no tocar la entrega:

```powershell
git switch -c defensa
```

Al terminar la defensa puedes volver con `git switch main`. Toda la preparación está en `GUIA_DEFENSA.md`.
