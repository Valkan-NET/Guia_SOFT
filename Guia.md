# Plan Avance 2 · Innovation Hub · paso a paso con código por día

## Contexto

**Avance 1.** Promedio 3.739 / 4, nivel "Bueno", nota 93.5, a 0.011 de "Excelente". Se perdieron puntos en detalle/visibilidad (7), formulario (8), eliminación (9), validación (10), perfil/solicitud (13) y README (14). Todas esas observaciones se corrigen en la versión React.

**Avance 2.** React (Vite) + API Express + MongoDB, mismo diseño Bootstrap/Sass. El contrato de la API se congela al entregar porque en diciembre el mismo cliente correrá sobre .NET + SQL. Producto 64 pts + defensa individual sin IA 16 pts (50/50), con modificaciones en caliente.

**Decisiones confirmadas.** Entrega sábado 7 nov 2026, 11:55 p. m. Trabajo individual. MongoDB en la PC Windows. Autenticación al final, con JWT Bearer + lista de revocación.

**Orden pedido.** Primero se convierte el Avance 1 a React usando los datos que ya existían; después se escribe el contrato, el servidor, MongoDB y la autenticación.

**Cómo se logra sin rehacer pantallas.** Desde el día 1 el cliente habla con la API a través de una sola capa (`src/api/`). Mientras no exista el servidor, esa capa responde con un **servidor simulado en el navegador** (`src/mock/`) que lee los JSON del Avance 1, ya traducidos al formato del contrato. El día 9 se cambia una variable de entorno, se borra `src/mock/` y las pantallas siguen igual, ahora contra Express.

**Riesgos.**
1. Se piden ≥30 commits distribuidos en las cinco semanas del bloque. Si no hay commits del Avance 2 desde finales de setiembre, esa distribución ya no se puede cumplir honestamente. No reescribir fechas. Hacer los commits indicados cada día (≈35 en total); si un paso tiene varios archivos, conviene partirlo en más commits pequeños. Si aplica, comentar la situación con el profesor.
2. La carpeta de Downloads es la copia de setiembre sin `.git`. Todo se hace en el clon real en la PC Windows.
3. La defensa es sin IA. Cada día termina con 15 minutos explicando en voz alta lo hecho, archivo por archivo.
4. El código de este plan se probó el 25 oct en una copia aislada (macOS, MongoDB 8 en Docker): el servidor final pasa la colección completa (78 peticiones, 258 aserciones, 0 fallos); el servidor de los días 6 a 9 pasa la colección en modo desarrollo (59 peticiones, 201 aserciones); el servidor simulado pasa 28 casos; el cliente final y el del día 8 compilan con Vite y se recorrieron en el navegador. Igual, cada día tiene su sección "Verificar" para repetirlo en tu PC Windows antes del último commit del día.

---

## Correcciones del Avance 1 y dónde quedan

| # | Observación | Se corrige en |
|---|---|---|
| 7 | Restringida mostraba propietario, fechas, estado, equipo | Día 1 (servidor simulado) y día 7 (Express): la API devuelve solo título, tipo, resumen, categoría, competencias y visibilidad. Día 2: `DetalleRestringido` |
| 8 | Visibilidad preseleccionada | Día 3: opción vacía obligatoria |
| 8 | Contador solo en título y resumen | Día 3: `CampoTexto` con contador en todo campo con límite |
| 8 | Cancelar era un enlace | Día 3: `AccionesFormulario` con `<button>` |
| 8 | Quitar competencia sin aviso | Día 3: `SelectorChips` con confirmación |
| 8 | Etiquetas como texto con comas | Día 3: `SelectorChips` en modo libre |
| 9 | Eliminar solo desde el detalle | Día 2: `AccionesPropietario` en la tarjeta del catálogo; día 4: en "Mis iniciativas" |
| 10 | Errores no se limpian al corregir | Día 3: `useFormulario` revalida en cada cambio |
| 10 | Ayuda de longitud inconsistente | Día 3: misma ayuda "Faltan N caracteres" en todos los formularios |
| 13 | Competencia = todas las del usuario | Día 4: intersección con las requeridas; día 8: el servidor la valida |
| 13 | Rol en texto libre | Día 1: catálogo `roles`; día 4: `<select>` |
| 13 | Cancelar volvía al catálogo | Día 4: vuelve al detalle de la iniciativa |
| 13 | Mensaje de cupo inconsistente | Día 1 y 8: la regla verifica estado antes que cupo, con el equipo real |
| 14 | README sin Sass, URL errónea, ortografía, tag | Día 0: se corrige el texto escrito a mano; la tabla generada por el script y los mensajes de commits viejos no se pueden corregir, y una nota fuera del bloque generado lo explica |

---

## Calendario

| Día | Fecha | Fase | Contenido |
|---|---|---|---|
| 0 | dom 25 oct | React | Repo, README del Avance 1, proyecto Vite, Sass, imágenes, layout, rutas, portada, 404 |
| 1 | lun 26 | React | Datos del Avance 1 en formato contrato, servidor simulado, capa `api/`, hooks, contextos, componentes de UI |
| 2 | mar 27 | React | Catálogo con filtros y paginación, tarjeta con acciones del propietario, detalle completo y restringido, equipo |
| 3 | mié 28 | React | Formulario de iniciativa crear/editar con todas las correcciones |
| 4 | jue 29 | React | Perfil, solicitud, mis solicitudes, recibidas, proyectos, GitHub |
| 5 | vie 30 | Contrato | `docs/api-contract.md`, `docs/modelo-datos.md`, instalar MongoDB en Windows |
| 6 | sáb 31 | Servidor | Base Express, configuración, errores, modelos, seed, catálogos |
| 7 | dom 1 nov | Servidor | Iniciativas y equipo |
| 8 | lun 2 | Servidor | Solicitudes y proyectos |
| 9 | mar 3 | Integración | Cliente contra Express, borrar el simulado, colección Postman |
| 10 | mié 4 | Seguridad | Autenticación JWT, usuarios, perfil, búsqueda de personas |
| 11 | jue 5 | Cierre | Auditoría, Newman, README final |
| 12 | vie 6 | Defensa | Ensayo de recorrido y modificaciones en caliente |
| 13 | sáb 7 | Entrega | Clon limpio, etiqueta `entrega-2`, Moodle |

---

## Convenciones para todos los días

- Terminal: **PowerShell** en Windows, desde la raíz del repo salvo que se indique otra carpeta.
- Cada commit con dos trailers para la tabla del README:

```powershell
git add <archivos>
git commit -m "Mensaje corto" --trailer "Seccion: Cliente" --trailer "Cambio: Qué cambió"
git push
```

- Los mensajes de commit se escriben con tildes y ortografía completa; los commits ya existentes no se modifican (nada de `rebase`, `commit --amend` sobre commits publicados ni `push --force`).
- Cada día empieza con `git pull` y termina con `git push`.
- Los archivos se muestran completos. Cuando un día modifica un archivo de un día anterior, se indica exactamente qué reemplazar.

---

# DÍA 0 · domingo 25 oct · Repo, README del Avance 1 y esqueleto React

### 0.1 Clonar y verificar la etiqueta del Avance 1

```powershell
git clone https://github.com/<tu-usuario>/innovation-hub.git
cd innovation-hub
git ls-remote --tags origin
```

Si `entrega-1` no aparece, ubicar el commit entregado el 27 de setiembre y etiquetarlo:

```powershell
git log --oneline --until="2026-09-28 00:00"
git tag -a entrega-1 <hash-del-primer-resultado> -m "Avance 1: prototipo funcional"
git push origin entrega-1
```

### 0.2 Corregir el README del Avance 1

En `README.md`:

1. Corregir tildes y redacción **solo en el texto escrito a mano** (descripción, decisiones de diseño, instrucciones): visualización, inspección, diseño, según, ejecutar, Pensé, opté, más, relación, estáticos, facilitaría, Correcciones. Buscar cada palabra con Ctrl+F en VS Code. Si la palabra aparece **entre** `<!-- INICIO TABLA COMMITS -->` y `<!-- FIN TABLA COMMITS -->`, se deja como está: ese bloque lo reescribe `herramientas/tabla-commits.sh` desde `git log` en cada commit, así que cualquier corrección manual se pierde en el siguiente commit.
2. **Los mensajes de commits antiguos no se pueden corregir.** Hacerlo exigiría reescribir el historial, lo que cambiaría los hashes ya revisados y rompería la etiqueta `entrega-1`. Las columnas "Sección del sistema" y "Cambio principal" de esos commits sí salen de `herramientas/commits-meta.tsv` si ese archivo existe en tu repo: ahí se pueden corregir tildes porque es texto escrito a mano. Fuera del bloque generado, justo **debajo** de la marca `<!-- FIN TABLA COMMITS -->` (zona que el script no toca), agregar esta nota:

```markdown
> Los mensajes de los commits anteriores al Avance 2 conservan su redacción original sin tildes:
> corregirlos exigiría reescribir el historial y cambiaría los identificadores ya entregados.
> Desde el Avance 2 los mensajes se escriben con ortografía completa.
```

3. Reemplazar la sección de ejecución del Avance 1 por:

```markdown
### Avance 1 · prototipo estático

**Compilar los estilos (solo si se modifican los .scss):**

    npm install
    npm run css

`npm install` instala Sass y el código fuente de Bootstrap. `npm run css` compila
`avance1/scss/main.scss` en `avance1/css/main.css`, que ya viene versionado.

**Ejecutar:** abrir la carpeta del repositorio en VS Code, clic derecho sobre
`avance1/index.html` → **Open with Live Server**. La dirección es
`http://127.0.0.1:5500/avance1/index.html` porque Live Server sirve desde la raíz
del repositorio.
```

```powershell
git add README.md
git commit -m "Corregir README del Avance 1: ortografía, Sass y ruta de ejecución" --trailer "Seccion: Documentación" --trailer "Cambio: Correcciones de la revisión del Avance 1"
```

### 0.3 Carpetas y `.gitignore`

```powershell
New-Item -ItemType Directory docs, servidor, pruebas | Out-Null
```

Agregar al final de `.gitignore`:

```
# Avance 2
.env
cliente/dist/
servidor/logs/
```

### 0.4 Crear el proyecto Vite

```powershell
npm create vite@latest cliente -- --template react
```

Si pregunta "Install with npm and start now?", responder **No**.

```powershell
cd cliente
npm install
npm install react-router-dom bootstrap bootstrap-icons
npm install -D sass
Remove-Item src\App.css, src\index.css, src\assets\react.svg, public\vite.svg
cd ..
```

```powershell
git add .gitignore cliente
git commit -m "Crear el cliente React con Vite y dependencias" --trailer "Seccion: Cliente" --trailer "Cambio: Proyecto Vite con React Router, Bootstrap y Sass"
```

### 0.5 Trasladar Sass e imágenes del Avance 1

```powershell
New-Item -ItemType Directory cliente\src\styles, cliente\public\img | Out-Null
Copy-Item avance1\scss\* cliente\src\styles\
Copy-Item avance1\img\* cliente\public\img\
```

Los siete `.scss` se copian **sin cambios**: el diseño del Avance 1 se conserva.

**`cliente/vite.config.js`** (reemplazar completo)

```js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: { port: 5173 },
  css: {
    preprocessorOptions: {
      scss: {
        // Bootstrap 5.3 todavía usa @import; se silencian sus avisos de deprecación.
        quietDeps: true,
        silenceDeprecations: ['import', 'global-builtin', 'color-functions'],
      },
    },
  },
});
```

**`cliente/index.html`** (reemplazar completo)

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Publicá ideas, necesidades y retos; encontrá competencias y formá equipos interdisciplinarios." />
    <link rel="icon" type="image/svg+xml" href="/img/logo.svg" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Sora:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet" />
    <title>Innovation Hub</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

```powershell
git add cliente
git commit -m "Trasladar los estilos Sass y las imágenes del Avance 1 al cliente" --trailer "Seccion: Estilos" --trailer "Cambio: Mismo diseño compilado por Vite"
```

### 0.6 Punto de entrada, layout y rutas

**`cliente/src/main.jsx`**

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import 'bootstrap-icons/font/bootstrap-icons.css';
import './styles/main.scss';
import App from './App.jsx';

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>
);
```

**`cliente/src/App.jsx`**

```jsx
import { Routes, Route } from 'react-router-dom';
import Layout from './components/layout/Layout.jsx';
import InicioPage from './features/inicio/InicioPage.jsx';
import NoEncontradoPage from './pages/NoEncontradoPage.jsx';

export default function App() {
  return (
    <Routes>
      <Route element={<Layout />}>
        <Route index element={<InicioPage />} />
        <Route path="*" element={<NoEncontradoPage />} />
      </Route>
    </Routes>
  );
}
```

**`cliente/src/hooks/useTituloPagina.js`**

```js
import { useEffect } from 'react';

// Actualiza el <title> del documento; cada página declara el suyo.
export function useTituloPagina(titulo) {
  useEffect(() => {
    document.title = titulo ? `${titulo} · Innovation Hub` : 'Innovation Hub';
  }, [titulo]);
}
```

**`cliente/src/components/layout/Layout.jsx`**

```jsx
import { useEffect } from 'react';
import { Outlet, Link, useLocation } from 'react-router-dom';
import Navbar from './Navbar.jsx';
import Footer from './Footer.jsx';

export default function Layout() {
  const { pathname } = useLocation();

  // Al cambiar de ruta, la nueva pantalla empieza arriba.
  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);

  return (
    <>
      <a className="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">
        Saltar al contenido
      </a>
      <header>
        <Navbar>
          <Link className="btn btn-primary btn-sm" to="/iniciativas/nueva">
            <i className="bi bi-plus-circle-fill" aria-hidden="true"></i> Publicar iniciativa
          </Link>
        </Navbar>
      </header>
      <main id="contenido" tabIndex={-1}>
        <Outlet />
      </main>
      <Footer />
    </>
  );
}
```

**`cliente/src/components/layout/Navbar.jsx`**

```jsx
import { useState } from 'react';
import { Link, NavLink } from 'react-router-dom';

const ENLACES = [
  { to: '/', texto: 'Inicio', icono: 'bi-house', fin: true },
  { to: '/iniciativas', texto: 'Iniciativas', icono: 'bi-grid-3x3-gap' },
  { to: '/proyectos', texto: 'Proyectos', icono: 'bi-rocket-takeoff' },
];

// children: controles de la derecha (sesión, publicar). Así el navbar no
// cambia cuando cambia la forma de iniciar sesión.
export default function Navbar({ children }) {
  const [abierto, setAbierto] = useState(false);
  const cerrar = () => setAbierto(false);

  return (
    <nav className="navbar navbar-expand-md" aria-label="Navegación principal">
      <div className="container">
        <Link className="navbar-brand" to="/" onClick={cerrar}>
          <img src="/img/logo.svg" alt="" width="28" height="28" />
          <span>Innovation Hub</span>
        </Link>
        <button
          className="navbar-toggler"
          type="button"
          aria-controls="navPrincipal"
          aria-expanded={abierto}
          aria-label="Mostrar u ocultar la navegación"
          onClick={() => setAbierto((valor) => !valor)}
        >
          <span className="navbar-toggler-icon"></span>
        </button>
        <div className={`collapse navbar-collapse${abierto ? ' show' : ''}`} id="navPrincipal">
          <ul className="navbar-nav me-auto mb-2 mb-md-0">
            {ENLACES.map((enlace) => (
              <li className="nav-item" key={enlace.to}>
                <NavLink className="nav-link" to={enlace.to} end={enlace.fin} onClick={cerrar}>
                  <i className={`bi ${enlace.icono}`} aria-hidden="true"></i> {enlace.texto}
                </NavLink>
              </li>
            ))}
          </ul>
          <div className="d-flex flex-column flex-md-row align-items-md-center gap-2">{children}</div>
        </div>
      </div>
    </nav>
  );
}
```

`NavLink` agrega la clase `active` y `aria-current="page"` por sí solo, que es lo que el Avance 1 hacía a mano.

**`cliente/src/components/layout/Footer.jsx`**

```jsx
export default function Footer() {
  return (
    <footer className="pie text-center py-4 small">
      <div className="container">Innovation Hub · Proyecto SOFT-12 · Universidad CENFOTEC · 2026</div>
    </footer>
  );
}
```

**`cliente/src/pages/NoEncontradoPage.jsx`**

```jsx
import { Link } from 'react-router-dom';
import { useTituloPagina } from '../hooks/useTituloPagina.js';

export default function NoEncontradoPage() {
  useTituloPagina('Página no encontrada');
  return (
    <div className="container py-5 text-center estado-vacio">
      <img src="/img/vacio-catalogo.svg" alt="" width="220" height="160" className="mb-3" />
      <h1 className="h3">No encontramos esa página</h1>
      <p className="text-body-secondary">La dirección no existe o cambió.</p>
      <Link className="btn btn-primary" to="/">
        <i className="bi bi-house" aria-hidden="true"></i> Volver al inicio
      </Link>
    </div>
  );
}
```

```powershell
git add cliente
git commit -m "Agregar layout, navbar, pie y manejo de rutas inexistentes" --trailer "Seccion: Cliente" --trailer "Cambio: Estructura de navegación con React Router"
```

### 0.7 Portada

**`cliente/src/features/inicio/TarjetaPaso.jsx`**

```jsx
export default function TarjetaPaso({ numero, titulo, imagen, alt, children }) {
  return (
    <article className="card h-100 text-center">
      <div className="card-body">
        <img className="paso-imagen mb-3" src={imagen} alt={alt} width="240" height="180" loading="lazy" />
        <span className="paso-numero mx-auto mb-3" aria-hidden="true">{numero}</span>
        <h3 className="h6">{titulo}</h3>
        <p className="small text-body-secondary mb-0">{children}</p>
      </div>
    </article>
  );
}
```

**`cliente/src/features/inicio/InicioPage.jsx`**

```jsx
import { Link } from 'react-router-dom';
import TarjetaPaso from './TarjetaPaso.jsx';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';

const PASOS = [
  { numero: 1, titulo: 'Explorá', imagen: '/img/paso-explorar.webp', alt: 'Lupa sobre tarjetas de iniciativas',
    texto: 'Buscá ideas, necesidades y retos por texto, tipo, categoría o competencia.' },
  { numero: 2, titulo: 'Publicá', imagen: '/img/paso-publicar.webp', alt: 'Persona lanzando una idea representada por una bombilla',
    texto: 'Registrá tu iniciativa con el problema, los beneficiarios y las competencias que necesitás.' },
  { numero: 3, titulo: 'Postulate', imagen: '/img/paso-postular.webp', alt: 'Mano entregando una pieza de rompecabezas que encaja',
    texto: 'Solicitá participar indicando qué competencia aportás, tu rol y tu disponibilidad.' },
  { numero: 4, titulo: 'Formá equipo', imagen: '/img/paso-equipo.webp', alt: 'Equipo interdisciplinario alrededor de una mesa con un cohete',
    texto: 'El autor acepta participantes y, con equipo, la iniciativa se convierte en proyecto.' },
];

export default function InicioPage() {
  useTituloPagina('Conectá ideas con personas');

  return (
    <>
      <section className="hero" aria-labelledby="t-hero">
        <div className="container">
          <div className="row align-items-center g-4">
            <div className="col-lg-7 text-center text-lg-start">
              <h1 id="t-hero">Conectá ideas con las personas que las hacen realidad</h1>
              <p className="lead mt-3 mb-4">
                Publicá ideas, necesidades y retos; encontrá las competencias que faltan y formá equipos
                interdisciplinarios dentro de la comunidad universitaria.
              </p>
              <div className="d-flex flex-column flex-sm-row gap-2 justify-content-center justify-content-lg-start">
                <Link className="btn btn-light btn-lg" to="/iniciativas">
                  <i className="bi bi-search" aria-hidden="true"></i> Explorar iniciativas
                </Link>
                <Link className="btn btn-outline-light btn-lg" to="/iniciativas/nueva">
                  <i className="bi bi-plus-circle" aria-hidden="true"></i> Publicar una iniciativa
                </Link>
              </div>
            </div>
            <div className="col-lg-5 d-none d-lg-block">
              <img className="hero-imagen" src="/img/hero.webp" alt="Estudiantes de distintas carreras conectando ideas en una red" width="600" height="450" />
            </div>
          </div>
        </div>
      </section>

      <section className="container py-5" aria-labelledby="t-como">
        <h2 id="t-como" className="h3 text-center mb-2">¿Cómo funciona?</h2>
        <p className="text-center text-body-secondary col-lg-8 mx-auto mb-4">
          Cuatro pasos para pasar de una idea suelta a un equipo trabajando.
        </p>
        <div className="row row-cols-1 row-cols-sm-2 row-cols-lg-4 g-4">
          {PASOS.map((paso) => (
            <div className="col" key={paso.numero}>
              <TarjetaPaso numero={paso.numero} titulo={paso.titulo} imagen={paso.imagen} alt={paso.alt}>
                {paso.texto}
              </TarjetaPaso>
            </div>
          ))}
        </div>
      </section>

      <section className="container pb-5" aria-labelledby="t-caminos">
        <h2 id="t-caminos" className="visually-hidden">Publicar o participar</h2>
        <div className="row g-4">
          <div className="col-md-6">
            <article className="card h-100">
              <div className="card-body">
                <h3 className="h5"><i className="bi bi-lightbulb-fill text-primary" aria-hidden="true"></i> Tenés una idea, una necesidad o un reto</h3>
                <ul className="small text-body-secondary mb-3">
                  <li>Describí el problema y a quién beneficia.</li>
                  <li>Indicá las competencias que buscás y cuántas personas necesitás.</li>
                  <li>Elegí la visibilidad: pública, institucional, restringida o privada.</li>
                  <li>Recibí solicitudes y armá tu equipo.</li>
                </ul>
                <Link className="btn btn-primary" to="/iniciativas/nueva">Publicar iniciativa <i className="bi bi-arrow-right" aria-hidden="true"></i></Link>
              </div>
            </article>
          </div>
          <div className="col-md-6">
            <article className="card h-100">
              <div className="card-body">
                <h3 className="h5"><i className="bi bi-people-fill text-info" aria-hidden="true"></i> Querés sumarte a un proyecto real</h3>
                <ul className="small text-body-secondary mb-3">
                  <li>Completá tu perfil con competencias e intereses.</li>
                  <li>Filtrá el catálogo por lo que sabés hacer.</li>
                  <li>Enviá una solicitud con tu mensaje de presentación.</li>
                  <li>Cuando te acepten, pasás a formar parte del equipo.</li>
                </ul>
                <Link className="btn btn-outline-primary" to="/iniciativas">Ver iniciativas <i className="bi bi-arrow-right" aria-hidden="true"></i></Link>
              </div>
            </article>
          </div>
        </div>
      </section>
    </>
  );
}
```

### 0.8 Verificar

```powershell
cd cliente
npm run dev
```

Abrir `http://localhost:5173`: la portada debe verse igual que en el Avance 1. Probar `http://localhost:5173/no-existe` → pantalla 404. Reducir el ancho a 390 px: el menú colapsa en el botón hamburguesa y abre/cierra.

```powershell
cd ..
git add cliente
git commit -m "Portar la portada del Avance 1 a componentes React" --trailer "Seccion: Portada" --trailer "Cambio: Pasos generados desde un arreglo con TarjetaPaso"
git push
```

**Explicar en voz alta:** qué hace `<Outlet />`, por qué `Navbar` recibe `children`, qué agrega `NavLink`, qué hace el efecto de `Layout` y su arreglo de dependencias.

---

# DÍA 1 · lunes 26 oct · Datos, servidor simulado y capa de API

Hoy no se ve nada nuevo en pantalla salvo las cifras de la portada y el selector "Ver como", pero queda lista toda la base que usan los días 2 a 4.

### 1.1 Datos del Avance 1 en el formato del contrato

Son los mismos datos de `avance1/datos/`, con nombres en inglés y valores controlados en inglés (así se llamarán en la API y en SQL). Los `id` de usuarios, iniciativas, solicitudes y proyectos ya tienen forma de ObjectId de MongoDB, para que el día 6 el seed use exactamente estos archivos y los `id` no cambien. Los catálogos usan códigos legibles.

```powershell
New-Item -ItemType Directory cliente\public\mock | Out-Null
```

**`cliente/public/mock/categories.json`**

```json
[
  { "id": "cat-tecnologia", "name": "Tecnología", "icon": "bi-cpu" },
  { "id": "cat-educacion", "name": "Educación", "icon": "bi-mortarboard" },
  { "id": "cat-sostenibilidad", "name": "Sostenibilidad", "icon": "bi-tree" },
  { "id": "cat-impacto-social", "name": "Impacto social", "icon": "bi-people" },
  { "id": "cat-negocios", "name": "Negocios y emprendimiento", "icon": "bi-graph-up-arrow" },
  { "id": "cat-salud", "name": "Salud y bienestar", "icon": "bi-heart-pulse" },
  { "id": "cat-cultura", "name": "Cultura y creatividad", "icon": "bi-palette" },
  { "id": "cat-otro", "name": "Otro", "icon": "bi-three-dots" }
]
```

**`cliente/public/mock/skills.json`**

```json
[
  { "id": "comp-javascript", "name": "JavaScript", "area": "Desarrollo" },
  { "id": "comp-diseno-ux", "name": "Diseño UX", "area": "Diseño" },
  { "id": "comp-diseno-grafico", "name": "Diseño gráfico", "area": "Diseño" },
  { "id": "comp-analisis-datos", "name": "Análisis de datos", "area": "Datos" },
  { "id": "comp-gestion", "name": "Gestión de proyectos", "area": "Gestión" },
  { "id": "comp-redes", "name": "Redes y ciberseguridad", "area": "Infraestructura" },
  { "id": "comp-marketing", "name": "Marketing digital", "area": "Negocios" },
  { "id": "comp-comunicacion", "name": "Comunicación", "area": "Negocios" }
]
```

**`cliente/public/mock/roles.json`** (nuevo catálogo: corrige el rol en texto libre)

```json
[
  { "id": "rol-frontend", "name": "Desarrollo frontend" },
  { "id": "rol-backend", "name": "Desarrollo backend" },
  { "id": "rol-ux", "name": "Diseño UX/UI" },
  { "id": "rol-datos", "name": "Análisis de datos" },
  { "id": "rol-coordinacion", "name": "Coordinación" },
  { "id": "rol-comunicacion", "name": "Comunicación y difusión" },
  { "id": "rol-investigacion", "name": "Investigación" },
  { "id": "rol-infraestructura", "name": "Infraestructura y seguridad" }
]
```

**`cliente/public/mock/users.json`**

```json
[
  { "id": "650000000000000000000001", "firstName": "Ana", "lastName": "Mora", "secondLastName": "Jiménez",
    "email": "ana.mora@ucenfotec.ac.cr", "memberType": "student", "career": "Ingeniería de Software",
    "bio": "Tercer año. Me interesa el impacto social de la tecnología.",
    "skills": [ { "skillId": "comp-javascript", "level": "advanced" }, { "skillId": "comp-gestion", "level": "intermediate" } ],
    "interests": ["comunidad", "educación"], "availability": { "hoursPerWeek": 6, "detail": "tardes y fines de semana" },
    "portfolioUrl": "https://github.com/anamora", "status": "active",
    "createdAt": "2026-09-01T14:00:00.000Z", "updatedAt": "2026-09-20T14:00:00.000Z" },

  { "id": "650000000000000000000002", "firstName": "Luis", "lastName": "Pérez", "secondLastName": null,
    "email": "luis.perez@ucenfotec.ac.cr", "memberType": "teacher", "career": "Ciberseguridad",
    "bio": "Docente de redes. Busco proyectos aplicados para mis cursos.",
    "skills": [ { "skillId": "comp-analisis-datos", "level": "advanced" }, { "skillId": "comp-redes", "level": "advanced" } ],
    "interests": ["energía", "datos"], "availability": { "hoursPerWeek": 4, "detail": "martes y jueves" },
    "portfolioUrl": null, "status": "active",
    "createdAt": "2026-09-01T14:00:00.000Z", "updatedAt": "2026-09-01T14:00:00.000Z" },

  { "id": "650000000000000000000003", "firstName": "María", "lastName": "Solís", "secondLastName": "Rojas",
    "email": "maria.solis@ucenfotec.ac.cr", "memberType": "student", "career": "Agronomía",
    "bio": "Coordino el huerto del campus.",
    "skills": [ { "skillId": "comp-gestion", "level": "intermediate" }, { "skillId": "comp-comunicacion", "level": "advanced" }, { "skillId": "comp-analisis-datos", "level": "basic" } ],
    "interests": ["ambiente", "voluntariado"], "availability": { "hoursPerWeek": 3, "detail": "mañanas" },
    "portfolioUrl": null, "status": "active",
    "createdAt": "2026-09-02T14:00:00.000Z", "updatedAt": "2026-09-02T14:00:00.000Z" },

  { "id": "650000000000000000000004", "firstName": "Diego", "lastName": "Vargas", "secondLastName": null,
    "email": "diego.vargas@ucenfotec.ac.cr", "memberType": "staff", "career": "Administración",
    "bio": "",
    "skills": [ { "skillId": "comp-marketing", "level": "advanced" }, { "skillId": "comp-diseno-ux", "level": "basic" } ],
    "interests": ["emprendimiento"], "availability": { "hoursPerWeek": 0, "detail": "" },
    "portfolioUrl": "https://www.behance.net/diegovargas", "status": "active",
    "createdAt": "2026-09-03T14:00:00.000Z", "updatedAt": "2026-09-03T14:00:00.000Z" }
]
```

**`cliente/public/mock/initiatives.json`**

El equipo va **embebido** en `members`; el propietario no se incluye porque es miembro implícito (RN-16).

```json
[
  { "id": "660000000000000000000001", "title": "Plataforma de voluntariado universitario", "type": "idea",
    "summary": "Coordinar voluntarios de distintas carreras en una sola aplicación.",
    "description": "Hoy cada facultad organiza su voluntariado por chat. Proponemos centralizar convocatorias, inscripciones y horas certificadas.",
    "problem": "Los estudiantes no encuentran oportunidades fuera de su carrera.",
    "beneficiaries": "Estudiantes y organizaciones comunitarias aliadas.",
    "categoryId": "cat-impacto-social", "requiredSkillIds": ["comp-javascript", "comp-diseno-ux", "comp-gestion"],
    "estimatedMembers": 4, "visibility": "public", "tags": ["voluntariado", "comunidad"],
    "ownerId": "650000000000000000000001",
    "members": [ { "userId": "650000000000000000000002", "roleId": "rol-backend", "joinedAt": "2026-09-12T15:00:00.000Z" } ],
    "status": "forming", "createdAt": "2026-09-08T15:00:00.000Z", "updatedAt": "2026-09-20T15:00:00.000Z" },

  { "id": "660000000000000000000002", "title": "Monitoreo de energía en laboratorios", "type": "challenge",
    "summary": "Sensores de bajo costo para medir consumo eléctrico en tiempo real.",
    "description": "Los laboratorios no saben cuánta energía consume cada equipo. Se busca un sistema de sensores accesible.",
    "problem": "Alto consumo eléctrico sin datos para optimizarlo.",
    "beneficiaries": "Escuela de Ingeniería y administración de planta física.",
    "categoryId": "cat-sostenibilidad", "requiredSkillIds": ["comp-analisis-datos", "comp-redes", "comp-javascript"],
    "estimatedMembers": 3, "visibility": "public", "tags": ["energía", "sensores"],
    "ownerId": "650000000000000000000002",
    "members": [
      { "userId": "650000000000000000000001", "roleId": "rol-frontend", "joinedAt": "2026-09-11T15:00:00.000Z" },
      { "userId": "650000000000000000000003", "roleId": "rol-datos", "joinedAt": "2026-09-11T15:00:00.000Z" } ],
    "status": "project", "createdAt": "2026-08-30T15:00:00.000Z", "updatedAt": "2026-09-20T15:00:00.000Z" },

  { "id": "660000000000000000000003", "title": "Tutorías entre pares para cálculo", "type": "need",
    "summary": "Apoyo académico entre estudiantes en cursos de matemática.",
    "description": "Muchos estudiantes reprueban cálculo por falta de acompañamiento fuera de clase.",
    "problem": "Alto índice de repitencia en cursos de cálculo.",
    "beneficiaries": "Estudiantes de primer ingreso.",
    "categoryId": "cat-educacion", "requiredSkillIds": ["comp-gestion", "comp-comunicacion"],
    "estimatedMembers": 5, "visibility": "institutional", "tags": ["educación", "tutorías"],
    "ownerId": "650000000000000000000003", "members": [],
    "status": "published", "createdAt": "2026-09-10T15:00:00.000Z", "updatedAt": "2026-09-10T15:00:00.000Z" },

  { "id": "660000000000000000000004", "title": "App de reciclaje en el campus", "type": "idea",
    "summary": "Mapa de puntos de reciclaje y guía de separación de residuos.",
    "description": "No existe un mapa claro de dónde reciclar cada tipo de material dentro del campus.",
    "problem": "Bajo aprovechamiento de los puntos de reciclaje existentes.",
    "beneficiaries": "Toda la comunidad universitaria.",
    "categoryId": "cat-sostenibilidad", "requiredSkillIds": ["comp-javascript", "comp-diseno-grafico"],
    "estimatedMembers": 3, "visibility": "public", "tags": ["ambiente", "reciclaje"],
    "ownerId": "650000000000000000000001", "members": [],
    "status": "published", "createdAt": "2026-09-14T15:00:00.000Z", "updatedAt": "2026-09-14T15:00:00.000Z" },

  { "id": "660000000000000000000005", "title": "Reducir tiempo de atención en soporte TI", "type": "challenge",
    "summary": "Optimizar el proceso de tickets del área de soporte técnico.",
    "description": "El tiempo promedio de resolución de tickets supera las 48 horas.",
    "problem": "Insatisfacción de usuarios por demoras en soporte técnico.",
    "beneficiaries": "Personal administrativo y docente.",
    "categoryId": "cat-tecnologia", "requiredSkillIds": ["comp-analisis-datos"],
    "estimatedMembers": 2, "visibility": "restricted", "tags": ["soporte", "procesos"],
    "ownerId": "650000000000000000000004", "members": [],
    "status": "published", "createdAt": "2026-09-12T15:00:00.000Z", "updatedAt": "2026-09-18T15:00:00.000Z" },

  { "id": "660000000000000000000006", "title": "Espacio de coworking para emprendedores", "type": "need",
    "summary": "Un lugar físico y una comunidad para estudiantes emprendedores.",
    "description": "Los emprendedores estudiantiles no tienen un espacio de trabajo colaborativo dentro del campus.",
    "problem": "Falta de infraestructura para el emprendimiento estudiantil.",
    "beneficiaries": "Estudiantes con proyectos propios.",
    "categoryId": "cat-negocios", "requiredSkillIds": ["comp-gestion", "comp-marketing"],
    "estimatedMembers": 6, "visibility": "public", "tags": ["emprendimiento", "comunidad"],
    "ownerId": "650000000000000000000002",
    "members": [ { "userId": "650000000000000000000004", "roleId": "rol-comunicacion", "joinedAt": "2026-09-13T15:00:00.000Z" } ],
    "status": "forming", "createdAt": "2026-09-05T15:00:00.000Z", "updatedAt": "2026-09-13T15:00:00.000Z" },

  { "id": "660000000000000000000007", "title": "Diario de bienestar estudiantil", "type": "idea",
    "summary": "Aplicación privada para registrar estado de ánimo y hábitos de estudio.",
    "description": "Herramienta de autorregistro para detectar señales tempranas de estrés académico.",
    "problem": "Poca información temprana sobre el bienestar emocional del estudiantado.",
    "beneficiaries": "Estudiantes que quieran monitorear su bienestar.",
    "categoryId": "cat-salud", "requiredSkillIds": ["comp-diseno-ux", "comp-javascript"],
    "estimatedMembers": 3, "visibility": "private", "tags": ["salud", "bienestar"],
    "ownerId": "650000000000000000000003",
    "members": [ { "userId": "650000000000000000000001", "roleId": "rol-frontend", "joinedAt": "2026-09-17T15:00:00.000Z" } ],
    "status": "forming", "createdAt": "2026-09-16T15:00:00.000Z", "updatedAt": "2026-09-19T15:00:00.000Z" },

  { "id": "660000000000000000000008", "title": "Festival de cortos universitarios", "type": "challenge",
    "summary": "Organizar una muestra anual de cortometrajes hechos por estudiantes.",
    "description": "Hay producción audiovisual estudiantil dispersa y sin un espacio para exhibirla ni criterios para premiarla.",
    "problem": "Falta de visibilidad para el trabajo creativo del estudiantado.",
    "beneficiaries": "Estudiantes de carreras creativas y comunidad universitaria.",
    "categoryId": "cat-cultura", "requiredSkillIds": ["comp-comunicacion", "comp-diseno-grafico", "comp-gestion"],
    "estimatedMembers": 4, "visibility": "public", "tags": ["cultura", "cine", "evento"],
    "ownerId": "650000000000000000000004", "members": [],
    "status": "published", "createdAt": "2026-09-21T15:00:00.000Z", "updatedAt": "2026-09-21T15:00:00.000Z" }
]
```

**`cliente/public/mock/requests.json`**

```json
[
  { "id": "670000000000000000000001", "initiativeId": "660000000000000000000002", "userId": "650000000000000000000003",
    "message": "Puedo aportar análisis de datos del consumo eléctrico.", "offeredSkillId": "comp-analisis-datos",
    "roleId": "rol-datos", "availability": "6 h/semana", "status": "accepted",
    "decisionComment": "Bienvenida al equipo.", "decidedAt": "2026-09-11T15:00:00.000Z",
    "createdAt": "2026-09-10T15:00:00.000Z", "updatedAt": "2026-09-11T15:00:00.000Z" },
  { "id": "670000000000000000000002", "initiativeId": "660000000000000000000006", "userId": "650000000000000000000004",
    "message": "Tengo experiencia en marketing para emprendimientos estudiantiles.", "offeredSkillId": "comp-marketing",
    "roleId": "rol-comunicacion", "availability": "4 h/semana", "status": "accepted",
    "decisionComment": null, "decidedAt": "2026-09-13T15:00:00.000Z",
    "createdAt": "2026-09-12T15:00:00.000Z", "updatedAt": "2026-09-13T15:00:00.000Z" },
  { "id": "670000000000000000000003", "initiativeId": "660000000000000000000001", "userId": "650000000000000000000003",
    "message": "Me gustaría ayudar con la gestión de voluntarios.", "offeredSkillId": "comp-gestion",
    "roleId": "rol-coordinacion", "availability": "3 h/semana", "status": "pending",
    "decisionComment": null, "decidedAt": null,
    "createdAt": "2026-09-16T15:00:00.000Z", "updatedAt": "2026-09-16T15:00:00.000Z" },
  { "id": "670000000000000000000004", "initiativeId": "660000000000000000000002", "userId": "650000000000000000000001",
    "message": "Puedo construir el tablero web con JavaScript.", "offeredSkillId": "comp-javascript",
    "roleId": "rol-frontend", "availability": "5 h/semana", "status": "accepted",
    "decisionComment": null, "decidedAt": "2026-09-11T15:00:00.000Z",
    "createdAt": "2026-09-10T16:00:00.000Z", "updatedAt": "2026-09-11T15:00:00.000Z" },
  { "id": "670000000000000000000005", "initiativeId": "660000000000000000000006", "userId": "650000000000000000000003",
    "message": "Quisiera coordinar las actividades del espacio.", "offeredSkillId": "comp-gestion",
    "roleId": "rol-coordinacion", "availability": "3 h/semana", "status": "rejected",
    "decisionComment": "Por ahora ya cubrimos la coordinación.", "decidedAt": "2026-09-15T15:00:00.000Z",
    "createdAt": "2026-09-14T15:00:00.000Z", "updatedAt": "2026-09-15T15:00:00.000Z" }
]
```

**`cliente/public/mock/projects.json`**

```json
[
  { "id": "680000000000000000000001", "initiativeId": "660000000000000000000002",
    "name": "Monitoreo de energía en laboratorios",
    "objective": "Medir el consumo de cada laboratorio y reducirlo un 15 % en un semestre.",
    "description": "Red de sensores de bajo costo, tablero web en tiempo real y alertas de consumo anómalo.",
    "technologies": ["Arduino", "Node.js", "Grafana"],
    "startDate": "2026-09-20", "estimatedEndDate": "2026-12-15",
    "repositoryUrl": "https://github.com/twbs/bootstrap", "status": "in_progress",
    "createdAt": "2026-09-20T15:00:00.000Z", "updatedAt": "2026-09-20T15:00:00.000Z" }
]
```

```powershell
git add cliente\public\mock
git commit -m "Traducir los datos del Avance 1 al formato del contrato" --trailer "Seccion: Datos" --trailer "Cambio: JSON con enums en inglés, equipo embebido y catálogo de roles"
```

### 1.2 Reglas de negocio y servidor simulado

El simulado responde las mismas rutas, formatos y códigos que tendrá Express. Las reglas se escriben como funciones puras para copiarlas al servidor el día 7 sin cambios.

**`cliente/src/api/errores.js`**

```js
// Error uniforme de la API: mismo formato que { error: { code, message, details } }.
export class ErrorApi extends Error {
  constructor(status, code, message, details = []) {
    super(message);
    this.name = 'ErrorApi';
    this.status = status;
    this.code = code;
    this.details = details;
  }
}
```

**`cliente/src/mock/reglas.js`**

```js
// Reglas de negocio puras. El día 7 se copian a servidor/src/services/reglas.js.
// String() permite comparar ids de texto (simulado) y ObjectId (MongoDB).
const mismoId = (a, b) => a != null && b != null && String(a) === String(b);

export const ESTADOS_QUE_RECIBEN = ['published', 'forming'];

export const esPropietario = (ini, usuario) => !!usuario && mismoId(ini.ownerId, usuario.id);
export const esMiembro = (ini, usuario) => !!usuario && ini.members.some((m) => mismoId(m.userId, usuario.id));

// RN-16: el propietario cuenta como integrante sin solicitud.
export const integrantes = (ini) => ini.members.length + 1;
export const espaciosDisponibles = (ini) => Math.max(0, ini.estimatedMembers - integrantes(ini));

// RN-20 y los cuatro niveles de visibilidad.
// 'full' ve todo · 'summary' solo resumen · 'login' debe autenticarse · 'none' no existe para él
export function nivelDeAcceso(ini, usuario) {
  if (esPropietario(ini, usuario) || esMiembro(ini, usuario)) return 'full';
  if (ini.visibility === 'public') return 'full';
  if (ini.visibility === 'institutional') return usuario ? 'full' : 'login';
  if (ini.visibility === 'restricted') return 'summary';
  return 'none';
}

const conflicto = (code, message) => ({ status: 409, code, message });

// Orden fijo: primero quién es, luego el estado, luego el cupo, luego duplicados.
// Así una iniciativa convertida en proyecto nunca dice "sin cupo".
export function verificarSolicitud(ini, usuario, pendientesDelUsuario) {
  if (esPropietario(ini, usuario)) return conflicto('SELF_REQUEST', 'No podés solicitar participación en tu propia iniciativa.');
  if (esMiembro(ini, usuario)) return conflicto('ALREADY_MEMBER', 'Ya formás parte del equipo de esta iniciativa.');
  if (!ESTADOS_QUE_RECIBEN.includes(ini.status)) return conflicto('INITIATIVE_CLOSED', 'Esta iniciativa ya no recibe solicitudes.');
  if (espaciosDisponibles(ini) === 0) return conflicto('NO_SPOTS', 'La iniciativa ya completó los espacios previstos.');
  if (pendientesDelUsuario > 0) return conflicto('DUPLICATE_PENDING', 'Ya tenés una solicitud pendiente para esta iniciativa.');
  return null;
}

// RN-17 y RN-18.
export function verificarConversion(ini) {
  if (ini.status === 'project') return conflicto('ALREADY_PROJECT', 'Esta iniciativa ya se convirtió en proyecto.');
  if (ini.status === 'archived') return conflicto('ARCHIVED', 'Una iniciativa archivada no puede convertirse en proyecto.');
  if (ini.members.length < 1) return conflicto('NO_MEMBERS', 'Se necesita al menos un integrante además del propietario.');
  return null;
}

// Competencias que el usuario puede ofrecer: las que pide la iniciativa y él tiene.
export function competenciasOfrecibles(ini, usuario) {
  const propias = new Set(usuario.skills.map((s) => String(s.skillId)));
  return ini.requiredSkillIds.map(String).filter((id) => propias.has(id));
}
```

**`cliente/src/mock/serializar.js`**

Convierte documentos internos en respuestas del contrato. Es lo que impide que se filtren campos: una iniciativa restringida vista por un tercero sale **sin** propietario, fechas, estado, equipo ni descripción.

```js
import { nivelDeAcceso, esPropietario, integrantes, espaciosDisponibles } from './reglas.js';

export const nombreCompleto = (u) => [u.firstName, u.lastName, u.secondLastName].filter(Boolean).join(' ');
const porId = (lista, id) => lista.find((x) => x.id === id);
const resumenUsuario = (db, id) => {
  const u = porId(db.users, id);
  return { id, fullName: u ? nombreCompleto(u) : 'Usuario no disponible', career: u?.career ?? '' };
};
const competencia = (db, id) => ({ id, name: porId(db.skills, id)?.name ?? id });
const rol = (db, id) => ({ id, name: porId(db.roles, id)?.name ?? id });

export function serializarUsuarioPublico(u, db) {
  return {
    id: u.id, firstName: u.firstName, lastName: u.lastName, secondLastName: u.secondLastName,
    fullName: nombreCompleto(u), memberType: u.memberType, career: u.career, bio: u.bio,
    skills: u.skills.map((s) => ({ skillId: s.skillId, name: competencia(db, s.skillId).name, level: s.level })),
    interests: u.interests, availability: u.availability, portfolioUrl: u.portfolioUrl,
  };
}

export const serializarUsuarioPropio = (u, db) => ({
  ...serializarUsuarioPublico(u, db), email: u.email, status: u.status, createdAt: u.createdAt, updatedAt: u.updatedAt,
});

export function serializarIniciativa(ini, db, usuario, { detalle = false } = {}) {
  const categoria = porId(db.categories, ini.categoryId);
  const base = {
    id: ini.id,
    accessLevel: nivelDeAcceso(ini, usuario) === 'full' ? 'full' : 'summary',
    title: ini.title, type: ini.type, summary: ini.summary,
    category: categoria ? { id: categoria.id, name: categoria.name, icon: categoria.icon } : null,
    requiredSkills: ini.requiredSkillIds.map((id) => competencia(db, id)),
    visibility: ini.visibility,
  };
  if (base.accessLevel !== 'full') return base;

  const proyecto = db.projects.find((p) => p.initiativeId === ini.id);
  const propietario = resumenUsuario(db, ini.ownerId);
  const completo = {
    ...base,
    tags: ini.tags, status: ini.status, estimatedMembers: ini.estimatedMembers,
    memberCount: integrantes(ini), availableSpots: espaciosDisponibles(ini),
    owner: { id: propietario.id, fullName: propietario.fullName },
    projectId: proyecto?.id ?? null, createdAt: ini.createdAt, updatedAt: ini.updatedAt,
  };
  if (detalle) {
    completo.description = ini.description;
    completo.problem = ini.problem;
    completo.beneficiaries = ini.beneficiaries;
    completo.members = ini.members.map((m) => {
      const u = resumenUsuario(db, m.userId);
      return { userId: m.userId, fullName: u.fullName, career: u.career, role: rol(db, m.roleId), joinedAt: m.joinedAt };
    });
  }
  if (esPropietario(ini, usuario)) {
    completo.relations = {
      members: ini.members.length,
      requests: db.requests.filter((r) => r.initiativeId === ini.id).length,
      hasProject: !!proyecto,
    };
  }
  return completo;
}

export function serializarSolicitud(r, db) {
  const ini = porId(db.initiatives, r.initiativeId);
  const solicitante = porId(db.users, r.userId);
  return {
    id: r.id,
    initiative: { id: r.initiativeId, title: ini?.title ?? 'Iniciativa eliminada' },
    applicant: solicitante
      ? { id: solicitante.id, fullName: nombreCompleto(solicitante), career: solicitante.career,
          skills: serializarUsuarioPublico(solicitante, db).skills }
      : { id: r.userId, fullName: 'Usuario no disponible', career: '', skills: [] },
    message: r.message, offeredSkill: competencia(db, r.offeredSkillId), role: rol(db, r.roleId),
    availability: r.availability, status: r.status,
    decision: r.decidedAt ? { comment: r.decisionComment, decidedAt: r.decidedAt } : null,
    createdAt: r.createdAt, updatedAt: r.updatedAt,
  };
}

export function serializarProyecto(p, db, { detalle = false } = {}) {
  const ini = porId(db.initiatives, p.initiativeId);
  const propietario = resumenUsuario(db, ini.ownerId);
  const proyecto = {
    id: p.id, initiative: { id: ini.id, title: ini.title },
    name: p.name, objective: p.objective, description: p.description, technologies: p.technologies,
    startDate: p.startDate, estimatedEndDate: p.estimatedEndDate, status: p.status, repositoryUrl: p.repositoryUrl,
    owner: { id: propietario.id, fullName: propietario.fullName }, memberCount: integrantes(ini),
    createdAt: p.createdAt, updatedAt: p.updatedAt,
  };
  if (detalle) {
    proyecto.members = ini.members.map((m) => {
      const u = resumenUsuario(db, m.userId);
      return { userId: m.userId, fullName: u.fullName, career: u.career, role: rol(db, m.roleId), joinedAt: m.joinedAt };
    });
  }
  return proyecto;
}
```

**`cliente/src/mock/servidorMock.js`**

Temporal: se borra el día 9. Guarda los cambios en `localStorage` solo para que el prototipo recuerde lo hecho mientras no hay base de datos.

```js
import { ErrorApi } from '../api/errores.js';
import {
  nivelDeAcceso, esPropietario, esMiembro, integrantes, espaciosDisponibles,
  verificarSolicitud, verificarConversion, competenciasOfrecibles,
} from './reglas.js';
import {
  nombreCompleto, serializarIniciativa, serializarSolicitud, serializarProyecto,
  serializarUsuarioPublico, serializarUsuarioPropio,
} from './serializar.js';

const CLAVE = 'ih.mock.v1';
const COLECCIONES = ['categories', 'skills', 'roles', 'users', 'initiatives', 'requests', 'projects'];
let db = null;
let cargando = null;

// Se guarda la promesa: si llegan varias peticiones a la vez, los JSON se descargan una sola vez.
function cargar() {
  cargando ??= (async () => {
    const guardado = localStorage.getItem(CLAVE);
    if (guardado) { db = JSON.parse(guardado); return; }
    const partes = await Promise.all(COLECCIONES.map(async (nombre) => {
      const respuesta = await fetch(`/mock/${nombre}.json`);
      if (!respuesta.ok) throw new ErrorApi(500, 'INTERNAL_ERROR', `No se pudo cargar ${nombre}.json`);
      return respuesta.json();
    }));
    db = Object.fromEntries(COLECCIONES.map((nombre, i) => [nombre, partes[i]]));
  })().catch((error) => { cargando = null; throw error; });
  return cargando;
}
const guardar = () => localStorage.setItem(CLAVE, JSON.stringify(db));
export function reiniciarDatosMock() { localStorage.removeItem(CLAVE); db = null; cargando = null; }

const ahora = () => new Date().toISOString();
const nuevoId = () => Array.from(crypto.getRandomValues(new Uint8Array(12)), (b) => b.toString(16).padStart(2, '0')).join('');
const fallo = (status, code, message, details = []) => new ErrorApi(status, code, message, details);

// ---------- utilidades comunes ----------
function exigirUsuario(usuario) {
  if (!usuario) throw fallo(401, 'UNAUTHORIZED', 'Necesitás iniciar sesión.');
}
function requeridos(cuerpo, campos) {
  const vacio = (v) => v === undefined || v === null || v === '' || (Array.isArray(v) && v.length === 0);
  const details = campos.filter((c) => vacio(cuerpo[c])).map((field) => ({ field, message: 'Este campo es obligatorio.' }));
  if (details.length) throw fallo(400, 'VALIDATION_ERROR', 'Hay datos inválidos.', details);
}
function iniciativaVisible(id, usuario) {
  const ini = db.initiatives.find((i) => i.id === id);
  const nivel = ini ? nivelDeAcceso(ini, usuario) : 'none';
  if (nivel === 'none') throw fallo(404, 'NOT_FOUND', 'La iniciativa no existe.');
  if (nivel === 'login') throw fallo(401, 'UNAUTHORIZED', 'Esta iniciativa es institucional: iniciá sesión para verla.');
  return { ini, nivel };
}
function iniciativaPropia(id, usuario) {
  exigirUsuario(usuario);
  const { ini } = iniciativaVisible(id, usuario);
  if (!esPropietario(ini, usuario)) throw fallo(403, 'FORBIDDEN', 'Solo el propietario puede realizar esta acción.');
  return ini;
}
function paginar(lista, query) {
  const size = Math.min(50, Math.max(1, parseInt(query.size, 10) || 12));
  const page = Math.max(1, parseInt(query.page, 10) || 1);
  const totalItems = lista.length;
  return { items: lista.slice((page - 1) * size, page * size), page, size, totalItems, totalPages: Math.max(1, Math.ceil(totalItems / size)) };
}
function ordenar(lista, sort, permitidos, porDefecto) {
  const valor = sort && permitidos.includes(sort.replace(/^-/, '')) ? sort : porDefecto;
  const campo = valor.replace(/^-/, '');
  const direccion = valor.startsWith('-') ? -1 : 1;
  return [...lista].sort((a, b) => String(a[campo]).localeCompare(String(b[campo]), 'es') * direccion);
}
function validarSolicitudContenido(cuerpo, ini, usuario) {
  requeridos(cuerpo, ['message', 'offeredSkillId', 'roleId', 'availability']);
  const details = [];
  if (!competenciasOfrecibles(ini, usuario).includes(cuerpo.offeredSkillId)) {
    details.push({ field: 'offeredSkillId', message: 'Elegí una competencia que la iniciativa requiera y que tengas en tu perfil.' });
  }
  if (!db.roles.some((r) => r.id === cuerpo.roleId)) details.push({ field: 'roleId', message: 'Elegí un rol válido.' });
  if (details.length) throw fallo(400, 'VALIDATION_ERROR', 'Hay datos inválidos.', details);
}

// ---------- iniciativas ----------
const CAMPOS_INICIATIVA = ['title', 'type', 'summary', 'description', 'problem', 'beneficiaries', 'categoryId', 'requiredSkillIds', 'estimatedMembers', 'visibility'];

function listarIniciativas({ query, usuario }) {
  let lista = db.initiatives.filter((i) => nivelDeAcceso(i, usuario) !== 'none');
  if (query.owner === 'me') lista = lista.filter((i) => esPropietario(i, usuario));
  else if (query.owner) lista = lista.filter((i) => i.ownerId === query.owner);
  if (query.member === 'me') lista = lista.filter((i) => esMiembro(i, usuario));
  const soloMias = query.owner === 'me' || query.member === 'me';
  if (query.status) lista = lista.filter((i) => i.status === query.status);
  else if (!soloMias) lista = lista.filter((i) => !['draft', 'archived'].includes(i.status));
  if (query.type) lista = lista.filter((i) => i.type === query.type);
  if (query.category) lista = lista.filter((i) => i.categoryId === query.category);
  if (query.skill) lista = lista.filter((i) => i.requiredSkillIds.includes(query.skill));
  if (query.hasSpots === 'true') lista = lista.filter((i) => espaciosDisponibles(i) > 0);
  const texto = query.q?.trim().toLowerCase();
  if (texto) lista = lista.filter((i) => [i.title, i.summary, ...i.tags].some((t) => t.toLowerCase().includes(texto)));
  lista = ordenar(lista, query.sort, ['createdAt', 'updatedAt', 'title'], '-createdAt');
  const pagina = paginar(lista, query);
  return { ...pagina, items: pagina.items.map((i) => serializarIniciativa(i, db, usuario)) };
}

function obtenerIniciativa({ params: [id], usuario }) {
  const { ini } = iniciativaVisible(id, usuario);
  return serializarIniciativa(ini, db, usuario, { detalle: true });
}

function crearIniciativa({ cuerpo, usuario }) {
  exigirUsuario(usuario);
  requeridos(cuerpo, CAMPOS_INICIATIVA);
  const ini = {
    id: nuevoId(), ...Object.fromEntries(CAMPOS_INICIATIVA.map((c) => [c, cuerpo[c]])),
    tags: cuerpo.tags ?? [], ownerId: usuario.id, members: [], status: 'published', createdAt: ahora(), updatedAt: ahora(),
  };
  db.initiatives.push(ini);
  guardar();
  return serializarIniciativa(ini, db, usuario, { detalle: true });
}

function actualizarIniciativa({ params: [id], cuerpo, usuario }) {
  const ini = iniciativaPropia(id, usuario);
  if (ini.status === 'archived') throw fallo(409, 'ARCHIVED', 'Una iniciativa archivada no se puede modificar.');
  requeridos(cuerpo, CAMPOS_INICIATIVA);
  if (Number(cuerpo.estimatedMembers) < integrantes(ini)) {
    throw fallo(409, 'PARTICIPANTS_BELOW_TEAM', `El equipo ya tiene ${integrantes(ini)} integrantes, incluido el propietario.`);
  }
  CAMPOS_INICIATIVA.forEach((c) => { ini[c] = cuerpo[c]; });
  ini.tags = cuerpo.tags ?? [];
  ini.updatedAt = ahora();
  guardar();
  return serializarIniciativa(ini, db, usuario, { detalle: true });
}

function eliminarIniciativa({ params: [id], usuario }) {
  const ini = iniciativaPropia(id, usuario);
  const tieneRelaciones = ini.members.length > 0
    || db.requests.some((r) => r.initiativeId === id)
    || db.projects.some((p) => p.initiativeId === id);
  if (tieneRelaciones) throw fallo(409, 'HAS_RELATED_DATA', 'La iniciativa tiene equipo, solicitudes o proyecto: archivala en lugar de eliminarla.');
  db.initiatives = db.initiatives.filter((i) => i.id !== id);
  guardar();
}

function archivarIniciativa({ params: [id], usuario }) {
  const ini = iniciativaPropia(id, usuario);
  if (ini.status === 'archived') throw fallo(409, 'ARCHIVED', 'La iniciativa ya está archivada.');
  ini.status = 'archived';
  ini.updatedAt = ahora();
  guardar();
  return serializarIniciativa(ini, db, usuario, { detalle: true });
}

function listarMiembros({ params: [id], query, usuario }) {
  const { ini, nivel } = iniciativaVisible(id, usuario);
  if (nivel !== 'full') throw fallo(403, 'FORBIDDEN', 'No tenés acceso al equipo de esta iniciativa.');
  let miembros = serializarIniciativa(ini, db, usuario, { detalle: true }).members;
  if (query.role) miembros = miembros.filter((m) => m.role.id === query.role);
  if (query.skill) {
    miembros = miembros.filter((m) => db.users.find((u) => u.id === m.userId)?.skills.some((s) => s.skillId === query.skill));
  }
  return miembros;
}

function retirarMiembro({ params: [id, userId], usuario }) {
  const ini = iniciativaPropia(id, usuario);
  if (userId === ini.ownerId) throw fallo(409, 'CANNOT_REMOVE_OWNER', 'El propietario no puede retirarse del equipo.');
  if (!ini.members.some((m) => m.userId === userId)) throw fallo(404, 'NOT_FOUND', 'Esa persona no es miembro del equipo.');
  ini.members = ini.members.filter((m) => m.userId !== userId);
  if (ini.status === 'forming' && ini.members.length === 0) ini.status = 'published';
  ini.updatedAt = ahora();
  guardar();
}

function convertirEnProyecto({ params: [id], cuerpo, usuario }) {
  const ini = iniciativaPropia(id, usuario);
  const regla = verificarConversion(ini);
  if (regla) throw fallo(regla.status, regla.code, regla.message);
  requeridos(cuerpo, ['name', 'objective', 'description', 'startDate']);
  const proyecto = {
    id: nuevoId(), initiativeId: ini.id, name: cuerpo.name, objective: cuerpo.objective, description: cuerpo.description,
    technologies: cuerpo.technologies ?? [], startDate: cuerpo.startDate, estimatedEndDate: cuerpo.estimatedEndDate || null,
    repositoryUrl: cuerpo.repositoryUrl || null, status: 'planning', createdAt: ahora(), updatedAt: ahora(),
  };
  db.projects.push(proyecto);
  ini.status = 'project';
  ini.updatedAt = ahora();
  guardar();
  return serializarProyecto(proyecto, db, { detalle: true });
}

// ---------- solicitudes ----------
function crearSolicitud({ params: [id], cuerpo, usuario }) {
  exigirUsuario(usuario);
  const { ini } = iniciativaVisible(id, usuario);
  const pendientes = db.requests.filter((r) => r.initiativeId === id && r.userId === usuario.id && r.status === 'pending').length;
  const regla = verificarSolicitud(ini, usuario, pendientes);
  if (regla) throw fallo(regla.status, regla.code, regla.message);
  validarSolicitudContenido(cuerpo, ini, usuario);
  const solicitud = {
    id: nuevoId(), initiativeId: id, userId: usuario.id, message: cuerpo.message, offeredSkillId: cuerpo.offeredSkillId,
    roleId: cuerpo.roleId, availability: cuerpo.availability, status: 'pending', decisionComment: null, decidedAt: null,
    createdAt: ahora(), updatedAt: ahora(),
  };
  db.requests.push(solicitud);
  guardar();
  return serializarSolicitud(solicitud, db);
}

function filtrarSolicitudes(lista, query) {
  if (query.status) lista = lista.filter((r) => r.status === query.status);
  if (query.initiativeId) lista = lista.filter((r) => r.initiativeId === query.initiativeId);
  if (query.skill) lista = lista.filter((r) => r.offeredSkillId === query.skill);
  if (query.role) lista = lista.filter((r) => r.roleId === query.role);
  if (query.from) lista = lista.filter((r) => r.createdAt.slice(0, 10) >= query.from);
  if (query.to) lista = lista.filter((r) => r.createdAt.slice(0, 10) <= query.to);
  return ordenar(lista, query.sort, ['createdAt', 'updatedAt'], '-createdAt');
}

function misSolicitudes({ query, usuario }) {
  exigirUsuario(usuario);
  const pagina = paginar(filtrarSolicitudes(db.requests.filter((r) => r.userId === usuario.id), query), query);
  return { ...pagina, items: pagina.items.map((r) => serializarSolicitud(r, db)) };
}

function solicitudesRecibidas({ query, usuario }) {
  exigirUsuario(usuario);
  const propias = new Set(db.initiatives.filter((i) => i.ownerId === usuario.id).map((i) => i.id));
  const pagina = paginar(filtrarSolicitudes(db.requests.filter((r) => propias.has(r.initiativeId)), query), query);
  return { ...pagina, items: pagina.items.map((r) => serializarSolicitud(r, db)) };
}

function buscarSolicitud(id, usuario) {
  exigirUsuario(usuario);
  const solicitud = db.requests.find((r) => r.id === id);
  if (!solicitud) throw fallo(404, 'NOT_FOUND', 'La solicitud no existe.');
  const ini = db.initiatives.find((i) => i.id === solicitud.initiativeId);
  return { solicitud, ini, esSolicitante: solicitud.userId === usuario.id, esDueno: esPropietario(ini, usuario) };
}

function obtenerSolicitud({ params: [id], usuario }) {
  const { solicitud, esSolicitante, esDueno } = buscarSolicitud(id, usuario);
  if (!esSolicitante && !esDueno) throw fallo(403, 'FORBIDDEN', 'No tenés acceso a esta solicitud.');
  return serializarSolicitud(solicitud, db);
}

function solicitudPropiaPendiente(id, usuario) {
  const datos = buscarSolicitud(id, usuario);
  if (!datos.esSolicitante) throw fallo(403, 'FORBIDDEN', 'Solo quien envió la solicitud puede modificarla.');
  if (datos.solicitud.status !== 'pending') throw fallo(409, 'NOT_PENDING', 'Solo las solicitudes pendientes pueden modificarse.');
  return datos;
}

function actualizarSolicitud({ params: [id], cuerpo, usuario }) {
  const { solicitud, ini } = solicitudPropiaPendiente(id, usuario);
  validarSolicitudContenido(cuerpo, ini, usuario);
  Object.assign(solicitud, {
    message: cuerpo.message, offeredSkillId: cuerpo.offeredSkillId, roleId: cuerpo.roleId,
    availability: cuerpo.availability, updatedAt: ahora(),
  });
  guardar();
  return serializarSolicitud(solicitud, db);
}

function cancelarSolicitud({ params: [id], usuario }) {
  const { solicitud } = solicitudPropiaPendiente(id, usuario);
  solicitud.status = 'cancelled';
  solicitud.updatedAt = ahora();
  guardar();
  return serializarSolicitud(solicitud, db);
}

function decidirSolicitud({ params: [id], cuerpo, usuario }) {
  const { solicitud, ini, esDueno } = buscarSolicitud(id, usuario);
  if (!esDueno) throw fallo(403, 'FORBIDDEN', 'Solo el propietario de la iniciativa puede resolver solicitudes.');
  if (!['accepted', 'rejected'].includes(cuerpo.decision)) {
    throw fallo(400, 'VALIDATION_ERROR', 'Hay datos inválidos.', [{ field: 'decision', message: 'La decisión debe ser accepted o rejected.' }]);
  }
  if (solicitud.status !== 'pending') throw fallo(409, 'NOT_PENDING', 'La solicitud ya fue resuelta o cancelada.');
  if (cuerpo.decision === 'accepted') {
    if (!['published', 'forming'].includes(ini.status)) throw fallo(409, 'INITIATIVE_CLOSED', 'La iniciativa ya no recibe integrantes.');
    if (espaciosDisponibles(ini) === 0) throw fallo(409, 'NO_SPOTS', 'La iniciativa ya completó los espacios previstos.');
    // RN-14: aceptar incorpora automáticamente al equipo.
    ini.members.push({ userId: solicitud.userId, roleId: solicitud.roleId, joinedAt: ahora() });
    if (ini.status === 'published') ini.status = 'forming';
    ini.updatedAt = ahora();
  }
  Object.assign(solicitud, { status: cuerpo.decision, decisionComment: cuerpo.comment || null, decidedAt: ahora(), updatedAt: ahora() });
  guardar();
  return serializarSolicitud(solicitud, db);
}

// ---------- proyectos ----------
function proyectosAccesibles(usuario) {
  return db.projects.filter((p) => {
    const ini = db.initiatives.find((i) => i.id === p.initiativeId);
    return ini && nivelDeAcceso(ini, usuario) === 'full';
  });
}

function listarProyectos({ query, usuario }) {
  exigirUsuario(usuario);
  let lista = proyectosAccesibles(usuario);
  if (query.status) lista = lista.filter((p) => p.status === query.status);
  const pagina = paginar(ordenar(lista, query.sort, ['createdAt', 'name', 'startDate'], '-createdAt'), query);
  return { ...pagina, items: pagina.items.map((p) => serializarProyecto(p, db)) };
}

function obtenerProyecto({ params: [id], usuario }) {
  exigirUsuario(usuario);
  const proyecto = proyectosAccesibles(usuario).find((p) => p.id === id);
  if (!proyecto) throw fallo(404, 'NOT_FOUND', 'El proyecto no existe o no tenés acceso.');
  return serializarProyecto(proyecto, db, { detalle: true });
}

// ---------- usuarios ----------
const CAMPOS_PERFIL = ['firstName', 'lastName', 'secondLastName', 'career', 'bio', 'skills', 'interests', 'availability', 'portfolioUrl'];

function actualizarPerfil({ cuerpo, usuario }) {
  exigirUsuario(usuario);
  if (cuerpo.email !== undefined && cuerpo.email !== usuario.email) {
    throw fallo(400, 'VALIDATION_ERROR', 'Hay datos inválidos.', [{ field: 'email', message: 'El correo no se puede modificar desde el perfil.' }]);
  }
  requeridos(cuerpo, ['firstName', 'lastName', 'career']);
  CAMPOS_PERFIL.forEach((c) => { if (cuerpo[c] !== undefined) usuario[c] = cuerpo[c]; });
  usuario.updatedAt = ahora();
  guardar();
  return serializarUsuarioPropio(usuario, db);
}

function buscarUsuarios({ query, usuario }) {
  exigirUsuario(usuario);
  let lista = db.users.filter((u) => u.status === 'active');
  const texto = query.q?.trim().toLowerCase();
  if (texto) lista = lista.filter((u) => nombreCompleto(u).toLowerCase().includes(texto));
  if (query.skill) lista = lista.filter((u) => u.skills.some((s) => s.skillId === query.skill));
  if (query.career) lista = lista.filter((u) => u.career.toLowerCase().includes(query.career.trim().toLowerCase()));
  if (query.available === 'true') lista = lista.filter((u) => u.availability.hoursPerWeek > 0);
  const pagina = paginar(ordenar(lista, query.sort, ['firstName', 'lastName', 'createdAt'], 'firstName'), query);
  return { ...pagina, items: pagina.items.map((u) => serializarUsuarioPublico(u, db)) };
}

function obtenerUsuario({ params: [id], usuario }) {
  exigirUsuario(usuario);
  const encontrado = db.users.find((u) => u.id === id && u.status === 'active');
  if (!encontrado) throw fallo(404, 'NOT_FOUND', 'La persona no existe.');
  return serializarUsuarioPublico(encontrado, db);
}

// ---------- tabla de rutas: mismo verbo y ruta que el contrato ----------
const RUTAS = [
  ['GET', /^\/dev\/users$/, () => db.users.map((u) => ({ id: u.id, fullName: nombreCompleto(u) }))],
  ['GET', /^\/categories$/, () => db.categories],
  ['GET', /^\/skills$/, () => db.skills],
  ['GET', /^\/roles$/, () => db.roles],
  ['GET', /^\/auth\/me$/, ({ usuario }) => { exigirUsuario(usuario); return serializarUsuarioPropio(usuario, db); }],
  ['GET', /^\/users$/, buscarUsuarios],
  ['PUT', /^\/users\/me$/, actualizarPerfil],
  ['GET', /^\/users\/([^/]+)$/, obtenerUsuario],
  ['GET', /^\/initiatives$/, listarIniciativas],
  ['POST', /^\/initiatives$/, crearIniciativa],
  ['GET', /^\/initiatives\/([^/]+)$/, obtenerIniciativa],
  ['PUT', /^\/initiatives\/([^/]+)$/, actualizarIniciativa],
  ['DELETE', /^\/initiatives\/([^/]+)$/, eliminarIniciativa],
  ['POST', /^\/initiatives\/([^/]+)\/archive$/, archivarIniciativa],
  ['GET', /^\/initiatives\/([^/]+)\/members$/, listarMiembros],
  ['DELETE', /^\/initiatives\/([^/]+)\/members\/([^/]+)$/, retirarMiembro],
  ['POST', /^\/initiatives\/([^/]+)\/project$/, convertirEnProyecto],
  ['POST', /^\/initiatives\/([^/]+)\/requests$/, crearSolicitud],
  ['GET', /^\/requests\/mine$/, misSolicitudes],
  ['GET', /^\/requests\/received$/, solicitudesRecibidas],
  ['GET', /^\/requests\/([^/]+)$/, obtenerSolicitud],
  ['PUT', /^\/requests\/([^/]+)$/, actualizarSolicitud],
  ['POST', /^\/requests\/([^/]+)\/cancel$/, cancelarSolicitud],
  ['POST', /^\/requests\/([^/]+)\/decision$/, decidirSolicitud],
  ['GET', /^\/projects$/, listarProyectos],
  ['GET', /^\/projects\/([^/]+)$/, obtenerProyecto],
];

export async function atenderMock(metodo, url, cuerpo, cabeceras) {
  await cargar();
  await new Promise((resolver) => setTimeout(resolver, 250)); // latencia para ver el estado de carga
  const [ruta, cadena = ''] = url.split('?');
  const query = Object.fromEntries(new URLSearchParams(cadena));
  const usuario = db.users.find((u) => u.id === cabeceras['X-Dev-User']) ?? null;
  for (const [verbo, patron, manejador] of RUTAS) {
    const coincidencia = verbo === metodo ? ruta.match(patron) : null;
    if (coincidencia) {
      const resultado = manejador({ params: coincidencia.slice(1), query, cuerpo: structuredClone(cuerpo ?? {}), usuario });
      return resultado === undefined ? null : structuredClone(resultado);
    }
  }
  throw fallo(404, 'NOT_FOUND', 'La ruta no existe.');
}
```

```powershell
git add cliente\src\api\errores.js cliente\src\mock
git commit -m "Agregar reglas de negocio puras y servidor simulado con las rutas del contrato" --trailer "Seccion: Cliente" --trailer "Cambio: Simulado temporal con visibilidad, solicitudes y proyectos"
```

### 1.3 Variables de entorno y capa `api/`

**`cliente/.env.example`** (se versiona) y copia local **`cliente/.env`** (no se versiona)

```
VITE_API_URL=http://localhost:3000/api
VITE_USE_MOCK=true
VITE_GITHUB_API_URL=https://api.github.com
```

```powershell
Copy-Item cliente\.env.example cliente\.env
```

**`cliente/src/api/sesion.js`**

```js
// Credencial que acompaña cada petición.
// Hasta el día 10 es el usuario elegido en "Ver como"; ese día se reemplaza por el token JWT.
const CLAVE = 'ih.devUser';

export const obtenerUsuarioDev = () => sessionStorage.getItem(CLAVE) || '';

export function fijarUsuarioDev(id) {
  if (id) sessionStorage.setItem(CLAVE, id);
  else sessionStorage.removeItem(CLAVE);
}

export function cabecerasAuth() {
  const id = obtenerUsuarioDev();
  return id ? { 'X-Dev-User': id } : {};
}
```

**`cliente/src/api/cliente.js`**

Único lugar que conoce la dirección del servidor. Las pantallas nunca llaman `fetch` directamente.

```js
import { ErrorApi } from './errores.js';
import { cabecerasAuth } from './sesion.js';

const BASE = import.meta.env.VITE_API_URL;
const USAR_MOCK = import.meta.env.VITE_USE_MOCK === 'true';

function construirQuery(params) {
  if (!params) return '';
  const definidos = Object.entries(params).filter(([, v]) => v !== undefined && v !== null && v !== '');
  return definidos.length ? `?${new URLSearchParams(definidos)}` : '';
}

export async function solicitar(metodo, ruta, { cuerpo, params, signal } = {}) {
  const url = ruta + construirQuery(params);

  if (USAR_MOCK) {
    const { atenderMock } = await import('../mock/servidorMock.js');
    return atenderMock(metodo, url, cuerpo, cabecerasAuth());
  }

  let respuesta;
  try {
    respuesta = await fetch(BASE + url, {
      method: metodo,
      headers: { 'Content-Type': 'application/json', ...cabecerasAuth() },
      body: cuerpo === undefined ? undefined : JSON.stringify(cuerpo),
      signal,
    });
  } catch (error) {
    if (error.name === 'AbortError') throw error;
    throw new ErrorApi(0, 'NETWORK_ERROR', 'No hay conexión con el servidor.');
  }

  if (respuesta.status === 204) return null;
  const datos = await respuesta.json().catch(() => null);
  if (!respuesta.ok) {
    const e = datos?.error;
    throw new ErrorApi(respuesta.status, e?.code ?? 'UNKNOWN_ERROR', e?.message ?? 'Ocurrió un error inesperado.', e?.details ?? []);
  }
  return datos;
}
```

**`cliente/src/api/catalogos.js`**

```js
import { solicitar } from './cliente.js';

export const listarCategorias = (signal) => solicitar('GET', '/categories', { signal });
export const listarCompetencias = (signal) => solicitar('GET', '/skills', { signal });
export const listarRoles = (signal) => solicitar('GET', '/roles', { signal });
```

**`cliente/src/api/iniciativas.js`**

```js
import { solicitar } from './cliente.js';

const ruta = (id) => `/initiatives/${encodeURIComponent(id)}`;

export const listarIniciativas = (params, signal) => solicitar('GET', '/initiatives', { params, signal });
export const obtenerIniciativa = (id, signal) => solicitar('GET', ruta(id), { signal });
export const crearIniciativa = (datos) => solicitar('POST', '/initiatives', { cuerpo: datos });
export const actualizarIniciativa = (id, datos) => solicitar('PUT', ruta(id), { cuerpo: datos });
export const eliminarIniciativa = (id) => solicitar('DELETE', ruta(id));
export const archivarIniciativa = (id) => solicitar('POST', `${ruta(id)}/archive`);
export const listarMiembros = (id, params, signal) => solicitar('GET', `${ruta(id)}/members`, { params, signal });
export const retirarMiembro = (id, userId) => solicitar('DELETE', `${ruta(id)}/members/${encodeURIComponent(userId)}`);
export const convertirEnProyecto = (id, datos) => solicitar('POST', `${ruta(id)}/project`, { cuerpo: datos });
export const crearSolicitud = (id, datos) => solicitar('POST', `${ruta(id)}/requests`, { cuerpo: datos });
```

**`cliente/src/api/solicitudes.js`**

```js
import { solicitar } from './cliente.js';

const ruta = (id) => `/requests/${encodeURIComponent(id)}`;

export const listarMisSolicitudes = (params, signal) => solicitar('GET', '/requests/mine', { params, signal });
export const listarSolicitudesRecibidas = (params, signal) => solicitar('GET', '/requests/received', { params, signal });
export const obtenerSolicitud = (id, signal) => solicitar('GET', ruta(id), { signal });
export const actualizarSolicitud = (id, datos) => solicitar('PUT', ruta(id), { cuerpo: datos });
export const cancelarSolicitud = (id) => solicitar('POST', `${ruta(id)}/cancel`);
export const decidirSolicitud = (id, decision, comment) => solicitar('POST', `${ruta(id)}/decision`, { cuerpo: { decision, comment } });
```

**`cliente/src/api/proyectos.js`**

```js
import { solicitar } from './cliente.js';

export const listarProyectos = (params, signal) => solicitar('GET', '/projects', { params, signal });
export const obtenerProyecto = (id, signal) => solicitar('GET', `/projects/${encodeURIComponent(id)}`, { signal });
```

**`cliente/src/api/usuarios.js`**

```js
import { solicitar } from './cliente.js';

export const obtenerPerfilPropio = (signal) => solicitar('GET', '/auth/me', { signal });
export const actualizarPerfil = (datos) => solicitar('PUT', '/users/me', { cuerpo: datos });
export const buscarUsuarios = (params, signal) => solicitar('GET', '/users', { params, signal });
export const obtenerUsuario = (id, signal) => solicitar('GET', `/users/${encodeURIComponent(id)}`, { signal });
```

**`cliente/src/api/dev.js`** (temporal, se borra el día 10)

```js
import { solicitar } from './cliente.js';

export const listarUsuariosDev = (signal) => solicitar('GET', '/dev/users', { signal });
```

### 1.4 Hook de consulta, utilidades y contextos

**`cliente/src/hooks/useConsulta.js`**

```js
import { useCallback, useEffect, useState } from 'react';

// consultar(signal) devuelve una promesa. deps indica cuándo repetir la consulta.
// Toda pantalla que consulta la API usa este hook y muestra carga, error o vacío.
export function useConsulta(consultar, deps) {
  const [estado, setEstado] = useState({ cargando: true, error: null, datos: null });
  const [version, setVersion] = useState(0);

  useEffect(() => {
    const control = new AbortController();
    setEstado((previo) => ({ ...previo, cargando: true, error: null }));
    consultar(control.signal)
      .then((datos) => { if (!control.signal.aborted) setEstado({ cargando: false, error: null, datos }); })
      .catch((error) => { if (!control.signal.aborted) setEstado({ cargando: false, error, datos: null }); });
    // Limpieza: si cambian las dependencias o el componente se desmonta,
    // se cancela la petición anterior y su respuesta se ignora.
    return () => control.abort();
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [...deps, version]);

  const recargar = useCallback(() => setVersion((v) => v + 1), []);
  return { ...estado, recargar };
}
```

**`cliente/src/utils/etiquetas.js`**

La API usa valores en inglés; esta tabla es la única que los traduce para la interfaz (antes estaba en `ui.js` del Avance 1).

```js
export const TIPOS = {
  idea: { texto: 'Idea', icono: 'bi-lightbulb-fill', color: 'primary' },
  need: { texto: 'Necesidad', icono: 'bi-life-preserver', color: 'success' },
  challenge: { texto: 'Reto', icono: 'bi-flag-fill', color: 'warning' },
};
export const ESTADOS = {
  draft: { texto: 'Borrador', icono: 'bi-pencil-square', color: 'light' },
  published: { texto: 'Publicada', icono: 'bi-broadcast', color: 'light' },
  forming: { texto: 'En formación de equipo', icono: 'bi-people-fill', color: 'light' },
  project: { texto: 'Convertida en proyecto', icono: 'bi-rocket-takeoff-fill', color: 'info' },
  archived: { texto: 'Archivada', icono: 'bi-archive', color: 'secondary' },
};
export const VISIBILIDADES = {
  public: { texto: 'Pública', icono: 'bi-globe2' },
  institutional: { texto: 'Institucional', icono: 'bi-building' },
  restricted: { texto: 'Restringida', icono: 'bi-eye-slash' },
  private: { texto: 'Privada', icono: 'bi-lock-fill' },
};
export const ESTADOS_SOLICITUD = {
  pending: { texto: 'Pendiente', icono: 'bi-hourglass-split', color: 'warning' },
  accepted: { texto: 'Aceptada', icono: 'bi-check-circle-fill', color: 'success' },
  rejected: { texto: 'Rechazada', icono: 'bi-x-circle-fill', color: 'danger' },
  cancelled: { texto: 'Cancelada', icono: 'bi-slash-circle', color: 'secondary' },
};
export const ESTADOS_PROYECTO = {
  planning: { texto: 'Planificación', icono: 'bi-clipboard-check', color: 'info' },
  in_progress: { texto: 'En desarrollo', icono: 'bi-gear-fill', color: 'primary' },
  paused: { texto: 'Pausado', icono: 'bi-pause-circle', color: 'warning' },
  finished: { texto: 'Finalizado', icono: 'bi-flag', color: 'success' },
  archived: { texto: 'Archivado', icono: 'bi-archive', color: 'secondary' },
};
export const NIVELES = { basic: 'básico', intermediate: 'intermedio', advanced: 'avanzado' };
export const TIPOS_MIEMBRO = {
  student: { texto: 'Estudiante', icono: 'bi-mortarboard' },
  teacher: { texto: 'Docente', icono: 'bi-easel' },
  staff: { texto: 'Administrativo', icono: 'bi-briefcase' },
};

// Convierte un mapa en opciones para <select>.
export const opciones = (mapa) => Object.entries(mapa).map(([valor, d]) => ({ valor, texto: d.texto ?? d }));
```

**`cliente/src/utils/fechas.js`**

```js
// Acepta "2026-09-08" o un ISO completo. Las fechas sin hora se leen como locales para no correr un día.
export function formatearFecha(valor) {
  if (!valor) return 'sin fecha';
  const fecha = valor.length === 10 ? new Date(`${valor}T00:00:00`) : new Date(valor);
  return fecha.toLocaleDateString('es-CR', { day: 'numeric', month: 'short', year: 'numeric' });
}
```

**`cliente/src/context/ToastContext.jsx`**

```jsx
import { createContext, useCallback, useContext, useState } from 'react';

const ToastContext = createContext(() => {});

export function ToastProvider({ children }) {
  const [toasts, setToasts] = useState([]);

  const quitar = useCallback((id) => setToasts((lista) => lista.filter((t) => t.id !== id)), []);

  const mostrar = useCallback((mensaje, tipo = 'success') => {
    const id = crypto.randomUUID();
    setToasts((lista) => [...lista, { id, mensaje, tipo }]);
    setTimeout(() => quitar(id), 4500);
  }, [quitar]);

  return (
    <ToastContext.Provider value={mostrar}>
      {children}
      <div className="toast-container position-fixed bottom-0 end-0 p-3">
        {toasts.map((t) => (
          <div key={t.id} className={`toast show align-items-center text-bg-${t.tipo} border-0`} role="status" aria-live="polite">
            <div className="d-flex">
              <div className="toast-body">{t.mensaje}</div>
              <button
                type="button"
                className={`btn-close me-2 m-auto${['success', 'warning', 'light'].includes(t.tipo) ? '' : ' btn-close-white'}`}
                aria-label="Cerrar"
                onClick={() => quitar(t.id)}
              ></button>
            </div>
          </div>
        ))}
      </div>
    </ToastContext.Provider>
  );
}

export const useToast = () => useContext(ToastContext);
```

**`cliente/src/context/SesionContext.jsx`** (versión de desarrollo; el día 10 se reemplaza completa)

```jsx
import { createContext, useContext, useState } from 'react';
import { useConsulta } from '../hooks/useConsulta.js';
import { listarUsuariosDev } from '../api/dev.js';
import { obtenerUsuarioDev, fijarUsuarioDev } from '../api/sesion.js';
import { obtenerPerfilPropio } from '../api/usuarios.js';

const SesionContext = createContext(null);

export function SesionProvider({ children }) {
  const [usuarioId, setUsuarioId] = useState(obtenerUsuarioDev);
  const usuarios = useConsulta((signal) => listarUsuariosDev(signal), []);
  const perfil = useConsulta(
    (signal) => (usuarioId ? obtenerPerfilPropio(signal) : Promise.resolve(null)),
    [usuarioId]
  );

  function cambiarUsuario(id) {
    fijarUsuarioDev(id);
    setUsuarioId(id);
  }

  const valor = {
    usuario: perfil.datos,
    cargandoSesion: perfil.cargando,
    recargarUsuario: perfil.recargar,
    usuarioIdDev: usuarioId,
    usuariosDev: usuarios.datos ?? [],
    cambiarUsuario,
  };
  return <SesionContext.Provider value={valor}>{children}</SesionContext.Provider>;
}

export const useSesion = () => useContext(SesionContext);
```

**`cliente/src/main.jsx`** (reemplazar completo)

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import 'bootstrap-icons/font/bootstrap-icons.css';
import './styles/main.scss';
import App from './App.jsx';
import { SesionProvider } from './context/SesionContext.jsx';
import { ToastProvider } from './context/ToastContext.jsx';

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <BrowserRouter>
      <ToastProvider>
        <SesionProvider>
          <App />
        </SesionProvider>
      </ToastProvider>
    </BrowserRouter>
  </StrictMode>
);
```

```powershell
git add cliente
git commit -m "Agregar la capa api, el hook useConsulta y los contextos de sesión y avisos" --trailer "Seccion: Cliente" --trailer "Cambio: URL del servidor en variable de entorno y estados de consulta"
```

### 1.5 Componentes de interfaz reutilizables

**`cliente/src/components/ui/EstadoCarga.jsx`**

```jsx
export default function EstadoCarga({ texto = 'Cargando…' }) {
  return (
    <div className="text-center py-5" role="status" aria-live="polite">
      <div className="spinner-border text-primary" aria-hidden="true"></div>
      <p className="small text-body-secondary mt-2 mb-0">{texto}</p>
    </div>
  );
}
```

**`cliente/src/components/ui/EstadoError.jsx`**

```jsx
export default function EstadoError({ error, titulo = 'No se pudo completar la consulta', onReintentar }) {
  const sinConexion = error?.status === 0;
  return (
    <div className="estado-error text-center py-5" role="alert">
      <img src="/img/error-carga.svg" alt="" width="220" height="160" className="mb-3" />
      <h2 className="h5">{titulo}</h2>
      <p className="text-body-secondary">
        {sinConexion ? 'No hay conexión con el servidor. Verificá que esté en ejecución.' : error?.message}
      </p>
      {onReintentar && (
        <button type="button" className="btn btn-outline-primary btn-sm" onClick={onReintentar}>
          <i className="bi bi-arrow-clockwise" aria-hidden="true"></i> Reintentar
        </button>
      )}
    </div>
  );
}
```

**`cliente/src/components/ui/EstadoVacio.jsx`**

```jsx
export default function EstadoVacio({ titulo, imagen = '/img/vacio-catalogo.svg', children, accion }) {
  return (
    <div className="estado-vacio text-center py-5" role="status">
      <img src={imagen} alt="" width="200" height="145" className="mb-3" />
      <h2 className="h5">{titulo}</h2>
      {children && <p className="text-body-secondary mb-3">{children}</p>}
      {accion}
    </div>
  );
}
```

**`cliente/src/components/ui/Modal.jsx`**

```jsx
import { useEffect, useRef } from 'react';
import { createPortal } from 'react-dom';

// Modal de Bootstrap controlado por estado de React. El contenido llega por children.
export default function Modal({ abierto, titulo, onCerrar, children, pie, id = 'modal' }) {
  const refDialogo = useRef(null);
  const refCerrar = useRef(onCerrar);
  useEffect(() => { refCerrar.current = onCerrar; });

  useEffect(() => {
    if (!abierto) return undefined;
    const anterior = document.activeElement;
    refDialogo.current?.querySelector('[data-autofocus], button')?.focus();
    const alPresionar = (e) => { if (e.key === 'Escape') refCerrar.current(); };
    document.addEventListener('keydown', alPresionar);
    document.body.classList.add('modal-open');
    return () => {
      document.removeEventListener('keydown', alPresionar);
      document.body.classList.remove('modal-open');
      anterior?.focus?.();
    };
  }, [abierto]);

  if (!abierto) return null;
  return createPortal(
    <>
      <div
        className="modal fade show d-block" role="dialog" aria-modal="true" aria-labelledby={`${id}-titulo`}
        onMouseDown={(e) => { if (e.target === e.currentTarget) onCerrar(); }}
      >
        <div className="modal-dialog modal-dialog-centered" ref={refDialogo}>
          <div className="modal-content">
            <div className="modal-header border-0 pb-0">
              <h2 className="modal-title h5" id={`${id}-titulo`}>{titulo}</h2>
              <button type="button" className="btn-close" aria-label="Cerrar" onClick={onCerrar}></button>
            </div>
            <div className="modal-body">{children}</div>
            {pie && <div className="modal-footer border-0 pt-0">{pie}</div>}
          </div>
        </div>
      </div>
      <div className="modal-backdrop fade show"></div>
    </>,
    document.body
  );
}
```

**`cliente/src/components/ui/ModalConfirmacion.jsx`**

```jsx
import Modal from './Modal.jsx';

export default function ModalConfirmacion({
  abierto, titulo, textoConfirmar = 'Confirmar', variante = 'danger', icono = 'bi-exclamation-triangle-fill',
  procesando = false, onConfirmar, onCancelar, children,
}) {
  return (
    <Modal
      abierto={abierto}
      id="confirmacion"
      onCerrar={procesando ? () => {} : onCancelar}
      titulo={<><i className={`bi ${icono}`} aria-hidden="true"></i> {titulo}</>}
      pie={
        <>
          <button type="button" className="btn btn-outline-secondary" onClick={onCancelar} disabled={procesando}>Cancelar</button>
          <button type="button" className={`btn btn-${variante}`} onClick={onConfirmar} disabled={procesando} data-autofocus>
            {procesando && <span className="spinner-border spinner-border-sm me-1" aria-hidden="true"></span>}
            {textoConfirmar}
          </button>
        </>
      }
    >
      {children}
    </Modal>
  );
}
```

**`cliente/src/components/ui/Badges.jsx`**

```jsx
import { TIPOS, ESTADOS, ESTADOS_SOLICITUD, ESTADOS_PROYECTO, VISIBILIDADES } from '../../utils/etiquetas.js';

// Icono + texto + color: el estado nunca depende solo del color.
function Badge({ definiciones, valor }) {
  const d = definiciones[valor] ?? { texto: valor, icono: 'bi-tag', color: 'light' };
  return (
    <span className={`badge text-bg-${d.color}${d.color === 'light' ? ' border' : ''}`}>
      <i className={`bi ${d.icono}`} aria-hidden="true"></i> {d.texto}
    </span>
  );
}

export const BadgeTipo = ({ tipo }) => <Badge definiciones={TIPOS} valor={tipo} />;
export const BadgeEstado = ({ estado }) => <Badge definiciones={ESTADOS} valor={estado} />;
export const BadgeSolicitud = ({ estado }) => <Badge definiciones={ESTADOS_SOLICITUD} valor={estado} />;
export const BadgeProyecto = ({ estado }) => <Badge definiciones={ESTADOS_PROYECTO} valor={estado} />;

export function ChipVisibilidad({ visibilidad }) {
  const d = VISIBILIDADES[visibilidad] ?? { texto: visibilidad, icono: 'bi-eye' };
  return <span className="chip"><i className={`bi ${d.icono}`} aria-hidden="true"></i> {d.texto}</span>;
}
```

**`cliente/src/components/ui/Chip.jsx`**

```jsx
import { NIVELES } from '../../utils/etiquetas.js';

export default function Chip({ icono, neutro = false, nivel, children }) {
  return (
    <span className={`chip${neutro ? ' chip--neutro' : ''}`}>
      {icono && <i className={`bi ${icono}`} aria-hidden="true"></i>} {children}
      {nivel && <span className="chip--nivel"> · {NIVELES[nivel] ?? nivel}</span>}
    </span>
  );
}
```

**`cliente/src/components/ui/Avatar.jsx`**

```jsx
const iniciales = (nombre = '?') => nombre.split(/\s+/).slice(0, 2).map((p) => p[0]).join('').toUpperCase();

export default function Avatar({ nombre, grande = false }) {
  return (
    <span className={`avatar${grande ? ' avatar--lg' : ''}`} role="img" aria-label={nombre}>
      {iniciales(nombre)}
    </span>
  );
}
```

**`cliente/src/components/ui/Paginacion.jsx`**

```jsx
export default function Paginacion({ pagina, totalPaginas, onCambiar }) {
  if (totalPaginas <= 1) return null;
  const inicio = Math.max(1, Math.min(pagina - 2, totalPaginas - 4));
  const numeros = Array.from({ length: Math.min(5, totalPaginas) }, (_, i) => inicio + i);

  return (
    <nav aria-label="Paginación de resultados" className="mt-4">
      <ul className="pagination justify-content-center">
        <li className={`page-item${pagina === 1 ? ' disabled' : ''}`}>
          <button type="button" className="page-link" onClick={() => onCambiar(pagina - 1)} disabled={pagina === 1} aria-label="Página anterior">
            <i className="bi bi-chevron-left" aria-hidden="true"></i>
          </button>
        </li>
        {numeros.map((n) => (
          <li key={n} className={`page-item${n === pagina ? ' active' : ''}`}>
            <button type="button" className="page-link" onClick={() => onCambiar(n)} aria-current={n === pagina ? 'page' : undefined}>{n}</button>
          </li>
        ))}
        <li className={`page-item${pagina === totalPaginas ? ' disabled' : ''}`}>
          <button type="button" className="page-link" onClick={() => onCambiar(pagina + 1)} disabled={pagina === totalPaginas} aria-label="Página siguiente">
            <i className="bi bi-chevron-right" aria-hidden="true"></i>
          </button>
        </li>
      </ul>
    </nav>
  );
}
```

### 1.6 Sesión en el navbar y rutas protegidas

**`cliente/src/components/layout/SelectorSesion.jsx`** (temporal, se borra el día 10)

```jsx
import { useSesion } from '../../context/SesionContext.jsx';

export default function SelectorSesion() {
  const { usuarioIdDev, usuariosDev, cambiarUsuario } = useSesion();
  return (
    <label className="ver-como mb-0" htmlFor="ver-como">
      <i className="bi bi-person-badge" aria-hidden="true"></i> Ver como
      <select id="ver-como" className="form-select form-select-sm" value={usuarioIdDev} onChange={(e) => cambiarUsuario(e.target.value)}>
        <option value="">Visitante</option>
        {usuariosDev.map((u) => <option key={u.id} value={u.id}>{u.fullName}</option>)}
      </select>
    </label>
  );
}
```

**`cliente/src/components/layout/ControlesSesion.jsx`**

```jsx
import { Link, NavLink } from 'react-router-dom';
import { useSesion } from '../../context/SesionContext.jsx';
import SelectorSesion from './SelectorSesion.jsx';

export default function ControlesSesion() {
  const { usuario } = useSesion();
  return (
    <>
      {usuario && (
        <>
          <NavLink className="nav-link" to="/solicitudes"><i className="bi bi-send" aria-hidden="true"></i> Solicitudes</NavLink>
          <NavLink className="nav-link" to="/perfil"><i className="bi bi-person-circle" aria-hidden="true"></i> Mi perfil</NavLink>
        </>
      )}
      <SelectorSesion />
      <Link className="btn btn-primary btn-sm" to="/iniciativas/nueva">
        <i className="bi bi-plus-circle-fill" aria-hidden="true"></i> Publicar iniciativa
      </Link>
    </>
  );
}
```

**`cliente/src/components/layout/RutaProtegida.jsx`** (el día 10 cambia el mensaje por una redirección al login)

```jsx
import { useSesion } from '../../context/SesionContext.jsx';
import EstadoCarga from '../ui/EstadoCarga.jsx';
import EstadoVacio from '../ui/EstadoVacio.jsx';

export default function RutaProtegida({ children }) {
  const { usuario, cargandoSesion } = useSesion();
  if (cargandoSesion) return <EstadoCarga texto="Verificando la sesión…" />;
  if (!usuario) {
    return (
      <div className="container py-5">
        <EstadoVacio titulo="Necesitás una sesión">
          Elegí un usuario en "Ver como", en la barra superior.
        </EstadoVacio>
      </div>
    );
  }
  return children;
}
```

**`cliente/src/components/layout/Layout.jsx`** (reemplazar completo)

`key` en el `<Outlet>` hace que al cambiar de usuario todas las pantallas se vuelvan a montar y a consultar con la nueva identidad.

```jsx
import { useEffect } from 'react';
import { Outlet, useLocation } from 'react-router-dom';
import Navbar from './Navbar.jsx';
import Footer from './Footer.jsx';
import ControlesSesion from './ControlesSesion.jsx';
import { useSesion } from '../../context/SesionContext.jsx';

export default function Layout() {
  const { pathname } = useLocation();
  const { usuario } = useSesion();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);

  return (
    <>
      <a className="visually-hidden-focusable position-absolute top-0 start-0 m-2 btn btn-primary btn-sm" href="#contenido">
        Saltar al contenido
      </a>
      <header>
        <Navbar>
          <ControlesSesion />
        </Navbar>
      </header>
      <main id="contenido" tabIndex={-1}>
        <Outlet key={usuario?.id ?? 'visitante'} />
      </main>
      <Footer />
    </>
  );
}
```

### 1.7 Cifras de la portada

**`cliente/src/features/inicio/CifrasPlataforma.jsx`**

```jsx
import { useConsulta } from '../../hooks/useConsulta.js';
import { listarIniciativas } from '../../api/iniciativas.js';
import { listarCategorias, listarCompetencias } from '../../api/catalogos.js';

export default function CifrasPlataforma() {
  const { cargando, error, datos } = useConsulta(
    (signal) => Promise.all([listarIniciativas({ size: 1 }, signal), listarCategorias(signal), listarCompetencias(signal)]),
    []
  );

  if (cargando) return <p className="text-center small text-body-secondary" role="status">Cargando cifras…</p>;
  if (error) return <p className="text-center small text-body-secondary" role="status">Las cifras no están disponibles en este momento.</p>;

  const [iniciativas, categorias, competencias] = datos;
  const cifras = [
    { valor: iniciativas.totalItems, texto: 'iniciativas abiertas' },
    { valor: categorias.length, texto: 'categorías' },
    { valor: competencias.length, texto: 'competencias en el catálogo' },
  ];
  return (
    <div className="row row-cols-1 row-cols-sm-3 g-3 text-center">
      {cifras.map((c) => (
        <div className="col" key={c.texto}>
          <div className="card"><div className="card-body">
            <p className="estadistica mb-1">{c.valor}</p>
            <p className="small text-body-secondary mb-0">{c.texto}</p>
          </div></div>
        </div>
      ))}
    </div>
  );
}
```

En **`InicioPage.jsx`**: agregar `import CifrasPlataforma from './CifrasPlataforma.jsx';` y, justo antes del `</>` final, la sección:

```jsx
      <section className="container pb-5" aria-label="Cifras de la plataforma">
        <CifrasPlataforma />
      </section>
```

### 1.8 Verificar

`npm run dev` en `cliente/`. La portada muestra 7 iniciativas abiertas, 8 categorías y 8 competencias (las archivadas y privadas no cuentan para un visitante). El selector "Ver como" lista a los cuatro usuarios; al elegir a Ana aparecen "Solicitudes" y "Mi perfil". En las herramientas del navegador, Application → Local Storage muestra `ih.mock.v1` después de la primera carga.

```powershell
git add cliente
git commit -m "Agregar componentes de estado, modal, badges, paginación y selector de sesión" --trailer "Seccion: Cliente" --trailer "Cambio: Componentes reutilizables con props y children"
git push
```

**Explicar en voz alta:** por qué el cliente nunca llama `fetch` fuera de `cliente.js`; qué hace la limpieza del efecto en `useConsulta`; por qué `Modal` guarda `onCerrar` en una referencia; qué logra `key` en el `<Outlet>`.

---

# DÍA 2 · martes 27 oct · Catálogo, tarjeta, detalle y equipo

### 2.1 Acciones del propietario (corrige el criterio 9)

Un solo componente para eliminar o archivar, usado en la tarjeta del catálogo, en el detalle y en "Mis iniciativas". El verbo se decide con `relations`, que la API solo envía al propietario.

**`cliente/src/features/iniciativas/AccionesPropietario.jsx`**

```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import ModalConfirmacion from '../../components/ui/ModalConfirmacion.jsx';
import { useToast } from '../../context/ToastContext.jsx';
import { archivarIniciativa, eliminarIniciativa } from '../../api/iniciativas.js';

export default function AccionesPropietario({ ini, variante = 'detalle', onCambio }) {
  const mostrarToast = useToast();
  const [confirmando, setConfirmando] = useState(false);
  const [procesando, setProcesando] = useState(false);

  const relaciones = ini.relations ?? { members: 0, requests: 0, hasProject: false };
  const debeArchivar = relaciones.members > 0 || relaciones.requests > 0 || relaciones.hasProject;
  const abierta = ['published', 'forming'].includes(ini.status);
  const puedeConvertir = abierta && relaciones.members > 0;
  const compacto = variante === 'tarjeta';

  if (ini.status === 'archived') {
    return <p className="small text-body-secondary mb-0"><i className="bi bi-archive" aria-hidden="true"></i> Archivada: solo lectura.</p>;
  }

  async function confirmar() {
    setProcesando(true);
    try {
      if (debeArchivar) {
        await archivarIniciativa(ini.id);
        mostrarToast('La iniciativa tenía equipo, solicitudes o proyecto: se archivó en lugar de eliminarse.', 'info');
      } else {
        await eliminarIniciativa(ini.id);
        mostrarToast('Iniciativa eliminada.', 'secondary');
      }
      setConfirmando(false);
      onCambio?.(debeArchivar ? 'archivada' : 'eliminada');
    } catch (error) {
      setConfirmando(false);
      mostrarToast(error.message, 'danger');
    } finally {
      setProcesando(false);
    }
  }

  const tamano = compacto ? ' btn-sm' : '';
  return (
    <>
      <div className={compacto ? 'd-flex flex-wrap gap-2' : 'd-grid gap-2'}>
        <Link className={`btn btn-outline-primary${tamano}`} to={`/iniciativas/${ini.id}/editar`}>
          <i className="bi bi-pencil" aria-hidden="true"></i> Editar
        </Link>
        <button type="button" className={`btn btn-outline-danger${tamano}`} onClick={() => setConfirmando(true)}>
          <i className={`bi ${debeArchivar ? 'bi-archive' : 'bi-trash3'}`} aria-hidden="true"></i> {debeArchivar ? 'Archivar' : 'Eliminar'}
        </button>
        {!compacto && abierta && (
          puedeConvertir
            ? <Link className="btn btn-primary" to={`/iniciativas/${ini.id}/proyecto`}><i className="bi bi-rocket-takeoff" aria-hidden="true"></i> Convertir en proyecto</Link>
            : <p className="small text-body-secondary mb-0">Para convertirla en proyecto necesita al menos un integrante además de vos.</p>
        )}
      </div>

      <ModalConfirmacion
        abierto={confirmando}
        titulo={debeArchivar ? 'Archivar iniciativa' : 'Eliminar iniciativa'}
        textoConfirmar={debeArchivar ? 'Archivar' : 'Eliminar'}
        procesando={procesando}
        onConfirmar={confirmar}
        onCancelar={() => setConfirmando(false)}
      >
        <p className="fw-semibold mb-1">{ini.title}</p>
        <p className="text-body-secondary mb-0">
          {debeArchivar
            ? `Tiene ${relaciones.members} integrante(s) y ${relaciones.requests} solicitud(es)${relaciones.hasProject ? ' y un proyecto' : ''}, así que se archivará en lugar de eliminarse.`
            : 'No tiene equipo ni solicitudes: se eliminará de forma definitiva.'}
        </p>
      </ModalConfirmacion>
    </>
  );
}
```

### 2.2 Tarjeta del catálogo

**`cliente/src/features/iniciativas/TarjetaIniciativa.jsx`**

```jsx
import { Link } from 'react-router-dom';
import { BadgeTipo, BadgeEstado } from '../../components/ui/Badges.jsx';
import Chip from '../../components/ui/Chip.jsx';
import AccionesPropietario from './AccionesPropietario.jsx';
import { useSesion } from '../../context/SesionContext.jsx';

// Clases del Avance 1: el borde superior cambia de color según el tipo.
const CLASE_TIPO = { idea: 'idea', need: 'necesidad', challenge: 'reto' };

export default function TarjetaIniciativa({ ini, onCambio }) {
  const { usuario } = useSesion();
  const completa = ini.accessLevel === 'full';
  const esDueno = completa && usuario?.id === ini.owner.id;

  return (
    <article className={`card h-100 tarjeta-iniciativa tarjeta-iniciativa--${CLASE_TIPO[ini.type]}`}>
      <div className="card-body d-flex flex-column">
        <div className="d-flex justify-content-between align-items-start gap-2">
          <h2 className="card-title mb-0">{ini.title}</h2>
          <BadgeTipo tipo={ini.type} />
        </div>
        <p className="card-text small text-body-secondary mt-2">{ini.summary}</p>
        <ul className="lista-chips mb-3">
          {ini.requiredSkills.slice(0, 3).map((s) => <li key={s.id}><Chip>{s.name}</Chip></li>)}
        </ul>
        <p className="small mb-2">
          <i className={`bi ${ini.category?.icon ?? 'bi-tag'}`} aria-hidden="true"></i> {ini.category?.name ?? 'Sin categoría'}
          {completa && <> · <i className="bi bi-person" aria-hidden="true"></i> {ini.owner.fullName}</>}
        </p>
        {completa ? (
          <p className="small mb-3 d-flex flex-wrap gap-2 align-items-center">
            <BadgeEstado estado={ini.status} />
            <span className="text-body-secondary">
              <i className="bi bi-person-plus" aria-hidden="true"></i> {ini.memberCount} {ini.memberCount === 1 ? 'integrante' : 'integrantes'} · {ini.availableSpots} {ini.availableSpots === 1 ? 'espacio' : 'espacios'}
            </span>
          </p>
        ) : (
          <p className="small mb-3"><Chip icono="bi-eye-slash" neutro>Contenido restringido</Chip></p>
        )}
        <div className="mt-auto d-flex flex-column gap-2">
          <Link className="btn btn-outline-primary btn-sm align-self-start" to={`/iniciativas/${ini.id}`}>
            Ver detalle <i className="bi bi-arrow-right" aria-hidden="true"></i>
          </Link>
          {esDueno && <AccionesPropietario ini={ini} variante="tarjeta" onCambio={onCambio} />}
        </div>
      </div>
    </article>
  );
}
```

### 2.3 Filtros y catálogo con paginación del servidor

Los filtros viven en la URL (`useSearchParams`): se pueden compartir, el botón atrás funciona y cada cambio pide al servidor solo la página necesaria.

**`cliente/src/features/iniciativas/FiltrosIniciativas.jsx`**

```jsx
import { useEffect, useState } from 'react';
import { opciones, TIPOS } from '../../utils/etiquetas.js';

const ORDENES = [
  { valor: '-createdAt', texto: 'Más recientes' },
  { valor: 'createdAt', texto: 'Más antiguas' },
  { valor: 'title', texto: 'Título (A–Z)' },
  { valor: '-updatedAt', texto: 'Actualizadas recientemente' },
];

export default function FiltrosIniciativas({ valores, categorias, competencias, onCambiar }) {
  const [texto, setTexto] = useState(valores.q);

  // Si la URL cambia desde afuera (por ejemplo al limpiar), el campo se sincroniza.
  useEffect(() => { setTexto(valores.q); }, [valores.q]);

  // Espera 350 ms sin teclear antes de buscar; la limpieza cancela el temporizador anterior.
  useEffect(() => {
    if (texto === valores.q) return undefined;
    const temporizador = setTimeout(() => onCambiar({ q: texto }), 350);
    return () => clearTimeout(temporizador);
  }, [texto, valores.q, onCambiar]);

  const seleccionar = (campo) => (e) => onCambiar({ [campo]: e.target.value });

  return (
    <form className="formulario-tarjeta row g-3 mb-4" role="search" aria-label="Buscar y filtrar iniciativas" onSubmit={(e) => e.preventDefault()}>
      <div className="col-12 col-lg-4">
        <label className="form-label" htmlFor="buscar"><i className="bi bi-search" aria-hidden="true"></i> Buscar</label>
        <input id="buscar" type="search" className="form-control" placeholder="Título, resumen o etiqueta" value={texto} onChange={(e) => setTexto(e.target.value)} />
      </div>
      <div className="col-6 col-md-4 col-lg-2">
        <label className="form-label" htmlFor="f-tipo">Tipo</label>
        <select id="f-tipo" className="form-select" value={valores.type} onChange={seleccionar('type')}>
          <option value="">Todos</option>
          {opciones(TIPOS).map((o) => <option key={o.valor} value={o.valor}>{o.texto}</option>)}
        </select>
      </div>
      <div className="col-6 col-md-4 col-lg-3">
        <label className="form-label" htmlFor="f-categoria">Categoría</label>
        <select id="f-categoria" className="form-select" value={valores.category} onChange={seleccionar('category')}>
          <option value="">Todas</option>
          {categorias.map((c) => <option key={c.id} value={c.id}>{c.name}</option>)}
        </select>
      </div>
      <div className="col-12 col-md-4 col-lg-3">
        <label className="form-label" htmlFor="f-competencia">Competencia</label>
        <select id="f-competencia" className="form-select" value={valores.skill} onChange={seleccionar('skill')}>
          <option value="">Todas</option>
          {competencias.map((c) => <option key={c.id} value={c.id}>{c.name}</option>)}
        </select>
      </div>
      <div className="col-12 col-md-6 col-lg-4">
        <label className="form-label" htmlFor="f-orden">Ordenar por</label>
        <select id="f-orden" className="form-select" value={valores.sort || '-createdAt'} onChange={seleccionar('sort')}>
          {ORDENES.map((o) => <option key={o.valor} value={o.valor}>{o.texto}</option>)}
        </select>
      </div>
      <div className="col-12 col-md-6 col-lg-4 d-flex align-items-end">
        <div className="form-check">
          <input id="f-espacios" type="checkbox" className="form-check-input" checked={valores.hasSpots === 'true'} onChange={(e) => onCambiar({ hasSpots: e.target.checked ? 'true' : '' })} />
          <label className="form-check-label" htmlFor="f-espacios">Solo con espacios disponibles</label>
        </div>
      </div>
      <div className="col-12 col-lg-4 d-flex align-items-end justify-content-lg-end">
        <button type="button" className="btn btn-outline-secondary btn-sm" onClick={() => onCambiar({ q: '', type: '', category: '', skill: '', hasSpots: '', sort: '' })}>
          <i className="bi bi-x-circle" aria-hidden="true"></i> Limpiar filtros
        </button>
      </div>
    </form>
  );
}
```

**`cliente/src/features/iniciativas/CatalogoPage.jsx`**

```jsx
import { useCallback } from 'react';
import { useSearchParams } from 'react-router-dom';
import FiltrosIniciativas from './FiltrosIniciativas.jsx';
import TarjetaIniciativa from './TarjetaIniciativa.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import Paginacion from '../../components/ui/Paginacion.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { listarIniciativas } from '../../api/iniciativas.js';
import { listarCategorias, listarCompetencias } from '../../api/catalogos.js';

const CLAVES = ['q', 'type', 'category', 'skill', 'hasSpots', 'sort'];
const TAMANO_PAGINA = 9;

export default function CatalogoPage() {
  useTituloPagina('Catálogo de iniciativas');
  const [params, setParams] = useSearchParams();
  const filtros = Object.fromEntries(CLAVES.map((k) => [k, params.get(k) ?? '']));
  const pagina = Number(params.get('page')) || 1;

  const catalogos = useConsulta((signal) => Promise.all([listarCategorias(signal), listarCompetencias(signal)]), []);
  const resultado = useConsulta(
    (signal) => listarIniciativas({ ...filtros, page: pagina, size: TAMANO_PAGINA }, signal),
    [params.toString()]
  );

  // Estado elevado: los filtros viven aquí (en la URL) y bajan por props.
  // Cambiar un filtro reinicia la página a 1.
  const cambiarFiltros = useCallback((nuevos) => {
    setParams((actuales) => {
      const siguiente = { ...Object.fromEntries(actuales), ...nuevos };
      delete siguiente.page;
      return Object.fromEntries(Object.entries(siguiente).filter(([, v]) => v));
    });
  }, [setParams]);

  const cambiarPagina = (numero) => setParams((actuales) => ({ ...Object.fromEntries(actuales), page: String(numero) }));

  const [categorias, competencias] = catalogos.datos ?? [[], []];
  const datos = resultado.datos;

  return (
    <div className="container py-4">
      <div className="d-flex flex-wrap justify-content-between align-items-end gap-2 mb-4">
        <div>
          <h1 className="h3 mb-1">Catálogo de iniciativas</h1>
          <p className="text-body-secondary mb-0">Ideas, necesidades y retos que buscan equipo.</p>
        </div>
        <p className="mb-0 small text-body-secondary" aria-live="polite">
          {datos && `${datos.totalItems} ${datos.totalItems === 1 ? 'iniciativa' : 'iniciativas'}`}
        </p>
      </div>

      {catalogos.error
        ? <EstadoError error={catalogos.error} titulo="No se pudieron cargar los filtros" onReintentar={catalogos.recargar} />
        : <FiltrosIniciativas valores={filtros} categorias={categorias} competencias={competencias} onCambiar={cambiarFiltros} />}

      {resultado.cargando && <EstadoCarga texto="Buscando iniciativas…" />}
      {resultado.error && <EstadoError error={resultado.error} onReintentar={resultado.recargar} />}
      {datos && !resultado.cargando && datos.items.length === 0 && (
        <EstadoVacio
          titulo="No encontramos iniciativas con esos criterios"
          accion={<button type="button" className="btn btn-outline-primary btn-sm" onClick={() => cambiarFiltros({ q: '', type: '', category: '', skill: '', hasSpots: '' })}>Limpiar filtros</button>}
        >
          Probá con otra palabra o quitá algún filtro.
        </EstadoVacio>
      )}
      {datos && !resultado.cargando && datos.items.length > 0 && (
        <>
          <div className="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-4">
            {datos.items.map((ini) => (
              <div className="col" key={ini.id}>
                <TarjetaIniciativa ini={ini} onCambio={resultado.recargar} />
              </div>
            ))}
          </div>
          <Paginacion pagina={datos.page} totalPaginas={datos.totalPages} onCambiar={cambiarPagina} />
        </>
      )}
    </div>
  );
}
```

```powershell
git add cliente
git commit -m "Agregar catálogo con filtros en la URL, orden y paginación" --trailer "Seccion: Catálogo" --trailer "Cambio: Filtros combinados resueltos por la API con estados de carga, error y vacío"
```

### 2.4 Equipo, detalle completo y detalle restringido (corrige el criterio 7)

**`cliente/src/features/equipo/ListaEquipo.jsx`**

```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import Avatar from '../../components/ui/Avatar.jsx';
import ModalConfirmacion from '../../components/ui/ModalConfirmacion.jsx';
import { useToast } from '../../context/ToastContext.jsx';
import { retirarMiembro } from '../../api/iniciativas.js';
import { formatearFecha } from '../../utils/fechas.js';

export default function ListaEquipo({ ini, puedeRetirar = false, onCambio }) {
  const mostrarToast = useToast();
  const [porRetirar, setPorRetirar] = useState(null);
  const [procesando, setProcesando] = useState(false);

  async function confirmar() {
    setProcesando(true);
    try {
      await retirarMiembro(ini.id, porRetirar.userId);
      mostrarToast(`${porRetirar.fullName} ya no forma parte del equipo.`, 'secondary');
      setPorRetirar(null);
      onCambio?.();
    } catch (error) {
      mostrarToast(error.message, 'danger');
      setPorRetirar(null);
    } finally {
      setProcesando(false);
    }
  }

  return (
    <>
      <ul className="list-unstyled mb-3">
        <li className="d-flex align-items-center gap-2 mb-2">
          <Avatar nombre={ini.owner.fullName} />
          <span><Link to={`/usuarios/${ini.owner.id}`}>{ini.owner.fullName}</Link> <span className="small text-body-secondary">· propietario</span></span>
        </li>
        {ini.members.map((m) => (
          <li key={m.userId} className="d-flex align-items-center gap-2 mb-2">
            <Avatar nombre={m.fullName} />
            <span className="flex-grow-1">
              <Link to={`/usuarios/${m.userId}`}>{m.fullName}</Link>
              <span className="d-block small text-body-secondary">{m.role.name} · {m.career} · desde {formatearFecha(m.joinedAt)}</span>
            </span>
            {puedeRetirar && (
              <button type="button" className="btn btn-sm btn-outline-danger" aria-label={`Retirar a ${m.fullName} del equipo`} onClick={() => setPorRetirar(m)}>
                <i className="bi bi-person-dash" aria-hidden="true"></i>
              </button>
            )}
          </li>
        ))}
        {ini.members.length === 0 && <li className="small text-body-secondary">Todavía no hay integrantes además del propietario.</li>}
      </ul>

      <ModalConfirmacion
        abierto={!!porRetirar}
        titulo="Retirar del equipo"
        textoConfirmar="Retirar"
        procesando={procesando}
        onConfirmar={confirmar}
        onCancelar={() => setPorRetirar(null)}
      >
        <p className="mb-0">¿Retirar a <strong>{porRetirar?.fullName}</strong> del equipo de "{ini.title}"?</p>
      </ModalConfirmacion>
    </>
  );
}
```

**`cliente/src/features/iniciativas/DetalleRestringido.jsx`**

Solo lo que el documento general permite para iniciativas restringidas: título, tipo, resumen, categoría y competencias requeridas, más el aviso. Sin propietario, fechas, estado, equipo ni acciones del propietario. La API tampoco envía esos datos.

```jsx
import { Link } from 'react-router-dom';
import { BadgeTipo, ChipVisibilidad } from '../../components/ui/Badges.jsx';
import Chip from '../../components/ui/Chip.jsx';
import { useSesion } from '../../context/SesionContext.jsx';

export default function DetalleRestringido({ ini }) {
  const { usuario } = useSesion();
  return (
    <article className="row justify-content-center">
      <div className="col-lg-8">
        <h1 className="h2 mb-2">{ini.title}</h1>
        <div className="d-flex flex-wrap gap-2 mb-3">
          <BadgeTipo tipo={ini.type} />
          <ChipVisibilidad visibilidad={ini.visibility} />
        </div>
        <div className="alert alert-warning d-flex gap-2" role="status">
          <i className="bi bi-info-circle-fill" aria-hidden="true"></i>
          <div>
            <strong>Contenido completo no disponible.</strong> Esta iniciativa es restringida: solo se muestra su resumen.
            La información completa es visible para el propietario y los miembros aceptados del equipo.
          </div>
        </div>
        <p className="lead">{ini.summary}</p>
        <h2 className="seccion-titulo mt-4">Categoría</h2>
        <p><i className={`bi ${ini.category?.icon ?? 'bi-tag'}`} aria-hidden="true"></i> {ini.category?.name ?? 'Sin categoría'}</p>
        <h2 className="seccion-titulo">Competencias requeridas</h2>
        <ul className="lista-chips mb-4">
          {ini.requiredSkills.map((s) => <li key={s.id}><Chip icono="bi-stars">{s.name}</Chip></li>)}
        </ul>
        {usuario && (
          <Link className="btn btn-primary" to={`/iniciativas/${ini.id}/solicitar`}>
            <i className="bi bi-send" aria-hidden="true"></i> Solicitar participación
          </Link>
        )}
      </div>
    </article>
  );
}
```

**`cliente/src/features/iniciativas/DetalleCompleto.jsx`**

```jsx
import { Link } from 'react-router-dom';
import { BadgeTipo, BadgeEstado, ChipVisibilidad } from '../../components/ui/Badges.jsx';
import Chip from '../../components/ui/Chip.jsx';
import ListaEquipo from '../equipo/ListaEquipo.jsx';
import AccionesPropietario from './AccionesPropietario.jsx';
import { useSesion } from '../../context/SesionContext.jsx';
import { formatearFecha } from '../../utils/fechas.js';

function AccionesVisitante({ ini, usuario }) {
  const esMiembro = ini.members.some((m) => m.userId === usuario?.id);
  if (esMiembro) return <p className="small text-success mb-0"><i className="bi bi-check-circle-fill" aria-hidden="true"></i> Formás parte del equipo.</p>;
  if (!['published', 'forming'].includes(ini.status)) return <p className="small text-body-secondary mb-0">Esta iniciativa ya no recibe solicitudes.</p>;
  if (!usuario) return <p className="small text-body-secondary mb-0">Iniciá sesión para solicitar participación.</p>;
  return (
    <Link className="btn btn-primary" to={`/iniciativas/${ini.id}/solicitar`}>
      <i className="bi bi-send" aria-hidden="true"></i> Solicitar participación
    </Link>
  );
}

export default function DetalleCompleto({ ini, onCambio }) {
  const { usuario } = useSesion();
  const esDueno = usuario?.id === ini.owner.id;

  return (
    <article className="row g-4">
      <div className="col-lg-8">
        <h1 className="h2 mb-2">{ini.title}</h1>
        <div className="d-flex flex-wrap gap-2 mb-2">
          <BadgeTipo tipo={ini.type} /> <BadgeEstado estado={ini.status} /> <ChipVisibilidad visibilidad={ini.visibility} />
        </div>
        <p className="small text-body-secondary d-flex flex-wrap gap-3 mb-3">
          <span><i className={`bi ${ini.category?.icon ?? 'bi-tag'}`} aria-hidden="true"></i> {ini.category?.name}</span>
          <span><i className="bi bi-person" aria-hidden="true"></i> <Link to={`/usuarios/${ini.owner.id}`}>{ini.owner.fullName}</Link></span>
          <span><i className="bi bi-calendar-event" aria-hidden="true"></i> Publicada {formatearFecha(ini.createdAt)}</span>
          <span><i className="bi bi-pencil" aria-hidden="true"></i> Editada {formatearFecha(ini.updatedAt)}</span>
        </p>
        {ini.projectId && (
          <div className="alert alert-info d-flex gap-2" role="status">
            <i className="bi bi-rocket-takeoff-fill" aria-hidden="true"></i>
            <div>Esta iniciativa ya es un proyecto. <Link to={`/proyectos/${ini.projectId}`}>Ver el proyecto</Link></div>
          </div>
        )}
        <p className="lead">{ini.summary}</p>
        <p>{ini.description}</p>
        <div className="row g-3 mt-2">
          <div className="col-md-6">
            <div className="bloque-acento h-100">
              <h2 className="seccion-titulo">Problema identificado</h2>
              <p className="mb-0">{ini.problem}</p>
            </div>
          </div>
          <div className="col-md-6">
            <div className="bloque-acento bloque-acento--teal h-100">
              <h2 className="seccion-titulo">Beneficiarios</h2>
              <p className="mb-0">{ini.beneficiaries}</p>
            </div>
          </div>
        </div>
        <h2 className="seccion-titulo mt-4">Etiquetas</h2>
        <ul className="lista-chips">
          {ini.tags.length ? ini.tags.map((t) => <li key={t}><Chip neutro>#{t}</Chip></li>) : <li className="small text-body-secondary">Sin etiquetas</li>}
        </ul>
      </div>

      <aside className="col-lg-4">
        <div className="card tarjeta-lateral">
          <div className="card-body">
            <div className="row g-2 mb-3">
              <div className="col-6">
                <div className="metrica">
                  <div className="seccion-titulo">Integrantes</div>
                  <div className="valor">{ini.memberCount} <small className="fs-6 fw-normal text-body-secondary">de {ini.estimatedMembers}</small></div>
                </div>
              </div>
              <div className="col-6">
                <div className="metrica metrica--acento">
                  <div className="seccion-titulo">Espacios</div>
                  <div className="valor">{ini.availableSpots} <small className="fs-6 fw-normal">libres</small></div>
                </div>
              </div>
            </div>
            <h2 className="seccion-titulo">Equipo</h2>
            <ListaEquipo ini={ini} puedeRetirar={esDueno && ini.status !== 'archived'} onCambio={onCambio} />
            <h2 className="seccion-titulo">Competencias requeridas</h2>
            <ul className="lista-chips mb-4">
              {ini.requiredSkills.map((s) => <li key={s.id}><Chip icono="bi-stars">{s.name}</Chip></li>)}
            </ul>
            {esDueno ? <AccionesPropietario ini={ini} variante="detalle" onCambio={onCambio} /> : <div className="d-grid"><AccionesVisitante ini={ini} usuario={usuario} /></div>}
          </div>
        </div>
      </aside>
    </article>
  );
}
```

**`cliente/src/features/iniciativas/DetallePage.jsx`**

```jsx
import { Link, useNavigate, useParams } from 'react-router-dom';
import DetalleCompleto from './DetalleCompleto.jsx';
import DetalleRestringido from './DetalleRestringido.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { obtenerIniciativa } from '../../api/iniciativas.js';

export default function DetallePage() {
  const { id } = useParams(); // el detalle se genera a partir del parámetro de la ruta
  const navigate = useNavigate();
  const { cargando, error, datos: ini, recargar } = useConsulta((signal) => obtenerIniciativa(id, signal), [id]);
  useTituloPagina(ini?.title ?? 'Detalle de iniciativa');

  const volver = <Link className="btn btn-outline-primary" to="/iniciativas">Volver al catálogo</Link>;
  let contenido;
  if (cargando) contenido = <EstadoCarga texto="Cargando iniciativa…" />;
  else if (error?.status === 404) contenido = <EstadoVacio titulo="No encontramos esa iniciativa" accion={volver}>Puede haberse eliminado o no estar disponible para vos.</EstadoVacio>;
  else if (error?.status === 401) contenido = <EstadoVacio titulo="Iniciativa institucional" accion={volver}>Solo la pueden consultar usuarios con sesión iniciada.</EstadoVacio>;
  else if (error) contenido = <EstadoError error={error} onReintentar={recargar} />;
  else if (ini.accessLevel === 'summary') contenido = <DetalleRestringido ini={ini} />;
  else {
    contenido = (
      <DetalleCompleto
        ini={ini}
        onCambio={(resultado) => (resultado === 'eliminada' ? navigate('/iniciativas') : recargar())}
      />
    );
  }

  return (
    <div className="container py-4">
      <Link className="small text-body-secondary d-inline-block mb-2" to="/iniciativas">
        <i className="bi bi-arrow-left" aria-hidden="true"></i> Volver al catálogo
      </Link>
      {contenido}
    </div>
  );
}
```

En **`App.jsx`** agregar los imports y estas rutas antes de `path="*"`:

```jsx
import CatalogoPage from './features/iniciativas/CatalogoPage.jsx';
import DetallePage from './features/iniciativas/DetallePage.jsx';
// ...
        <Route path="iniciativas" element={<CatalogoPage />} />
        <Route path="iniciativas/:id" element={<DetallePage />} />
```

### 2.5 Verificar

| Prueba | Esperado |
|---|---|
| Catálogo como Visitante | 7 tarjetas en una página de 9; la privada no aparece; la restringida y la institucional dicen "Contenido restringido" |
| Escribir "cine" | Tras 350 ms, solo "Festival de cortos" (busca en etiquetas); la URL cambia a `?q=cine` |
| Tipo Reto + Sostenibilidad | "Monitoreo de energía"; Limpiar filtros vuelve a 7 |
| Ver como Ana, catálogo | Sus dos iniciativas muestran Editar y Eliminar/Archivar en la tarjeta |
| Ana elimina "App de reciclaje" desde la tarjeta | Modal "Eliminar iniciativa"; tras confirmar, toast y la tarjeta desaparece |
| Ana en "Plataforma de voluntariado" | Botón "Archivar" porque tiene equipo y solicitudes |
| Visitante abre "Soporte TI" (restringida) | Solo título, tipo, resumen, categoría, competencias y aviso; nada del propietario |
| Diego abre "Soporte TI" | Detalle completo con acciones del propietario |
| Visitante abre "Tutorías" (institucional) | "Iniciativa institucional" |
| Visitante abre `/iniciativas/123` | "No encontramos esa iniciativa" |
| Luis retira a Diego del coworking | Modal, toast, el equipo se actualiza |

Para volver a los datos originales en cualquier momento: en la consola del navegador `localStorage.removeItem('ih.mock.v1')` y recargar.

```powershell
git add cliente
git commit -m "Agregar detalle completo y restringido, equipo y acciones del propietario" --trailer "Seccion: Detalle" --trailer "Cambio: RN-03 estricta y eliminación desde la lista"
git push
```

**Explicar en voz alta:** por qué los filtros viven en la URL; qué es "elevar el estado" en este catálogo; por qué el detalle restringido no puede confiar en ocultar elementos y la API no envía los datos; qué hace `useParams`.

---

# DÍA 3 · miércoles 28 oct · Formulario de iniciativa (corrige criterios 8 y 10)

### 3.1 Reglas de validación compartidas

Mismos límites que usará el servidor el día 6 (`LIMITES` se copia tal cual).

**`cliente/src/utils/validacion.js`**

```js
// Cada regla devuelve null si el valor es válido o un mensaje específico si no.
export const LIMITES = {
  title: [5, 80], summary: [10, 160], description: [20, 2000], problem: [10, 1000],
  beneficiaries: [5, 500], message: [15, 300], name: [5, 80], objective: [10, 300], bio: [0, 500],
};

export const REGEX_ETIQUETA = /^[\p{L}\p{N}][\p{L}\p{N}\- ]{1,29}$/u;
export const REGEX_DISPONIBILIDAD = /^\d{1,2}\s?h\/semana$/i;
export const REGEX_GITHUB = /^https:\/\/github\.com\/[\w.-]+\/[\w.-]+\/?$/;
export const REGEX_URL = /^https?:\/\/\S+$/;
export const REGEX_CORREO = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

const vacio = (v) => v === undefined || v === null || String(v).trim() === '' || (Array.isArray(v) && v.length === 0);

export const requerido = (v) => (vacio(v) ? 'Este campo es obligatorio.' : null);

export const longitud = (min, max) => (v) => {
  const n = String(v ?? '').trim().length;
  if (min && n < min) return `Debe tener al menos ${min} caracteres (faltan ${min - n}).`;
  if (max && n > max) return `No debe superar los ${max} caracteres.`;
  return null;
};

export const rango = (min, max) => (v) => {
  if (vacio(v)) return 'Este campo es obligatorio.';
  const n = Number(v);
  if (!Number.isInteger(n)) return 'Debe ser un número entero.';
  return n < min || n > max ? `Debe estar entre ${min} y ${max}.` : null;
};

export const seleccionValida = (permitidos) => (v) => {
  if (vacio(v)) return 'Elegí una opción.';
  return permitidos.includes(v) ? null : 'Elegí una opción válida.';
};

export const formato = (regex, mensaje) => (v) => (vacio(v) || regex.test(String(v).trim()) ? null : mensaje);

export const cantidad = (min, max) => (lista = []) => {
  if (lista.length < min) return `Agregá al menos ${min}.`;
  if (max && lista.length > max) return `Podés agregar como máximo ${max}.`;
  return null;
};

// Ejecuta reglas en orden y devuelve el primer error.
export const combinar = (...reglas) => (v, todos) => {
  for (const regla of reglas) {
    const mensaje = regla(v, todos);
    if (mensaje) return mensaje;
  }
  return null;
};

// La regla solo se aplica si el campo tiene valor.
export const opcional = (regla) => (v, todos) => (vacio(v) ? null : regla(v, todos));
```

### 3.2 Hook de formulario controlado

**`cliente/src/hooks/useFormulario.js`**

```js
import { useState } from 'react';

export function useFormulario(inicial, reglas) {
  const [valores, setValores] = useState(inicial);
  const [errores, setErrores] = useState({});
  const [tocados, setTocados] = useState({});
  const [enviado, setEnviado] = useState(false);

  const validar = (nombre, todos) => reglas[nombre]?.(todos[nombre], todos) ?? null;

  // Corrige el criterio 10: después del primer intento (o de salir del campo),
  // cada cambio revalida y el mensaje desaparece en cuanto el dato es válido.
  // También se revalidan los campos que ya tenían error, para reglas entre campos.
  function cambiar(nombre, valor) {
    const siguiente = { ...valores, [nombre]: valor };
    setValores(siguiente);
    setErrores((previos) => {
      const nuevos = { ...previos };
      for (const n of Object.keys(reglas)) {
        const revalidar = n === nombre ? tocados[n] || enviado : Boolean(previos[n]);
        if (revalidar) nuevos[n] = validar(n, siguiente);
      }
      return nuevos;
    });
  }

  function tocar(nombre) {
    setTocados((previos) => ({ ...previos, [nombre]: true }));
    setErrores((previos) => ({ ...previos, [nombre]: validar(nombre, valores) }));
  }

  function validarTodo() {
    setEnviado(true);
    const nuevos = Object.fromEntries(Object.keys(reglas).map((n) => [n, validar(n, valores)]));
    setErrores(nuevos);
    const primero = Object.keys(nuevos).find((n) => nuevos[n]);
    if (primero) document.getElementById(primero)?.focus();
    return !primero;
  }

  // Muestra en cada campo los detalles de un 400 del servidor.
  function erroresDelServidor(details = []) {
    setErrores((previos) => ({ ...previos, ...Object.fromEntries(details.map((d) => [d.field, d.message])) }));
  }

  function reiniciar(nuevos) {
    setValores(nuevos);
    setErrores({});
    setTocados({});
    setEnviado(false);
  }

  const campo = (nombre) => ({
    id: nombre,
    valor: valores[nombre],
    error: errores[nombre],
    onChange: (v) => cambiar(nombre, v),
    onBlur: () => tocar(nombre),
  });

  return { valores, errores, cambiar, validarTodo, erroresDelServidor, reiniciar, campo };
}
```

### 3.3 Campos reutilizables

**`cliente/src/components/ui/CampoTexto.jsx`**

Un solo componente para todos los campos de texto de la aplicación, así la ayuda de longitud es idéntica en todos los formularios.

```jsx
export default function CampoTexto({
  id, etiqueta, valor, onChange, onBlur, error, requerido = false, min, max,
  multilinea = false, filas = 3, tipo = 'text', placeholder, ayuda, className = 'mb-3', ...resto
}) {
  const largo = String(valor ?? '').trim().length;
  let contador = null;
  if (min && largo < min) contador = `Faltan ${min - largo} caracteres para el mínimo de ${min}.`;
  else if (max) contador = `${largo} / ${max}`;

  const idAyuda = `${id}-ayuda`;
  const idError = `${id}-error`;
  const describe = [ayuda || contador ? idAyuda : null, error ? idError : null].filter(Boolean).join(' ') || undefined;
  const control = {
    id, placeholder, onBlur,
    className: `form-control${error ? ' is-invalid' : ''}`,
    value: valor ?? '',
    onChange: (e) => onChange(e.target.value),
    'aria-invalid': Boolean(error),
    'aria-describedby': describe,
    'aria-required': requerido || undefined,
    ...resto,
  };

  return (
    <div className={className}>
      <label className="form-label" htmlFor={id}>
        {etiqueta}{requerido && <span className="text-danger" aria-hidden="true"> *</span>}
      </label>
      {multilinea ? <textarea rows={filas} {...control} /> : <input type={tipo} {...control} />}
      {(ayuda || contador) && (
        <div id={idAyuda} className="form-text d-flex justify-content-between gap-2">
          <span>{ayuda}</span>
          {contador && <span className={min && largo < min ? 'text-warning-emphasis' : ''}>{contador}</span>}
        </div>
      )}
      {error && (
        <div id={idError} className="invalid-feedback d-block">
          <i className="bi bi-exclamation-triangle-fill" aria-hidden="true"></i> {error}
        </div>
      )}
    </div>
  );
}
```

**`cliente/src/components/ui/CampoSelect.jsx`**

```jsx
export default function CampoSelect({
  id, etiqueta, valor, onChange, onBlur, error, requerido = false, opciones, vacio = 'Elegí una opción…', ayuda, className = 'mb-3',
}) {
  const idError = `${id}-error`;
  const idAyuda = `${id}-ayuda`;
  return (
    <div className={className}>
      <label className="form-label" htmlFor={id}>
        {etiqueta}{requerido && <span className="text-danger" aria-hidden="true"> *</span>}
      </label>
      <select
        id={id} className={`form-select${error ? ' is-invalid' : ''}`} value={valor ?? ''}
        onChange={(e) => onChange(e.target.value)} onBlur={onBlur}
        aria-invalid={Boolean(error)} aria-required={requerido || undefined}
        aria-describedby={[ayuda ? idAyuda : null, error ? idError : null].filter(Boolean).join(' ') || undefined}
      >
        <option value="">{vacio}</option>
        {opciones.map((o) => <option key={o.valor} value={o.valor}>{o.texto}</option>)}
      </select>
      {ayuda && <div id={idAyuda} className="form-text">{ayuda}</div>}
      {error && <div id={idError} className="invalid-feedback d-block"><i className="bi bi-exclamation-triangle-fill" aria-hidden="true"></i> {error}</div>}
    </div>
  );
}
```

**`cliente/src/components/ui/SelectorChips.jsx`**

Sirve para competencias (opciones de un catálogo) y para etiquetas o tecnologías (texto libre). Cada elemento es un chip removible.

```jsx
import { useState } from 'react';
import ModalConfirmacion from './ModalConfirmacion.jsx';

export default function SelectorChips({
  id, etiqueta, valores, onChange, opciones = [], libre = false, max, requerido = false, error, ayuda,
  confirmarAlQuitar = false, validarNuevo, placeholder, className = 'mb-3',
}) {
  const [entrada, setEntrada] = useState('');
  const [errorEntrada, setErrorEntrada] = useState(null);
  const [porQuitar, setPorQuitar] = useState(null);

  const nombre = (v) => (libre ? v : opciones.find((o) => o.valor === v)?.texto ?? v);
  const mensaje = errorEntrada ?? error;
  const idMensaje = `${id}-error`;
  const idAyuda = `${id}-ayuda`;

  function agregar() {
    const valor = entrada.trim();
    if (!valor) return setErrorEntrada(libre ? 'Escribí un valor antes de agregar.' : 'Elegí una opción antes de agregar.');
    if (valores.includes(valor)) return setErrorEntrada('Ya está en la lista.');
    if (max && valores.length >= max) return setErrorEntrada(`Podés agregar como máximo ${max}.`);
    const problema = validarNuevo?.(valor);
    if (problema) return setErrorEntrada(problema);
    onChange([...valores, valor]);
    setEntrada('');
    setErrorEntrada(null);
    return undefined;
  }

  function alTeclear(e) {
    if (libre && (e.key === 'Enter' || e.key === ',')) {
      e.preventDefault();
      agregar();
    }
  }

  function quitar(valor) {
    onChange(valores.filter((v) => v !== valor));
    setPorQuitar(null);
  }

  const propsControl = {
    id, value: entrada,
    className: `${libre ? 'form-control' : 'form-select'}${mensaje ? ' is-invalid' : ''}`,
    onChange: (e) => { setEntrada(e.target.value); setErrorEntrada(null); },
    'aria-invalid': Boolean(mensaje),
    'aria-describedby': [ayuda ? idAyuda : null, mensaje ? idMensaje : null].filter(Boolean).join(' ') || undefined,
  };

  return (
    <div className={className}>
      <label className="form-label" htmlFor={id}>
        {etiqueta}{requerido && <span className="text-danger" aria-hidden="true"> *</span>}
      </label>
      <div className="input-group">
        {libre ? (
          <input {...propsControl} onKeyDown={alTeclear} placeholder={placeholder} />
        ) : (
          <select {...propsControl}>
            <option value="">Elegí una opción…</option>
            {opciones.map((o) => <option key={o.valor} value={o.valor} disabled={valores.includes(o.valor)}>{o.texto}</option>)}
          </select>
        )}
        <button type="button" className="btn btn-outline-primary" onClick={agregar}>
          <i className="bi bi-plus-lg" aria-hidden="true"></i> Agregar
        </button>
      </div>
      {ayuda && <div id={idAyuda} className="form-text">{ayuda}{max ? ` (${valores.length} de ${max})` : ''}</div>}
      {mensaje && <div id={idMensaje} className="invalid-feedback d-block"><i className="bi bi-exclamation-triangle-fill" aria-hidden="true"></i> {mensaje}</div>}
      <ul className="lista-chips mt-2" aria-label={`${etiqueta}: elementos agregados`}>
        {valores.map((v) => (
          <li key={v} className="badge text-bg-primary competencia-elegida">
            {nombre(v)}
            <button
              type="button" className="btn-close btn-close-white" aria-label={`Quitar ${nombre(v)}`}
              onClick={() => (confirmarAlQuitar ? setPorQuitar(v) : quitar(v))}
            ></button>
          </li>
        ))}
        {valores.length === 0 && <li className="small text-body-secondary">Todavía no agregaste ninguno.</li>}
      </ul>

      <ModalConfirmacion
        abierto={porQuitar !== null}
        titulo="Quitar de la lista"
        textoConfirmar="Quitar"
        variante="primary"
        icono="bi-question-circle"
        onConfirmar={() => quitar(porQuitar)}
        onCancelar={() => setPorQuitar(null)}
      >
        <p className="mb-0">¿Querés quitar <strong>{porQuitar !== null && nombre(porQuitar)}</strong> de {etiqueta.toLowerCase()}?</p>
      </ModalConfirmacion>
    </div>
  );
}
```

**`cliente/src/components/ui/AccionesFormulario.jsx`**

```jsx
// Guardar y Cancelar son botones en todos los formularios; Cancelar navega al origen lógico.
export default function AccionesFormulario({ textoGuardar = 'Guardar', procesando = false, onCancelar }) {
  return (
    <div className="mt-4 d-flex flex-column flex-sm-row gap-2">
      <button type="submit" className="btn btn-primary" disabled={procesando}>
        {procesando
          ? <span className="spinner-border spinner-border-sm me-1" aria-hidden="true"></span>
          : <i className="bi bi-check2" aria-hidden="true"></i>} {textoGuardar}
      </button>
      <button type="button" className="btn btn-outline-secondary" onClick={onCancelar} disabled={procesando}>Cancelar</button>
    </div>
  );
}
```

```powershell
git add cliente
git commit -m "Agregar validación compartida, useFormulario y campos con contador y chips" --trailer "Seccion: Formularios" --trailer "Cambio: Errores que se limpian al corregir y ayuda de longitud uniforme"
```

### 3.4 Página del formulario (crear y editar)

**`cliente/src/features/iniciativas/FormularioIniciativaPage.jsx`**

```jsx
import { useEffect, useState } from 'react';
import { useNavigate, useParams } from 'react-router-dom';
import CampoTexto from '../../components/ui/CampoTexto.jsx';
import CampoSelect from '../../components/ui/CampoSelect.jsx';
import SelectorChips from '../../components/ui/SelectorChips.jsx';
import AccionesFormulario from '../../components/ui/AccionesFormulario.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useFormulario } from '../../hooks/useFormulario.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useToast } from '../../context/ToastContext.jsx';
import { useSesion } from '../../context/SesionContext.jsx';
import { listarCategorias, listarCompetencias } from '../../api/catalogos.js';
import { actualizarIniciativa, crearIniciativa, obtenerIniciativa } from '../../api/iniciativas.js';
import { opciones, TIPOS, VISIBILIDADES } from '../../utils/etiquetas.js';
import {
  LIMITES, REGEX_ETIQUETA, cantidad, combinar, formato, longitud, rango, requerido, seleccionValida,
} from '../../utils/validacion.js';

const VACIO = {
  title: '', type: '', summary: '', description: '', problem: '', beneficiaries: '',
  categoryId: '', estimatedMembers: '', visibility: '', requiredSkillIds: [], tags: [],
};
const TEXTOS = ['title', 'summary', 'description', 'problem', 'beneficiaries'];
const texto = (campo) => combinar(requerido, longitud(...LIMITES[campo]));

export default function FormularioIniciativaPage() {
  const { id } = useParams();
  const edicion = Boolean(id);
  const navigate = useNavigate();
  const mostrarToast = useToast();
  const { usuario } = useSesion();
  useTituloPagina(edicion ? 'Editar iniciativa' : 'Publicar iniciativa');

  const carga = useConsulta(
    (signal) => Promise.all([
      listarCategorias(signal),
      listarCompetencias(signal),
      edicion ? obtenerIniciativa(id, signal) : Promise.resolve(null),
    ]),
    [id]
  );
  const [categorias, competencias, original] = carga.datos ?? [[], [], null];
  const minimoIntegrantes = original?.memberCount ?? 1;

  const form = useFormulario(VACIO, {
    title: texto('title'),
    type: seleccionValida(Object.keys(TIPOS)),
    summary: texto('summary'),
    description: texto('description'),
    problem: texto('problem'),
    beneficiaries: texto('beneficiaries'),
    categoryId: seleccionValida(categorias.map((c) => c.id)),
    // Regla entre campos: no menos integrantes que el equipo actual.
    estimatedMembers: combinar(rango(1, 20), (v) => (Number(v) < minimoIntegrantes
      ? `El equipo ya tiene ${minimoIntegrantes} integrantes, incluido vos. No podés indicar menos.` : null)),
    visibility: seleccionValida(Object.keys(VISIBILIDADES)),
    requiredSkillIds: cantidad(1, 8),
    tags: cantidad(0, 5),
  });
  const [procesando, setProcesando] = useState(false);

  // En modo edición, cuando llega la iniciativa se copian sus valores al formulario.
  useEffect(() => {
    if (!original || original.accessLevel !== 'full') return;
    form.reiniciar({
      title: original.title, type: original.type, summary: original.summary, description: original.description,
      problem: original.problem, beneficiaries: original.beneficiaries, categoryId: original.category?.id ?? '',
      estimatedMembers: String(original.estimatedMembers), visibility: original.visibility,
      requiredSkillIds: original.requiredSkills.map((s) => s.id), tags: original.tags,
    });
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [original]);

  const cancelar = () => navigate(edicion ? `/iniciativas/${id}` : '/iniciativas');

  async function enviar(e) {
    e.preventDefault();
    if (!form.validarTodo()) return;
    const v = form.valores;
    const cuerpo = {
      ...v,
      ...Object.fromEntries(TEXTOS.map((c) => [c, v[c].trim()])),
      estimatedMembers: Number(v.estimatedMembers),
    };
    setProcesando(true);
    try {
      const guardada = edicion ? await actualizarIniciativa(id, cuerpo) : await crearIniciativa(cuerpo);
      mostrarToast(edicion ? 'Cambios guardados.' : 'Iniciativa publicada.');
      navigate(`/iniciativas/${guardada.id}`);
    } catch (error) {
      if (error.status === 400) form.erroresDelServidor(error.details);
      mostrarToast(error.message, 'danger');
      setProcesando(false);
    }
  }

  let bloqueo = null;
  if (original && original.owner?.id !== usuario.id) bloqueo = 'Solo el propietario puede modificar esta iniciativa.';
  else if (original?.status === 'archived') bloqueo = 'Una iniciativa archivada no se puede modificar.';

  return (
    <div className="container py-4" style={{ maxWidth: '56rem' }}>
      <h1 className="h3 mb-1">{edicion ? 'Editar iniciativa' : 'Publicar iniciativa'}</h1>
      <p className="text-body-secondary mb-4">
        Los campos marcados con <span className="text-danger" aria-hidden="true">*</span>
        <span className="visually-hidden">asterisco</span> son obligatorios.
      </p>

      {carga.cargando && <EstadoCarga texto="Cargando el formulario…" />}
      {carga.error?.status === 404 && <EstadoVacio titulo="No encontramos esa iniciativa" />}
      {carga.error && carga.error.status !== 404 && <EstadoError error={carga.error} onReintentar={carga.recargar} />}
      {bloqueo && <div className="alert alert-warning" role="status">{bloqueo}</div>}

      {carga.datos && !bloqueo && (
        <form className="formulario-tarjeta" noValidate onSubmit={enviar}>
          <div className="row g-3">
            <CampoTexto className="col-12 col-md-8" etiqueta="Título" requerido min={LIMITES.title[0]} max={LIMITES.title[1]} {...form.campo('title')} />
            <CampoSelect className="col-12 col-md-4" etiqueta="Tipo" requerido opciones={opciones(TIPOS)} vacio="Elegí un tipo…" {...form.campo('type')} />
            <CampoTexto className="col-12" etiqueta="Resumen" requerido min={LIMITES.summary[0]} max={LIMITES.summary[1]}
              placeholder="Una frase que explique la iniciativa" {...form.campo('summary')} />
            <CampoTexto className="col-12" etiqueta="Descripción detallada" requerido multilinea filas={4}
              min={LIMITES.description[0]} max={LIMITES.description[1]} {...form.campo('description')} />
            <CampoTexto className="col-12 col-md-6" etiqueta="Problema o situación identificada" requerido multilinea
              min={LIMITES.problem[0]} max={LIMITES.problem[1]} {...form.campo('problem')} />
            <CampoTexto className="col-12 col-md-6" etiqueta="Población o usuarios beneficiados" requerido multilinea
              min={LIMITES.beneficiaries[0]} max={LIMITES.beneficiaries[1]} {...form.campo('beneficiaries')} />
            <CampoSelect className="col-12 col-md-6" etiqueta="Categoría" requerido vacio="Elegí una categoría…"
              opciones={categorias.map((c) => ({ valor: c.id, texto: c.name }))} {...form.campo('categoryId')} />
            <CampoTexto className="col-6 col-md-3" etiqueta="Integrantes estimados" requerido tipo="number" inputMode="numeric"
              placeholder="1 a 20" ayuda={edicion ? `Mínimo ${minimoIntegrantes} (equipo actual)` : 'Incluyéndote'} {...form.campo('estimatedMembers')} />
            {/* Corrige el criterio 8: la visibilidad empieza vacía y es obligatoria. */}
            <CampoSelect className="col-6 col-md-3" etiqueta="Visibilidad" requerido vacio="Elegí…"
              opciones={opciones(VISIBILIDADES)} {...form.campo('visibility')} />
            <SelectorChips
              className="col-12" id="requiredSkillIds" etiqueta="Competencias necesarias" requerido max={8} confirmarAlQuitar
              opciones={competencias.map((c) => ({ valor: c.id, texto: c.name }))}
              valores={form.valores.requiredSkillIds} error={form.errores.requiredSkillIds}
              onChange={(lista) => form.cambiar('requiredSkillIds', lista)}
              ayuda="Elegí una competencia y presioná Agregar"
            />
            <SelectorChips
              className="col-12" id="tags" etiqueta="Etiquetas" libre max={5} placeholder="voluntariado"
              valores={form.valores.tags} error={form.errores.tags}
              onChange={(lista) => form.cambiar('tags', lista)}
              validarNuevo={formato(REGEX_ETIQUETA, 'Usá letras, números, espacios o guiones (2 a 30 caracteres).')}
              ayuda="Escribí una etiqueta y presioná Enter, coma o Agregar"
            />
          </div>
          <AccionesFormulario textoGuardar={edicion ? 'Guardar cambios' : 'Publicar iniciativa'} procesando={procesando} onCancelar={cancelar} />
        </form>
      )}
    </div>
  );
}
```

En **`App.jsx`** agregar:

```jsx
import RutaProtegida from './components/layout/RutaProtegida.jsx';
import FormularioIniciativaPage from './features/iniciativas/FormularioIniciativaPage.jsx';
// ...
        <Route path="iniciativas/nueva" element={<RutaProtegida><FormularioIniciativaPage /></RutaProtegida>} />
        <Route path="iniciativas/:id/editar" element={<RutaProtegida><FormularioIniciativaPage /></RutaProtegida>} />
```

### 3.5 Verificar

| Prueba | Esperado |
|---|---|
| Visitante abre "Publicar iniciativa" | "Necesitás una sesión" |
| Ana, enviar el formulario vacío | Todos los errores a la vez; el foco va a Título |
| Escribir 3 letras en Título | Ayuda "Faltan 2 caracteres para el mínimo de 5"; al llegar a 5, el error desaparece sin volver a enviar |
| Visibilidad | Empieza en "Elegí…" y es obligatoria |
| Agregar JavaScript dos veces | La opción ya agregada queda deshabilitada |
| Quitar una competencia | Modal "Quitar de la lista" |
| Etiqueta "#x" | "Usá letras, números…" bajo el campo |
| Sexta etiqueta | "Podés agregar como máximo 5." |
| Publicar completa | Toast "Iniciativa publicada." y abre su detalle |
| Ana edita "Plataforma de voluntariado", integrantes = 1 | "El equipo ya tiene 2 integrantes…" |
| Cancelar en edición | Vuelve al detalle de la iniciativa |
| Luis abre `/iniciativas/<id de Ana>/editar` | "Solo el propietario puede modificar esta iniciativa." |

```powershell
git add cliente
git commit -m "Agregar formulario controlado para crear y editar iniciativas" --trailer "Seccion: Formulario" --trailer "Cambio: Visibilidad vacía, etiquetas como chips, confirmación al quitar y Cancelar como botón"
git push
```

**Explicar en voz alta:** qué es un formulario controlado; cómo `useFormulario` decide cuándo revalidar; por qué `estimatedMembers` es una regla entre campos; por qué el cliente valida si el servidor también lo hará.

---

# DÍA 4 · jueves 29 oct · Perfil, solicitudes, proyectos y GitHub

Al terminar hoy, la conversión del Avance 1 a React está completa.

### 4.1 Perfil con pestañas accesibles

**`cliente/src/features/usuarios/TarjetaPerfil.jsx`**

```jsx
import Avatar from '../../components/ui/Avatar.jsx';
import Chip from '../../components/ui/Chip.jsx';
import { TIPOS_MIEMBRO } from '../../utils/etiquetas.js';

export default function TarjetaPerfil({ persona, propio = false, children }) {
  const disponible = persona.availability?.hoursPerWeek > 0;
  const tipo = TIPOS_MIEMBRO[persona.memberType];
  return (
    <div className="card">
      <div className="card-body">
        <div className="text-center">
          <Avatar nombre={persona.fullName} grande />
          <h2 className="h4 mt-3 mb-1">{persona.fullName}</h2>
          <div className="d-flex justify-content-center flex-wrap gap-2 mb-2">
            {tipo && <Chip icono={tipo.icono}>{tipo.texto}</Chip>}
            <span className={`badge ${disponible ? 'text-bg-success' : 'text-bg-secondary'}`}>
              <i className={`bi ${disponible ? 'bi-check-circle-fill' : 'bi-pause-circle'}`} aria-hidden="true"></i> {disponible ? 'Disponible' : 'Sin disponibilidad'}
            </span>
          </div>
          <p className="small text-body-secondary mb-1"><i className="bi bi-book" aria-hidden="true"></i> {persona.career}</p>
          <p className="small text-body-secondary mb-1">
            <i className="bi bi-clock" aria-hidden="true"></i> {disponible ? `${persona.availability.hoursPerWeek} h/semana · ${persona.availability.detail}` : 'No disponible por ahora'}
          </p>
          {propio && <p className="small text-body-secondary mb-1"><i className="bi bi-envelope" aria-hidden="true"></i> {persona.email}</p>}
          {persona.portfolioUrl && (
            <p className="small mb-2">
              <a href={persona.portfolioUrl} target="_blank" rel="noopener noreferrer">
                <i className="bi bi-link-45deg" aria-hidden="true"></i> {persona.portfolioUrl.replace(/^https?:\/\//, '')}
              </a>
            </p>
          )}
          <p className="small">{persona.bio || 'Sin descripción todavía.'}</p>
        </div>
        <h3 className="seccion-titulo mt-3">Competencias</h3>
        <ul className="lista-chips mb-3">
          {persona.skills.length
            ? persona.skills.map((s) => <li key={s.skillId}><Chip nivel={s.level}>{s.name}</Chip></li>)
            : <li className="small text-body-secondary">Sin competencias registradas.</li>}
        </ul>
        <h3 className="seccion-titulo">Intereses</h3>
        <ul className="lista-chips">
          {persona.interests.length
            ? persona.interests.map((i) => <li key={i}><Chip neutro>#{i}</Chip></li>)
            : <li className="small text-body-secondary">Sin intereses registrados.</li>}
        </ul>
        {children}
      </div>
    </div>
  );
}
```

**`cliente/src/features/usuarios/ListaIniciativasPerfil.jsx`**

Desde aquí también se elimina o archiva (corrige el criterio 9).

```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import { BadgeEstado, BadgeTipo } from '../../components/ui/Badges.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import Paginacion from '../../components/ui/Paginacion.jsx';
import AccionesPropietario from '../iniciativas/AccionesPropietario.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { listarIniciativas } from '../../api/iniciativas.js';

export default function ListaIniciativasPerfil({ filtro, conAcciones = false, textoVacio }) {
  const [pagina, setPagina] = useState(1);
  const { cargando, error, datos, recargar } = useConsulta(
    (signal) => listarIniciativas({ ...filtro, page: pagina, size: 8 }, signal),
    [JSON.stringify(filtro), pagina]
  );

  if (cargando) return <EstadoCarga />;
  if (error) return <EstadoError error={error} onReintentar={recargar} />;
  if (datos.items.length === 0) return <EstadoVacio titulo={textoVacio} />;

  return (
    <>
      {datos.items.map((ini) => (
        <div key={ini.id} className="fila-lista flex-wrap">
          <span>
            <Link className="fw-semibold d-block" to={`/iniciativas/${ini.id}`}>{ini.title}</Link>
            <span className="small text-body-secondary">
              <i className={`bi ${ini.category?.icon ?? 'bi-tag'}`} aria-hidden="true"></i> {ini.category?.name}
              {ini.accessLevel === 'full' && ` · ${ini.memberCount} de ${ini.estimatedMembers} integrantes previstos`}
            </span>
          </span>
          <span className="d-flex flex-wrap gap-2 align-items-center">
            <BadgeTipo tipo={ini.type} />
            {ini.accessLevel === 'full' && <BadgeEstado estado={ini.status} />}
            {conAcciones && <AccionesPropietario ini={ini} variante="tarjeta" onCambio={recargar} />}
          </span>
        </div>
      ))}
      <Paginacion pagina={datos.page} totalPaginas={datos.totalPages} onCambiar={setPagina} />
    </>
  );
}
```

**`cliente/src/features/usuarios/PerfilPage.jsx`**

Sirve para `/perfil` (propio) y `/usuarios/:id` (de otra persona).

```jsx
import { useRef, useState } from 'react';
import { useParams } from 'react-router-dom';
import TarjetaPerfil from './TarjetaPerfil.jsx';
import ListaIniciativasPerfil from './ListaIniciativasPerfil.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { obtenerPerfilPropio, obtenerUsuario } from '../../api/usuarios.js';

const PESTANAS = [
  { id: 'propias', texto: 'Mis iniciativas', icono: 'bi-lightbulb', filtro: { owner: 'me' }, acciones: true, vacio: 'Todavía no publicaste iniciativas' },
  { id: 'participo', texto: 'Participo en', icono: 'bi-people', filtro: { member: 'me' }, acciones: false, vacio: 'Todavía no formás parte de ningún equipo' },
];

function Pestanas() {
  const [activa, setActiva] = useState(PESTANAS[0].id);
  const refs = useRef({});

  // Patrón de pestañas WAI-ARIA: flechas para moverse, una sola pestaña en el orden de tabulación.
  function alTeclear(e) {
    if (!['ArrowRight', 'ArrowLeft'].includes(e.key)) return;
    e.preventDefault();
    const i = PESTANAS.findIndex((p) => p.id === activa);
    const siguiente = PESTANAS[(i + (e.key === 'ArrowRight' ? 1 : PESTANAS.length - 1)) % PESTANAS.length];
    setActiva(siguiente.id);
    refs.current[siguiente.id]?.focus();
  }

  const actual = PESTANAS.find((p) => p.id === activa);
  return (
    <>
      <div className="nav nav-tabs" role="tablist" aria-label="Actividad" onKeyDown={alTeclear}>
        {PESTANAS.map((p) => (
          <button
            key={p.id} id={`tab-${p.id}`} type="button" role="tab" ref={(el) => { refs.current[p.id] = el; }}
            className={`nav-link${p.id === activa ? ' active' : ''}`} aria-selected={p.id === activa}
            aria-controls="panel-perfil" tabIndex={p.id === activa ? 0 : -1} onClick={() => setActiva(p.id)}
          >
            <i className={`bi ${p.icono}`} aria-hidden="true"></i> {p.texto}
          </button>
        ))}
      </div>
      <div id="panel-perfil" role="tabpanel" aria-labelledby={`tab-${activa}`} tabIndex={0} className="pt-2">
        <ListaIniciativasPerfil key={actual.id} filtro={actual.filtro} conAcciones={actual.acciones} textoVacio={actual.vacio} />
      </div>
    </>
  );
}

export default function PerfilPage() {
  const { id } = useParams();
  const propio = !id;
  const { cargando, error, datos: persona, recargar } = useConsulta(
    (signal) => (propio ? obtenerPerfilPropio(signal) : obtenerUsuario(id, signal)),
    [id]
  );
  useTituloPagina(propio ? 'Mi perfil' : persona?.fullName ?? 'Perfil');

  return (
    <div className="container py-4">
      <h1 className="h3 mb-4">{propio ? 'Mi perfil' : 'Perfil'}</h1>
      {cargando && <EstadoCarga texto="Cargando perfil…" />}
      {error?.status === 404 && <EstadoVacio titulo="No encontramos a esa persona" />}
      {error && error.status !== 404 && <EstadoError error={error} onReintentar={recargar} />}
      {persona && (
        <div className="row g-4">
          <div className="col-md-5 col-lg-4">
            <TarjetaPerfil persona={persona} propio={propio} />
          </div>
          <div className="col-md-7 col-lg-8">
            {propio ? <Pestanas /> : (
              <>
                <h2 className="h5">Iniciativas que publicó</h2>
                <ListaIniciativasPerfil filtro={{ owner: persona.id }} textoVacio="No tiene iniciativas visibles para vos" />
              </>
            )}
          </div>
        </div>
      )}
    </div>
  );
}
```

```powershell
git add cliente
git commit -m "Agregar perfil propio y de otras personas con pestañas accesibles" --trailer "Seccion: Perfil" --trailer "Cambio: Eliminar y archivar también desde Mis iniciativas"
```

### 4.2 Formulario de solicitud (corrige el criterio 13)

**`cliente/src/features/solicitudes/FormularioSolicitudPage.jsx`**

Rutas: `/iniciativas/:id/solicitar` (crear) y `/solicitudes/:solicitudId/editar` (modificar mientras está pendiente).

```jsx
import { useEffect, useState } from 'react';
import { Link, useNavigate, useParams } from 'react-router-dom';
import CampoTexto from '../../components/ui/CampoTexto.jsx';
import CampoSelect from '../../components/ui/CampoSelect.jsx';
import AccionesFormulario from '../../components/ui/AccionesFormulario.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import Chip from '../../components/ui/Chip.jsx';
import { BadgeTipo } from '../../components/ui/Badges.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useFormulario } from '../../hooks/useFormulario.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useSesion } from '../../context/SesionContext.jsx';
import { useToast } from '../../context/ToastContext.jsx';
import { obtenerIniciativa, crearSolicitud } from '../../api/iniciativas.js';
import { obtenerSolicitud, actualizarSolicitud } from '../../api/solicitudes.js';
import { listarRoles } from '../../api/catalogos.js';
import { NIVELES } from '../../utils/etiquetas.js';
import { LIMITES, REGEX_DISPONIBILIDAD, combinar, formato, longitud, requerido, seleccionValida } from '../../utils/validacion.js';

export default function FormularioSolicitudPage() {
  const { id, solicitudId } = useParams();
  const edicion = Boolean(solicitudId);
  const navigate = useNavigate();
  const mostrarToast = useToast();
  const { usuario } = useSesion();
  useTituloPagina(edicion ? 'Modificar solicitud' : 'Solicitar participación');

  const carga = useConsulta(async (signal) => {
    const solicitud = edicion ? await obtenerSolicitud(solicitudId, signal) : null;
    const [ini, roles] = await Promise.all([obtenerIniciativa(edicion ? solicitud.initiative.id : id, signal), listarRoles(signal)]);
    return { solicitud, ini, roles };
  }, [id, solicitudId]);
  const { solicitud, ini, roles } = carga.datos ?? { roles: [] };

  // Solo las competencias que la iniciativa requiere y que el usuario tiene.
  const ofrecibles = ini ? usuario.skills.filter((s) => ini.requiredSkills.some((r) => r.id === s.skillId)) : [];

  const form = useFormulario(
    { message: '', offeredSkillId: '', roleId: '', availability: '' },
    {
      message: combinar(requerido, longitud(...LIMITES.message)),
      offeredSkillId: seleccionValida(ofrecibles.map((s) => s.skillId)),
      roleId: seleccionValida(roles.map((r) => r.id)),
      availability: combinar(requerido, formato(REGEX_DISPONIBILIDAD, 'Usá el formato "N h/semana", por ejemplo 4 h/semana.')),
    }
  );
  const [procesando, setProcesando] = useState(false);

  useEffect(() => {
    if (!carga.datos) return;
    form.reiniciar(solicitud
      ? { message: solicitud.message, offeredSkillId: solicitud.offeredSkill.id, roleId: solicitud.role.id, availability: solicitud.availability }
      : { message: '', offeredSkillId: '', roleId: '', availability: usuario.availability?.hoursPerWeek ? `${usuario.availability.hoursPerWeek} h/semana` : '' });
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [carga.datos]);

  if (carga.cargando) return <div className="container py-4"><EstadoCarga /></div>;
  if (carga.error) return <div className="container py-4"><EstadoError error={carga.error} onReintentar={carga.recargar} /></div>;

  const volver = () => navigate(`/iniciativas/${ini.id}`); // Cancelar vuelve a la iniciativa, no al catálogo
  const completa = ini.accessLevel === 'full';
  let bloqueo = null;
  if (edicion && solicitud.status !== 'pending') bloqueo = 'Solo las solicitudes pendientes pueden modificarse.';
  else if (!edicion && completa && ini.owner.id === usuario.id) bloqueo = 'No podés solicitar participación en tu propia iniciativa.';
  else if (!edicion && completa && ini.members.some((m) => m.userId === usuario.id)) bloqueo = 'Ya formás parte del equipo de esta iniciativa.';
  else if (!edicion && completa && !['published', 'forming'].includes(ini.status)) bloqueo = 'Esta iniciativa ya no recibe solicitudes.';
  else if (!edicion && completa && ini.availableSpots === 0) bloqueo = 'La iniciativa ya completó los espacios previstos.';

  async function enviar(e) {
    e.preventDefault();
    if (!form.validarTodo()) return;
    const cuerpo = { ...form.valores, message: form.valores.message.trim(), availability: form.valores.availability.trim() };
    setProcesando(true);
    try {
      if (edicion) {
        await actualizarSolicitud(solicitudId, cuerpo);
        mostrarToast('Solicitud actualizada.');
        navigate('/solicitudes');
      } else {
        await crearSolicitud(ini.id, cuerpo);
        mostrarToast('Solicitud enviada. Queda pendiente hasta que el propietario la resuelva.');
        volver();
      }
    } catch (error) {
      if (error.status === 400) form.erroresDelServidor(error.details);
      mostrarToast(error.message, 'danger');
      setProcesando(false);
    }
  }

  return (
    <div className="container py-4">
      <h1 className="h3 mb-4">{edicion ? 'Modificar solicitud' : 'Solicitar participación'}</h1>
      <div className="row g-4">
        <div className="col-lg-7">
          {bloqueo && <div className="alert alert-warning" role="status">{bloqueo} <Link to={`/iniciativas/${ini.id}`}>Volver a la iniciativa</Link></div>}
          {!bloqueo && ofrecibles.length === 0 && (
            <EstadoVacio titulo="No tenés competencias que esta iniciativa requiera" accion={<Link className="btn btn-outline-primary btn-sm" to="/perfil">Ir a mi perfil</Link>}>
              Agregá a tu perfil alguna de las competencias que se buscan para poder postularte.
            </EstadoVacio>
          )}
          {!bloqueo && ofrecibles.length > 0 && (
            <form className="formulario-tarjeta" noValidate onSubmit={enviar}>
              <CampoTexto etiqueta="Mensaje de presentación" requerido multilinea filas={4} min={LIMITES.message[0]} max={LIMITES.message[1]}
                placeholder="Contá por qué querés participar y qué podés aportar." {...form.campo('message')} />
              <div className="row g-3">
                <CampoSelect className="col-md-6" etiqueta="Competencia principal que aportás" requerido vacio="Elegí una competencia…"
                  ayuda="Solo las que la iniciativa requiere y tenés en tu perfil."
                  opciones={ofrecibles.map((s) => ({ valor: s.skillId, texto: `${s.name} · ${NIVELES[s.level]}` }))} {...form.campo('offeredSkillId')} />
                <CampoSelect className="col-md-6" etiqueta="Rol que querés desempeñar" requerido vacio="Elegí un rol…"
                  opciones={roles.map((r) => ({ valor: r.id, texto: r.name }))} {...form.campo('roleId')} />
              </div>
              <CampoTexto etiqueta="Disponibilidad" requerido placeholder="4 h/semana" ayuda='Número de horas seguido de "h/semana".' {...form.campo('availability')} />
              <AccionesFormulario textoGuardar={edicion ? 'Guardar cambios' : 'Enviar solicitud'} procesando={procesando} onCancelar={volver} />
            </form>
          )}
        </div>
        <aside className="col-lg-5">
          <div className="card tarjeta-lateral">
            <div className="card-body">
              <h2 className="seccion-titulo">Iniciativa</h2>
              <p className="fw-semibold mb-2"><Link to={`/iniciativas/${ini.id}`}>{ini.title}</Link></p>
              <div className="d-flex flex-wrap gap-2 mb-3">
                <BadgeTipo tipo={ini.type} />
                {completa && <Chip icono="bi-person-plus">{ini.availableSpots} espacios libres</Chip>}
              </div>
              <h2 className="seccion-titulo">Buscan</h2>
              <ul className="lista-chips mb-3">
                {ini.requiredSkills.map((s) => <li key={s.id}><Chip icono="bi-stars">{s.name}</Chip></li>)}
              </ul>
              <p className="small text-body-secondary mb-0">
                <i className="bi bi-person" aria-hidden="true"></i> Tu solicitud llegará a {completa ? <strong>{ini.owner.fullName}</strong> : 'el propietario'}.
              </p>
            </div>
          </div>
        </aside>
      </div>
    </div>
  );
}
```

```powershell
git add cliente
git commit -m "Agregar formulario de solicitud con competencias en intersección y catálogo de roles" --trailer "Seccion: Solicitudes" --trailer "Cambio: Correcciones del criterio 13 del Avance 1"
```

### 4.3 Mis solicitudes y solicitudes recibidas

**`cliente/src/features/solicitudes/SolicitudesLayout.jsx`**

Rutas anidadas: las dos listas comparten el encabezado y las pestañas, y cada una se dibuja en el `<Outlet>`.

```jsx
import { NavLink, Outlet } from 'react-router-dom';

export default function SolicitudesLayout() {
  return (
    <div className="container py-4">
      <h1 className="h3 mb-3">Solicitudes de participación</h1>
      <nav className="nav nav-tabs mb-3" aria-label="Tipo de solicitudes">
        <NavLink className="nav-link" to="/solicitudes" end><i className="bi bi-send" aria-hidden="true"></i> Enviadas</NavLink>
        <NavLink className="nav-link" to="/solicitudes/recibidas"><i className="bi bi-inbox" aria-hidden="true"></i> Recibidas</NavLink>
      </nav>
      <Outlet />
    </div>
  );
}
```

**`cliente/src/features/solicitudes/MisSolicitudesPage.jsx`**

```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import { BadgeSolicitud } from '../../components/ui/Badges.jsx';
import ModalConfirmacion from '../../components/ui/ModalConfirmacion.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import Paginacion from '../../components/ui/Paginacion.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useToast } from '../../context/ToastContext.jsx';
import { cancelarSolicitud, listarMisSolicitudes } from '../../api/solicitudes.js';
import { ESTADOS_SOLICITUD, opciones } from '../../utils/etiquetas.js';
import { formatearFecha } from '../../utils/fechas.js';

export default function MisSolicitudesPage() {
  useTituloPagina('Mis solicitudes');
  const mostrarToast = useToast();
  const [estado, setEstado] = useState('');
  const [pagina, setPagina] = useState(1);
  const [porCancelar, setPorCancelar] = useState(null);
  const [procesando, setProcesando] = useState(false);
  const { cargando, error, datos, recargar } = useConsulta(
    (signal) => listarMisSolicitudes({ status: estado, page: pagina, size: 10 }, signal),
    [estado, pagina]
  );

  async function confirmarCancelacion() {
    setProcesando(true);
    try {
      await cancelarSolicitud(porCancelar.id);
      mostrarToast('Solicitud cancelada.', 'secondary');
      recargar();
    } catch (e) {
      mostrarToast(e.message, 'danger');
    } finally {
      setPorCancelar(null);
      setProcesando(false);
    }
  }

  return (
    <>
      <div className="row g-2 mb-3">
        <div className="col-sm-6 col-lg-3">
          <label className="form-label" htmlFor="f-estado">Estado</label>
          <select id="f-estado" className="form-select" value={estado} onChange={(e) => { setEstado(e.target.value); setPagina(1); }}>
            <option value="">Todos</option>
            {opciones(ESTADOS_SOLICITUD).map((o) => <option key={o.valor} value={o.valor}>{o.texto}</option>)}
          </select>
        </div>
      </div>

      {cargando && <EstadoCarga texto="Cargando tus solicitudes…" />}
      {error && <EstadoError error={error} onReintentar={recargar} />}
      {datos && !cargando && datos.items.length === 0 && (
        <EstadoVacio titulo="No hay solicitudes para mostrar" accion={<Link className="btn btn-outline-primary btn-sm" to="/iniciativas">Explorar iniciativas</Link>} />
      )}
      {datos && !cargando && datos.items.map((s) => (
        <article key={s.id} className="card mb-3">
          <div className="card-body">
            <div className="d-flex flex-wrap justify-content-between gap-2">
              <h2 className="h6 mb-1"><Link to={`/iniciativas/${s.initiative.id}`}>{s.initiative.title}</Link></h2>
              <BadgeSolicitud estado={s.status} />
            </div>
            <p className="small text-body-secondary mb-2">
              {s.role.name} · {s.offeredSkill.name} · {s.availability} · enviada {formatearFecha(s.createdAt)}
            </p>
            <p className="mb-2">{s.message}</p>
            {s.decision?.comment && <p className="small mb-2"><strong>Respuesta del propietario:</strong> {s.decision.comment}</p>}
            {s.status === 'pending' && (
              <div className="d-flex gap-2">
                <Link className="btn btn-sm btn-outline-primary" to={`/solicitudes/${s.id}/editar`}><i className="bi bi-pencil" aria-hidden="true"></i> Modificar</Link>
                <button type="button" className="btn btn-sm btn-outline-danger" onClick={() => setPorCancelar(s)}><i className="bi bi-x-circle" aria-hidden="true"></i> Cancelar solicitud</button>
              </div>
            )}
          </div>
        </article>
      ))}
      {datos && !cargando && <Paginacion pagina={datos.page} totalPaginas={datos.totalPages} onCambiar={setPagina} />}

      <ModalConfirmacion
        abierto={!!porCancelar} titulo="Cancelar solicitud" textoConfirmar="Cancelar solicitud" procesando={procesando}
        onConfirmar={confirmarCancelacion} onCancelar={() => setPorCancelar(null)}
      >
        <p className="mb-0">¿Cancelar tu solicitud para <strong>{porCancelar?.initiative.title}</strong>? Quedará registrada como cancelada.</p>
      </ModalConfirmacion>
    </>
  );
}
```

**`cliente/src/features/solicitudes/SolicitudesRecibidasPage.jsx`**

```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import { BadgeSolicitud } from '../../components/ui/Badges.jsx';
import Avatar from '../../components/ui/Avatar.jsx';
import Chip from '../../components/ui/Chip.jsx';
import Modal from '../../components/ui/Modal.jsx';
import CampoTexto from '../../components/ui/CampoTexto.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import Paginacion from '../../components/ui/Paginacion.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useToast } from '../../context/ToastContext.jsx';
import { decidirSolicitud, listarSolicitudesRecibidas } from '../../api/solicitudes.js';
import { listarIniciativas } from '../../api/iniciativas.js';
import { listarCompetencias, listarRoles } from '../../api/catalogos.js';
import { ESTADOS_SOLICITUD, opciones } from '../../utils/etiquetas.js';
import { formatearFecha } from '../../utils/fechas.js';

const FILTROS_INICIALES = { initiativeId: '', status: 'pending', skill: '', role: '', from: '', to: '' };

export default function SolicitudesRecibidasPage() {
  useTituloPagina('Solicitudes recibidas');
  const mostrarToast = useToast();
  const [filtros, setFiltros] = useState(FILTROS_INICIALES);
  const [pagina, setPagina] = useState(1);
  const [decision, setDecision] = useState(null); // { solicitud, tipo }
  const [comentario, setComentario] = useState('');
  const [procesando, setProcesando] = useState(false);

  const opcionesFiltro = useConsulta(
    (signal) => Promise.all([listarIniciativas({ owner: 'me', size: 50 }, signal), listarCompetencias(signal), listarRoles(signal)]),
    []
  );
  const { cargando, error, datos, recargar } = useConsulta(
    (signal) => listarSolicitudesRecibidas({ ...filtros, page: pagina, size: 10 }, signal),
    [JSON.stringify(filtros), pagina]
  );
  const [propias, competencias, roles] = opcionesFiltro.datos ?? [{ items: [] }, [], []];

  const cambiar = (campo) => (e) => { setFiltros((f) => ({ ...f, [campo]: e.target.value })); setPagina(1); };

  async function confirmarDecision() {
    setProcesando(true);
    try {
      await decidirSolicitud(decision.solicitud.id, decision.tipo, comentario.trim() || undefined);
      mostrarToast(decision.tipo === 'accepted'
        ? `${decision.solicitud.applicant.fullName} se incorporó al equipo.`
        : 'Solicitud rechazada.', decision.tipo === 'accepted' ? 'success' : 'secondary');
      recargar();
    } catch (e) {
      mostrarToast(e.message, 'danger');
    } finally {
      setDecision(null);
      setComentario('');
      setProcesando(false);
    }
  }

  const select = (id, etiqueta, valor, lista, todos = 'Todas') => (
    <div className="col-6 col-lg-2">
      <label className="form-label" htmlFor={id}>{etiqueta}</label>
      <select id={id} className="form-select" value={filtros[valor]} onChange={cambiar(valor)}>
        <option value="">{todos}</option>
        {lista.map((o) => <option key={o.valor} value={o.valor}>{o.texto}</option>)}
      </select>
    </div>
  );

  return (
    <>
      <form className="formulario-tarjeta row g-2 mb-3" aria-label="Filtrar solicitudes recibidas" onSubmit={(e) => e.preventDefault()}>
        <div className="col-12 col-lg-4">
          <label className="form-label" htmlFor="r-iniciativa">Iniciativa</label>
          <select id="r-iniciativa" className="form-select" value={filtros.initiativeId} onChange={cambiar('initiativeId')}>
            <option value="">Todas mis iniciativas</option>
            {propias.items.map((i) => <option key={i.id} value={i.id}>{i.title}</option>)}
          </select>
        </div>
        {select('r-estado', 'Estado', 'status', opciones(ESTADOS_SOLICITUD), 'Todos')}
        {select('r-competencia', 'Competencia', 'skill', competencias.map((c) => ({ valor: c.id, texto: c.name })))}
        {select('r-rol', 'Rol', 'role', roles.map((r) => ({ valor: r.id, texto: r.name })), 'Todos')}
        <div className="col-6 col-lg-1">
          <label className="form-label" htmlFor="r-desde">Desde</label>
          <input id="r-desde" type="date" className="form-control" value={filtros.from} onChange={cambiar('from')} />
        </div>
        <div className="col-6 col-lg-1">
          <label className="form-label" htmlFor="r-hasta">Hasta</label>
          <input id="r-hasta" type="date" className="form-control" value={filtros.to} onChange={cambiar('to')} />
        </div>
      </form>

      {cargando && <EstadoCarga texto="Cargando solicitudes recibidas…" />}
      {error && <EstadoError error={error} onReintentar={recargar} />}
      {datos && !cargando && datos.items.length === 0 && <EstadoVacio titulo="No hay solicitudes con esos filtros" />}
      {datos && !cargando && datos.items.map((s) => (
        <article key={s.id} className="card mb-3">
          <div className="card-body">
            <div className="d-flex flex-wrap justify-content-between gap-2 mb-2">
              <div className="d-flex align-items-center gap-2">
                <Avatar nombre={s.applicant.fullName} />
                <div>
                  <Link className="fw-semibold" to={`/usuarios/${s.applicant.id}`}>{s.applicant.fullName}</Link>
                  <span className="d-block small text-body-secondary">{s.applicant.career} · para <Link to={`/iniciativas/${s.initiative.id}`}>{s.initiative.title}</Link></span>
                </div>
              </div>
              <BadgeSolicitud estado={s.status} />
            </div>
            <p className="mb-2">{s.message}</p>
            <p className="small text-body-secondary mb-2">
              Aporta <strong>{s.offeredSkill.name}</strong> como <strong>{s.role.name}</strong> · {s.availability} · {formatearFecha(s.createdAt)}
            </p>
            <ul className="lista-chips mb-3" aria-label="Competencias del solicitante">
              {s.applicant.skills.map((c) => <li key={c.skillId}><Chip nivel={c.level}>{c.name}</Chip></li>)}
            </ul>
            {s.status === 'pending' && (
              <div className="d-flex gap-2">
                <button type="button" className="btn btn-sm btn-success" onClick={() => setDecision({ solicitud: s, tipo: 'accepted' })}><i className="bi bi-check-lg" aria-hidden="true"></i> Aceptar</button>
                <button type="button" className="btn btn-sm btn-outline-danger" onClick={() => setDecision({ solicitud: s, tipo: 'rejected' })}><i className="bi bi-x-lg" aria-hidden="true"></i> Rechazar</button>
              </div>
            )}
          </div>
        </article>
      ))}
      {datos && !cargando && <Paginacion pagina={datos.page} totalPaginas={datos.totalPages} onCambiar={setPagina} />}

      <Modal
        abierto={!!decision} id="decision" onCerrar={() => !procesando && setDecision(null)}
        titulo={decision?.tipo === 'accepted' ? 'Aceptar solicitud' : 'Rechazar solicitud'}
        pie={
          <>
            <button type="button" className="btn btn-outline-secondary" onClick={() => setDecision(null)} disabled={procesando}>Cancelar</button>
            <button type="button" className={`btn btn-${decision?.tipo === 'accepted' ? 'success' : 'danger'}`} onClick={confirmarDecision} disabled={procesando}>
              {decision?.tipo === 'accepted' ? 'Aceptar e incorporar' : 'Rechazar'}
            </button>
          </>
        }
      >
        <p>
          {decision?.tipo === 'accepted'
            ? <><strong>{decision?.solicitud.applicant.fullName}</strong> se incorporará automáticamente al equipo.</>
            : <>La solicitud de <strong>{decision?.solicitud.applicant.fullName}</strong> quedará rechazada.</>}
        </p>
        <CampoTexto id="comentario" etiqueta="Comentario (opcional)" multilinea max={300} valor={comentario} onChange={setComentario} className="mb-0" />
      </Modal>
    </>
  );
}
```

```powershell
git add cliente
git commit -m "Agregar mis solicitudes y solicitudes recibidas con filtros y resolución" --trailer "Seccion: Solicitudes" --trailer "Cambio: Modificar, cancelar, aceptar y rechazar con confirmación"
```

### 4.4 Proyectos y API de GitHub

**`cliente/src/api/github.js`**

El consumo de GitHub se hace desde el cliente, como pide la consigna. La URL base viene de `VITE_GITHUB_API_URL`. No se usa token: toda variable `VITE_` termina visible en el navegador.

```js
const BASE = import.meta.env.VITE_GITHUB_API_URL;

export class ErrorGithub extends Error {
  constructor(tipo, mensaje) {
    super(mensaje);
    this.tipo = tipo; // 'inexistente' | 'privado' | 'limite' | 'no-disponible' | 'invalida' | 'http'
  }
}

export function extraerRepositorio(url) {
  const m = /^https:\/\/github\.com\/([\w.-]+)\/([\w.-]+?)(?:\.git)?\/?$/.exec(url ?? '');
  return m ? { owner: m[1], repo: m[2] } : null;
}

export async function obtenerRepositorio(url, signal) {
  const partes = extraerRepositorio(url);
  if (!partes) throw new ErrorGithub('invalida', 'La dirección no tiene el formato https://github.com/usuario/repositorio.');

  let respuesta;
  try {
    respuesta = await fetch(`${BASE}/repos/${partes.owner}/${partes.repo}`, { headers: { Accept: 'application/vnd.github+json' }, signal });
  } catch (error) {
    if (error.name === 'AbortError') throw error;
    throw new ErrorGithub('no-disponible', 'No se pudo contactar a GitHub. Revisá la conexión o intentá más tarde.');
  }

  if (respuesta.status === 404) {
    throw new ErrorGithub('inexistente', 'El repositorio no existe o es privado. Sin credenciales, GitHub responde 404 en ambos casos.');
  }
  if ((respuesta.status === 403 || respuesta.status === 429) && respuesta.headers.get('x-ratelimit-remaining') === '0') {
    const reinicio = Number(respuesta.headers.get('x-ratelimit-reset')) * 1000;
    const hora = reinicio ? new Date(reinicio).toLocaleTimeString('es-CR', { hour: '2-digit', minute: '2-digit' }) : 'más tarde';
    throw new ErrorGithub('limite', `Se alcanzó el límite de consultas a GitHub. Podés intentar de nuevo a las ${hora}.`);
  }
  if (respuesta.status === 401 || respuesta.status === 403) throw new ErrorGithub('privado', 'GitHub negó el acceso: el repositorio es privado o requiere permisos.');
  if (respuesta.status >= 500) throw new ErrorGithub('no-disponible', 'GitHub no está disponible en este momento.');
  if (!respuesta.ok) throw new ErrorGithub('http', `GitHub respondió con un error inesperado (${respuesta.status}).`);

  let datos;
  try {
    datos = await respuesta.json();
  } catch {
    throw new ErrorGithub('invalida', 'GitHub devolvió una respuesta que no se pudo interpretar.');
  }
  if (!datos || typeof datos.name !== 'string' || !datos.owner?.login) {
    throw new ErrorGithub('invalida', 'La respuesta de GitHub no tiene los datos esperados.');
  }
  if (datos.private) throw new ErrorGithub('privado', 'El repositorio es privado.');

  return {
    nombre: datos.name, propietario: datos.owner.login, descripcion: datos.description, lenguaje: datos.language,
    rama: datos.default_branch, actualizado: datos.pushed_at ?? datos.updated_at, issuesAbiertos: datos.open_issues_count,
    enlace: datos.html_url,
  };
}
```

**`cliente/src/features/proyectos/RepositorioGithub.jsx`**

```jsx
import { useConsulta } from '../../hooks/useConsulta.js';
import { obtenerRepositorio } from '../../api/github.js';
import { formatearFecha } from '../../utils/fechas.js';

const ICONOS_ERROR = {
  inexistente: 'bi-question-circle', privado: 'bi-lock-fill', limite: 'bi-hourglass-split',
  'no-disponible': 'bi-cloud-slash', invalida: 'bi-file-earmark-x', http: 'bi-exclamation-triangle',
};

export default function RepositorioGithub({ url }) {
  const { cargando, error, datos, recargar } = useConsulta((signal) => obtenerRepositorio(url, signal), [url]);

  return (
    <section className="card" aria-labelledby="t-repo">
      <div className="card-body">
        <h2 id="t-repo" className="h5"><i className="bi bi-github" aria-hidden="true"></i> Repositorio</h2>
        {cargando && <p className="small text-body-secondary mb-0" role="status"><span className="spinner-border spinner-border-sm me-1" aria-hidden="true"></span> Consultando GitHub…</p>}
        {error && (
          <div className="alert alert-warning d-flex gap-2 mb-0" role="alert">
            <i className={`bi ${ICONOS_ERROR[error.tipo] ?? 'bi-exclamation-triangle'}`} aria-hidden="true"></i>
            <div>
              {error.message}
              {['limite', 'no-disponible'].includes(error.tipo) && (
                <button type="button" className="btn btn-link btn-sm p-0 ms-2 align-baseline" onClick={recargar}>Reintentar</button>
              )}
            </div>
          </div>
        )}
        {datos && (
          <dl className="row mb-0 small">
            <dt className="col-sm-5">Nombre</dt><dd className="col-sm-7">{datos.nombre}</dd>
            <dt className="col-sm-5">Propietario</dt><dd className="col-sm-7">{datos.propietario}</dd>
            <dt className="col-sm-5">Descripción</dt><dd className="col-sm-7">{datos.descripcion || 'Sin descripción'}</dd>
            <dt className="col-sm-5">Lenguaje principal</dt><dd className="col-sm-7">{datos.lenguaje || 'No indicado'}</dd>
            <dt className="col-sm-5">Rama principal</dt><dd className="col-sm-7">{datos.rama}</dd>
            <dt className="col-sm-5">Última actualización</dt><dd className="col-sm-7">{formatearFecha(datos.actualizado)}</dd>
            <dt className="col-sm-5">Issues abiertos</dt><dd className="col-sm-7">{datos.issuesAbiertos}</dd>
            <dt className="col-sm-5">Enlace</dt>
            <dd className="col-sm-7"><a href={datos.enlace} target="_blank" rel="noopener noreferrer">Abrir en GitHub <i className="bi bi-box-arrow-up-right" aria-hidden="true"></i></a></dd>
          </dl>
        )}
      </div>
    </section>
  );
}
```

**`cliente/src/features/proyectos/ConvertirProyectoPage.jsx`**

```jsx
import { useEffect, useState } from 'react';
import { useNavigate, useParams } from 'react-router-dom';
import CampoTexto from '../../components/ui/CampoTexto.jsx';
import SelectorChips from '../../components/ui/SelectorChips.jsx';
import AccionesFormulario from '../../components/ui/AccionesFormulario.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useFormulario } from '../../hooks/useFormulario.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useSesion } from '../../context/SesionContext.jsx';
import { useToast } from '../../context/ToastContext.jsx';
import { convertirEnProyecto, obtenerIniciativa } from '../../api/iniciativas.js';
import { LIMITES, REGEX_GITHUB, cantidad, combinar, formato, longitud, opcional, requerido } from '../../utils/validacion.js';

export default function ConvertirProyectoPage() {
  const { id } = useParams();
  const navigate = useNavigate();
  const mostrarToast = useToast();
  const { usuario } = useSesion();
  useTituloPagina('Convertir en proyecto');
  const { cargando, error, datos: ini, recargar } = useConsulta((signal) => obtenerIniciativa(id, signal), [id]);
  const [procesando, setProcesando] = useState(false);

  const form = useFormulario(
    { name: '', objective: '', description: '', technologies: [], startDate: '', estimatedEndDate: '', repositoryUrl: '' },
    {
      name: combinar(requerido, longitud(...LIMITES.name)),
      objective: combinar(requerido, longitud(...LIMITES.objective)),
      description: combinar(requerido, longitud(...LIMITES.description)),
      technologies: cantidad(0, 10),
      startDate: requerido,
      // Regla entre campos: la finalización estimada no puede ser anterior al inicio.
      estimatedEndDate: opcional((v, todos) => (todos.startDate && v < todos.startDate ? 'Debe ser igual o posterior a la fecha de inicio.' : null)),
      repositoryUrl: opcional(formato(REGEX_GITHUB, 'Usá el formato https://github.com/usuario/repositorio.')),
    }
  );

  useEffect(() => {
    if (ini?.accessLevel === 'full') {
      form.reiniciar({ ...form.valores, name: ini.title, description: ini.description, startDate: new Date().toISOString().slice(0, 10) });
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [ini]);

  if (cargando) return <div className="container py-4"><EstadoCarga /></div>;
  if (error) return <div className="container py-4"><EstadoError error={error} onReintentar={recargar} /></div>;

  let bloqueo = null;
  if (ini.accessLevel !== 'full' || ini.owner.id !== usuario.id) bloqueo = 'Solo el propietario puede convertir la iniciativa en proyecto.';
  else if (ini.status === 'project') bloqueo = 'Esta iniciativa ya se convirtió en proyecto.';
  else if (ini.members.length === 0) bloqueo = 'Se necesita al menos un integrante además del propietario (RN-17).';

  async function enviar(e) {
    e.preventDefault();
    if (!form.validarTodo()) return;
    const v = form.valores;
    setProcesando(true);
    try {
      const proyecto = await convertirEnProyecto(id, {
        ...v, name: v.name.trim(), objective: v.objective.trim(), description: v.description.trim(),
        estimatedEndDate: v.estimatedEndDate || null, repositoryUrl: v.repositoryUrl.trim() || null,
      });
      mostrarToast('La iniciativa ahora es un proyecto.', 'info');
      navigate(`/proyectos/${proyecto.id}`);
    } catch (err) {
      if (err.status === 400) form.erroresDelServidor(err.details);
      mostrarToast(err.message, 'danger');
      setProcesando(false);
    }
  }

  return (
    <div className="container py-4" style={{ maxWidth: '48rem' }}>
      <h1 className="h3 mb-1">Convertir en proyecto</h1>
      <p className="text-body-secondary mb-4">Iniciativa de origen: <strong>{ini.title}</strong></p>
      {bloqueo ? <div className="alert alert-warning" role="status">{bloqueo}</div> : (
        <form className="formulario-tarjeta" noValidate onSubmit={enviar}>
          <CampoTexto etiqueta="Nombre del proyecto" requerido min={LIMITES.name[0]} max={LIMITES.name[1]} {...form.campo('name')} />
          <CampoTexto etiqueta="Objetivo" requerido multilinea filas={2} min={LIMITES.objective[0]} max={LIMITES.objective[1]} {...form.campo('objective')} />
          <CampoTexto etiqueta="Descripción" requerido multilinea filas={4} min={LIMITES.description[0]} max={LIMITES.description[1]} {...form.campo('description')} />
          <SelectorChips id="technologies" etiqueta="Tecnologías previstas" libre max={10} placeholder="React"
            valores={form.valores.technologies} error={form.errores.technologies}
            onChange={(lista) => form.cambiar('technologies', lista)} ayuda="Escribí una tecnología y presioná Enter" />
          <div className="row g-3">
            <CampoTexto className="col-sm-6" etiqueta="Fecha de inicio" requerido tipo="date" {...form.campo('startDate')} />
            <CampoTexto className="col-sm-6" etiqueta="Fecha estimada de finalización" tipo="date" ayuda="Opcional" {...form.campo('estimatedEndDate')} />
          </div>
          <CampoTexto etiqueta="Repositorio de GitHub" tipo="url" placeholder="https://github.com/usuario/repositorio" ayuda="Opcional, debe ser público" {...form.campo('repositoryUrl')} />
          <AccionesFormulario textoGuardar="Crear proyecto" procesando={procesando} onCancelar={() => navigate(`/iniciativas/${id}`)} />
        </form>
      )}
    </div>
  );
}
```

**`cliente/src/features/proyectos/ProyectosPage.jsx`**

```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import { BadgeProyecto } from '../../components/ui/Badges.jsx';
import Chip from '../../components/ui/Chip.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import Paginacion from '../../components/ui/Paginacion.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { listarProyectos } from '../../api/proyectos.js';

export default function ProyectosPage() {
  useTituloPagina('Proyectos');
  const [pagina, setPagina] = useState(1);
  const { cargando, error, datos, recargar } = useConsulta((signal) => listarProyectos({ page: pagina, size: 9 }, signal), [pagina]);

  return (
    <div className="container py-4">
      <h1 className="h3 mb-1">Proyectos</h1>
      <p className="text-body-secondary mb-4">Iniciativas que ya formaron equipo y comenzaron su desarrollo.</p>
      {cargando && <EstadoCarga texto="Cargando proyectos…" />}
      {error && <EstadoError error={error} onReintentar={recargar} />}
      {datos && !cargando && datos.items.length === 0 && <EstadoVacio titulo="Todavía no hay proyectos visibles para vos" />}
      {datos && !cargando && datos.items.length > 0 && (
        <>
          <div className="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-4">
            {datos.items.map((p) => (
              <div className="col" key={p.id}>
                <article className="card h-100 tarjeta-iniciativa">
                  <div className="card-body d-flex flex-column">
                    <div className="d-flex justify-content-between gap-2">
                      <h2 className="card-title mb-0">{p.name}</h2>
                      <BadgeProyecto estado={p.status} />
                    </div>
                    <p className="small text-body-secondary mt-2">{p.objective}</p>
                    <ul className="lista-chips mb-3">{p.technologies.slice(0, 4).map((t) => <li key={t}><Chip neutro>{t}</Chip></li>)}</ul>
                    <p className="small mb-3">
                      <i className="bi bi-person" aria-hidden="true"></i> {p.owner.fullName} · <i className="bi bi-people" aria-hidden="true"></i> {p.memberCount} integrantes
                    </p>
                    <Link className="btn btn-outline-primary btn-sm mt-auto align-self-start" to={`/proyectos/${p.id}`}>Ver proyecto <i className="bi bi-arrow-right" aria-hidden="true"></i></Link>
                  </div>
                </article>
              </div>
            ))}
          </div>
          <Paginacion pagina={datos.page} totalPaginas={datos.totalPages} onCambiar={setPagina} />
        </>
      )}
    </div>
  );
}
```

**`cliente/src/features/proyectos/DetalleProyectoPage.jsx`**

```jsx
import { Link, useParams } from 'react-router-dom';
import { BadgeProyecto } from '../../components/ui/Badges.jsx';
import Chip from '../../components/ui/Chip.jsx';
import Avatar from '../../components/ui/Avatar.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import RepositorioGithub from './RepositorioGithub.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { obtenerProyecto } from '../../api/proyectos.js';
import { formatearFecha } from '../../utils/fechas.js';

export default function DetalleProyectoPage() {
  const { id } = useParams();
  const { cargando, error, datos: p, recargar } = useConsulta((signal) => obtenerProyecto(id, signal), [id]);
  useTituloPagina(p?.name ?? 'Proyecto');

  if (cargando) return <div className="container py-4"><EstadoCarga texto="Cargando proyecto…" /></div>;
  if (error?.status === 404) return <div className="container py-4"><EstadoVacio titulo="No encontramos ese proyecto" accion={<Link className="btn btn-outline-primary" to="/proyectos">Ver proyectos</Link>} /></div>;
  if (error) return <div className="container py-4"><EstadoError error={error} onReintentar={recargar} /></div>;

  return (
    <div className="container py-4">
      <Link className="small text-body-secondary d-inline-block mb-2" to="/proyectos"><i className="bi bi-arrow-left" aria-hidden="true"></i> Volver a proyectos</Link>
      <article className="row g-4">
        <div className="col-lg-7">
          <h1 className="h2 mb-2">{p.name}</h1>
          <div className="d-flex flex-wrap gap-2 mb-3"><BadgeProyecto estado={p.status} /></div>
          <p className="small text-body-secondary">
            Iniciativa de origen: <Link to={`/iniciativas/${p.initiative.id}`}>{p.initiative.title}</Link> ·
            Inicio {formatearFecha(p.startDate)} · Fin estimado {p.estimatedEndDate ? formatearFecha(p.estimatedEndDate) : 'sin definir'}
          </p>
          <h2 className="seccion-titulo mt-3">Objetivo</h2>
          <p>{p.objective}</p>
          <h2 className="seccion-titulo">Descripción</h2>
          <p>{p.description}</p>
          <h2 className="seccion-titulo">Tecnologías</h2>
          <ul className="lista-chips mb-4">{p.technologies.length ? p.technologies.map((t) => <li key={t}><Chip neutro>{t}</Chip></li>) : <li className="small text-body-secondary">Sin definir</li>}</ul>
          {p.repositoryUrl ? <RepositorioGithub url={p.repositoryUrl} /> : <p className="small text-body-secondary">Sin repositorio asociado.</p>}
        </div>
        <aside className="col-lg-5">
          <div className="card tarjeta-lateral"><div className="card-body">
            <h2 className="seccion-titulo">Equipo</h2>
            <ul className="list-unstyled mb-0">
              <li className="d-flex align-items-center gap-2 mb-2"><Avatar nombre={p.owner.fullName} /> {p.owner.fullName} <span className="small text-body-secondary">· propietario</span></li>
              {p.members.map((m) => (
                <li key={m.userId} className="d-flex align-items-center gap-2 mb-2">
                  <Avatar nombre={m.fullName} />
                  <span>{m.fullName}<span className="d-block small text-body-secondary">{m.role.name}</span></span>
                </li>
              ))}
            </ul>
          </div></div>
        </aside>
      </article>
    </div>
  );
}
```

### 4.5 Todas las rutas

**`cliente/src/App.jsx`** (reemplazar completo)

```jsx
import { Routes, Route } from 'react-router-dom';
import Layout from './components/layout/Layout.jsx';
import RutaProtegida from './components/layout/RutaProtegida.jsx';
import InicioPage from './features/inicio/InicioPage.jsx';
import CatalogoPage from './features/iniciativas/CatalogoPage.jsx';
import DetallePage from './features/iniciativas/DetallePage.jsx';
import FormularioIniciativaPage from './features/iniciativas/FormularioIniciativaPage.jsx';
import FormularioSolicitudPage from './features/solicitudes/FormularioSolicitudPage.jsx';
import SolicitudesLayout from './features/solicitudes/SolicitudesLayout.jsx';
import MisSolicitudesPage from './features/solicitudes/MisSolicitudesPage.jsx';
import SolicitudesRecibidasPage from './features/solicitudes/SolicitudesRecibidasPage.jsx';
import ConvertirProyectoPage from './features/proyectos/ConvertirProyectoPage.jsx';
import ProyectosPage from './features/proyectos/ProyectosPage.jsx';
import DetalleProyectoPage from './features/proyectos/DetalleProyectoPage.jsx';
import PerfilPage from './features/usuarios/PerfilPage.jsx';
import NoEncontradoPage from './pages/NoEncontradoPage.jsx';

const protegida = (elemento) => <RutaProtegida>{elemento}</RutaProtegida>;

export default function App() {
  return (
    <Routes>
      <Route element={<Layout />}>
        <Route index element={<InicioPage />} />
        <Route path="iniciativas" element={<CatalogoPage />} />
        <Route path="iniciativas/nueva" element={protegida(<FormularioIniciativaPage />)} />
        <Route path="iniciativas/:id" element={<DetallePage />} />
        <Route path="iniciativas/:id/editar" element={protegida(<FormularioIniciativaPage />)} />
        <Route path="iniciativas/:id/solicitar" element={protegida(<FormularioSolicitudPage />)} />
        <Route path="iniciativas/:id/proyecto" element={protegida(<ConvertirProyectoPage />)} />
        <Route path="solicitudes" element={protegida(<SolicitudesLayout />)}>
          <Route index element={<MisSolicitudesPage />} />
          <Route path="recibidas" element={<SolicitudesRecibidasPage />} />
        </Route>
        <Route path="solicitudes/:solicitudId/editar" element={protegida(<FormularioSolicitudPage />)} />
        <Route path="proyectos" element={protegida(<ProyectosPage />)} />
        <Route path="proyectos/:id" element={protegida(<DetalleProyectoPage />)} />
        <Route path="perfil" element={protegida(<PerfilPage />)} />
        <Route path="usuarios/:id" element={protegida(<PerfilPage />)} />
        <Route path="*" element={<NoEncontradoPage />} />
      </Route>
    </Routes>
  );
}
```

React Router elige la ruta más específica: `/solicitudes/abc/editar` no coincide con ninguna hija del layout, así que usa la ruta hermana y el formulario se muestra sin las pestañas.

### 4.6 Verificar la conversión completa

Antes de probar: `localStorage.removeItem('ih.mock.v1')` en la consola y recargar.

| Prueba | Esperado |
|---|---|
| Ana → Mi perfil | Tarjeta con correo, competencias con nivel, intereses; pestañas con flechas del teclado; Eliminar/Archivar en "Mis iniciativas" |
| Ana → perfil de Luis desde el equipo | Tarjeta sin correo y sus iniciativas visibles |
| María → solicitar en "Coworking" | Competencia: solo "Gestión de proyectos"; rol en select; Cancelar vuelve al detalle |
| María de nuevo en "Coworking" | "Ya tenés una solicitud pendiente…" al enviar |
| Diego → solicitar en "Monitoreo de energía" | "Esta iniciativa ya no recibe solicitudes." (estado proyecto, no cupo) |
| Luis → Recibidas → aceptar a María | Toast "se incorporó al equipo"; el detalle muestra a María en el equipo |
| María → Enviadas | La solicitud figura Aceptada |
| Ana → "Plataforma de voluntariado" → Convertir | Formulario; fin anterior al inicio da error; crea y abre el proyecto |
| Proyecto "Monitoreo de energía" | Datos de `twbs/bootstrap` desde GitHub |
| Cambiar la URL del proyecto simulado a `https://github.com/no-existe-xyz/nada` (editando `projects.json` y limpiando localStorage) | "El repositorio no existe o es privado…" |
| Desconectar internet y recargar el proyecto | "No se pudo contactar a GitHub…" con Reintentar |
| Ancho 390 px en todas las pantallas | Sin desplazamiento horizontal |

```powershell
git add cliente
git commit -m "Agregar proyectos, consumo de GitHub con manejo de errores y todas las rutas" --trailer "Seccion: Proyectos" --trailer "Cambio: Conversión del Avance 1 a React completa sobre el servidor simulado"
git push
```

**Explicar en voz alta:** rutas anidadas y `<Outlet>` en Solicitudes; cómo se calcula la intersección de competencias; los seis casos de error de GitHub y por qué 404 significa "inexistente o privado"; por qué GitHub se consulta desde el cliente y no se usa token.

---

# DÍA 5 · viernes 30 oct · Contrato de la API, modelo de datos y MongoDB

El contrato documenta exactamente lo que el servidor simulado ya responde. Desde la entrega no cambia.

### 5.1 `docs/api-contract.md`

````markdown
# Innovation Hub · Contrato de la API

Versión 1.0 · congelado en la etiqueta `entrega-2`. Lo implementan el servidor Node.js + Express
(Avance 2) y el servidor ASP.NET Core (entrega final) sin diferencias en rutas, verbos, cuerpos,
respuestas ni códigos de estado.

## 1. Convenciones

| Tema | Regla |
|---|---|
| Base | `http://<host>:<puerto>/api`. El cliente la lee de `VITE_API_URL`. |
| Formato | JSON UTF-8. `Content-Type: application/json` en peticiones con cuerpo. |
| Nombres | camelCase en inglés. |
| Identificadores | `id` es un string opaco. El cliente no asume su forma (ObjectId en MongoDB, GUID en SQL). Los catálogos usan códigos legibles (`cat-tecnologia`). |
| Fechas | Fecha y hora: ISO 8601 UTC (`2026-09-08T15:00:00.000Z`). Solo fecha: `YYYY-MM-DD` (`startDate`, `estimatedEndDate`). |
| Autenticación | `Authorization: Bearer <token>`. El token se obtiene en `POST /auth/login`. |
| Campos nulos | Los opcionales sin valor se envían como `null`, nunca se omiten en las respuestas. |
| Datos sensibles | Ninguna respuesta incluye contraseñas, hashes, tokens ajenos, cadenas de conexión ni trazas. |

### 1.1 Valores controlados

| Campo | Valores |
|---|---|
| `type` | `idea`, `need`, `challenge` |
| `visibility` | `public`, `institutional`, `restricted`, `private` |
| `status` (iniciativa) | `draft`, `published`, `forming`, `project`, `archived` |
| `status` (solicitud) | `pending`, `accepted`, `rejected`, `cancelled` |
| `status` (proyecto) | `planning`, `in_progress`, `paused`, `finished`, `archived` |
| `level` | `basic`, `intermediate`, `advanced` |
| `memberType` | `student`, `teacher`, `staff` |
| `accessLevel` | `full`, `summary` |

### 1.2 Paginación y orden

Todo listado acepta `page` (entero ≥ 1, por defecto 1) y `size` (entero ≥ 1, por defecto 12; valores
mayores a 50 se limitan a 50). Una página fuera de rango devuelve `items` vacío.

```json
{ "items": [], "page": 1, "size": 12, "totalItems": 37, "totalPages": 4 }
```

`sort` recibe un campo de la lista blanca de cada recurso; con `-` delante es descendente. Un valor
fuera de la lista se ignora y se usa el orden por defecto.

### 1.3 Errores

Formato único en toda la API:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Hay datos inválidos.",
    "details": [ { "field": "title", "message": "Debe tener al menos 5 caracteres." } ]
  }
}
```

`details` es un arreglo (vacío si no aplica). `field` usa el mismo nombre del cuerpo o del parámetro.

| Código HTTP | `code` | Cuándo |
|---|---|---|
| 400 | `VALIDATION_ERROR` | Forma, tipo, longitud, rango, formato, selección inválida o regla entre campos |
| 401 | `UNAUTHORIZED` | Falta el token, es inválido, venció o fue revocado; o la iniciativa es institucional y no hay sesión |
| 401 | `INVALID_CREDENTIALS` | Correo o contraseña incorrectos en el login |
| 403 | `FORBIDDEN` | Hay sesión pero no hay permiso sobre el recurso |
| 404 | `NOT_FOUND` | El recurso o la ruta no existen, o es privado y el usuario no es propietario ni miembro |
| 409 | ver 1.4 | El estado actual impide la operación |
| 429 | `TOO_MANY_REQUESTS` | Más de 20 intentos de login fallidos desde la misma IP en 15 minutos |
| 500 | `INTERNAL_ERROR` | Error inesperado; mensaje genérico, sin detalles internos |

### 1.4 Conflictos (409)

| `code` | Significado |
|---|---|
| `EMAIL_TAKEN` | El correo ya está registrado (RN-01) |
| `ARCHIVED` | La iniciativa está archivada |
| `PARTICIPANTS_BELOW_TEAM` | `estimatedMembers` menor que el equipo actual incluido el propietario |
| `HAS_RELATED_DATA` | Tiene equipo, solicitudes o proyecto: debe archivarse (RN-08, RN-09) |
| `SELF_REQUEST` | El propietario no puede solicitar en su iniciativa (RN-10) |
| `ALREADY_MEMBER` | Ya es miembro del equipo (RN-15) |
| `INITIATIVE_CLOSED` | La iniciativa no está en `published` ni `forming` |
| `NO_SPOTS` | No quedan espacios |
| `DUPLICATE_PENDING` | Ya existe una solicitud pendiente del mismo usuario (RN-11) |
| `NOT_PENDING` | La solicitud ya no está pendiente (RN-12, RN-13) |
| `CANNOT_REMOVE_OWNER` | El propietario no puede retirarse del equipo |
| `NO_MEMBERS` | Conversión sin integrantes además del propietario (RN-17) |
| `ALREADY_PROJECT` | La iniciativa ya generó un proyecto (RN-18) |

Al crear una solicitud las reglas se evalúan siempre en este orden y se devuelve la primera que falle:
`SELF_REQUEST` → `ALREADY_MEMBER` → `INITIATIVE_CLOSED` → `NO_SPOTS` → `DUPLICATE_PENDING` → validación del cuerpo.

## 2. Visibilidad (RN-20)

| `visibility` | Visitante | Usuario autenticado | Propietario o miembro |
|---|---|---|---|
| `public` | `full` | `full` | `full` |
| `institutional` | listado `summary`; detalle 401 | `full` | `full` |
| `restricted` | `summary` | `summary` | `full` |
| `private` | no aparece; detalle 404 | no aparece; detalle 404 | `full` |

Una respuesta `summary` contiene **solo** `id`, `accessLevel`, `title`, `type`, `summary`, `category`,
`requiredSkills` y `visibility` (documento general, nivel Restringida). El listado general excluye
`draft` y `archived` salvo que se filtre por `status` o se pida `owner=me` / `member=me`.

## 3. Representaciones

```jsonc
// Category
{ "id": "cat-tecnologia", "name": "Tecnología", "icon": "bi-cpu" }
// Skill
{ "id": "comp-javascript", "name": "JavaScript", "area": "Desarrollo" }
// Role
{ "id": "rol-frontend", "name": "Desarrollo frontend" }

// UserPublic
{ "id": "…", "firstName": "Ana", "lastName": "Mora", "secondLastName": "Jiménez", "fullName": "Ana Mora Jiménez",
  "memberType": "student", "career": "Ingeniería de Software", "bio": "…",
  "skills": [ { "skillId": "comp-javascript", "name": "JavaScript", "level": "advanced" } ],
  "interests": ["comunidad"], "availability": { "hoursPerWeek": 6, "detail": "tardes" },
  "portfolioUrl": "https://github.com/anamora" }
// UserMe = UserPublic + { "email", "status": "active", "createdAt", "updatedAt" }

// InitiativeSummary (accessLevel "summary")
{ "id": "…", "accessLevel": "summary", "title": "…", "type": "challenge", "summary": "…",
  "category": { "id": "cat-tecnologia", "name": "Tecnología", "icon": "bi-cpu" },
  "requiredSkills": [ { "id": "comp-analisis-datos", "name": "Análisis de datos" } ], "visibility": "restricted" }

// InitiativeItem (accessLevel "full", en listados)
InitiativeSummary + { "tags": [], "status": "published", "estimatedMembers": 4, "memberCount": 2,
  "availableSpots": 2, "owner": { "id": "…", "fullName": "…" }, "projectId": null, "createdAt": "…", "updatedAt": "…",
  "relations": { "members": 1, "requests": 1, "hasProject": false } }   // relations solo para el propietario

// InitiativeDetail (GET /initiatives/{id} con accessLevel "full")
InitiativeItem + { "description": "…", "problem": "…", "beneficiaries": "…",
  "members": [ { "userId": "…", "fullName": "…", "career": "…", "role": { "id": "rol-backend", "name": "…" }, "joinedAt": "…" } ] }

// Request
{ "id": "…", "initiative": { "id": "…", "title": "…" },
  "applicant": { "id": "…", "fullName": "…", "career": "…", "skills": [ /* como UserPublic.skills */ ] },
  "message": "…", "offeredSkill": { "id": "…", "name": "…" }, "role": { "id": "…", "name": "…" },
  "availability": "4 h/semana", "status": "pending",
  "decision": null /* o { "comment": "…" | null, "decidedAt": "…" } */, "createdAt": "…", "updatedAt": "…" }

// Project (listado); ProjectDetail agrega "members" igual que InitiativeDetail
{ "id": "…", "initiative": { "id": "…", "title": "…" }, "name": "…", "objective": "…", "description": "…",
  "technologies": ["Node.js"], "startDate": "2026-09-20", "estimatedEndDate": "2026-12-15" /* o null */,
  "status": "planning", "repositoryUrl": "https://github.com/…" /* o null */,
  "owner": { "id": "…", "fullName": "…" }, "memberCount": 3, "createdAt": "…", "updatedAt": "…" }
```

## 4. Validación de entrada

| Campo | Regla |
|---|---|
| `title` | texto 5–80 |
| `summary` | texto 10–160 |
| `description` | texto 20–2000 |
| `problem` | texto 10–1000 |
| `beneficiaries` | texto 5–500 |
| `categoryId` | id existente en categorías |
| `requiredSkillIds` | 1–8 ids existentes, sin repetir |
| `estimatedMembers` | entero 1–20 |
| `visibility`, `type` | valores de 1.1 |
| `tags` | 0–5, cada una `^[\p{L}\p{N}][\p{L}\p{N}\- ]{1,29}$`, sin repetir |
| `message` | texto 15–300 |
| `offeredSkillId` | requerida por la iniciativa **y** presente en el perfil del solicitante |
| `roleId` | id existente en roles |
| `availability` | `^\d{1,2}\s?h\/semana$` |
| `name` (proyecto) | texto 5–80 |
| `objective` | texto 10–300 |
| `technologies` | 0–10 textos de 1–40 |
| `startDate` | `YYYY-MM-DD` |
| `estimatedEndDate` | `YYYY-MM-DD` o null, ≥ `startDate` |
| `repositoryUrl` | null o `^https://github\.com/[\w.-]+/[\w.-]+/?$` |
| `email` | formato de correo, máx. 120 |
| `password` | 8–72, al menos una letra y un número |
| `confirmPassword` | igual a `password` |
| `firstName`, `lastName`, `career` | texto 2–60; `secondLastName` opcional 2–60 |
| `bio` | 0–500 |
| `skills` (perfil) | 0–15 `{ skillId existente, level }`, sin repetir `skillId` |
| `interests` | 0–10 textos 2–30 |
| `availability` (perfil) | `{ hoursPerWeek: entero 0–40, detail: texto 0–100 }` |
| `portfolioUrl` | null o URL http(s) máx. 200 |

Los textos se recortan (trim) antes de validar. Campos desconocidos se ignoran.

## 5. Endpoints

Acceso: **público** (sin token), **opcional** (con o sin token; cambia la respuesta), **auth** (token obligatorio).

### 5.1 Autenticación

| Verbo y ruta | Acceso | Cuerpo | Éxito | Errores |
|---|---|---|---|---|
| `POST /auth/register` | público | `{ firstName, lastName, secondLastName?, email, password, confirmPassword, memberType, career, acceptTerms: true }` | 201 `UserMe` + `Location: /api/users/{id}` | 400, 409 `EMAIL_TAKEN` |
| `POST /auth/login` | público | `{ email, password }` | 200 `{ token, tokenType: "Bearer", expiresAt, user: UserMe }` | 400, 401 `INVALID_CREDENTIALS`, 429 |
| `POST /auth/logout` | auth | — | 204; el token queda revocado | 401 |
| `GET /auth/me` | auth | — | 200 `UserMe` | 401 |

### 5.2 Usuarios

| Verbo y ruta | Acceso | Parámetros o cuerpo | Éxito | Errores |
|---|---|---|---|---|
| `GET /users` | auth | `q` (nombre), `skill`, `career`, `available=true`, `page`, `size`, `sort` ∈ firstName, lastName, createdAt | 200 página de `UserPublic` | 400, 401 |
| `GET /users/{id}` | auth | — | 200 `UserPublic` | 401, 404 |
| `PUT /users/me` | auth | `{ firstName, lastName, secondLastName, career, bio, skills, interests, availability, portfolioUrl }`; `email` no editable | 200 `UserMe` | 400 (también si `email` viene distinto), 401 |

### 5.3 Catálogos

| Verbo y ruta | Acceso | Éxito |
|---|---|---|
| `GET /categories` | público | 200 `Category[]` |
| `GET /skills` | público | 200 `Skill[]` |
| `GET /roles` | público | 200 `Role[]` |

### 5.4 Iniciativas

| Verbo y ruta | Acceso | Parámetros o cuerpo | Éxito | Errores |
|---|---|---|---|---|
| `GET /initiatives` | opcional | `q` (título, resumen, etiquetas), `type`, `category`, `skill`, `status`, `hasSpots=true`, `owner` (id o `me`), `member=me`, `page`, `size`, `sort` ∈ createdAt, updatedAt, title (defecto `-createdAt`) | 200 página de `InitiativeSummary` o `InitiativeItem` según 2 | 400; 401 si usa `me` sin token |
| `GET /initiatives/{id}` | opcional | — | 200 `InitiativeDetail` o `InitiativeSummary` | 401 (institucional sin sesión), 404 |
| `POST /initiatives` | auth | `{ title, type, summary, description, problem, beneficiaries, categoryId, requiredSkillIds, estimatedMembers, visibility, tags }` | 201 `InitiativeDetail` (`status: "published"`) + `Location` | 400, 401 |
| `PUT /initiatives/{id}` | propietario | mismo cuerpo que POST, completo | 200 `InitiativeDetail` | 400, 401, 403, 404, 409 `ARCHIVED` / `PARTICIPANTS_BELOW_TEAM` |
| `DELETE /initiatives/{id}` | propietario | — | 204 | 401, 403, 404, 409 `HAS_RELATED_DATA` |
| `POST /initiatives/{id}/archive` | propietario | — | 200 `InitiativeDetail` | 401, 403, 404, 409 `ARCHIVED` |

### 5.5 Equipo

| Verbo y ruta | Acceso | Parámetros | Éxito | Errores |
|---|---|---|---|---|
| `GET /initiatives/{id}/members` | acceso `full` | `skill`, `role` | 200 `Member[]` | 401, 403 (acceso `summary`), 404 |
| `DELETE /initiatives/{id}/members/{userId}` | propietario | — | 204 | 401, 403, 404, 409 `CANNOT_REMOVE_OWNER` |

### 5.6 Solicitudes

| Verbo y ruta | Acceso | Parámetros o cuerpo | Éxito | Errores |
|---|---|---|---|---|
| `POST /initiatives/{id}/requests` | auth | `{ message, offeredSkillId, roleId, availability }` | 201 `Request` (`pending`) + `Location` | 400, 401, 404, 409 (orden de 1.4) |
| `GET /requests/mine` | auth | `status`, `page`, `size`, `sort` ∈ createdAt, updatedAt | 200 página de `Request` | 400, 401 |
| `GET /requests/received` | auth | `initiativeId`, `status`, `skill`, `role`, `from`, `to` (`YYYY-MM-DD`), `page`, `size`, `sort` | 200 página de `Request` de iniciativas propias | 400, 401 |
| `GET /requests/{id}` | solicitante o propietario | — | 200 `Request` | 401, 403, 404 |
| `PUT /requests/{id}` | solicitante | `{ message, offeredSkillId, roleId, availability }` | 200 `Request` | 400, 401, 403, 404, 409 `NOT_PENDING` |
| `POST /requests/{id}/cancel` | solicitante | — | 200 `Request` (`cancelled`) | 401, 403, 404, 409 `NOT_PENDING` |
| `POST /requests/{id}/decision` | propietario de la iniciativa | `{ decision: "accepted" \| "rejected", comment?: texto 0–300 }` | 200 `Request`; si `accepted`, el solicitante queda en `members` y la iniciativa pasa a `forming` (RN-14) | 400, 401, 403, 404, 409 `NOT_PENDING` / `INITIATIVE_CLOSED` / `NO_SPOTS` |

### 5.7 Proyectos

| Verbo y ruta | Acceso | Parámetros o cuerpo | Éxito | Errores |
|---|---|---|---|---|
| `POST /initiatives/{id}/project` | propietario | `{ name, objective, description, technologies, startDate, estimatedEndDate, repositoryUrl }` | 201 `ProjectDetail` (`planning`); la iniciativa pasa a `project` + `Location` | 400, 401, 403, 404, 409 `NO_MEMBERS` / `ALREADY_PROJECT` / `ARCHIVED` |
| `GET /projects` | auth | `status`, `page`, `size`, `sort` ∈ createdAt, name, startDate | 200 página de `Project` cuyo origen el usuario ve con acceso `full` | 400, 401 |
| `GET /projects/{id}` | auth, acceso `full` al origen | — | 200 `ProjectDetail` | 401, 404 |

## 6. Ejemplo completo

```http
POST /api/initiatives/660000000000000000000006/requests
Authorization: Bearer eyJhbGciOi…
Content-Type: application/json

{ "message": "Puedo coordinar las actividades del espacio.", "offeredSkillId": "comp-gestion",
  "roleId": "rol-coordinacion", "availability": "3 h/semana" }
```

```http
HTTP/1.1 201 Created
Location: /api/requests/6720f1c2a9b3e4d5f6a7b8c9
Content-Type: application/json

{ "id": "6720f1c2a9b3e4d5f6a7b8c9", "initiative": { "id": "660000000000000000000006", "title": "Espacio de coworking para emprendedores" },
  "applicant": { "id": "650000000000000000000003", "fullName": "María Solís Rojas", "career": "Agronomía", "skills": [ … ] },
  "message": "Puedo coordinar las actividades del espacio.", "offeredSkill": { "id": "comp-gestion", "name": "Gestión de proyectos" },
  "role": { "id": "rol-coordinacion", "name": "Coordinación" }, "availability": "3 h/semana", "status": "pending",
  "decision": null, "createdAt": "2026-11-02T18:00:00.000Z", "updatedAt": "2026-11-02T18:00:00.000Z" }
```

## 7. Decisiones pensadas para la reimplementación en .NET

- `DELETE` nunca archiva en silencio: responde 409 y el cliente llama a `/archive`. Un verbo, un efecto.
- Privadas responden 404 a terceros para no revelar que existen.
- Las acciones de estado (`archive`, `cancel`, `decision`, `project`) son subrecursos `POST`, que se mapean igual en controladores de ASP.NET Core.
- Los catálogos usan códigos de texto como clave, válidos tanto en MongoDB como en una columna `varchar` con clave primaria.
- Token Bearer en encabezado: ASP.NET Core lo valida con `JwtBearer` sin cambiar el cliente.
````

```powershell
git add docs\api-contract.md
git commit -m "Documentar el contrato de la API" --trailer "Seccion: Contrato" --trailer "Cambio: Rutas, verbos, cuerpos, respuestas, errores y visibilidad"
```

### 5.2 `docs/modelo-datos.md`

````markdown
# Innovation Hub · Modelo de datos en MongoDB

Base: `innovation_hub`. Las colecciones de dominio usan `_id` ObjectId; los catálogos usan `_id` de texto.

## Colecciones

| Colección | Campos principales |
|---|---|
| `categories` | `_id` (string), `name`, `icon` |
| `skills` | `_id` (string), `name`, `area` |
| `roles` | `_id` (string), `name` |
| `users` | `firstName`, `lastName`, `secondLastName`, `email` (único), `passwordHash` (`select: false`), `memberType`, `career`, `bio`, `skills[] { skillId → skills, level }`, `interests[]`, `availability { hoursPerWeek, detail }`, `portfolioUrl`, `status`, `acceptedTermsAt`, `createdAt`, `updatedAt` |
| `initiatives` | `title`, `type`, `summary`, `description`, `problem`, `beneficiaries`, `categoryId → categories`, `requiredSkillIds[] → skills`, `estimatedMembers`, `visibility`, `tags[]`, `ownerId → users`, `members[] { userId → users, roleId → roles, joinedAt }`, `status`, `createdAt`, `updatedAt` |
| `requests` | `initiativeId → initiatives`, `userId → users`, `message`, `offeredSkillId → skills`, `roleId → roles`, `availability`, `status`, `decisionComment`, `decidedAt`, `createdAt`, `updatedAt` |
| `projects` | `initiativeId → initiatives` (único), `name`, `objective`, `description`, `technologies[]`, `startDate`, `estimatedEndDate`, `repositoryUrl`, `status`, `createdAt`, `updatedAt` |
| `revokedtokens` | `jti` (único), `expiresAt` (índice TTL) |

## Decisiones de embebido o referencia

| Relación | Decisión | Por qué | En SQL (diciembre) |
|---|---|---|---|
| Usuario → competencias | **Embebido** `{ skillId, level }` con **referencia** al catálogo | Pocas por usuario, siempre se leen y editan con el perfil; el nombre vive en el catálogo para no duplicarlo | `UserSkills(UserId, SkillId, Level)` |
| Iniciativa → competencias requeridas | **Arreglo de referencias** | Relación muchos a muchos con un catálogo estable; se filtra por `requiredSkillIds` | `InitiativeSkills(InitiativeId, SkillId)` |
| Iniciativa → etiquetas | **Embebido** como textos | No tienen identidad propia ni se consultan aparte | `InitiativeTags(InitiativeId, Tag)` |
| Iniciativa → propietario | **Referencia** `ownerId` | El usuario existe por sí mismo y cambia su perfil | FK `OwnerId` |
| Iniciativa → equipo | **Embebido** `members[]` | Acotado (máximo 20), siempre se muestra con la iniciativa, y agregar un miembro con control de cupo es una sola actualización atómica del documento | `InitiativeMembers(InitiativeId, UserId, RoleId, JoinedAt)` |
| Solicitudes | **Colección propia con referencias** | Crece sin límite, se consulta por solicitante y por propietario con filtros y paginación, y tiene ciclo de vida propio | `Requests` con FK a iniciativa, usuario, competencia y rol |
| Proyecto → iniciativa | **Colección propia**, `initiativeId` único | Relación 1 a 1 (RN-18) con estado y datos propios; la iniciativa se conserva (RN-19) | `Projects` con FK única |
| Logout | **Colección `revokedtokens`** con TTL | MongoDB borra solo las entradas vencidas | Tabla con limpieza programada o caché |

Lo único que se repite es el nombre de la persona en las respuestas: no se guarda copiado, se resuelve al serializar.

## Índices

| Colección | Índice | Para qué |
|---|---|---|
| `users` | `{ email: 1 }` único | RN-01 |
| `initiatives` | `{ status: 1, type: 1, categoryId: 1 }` | filtros del catálogo |
| `initiatives` | `{ ownerId: 1 }`, `{ 'members.userId': 1 }`, `{ requiredSkillIds: 1 }` | "mis iniciativas", "participo en", filtro por competencia |
| `requests` | `{ initiativeId: 1, userId: 1 }` único parcial donde `status = 'pending'` | RN-11 garantizada por la base |
| `requests` | `{ userId: 1, createdAt: -1 }` | mis solicitudes |
| `projects` | `{ initiativeId: 1 }` único | RN-18 |
| `revokedtokens` | `{ expiresAt: 1 }` con `expireAfterSeconds: 0` | limpieza automática |

La búsqueda por texto usa expresiones regulares sin distinguir mayúsculas sobre título, resumen y
etiquetas, para coincidir con fragmentos de palabras (el índice de texto de MongoDB solo encuentra
palabras completas).

## Atomicidad

MongoDB instalado como servicio en Windows es un servidor único y no admite transacciones con varios
documentos. Aceptar una solicitud toca dos documentos y se resuelve así:

1. `requests.updateOne({ _id, status: 'pending' }, { status: 'accepted', … })`: si no modifica nada, la solicitud ya no estaba pendiente → 409.
2. `initiatives.updateOne({ _id, status ∈ [published, forming], 'members.userId' ≠ usuario, cupo disponible }, { $push: members, status: 'forming' })`.
3. Si el paso 2 no modifica nada, se revierte el paso 1 y se responde 409.

En SQL este flujo será una transacción.
````

```powershell
git add docs\modelo-datos.md
git commit -m "Documentar el modelo de datos con decisiones de embebido y referencia" --trailer "Seccion: Modelo de datos" --trailer "Cambio: Colecciones, índices, atomicidad y equivalencia en SQL"
```

### 5.3 Instalar MongoDB en Windows

1. Descargar **MongoDB Community Server** (versión 8, paquete `.msi`) desde mongodb.com/try/download/community.
2. Instalación "Complete", marcar **Install MongoD as a Service** (usuario Network Service). Dejar marcado **Install MongoDB Compass**.
3. Descargar e instalar **mongosh** (MongoDB Shell) desde mongodb.com/try/download/shell.
4. Verificar:

```powershell
Get-Service MongoDB
mongosh "mongodb://127.0.0.1:27017" --eval "db.runCommand({ ping: 1 })"
```

Debe mostrar `Running` y `{ ok: 1 }`. Si el servicio está detenido: `Start-Service MongoDB` en PowerShell como administrador.

```powershell
git push
```

**Explicar en voz alta:** cada fila de la tabla de embebido o referencia y qué tabla será en SQL; por qué privadas dan 404; por qué `DELETE` responde 409 en vez de archivar.

---

# DÍA 6 · sábado 31 oct · Servidor base, modelos y seed

### 6.1 Proyecto y dependencias

```powershell
cd servidor
npm init -y
npm install express@5 mongoose cors helmet morgan dotenv zod@3 bcryptjs jsonwebtoken express-rate-limit
npm install -D newman
New-Item -ItemType Directory src\config, src\db, src\models, src\repositories, src\services, src\controllers, src\routes, src\validators, src\middleware, src\utils, src\seed\datos | Out-Null
Copy-Item ..\cliente\public\mock\*.json src\seed\datos\
cd ..
```

**`servidor/package.json`**: reemplazar las claves `type`, `main` y `scripts` por:

```json
"type": "module",
"main": "src/server.js",
"scripts": {
  "dev": "node --watch src/server.js",
  "start": "node src/server.js",
  "seed": "node src/seed/seed.js",
  "test:api": "node src/seed/seed.js && newman run ../pruebas/innovation-hub.postman_collection.json -e ../pruebas/innovation-hub.postman_environment.json"
}
```

**`servidor/.env.example`** (se versiona; copiar a `servidor/.env`, que no se versiona)

```
PORT=3000
NODE_ENV=development
MONGODB_URI=mongodb://127.0.0.1:27017/innovation_hub
CLIENT_ORIGIN=http://localhost:5173
JWT_SECRET=reemplazar-con-una-cadena-aleatoria-de-al-menos-64-caracteres
JWT_EXPIRES_IN=2h
BCRYPT_ROUNDS=12
DEV_AUTH=true
```

```powershell
Copy-Item servidor\.env.example servidor\.env
node -e "console.log(require('crypto').randomBytes(48).toString('hex'))"
```

Pegar el resultado como `JWT_SECRET` en `servidor/.env`.

### 6.2 Configuración, conexión y errores

**`servidor/src/config/env.js`**

```js
import 'dotenv/config';

const requeridas = ['MONGODB_URI', 'CLIENT_ORIGIN', 'JWT_SECRET'];
const faltantes = requeridas.filter((nombre) => !process.env[nombre]);
if (faltantes.length) {
  console.error(`Faltan variables de entorno: ${faltantes.join(', ')}. Copiá .env.example a .env y completalo.`);
  process.exit(1);
}

export const config = Object.freeze({
  puerto: Number(process.env.PORT) || 3000,
  entorno: process.env.NODE_ENV || 'development',
  mongoUri: process.env.MONGODB_URI,
  origenCliente: process.env.CLIENT_ORIGIN,
  jwtSecreto: process.env.JWT_SECRET,
  jwtExpira: process.env.JWT_EXPIRES_IN || '2h',
  rondasBcrypt: Number(process.env.BCRYPT_ROUNDS) || 12,
  // Temporal hasta el día 10. Nunca se activa en producción.
  authDev: process.env.DEV_AUTH === 'true' && process.env.NODE_ENV !== 'production',
});
```

**`servidor/src/db/conexion.js`**

```js
import mongoose from 'mongoose';
import { config } from '../config/env.js';

mongoose.set('strictQuery', true);

export async function conectar() {
  await mongoose.connect(config.mongoUri);
  console.log('MongoDB conectado');
}

export const desconectar = () => mongoose.disconnect();
```

**`servidor/src/utils/ErrorApi.js`**

```js
// Error de negocio con código HTTP y código de contrato. El manejador central lo convierte en JSON.
export class ErrorApi extends Error {
  constructor(status, code, message, details = []) {
    super(message);
    this.status = status;
    this.code = code;
    this.details = details;
  }

  static validacion(details, message = 'Hay datos inválidos.') { return new ErrorApi(400, 'VALIDATION_ERROR', message, details); }
  static noAutenticado(message = 'Necesitás iniciar sesión.') { return new ErrorApi(401, 'UNAUTHORIZED', message); }
  static prohibido(message = 'No tenés permiso para realizar esta acción.') { return new ErrorApi(403, 'FORBIDDEN', message); }
  static noEncontrado(message = 'El recurso no existe.') { return new ErrorApi(404, 'NOT_FOUND', message); }
  static conflicto(code, message) { return new ErrorApi(409, code, message); }
}
```

**`servidor/src/middleware/manejadorErrores.js`**

```js
import mongoose from 'mongoose';
import { ZodError } from 'zod';
import { ErrorApi } from '../utils/ErrorApi.js';

function conflictoPorDuplicado(err) {
  const campos = Object.keys(err.keyPattern ?? {});
  if (campos.includes('email')) return ErrorApi.conflicto('EMAIL_TAKEN', 'Ya existe una cuenta con ese correo.');
  if (campos.includes('userId')) return ErrorApi.conflicto('DUPLICATE_PENDING', 'Ya tenés una solicitud pendiente para esta iniciativa.');
  if (campos.includes('initiativeId')) return ErrorApi.conflicto('ALREADY_PROJECT', 'Esta iniciativa ya se convirtió en proyecto.');
  return ErrorApi.conflicto('CONFLICT', 'El recurso ya existe.');
}

// Último middleware: todo error termina aquí con el formato único del contrato.
// Nunca se envían trazas ni mensajes de MongoDB al cliente.
export function manejadorErrores(err, req, res, _next) {
  let error = err;
  if (err instanceof ZodError) {
    error = ErrorApi.validacion(err.issues.map((i) => ({ field: i.path.join('.'), message: i.message })));
  } else if (err?.type === 'entity.parse.failed') {
    error = ErrorApi.validacion([], 'El cuerpo de la petición no es JSON válido.');
  } else if (err?.type === 'entity.too.large') {
    error = ErrorApi.validacion([], 'El cuerpo de la petición es demasiado grande.');
  } else if (err instanceof mongoose.Error.CastError) {
    error = ErrorApi.noEncontrado();
  } else if (err?.code === 11000) {
    error = conflictoPorDuplicado(err);
  }

  if (!(error instanceof ErrorApi)) {
    console.error(err);
    error = new ErrorApi(500, 'INTERNAL_ERROR', 'Ocurrió un error inesperado.');
  }
  res.status(error.status).json({ error: { code: error.code, message: error.message, details: error.details } });
}
```

**`servidor/src/middleware/noEncontrado.js`**

```js
import { ErrorApi } from '../utils/ErrorApi.js';

export function noEncontrado(req, _res, next) {
  next(ErrorApi.noEncontrado(`La ruta ${req.method} ${req.originalUrl} no existe.`));
}
```

**`servidor/src/middleware/validar.js`**

```js
// Valida y normaliza params, query y body con esquemas zod.
// El resultado queda en req.validado; un ZodError llega al manejador como 400.
export const validar = (esquemas) => (req, _res, next) => {
  req.validado = {};
  for (const parte of ['params', 'query', 'body']) {
    if (esquemas[parte]) req.validado[parte] = esquemas[parte].parse(req[parte] ?? {});
  }
  next();
};
```

**`servidor/src/middleware/autenticacion.js`** (versión de desarrollo; el día 10 se reemplaza completa sin tocar las rutas)

```js
import mongoose from 'mongoose';
import { User } from '../models/User.js';
import { ErrorApi } from '../utils/ErrorApi.js';
import { config } from '../config/env.js';

// TEMPORAL: identifica al usuario con el encabezado X-Dev-User mientras no hay login.
export async function identificar(req, _res, next) {
  req.usuario = null;
  const id = req.get('X-Dev-User');
  if (config.authDev && id && mongoose.isValidObjectId(id)) {
    const usuario = await User.findOne({ _id: id, status: 'active' }).lean();
    if (usuario) req.usuario = { ...usuario, id: String(usuario._id) };
  }
  next();
}

export function requerirAuth(req, _res, next) {
  if (!req.usuario) throw ErrorApi.noAutenticado();
  next();
}
```

### 6.3 Modelos

**`servidor/src/models/enums.js`**

```js
export const TIPOS = ['idea', 'need', 'challenge'];
export const VISIBILIDADES = ['public', 'institutional', 'restricted', 'private'];
export const ESTADOS_INICIATIVA = ['draft', 'published', 'forming', 'project', 'archived'];
export const ESTADOS_SOLICITUD = ['pending', 'accepted', 'rejected', 'cancelled'];
export const ESTADOS_PROYECTO = ['planning', 'in_progress', 'paused', 'finished', 'archived'];
export const NIVELES = ['basic', 'intermediate', 'advanced'];
export const TIPOS_MIEMBRO = ['student', 'teacher', 'staff'];
```

**`servidor/src/models/Catalogos.js`**

```js
import { Schema, model } from 'mongoose';

// Catálogos con _id de texto legible: 'cat-tecnologia', 'comp-javascript', 'rol-frontend'.
const opciones = { versionKey: false };
export const Category = model('Category', new Schema({ _id: String, name: { type: String, required: true }, icon: String }, opciones));
export const Skill = model('Skill', new Schema({ _id: String, name: { type: String, required: true }, area: String }, opciones));
export const Role = model('Role', new Schema({ _id: String, name: { type: String, required: true } }, opciones));
```

**`servidor/src/models/User.js`**

```js
import { Schema, model } from 'mongoose';
import { NIVELES, TIPOS_MIEMBRO } from './enums.js';

const competenciaSchema = new Schema({
  skillId: { type: String, ref: 'Skill', required: true },
  level: { type: String, enum: NIVELES, required: true },
}, { _id: false });

const usuarioSchema = new Schema({
  firstName: { type: String, required: true, trim: true },
  lastName: { type: String, required: true, trim: true },
  secondLastName: { type: String, default: null },
  email: { type: String, required: true, unique: true, lowercase: true, trim: true },
  passwordHash: { type: String, required: true, select: false }, // nunca sale en consultas por defecto
  memberType: { type: String, enum: TIPOS_MIEMBRO, required: true },
  career: { type: String, required: true },
  bio: { type: String, default: '' },
  skills: { type: [competenciaSchema], default: [] },
  interests: { type: [String], default: [] },
  availability: {
    hoursPerWeek: { type: Number, default: 0 },
    detail: { type: String, default: '' },
  },
  portfolioUrl: { type: String, default: null },
  status: { type: String, enum: ['active', 'inactive'], default: 'active' },
  acceptedTermsAt: { type: Date, default: null },
}, { timestamps: true, versionKey: false });

export const User = model('User', usuarioSchema);
```

**`servidor/src/models/Initiative.js`**

```js
import { Schema, model } from 'mongoose';
import { TIPOS, VISIBILIDADES, ESTADOS_INICIATIVA } from './enums.js';

// Equipo embebido: acotado a 20 y siempre se lee con la iniciativa.
const miembroSchema = new Schema({
  userId: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  roleId: { type: String, ref: 'Role', required: true },
  joinedAt: { type: Date, default: Date.now },
}, { _id: false });

const iniciativaSchema = new Schema({
  title: { type: String, required: true },
  type: { type: String, enum: TIPOS, required: true },
  summary: { type: String, required: true },
  description: { type: String, required: true },
  problem: { type: String, required: true },
  beneficiaries: { type: String, required: true },
  categoryId: { type: String, ref: 'Category', required: true },
  requiredSkillIds: [{ type: String, ref: 'Skill' }],
  estimatedMembers: { type: Number, min: 1, max: 20, required: true },
  visibility: { type: String, enum: VISIBILIDADES, required: true },
  tags: { type: [String], default: [] },
  ownerId: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  members: { type: [miembroSchema], default: [] },
  status: { type: String, enum: ESTADOS_INICIATIVA, default: 'published' },
}, { timestamps: true, versionKey: false });

iniciativaSchema.index({ status: 1, type: 1, categoryId: 1 });
iniciativaSchema.index({ ownerId: 1 });
iniciativaSchema.index({ 'members.userId': 1 });
iniciativaSchema.index({ requiredSkillIds: 1 });

export const Initiative = model('Initiative', iniciativaSchema);
```

**`servidor/src/models/Request.js`**

```js
import { Schema, model } from 'mongoose';
import { ESTADOS_SOLICITUD } from './enums.js';

const solicitudSchema = new Schema({
  initiativeId: { type: Schema.Types.ObjectId, ref: 'Initiative', required: true },
  userId: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  message: { type: String, required: true },
  offeredSkillId: { type: String, ref: 'Skill', required: true },
  roleId: { type: String, ref: 'Role', required: true },
  availability: { type: String, required: true },
  status: { type: String, enum: ESTADOS_SOLICITUD, default: 'pending' },
  decisionComment: { type: String, default: null },
  decidedAt: { type: Date, default: null },
}, { timestamps: true, versionKey: false });

// RN-11 garantizada por la base: una sola pendiente por usuario e iniciativa.
solicitudSchema.index({ initiativeId: 1, userId: 1 }, { unique: true, partialFilterExpression: { status: 'pending' } });
solicitudSchema.index({ userId: 1, createdAt: -1 });

export const Request = model('Request', solicitudSchema);
```

**`servidor/src/models/Project.js`**

```js
import { Schema, model } from 'mongoose';
import { ESTADOS_PROYECTO } from './enums.js';

const proyectoSchema = new Schema({
  initiativeId: { type: Schema.Types.ObjectId, ref: 'Initiative', required: true, unique: true }, // RN-18
  name: { type: String, required: true },
  objective: { type: String, required: true },
  description: { type: String, required: true },
  technologies: { type: [String], default: [] },
  startDate: { type: String, required: true },        // YYYY-MM-DD
  estimatedEndDate: { type: String, default: null },  // YYYY-MM-DD
  repositoryUrl: { type: String, default: null },
  status: { type: String, enum: ESTADOS_PROYECTO, default: 'planning' },
}, { timestamps: true, versionKey: false });

export const Project = model('Project', proyectoSchema);
```

**`servidor/src/models/RevokedToken.js`**

```js
import { Schema, model } from 'mongoose';

const tokenRevocadoSchema = new Schema({
  jti: { type: String, required: true, unique: true },
  expiresAt: { type: Date, required: true },
}, { versionKey: false });

// TTL: MongoDB borra el documento cuando pasa expiresAt.
tokenRevocadoSchema.index({ expiresAt: 1 }, { expireAfterSeconds: 0 });

export const RevokedToken = model('RevokedToken', tokenRevocadoSchema);
```

```powershell
git add servidor .gitignore
git commit -m "Crear el servidor Express con configuración, manejo central de errores y modelos" --trailer "Seccion: Servidor" --trailer "Cambio: Esquemas Mongoose con índices y formato de error del contrato"
```

### 6.4 Seed

**`servidor/src/seed/seed.js`**

Usa los mismos JSON del cliente, así los `id` coinciden con los del servidor simulado y con la colección Postman.

```js
import { readFile } from 'node:fs/promises';
import bcrypt from 'bcryptjs';
import mongoose from 'mongoose';
import { config } from '../config/env.js';
import { conectar, desconectar } from '../db/conexion.js';
import { Category, Skill, Role } from '../models/Catalogos.js';
import { User } from '../models/User.js';
import { Initiative } from '../models/Initiative.js';
import { Request } from '../models/Request.js';
import { Project } from '../models/Project.js';
import { RevokedToken } from '../models/RevokedToken.js';

export const CONTRASENA_PRUEBA = 'Prueba123!';

const leer = async (nombre) => JSON.parse(await readFile(new URL(`./datos/${nombre}.json`, import.meta.url), 'utf8'));
const oid = (id) => new mongoose.Types.ObjectId(id);
const fecha = (valor) => (valor ? new Date(valor) : null);

await conectar();
await mongoose.connection.dropDatabase();
for (const modelo of [Category, Skill, Role, User, Initiative, Request, Project, RevokedToken]) await modelo.createIndexes();

const catalogo = async (modelo, nombre) => modelo.collection.insertMany((await leer(nombre)).map(({ id, ...resto }) => ({ _id: id, ...resto })));
await catalogo(Category, 'categories');
await catalogo(Skill, 'skills');
await catalogo(Role, 'roles');

const hash = await bcrypt.hash(CONTRASENA_PRUEBA, config.rondasBcrypt);
await User.collection.insertMany((await leer('users')).map(({ id, createdAt, updatedAt, ...resto }) => ({
  _id: oid(id), ...resto, passwordHash: hash, acceptedTermsAt: fecha(createdAt), createdAt: fecha(createdAt), updatedAt: fecha(updatedAt),
})));

await Initiative.collection.insertMany((await leer('initiatives')).map(({ id, ownerId, members, createdAt, updatedAt, ...resto }) => ({
  _id: oid(id), ...resto, ownerId: oid(ownerId),
  members: members.map((m) => ({ userId: oid(m.userId), roleId: m.roleId, joinedAt: fecha(m.joinedAt) })),
  createdAt: fecha(createdAt), updatedAt: fecha(updatedAt),
})));

await Request.collection.insertMany((await leer('requests')).map(({ id, initiativeId, userId, decidedAt, createdAt, updatedAt, ...resto }) => ({
  _id: oid(id), ...resto, initiativeId: oid(initiativeId), userId: oid(userId),
  decidedAt: fecha(decidedAt), createdAt: fecha(createdAt), updatedAt: fecha(updatedAt),
})));

await Project.collection.insertMany((await leer('projects')).map(({ id, initiativeId, createdAt, updatedAt, ...resto }) => ({
  _id: oid(id), ...resto, initiativeId: oid(initiativeId), createdAt: fecha(createdAt), updatedAt: fecha(updatedAt),
})));

const usuarios = await User.find().select('email').lean();
console.log('Datos de prueba cargados. Usuarios (contraseña para todos: %s):', CONTRASENA_PRUEBA);
usuarios.forEach((u) => console.log(`  ${u._id}  ${u.email}`));
await desconectar();
```

### 6.5 Catálogos, ruta de desarrollo y arranque

**`servidor/src/repositories/catalogos.repository.js`**

```js
import { Category, Skill, Role } from '../models/Catalogos.js';

export const listarCategorias = () => Category.find().sort({ name: 1 }).lean();
export const listarCompetencias = () => Skill.find().sort({ name: 1 }).lean();
export const listarRoles = () => Role.find().sort({ name: 1 }).lean();

// Mapas id → documento para serializar sin consultas repetidas.
export async function mapasCatalogo() {
  const [categorias, competencias, roles] = await Promise.all([listarCategorias(), listarCompetencias(), listarRoles()]);
  const mapa = (lista) => new Map(lista.map((x) => [x._id, x]));
  return { categorias: mapa(categorias), competencias: mapa(competencias), roles: mapa(roles) };
}
```

**`servidor/src/controllers/catalogos.controller.js`**

```js
import * as repo from '../repositories/catalogos.repository.js';

export async function categorias(_req, res) {
  res.json((await repo.listarCategorias()).map((c) => ({ id: c._id, name: c.name, icon: c.icon })));
}
export async function competencias(_req, res) {
  res.json((await repo.listarCompetencias()).map((c) => ({ id: c._id, name: c.name, area: c.area })));
}
export async function roles(_req, res) {
  res.json((await repo.listarRoles()).map((r) => ({ id: r._id, name: r.name })));
}
```

**`servidor/src/routes/catalogos.routes.js`**

```js
import { Router } from 'express';
import * as controlador from '../controllers/catalogos.controller.js';

const router = Router();
router.get('/categories', controlador.categorias);
router.get('/skills', controlador.competencias);
router.get('/roles', controlador.roles);
export default router;
```

**`servidor/src/routes/dev.routes.js`** (temporal, se borra el día 10)

```js
import { Router } from 'express';
import { User } from '../models/User.js';

const router = Router();
router.get('/users', async (_req, res) => {
  const usuarios = await User.find({ status: 'active' }).sort({ firstName: 1 }).lean();
  res.json(usuarios.map((u) => ({ id: String(u._id), fullName: [u.firstName, u.lastName, u.secondLastName].filter(Boolean).join(' ') })));
});
export default router;
```

**`servidor/src/routes/index.js`**

```js
import { Router } from 'express';
import { config } from '../config/env.js';
import catalogos from './catalogos.routes.js';
import dev from './dev.routes.js';

const router = Router();
router.use(catalogos);
if (config.authDev) router.use('/dev', dev);
export default router;
```

**`servidor/src/app.js`**

```js
import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import morgan from 'morgan';
import { config } from './config/env.js';
import rutas from './routes/index.js';
import { identificar } from './middleware/autenticacion.js';
import { noEncontrado } from './middleware/noEncontrado.js';
import { manejadorErrores } from './middleware/manejadorErrores.js';

export function crearApp() {
  const app = express();
  app.disable('x-powered-by');
  app.use(helmet());
  // CORS explícito: solo el origen del cliente React.
  app.use(cors({
    origin: config.origenCliente,
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization', ...(config.authDev ? ['X-Dev-User'] : [])],
    exposedHeaders: ['Location'],
  }));
  app.use(express.json({ limit: '100kb' }));
  app.use(morgan('dev'));
  app.use('/api', identificar, rutas);
  app.use(noEncontrado);
  app.use(manejadorErrores);
  return app;
}
```

**`servidor/src/server.js`**

```js
import { config } from './config/env.js';
import { conectar } from './db/conexion.js';
import { crearApp } from './app.js';

try {
  await conectar();
} catch (error) {
  console.error('No se pudo conectar a MongoDB. ¿Está corriendo el servicio?', error.message);
  process.exit(1);
}

crearApp().listen(config.puerto, () => console.log(`API escuchando en http://localhost:${config.puerto}/api`));
```

### 6.6 Verificar

```powershell
cd servidor
npm run seed
npm run dev
```

En otra terminal:

```powershell
Invoke-RestMethod http://localhost:3000/api/categories
curl.exe -i http://localhost:3000/api/no-existe
curl.exe -i -X POST http://localhost:3000/api/roles -H "Content-Type: application/json" -d "{mal json"
```

| Prueba | Esperado |
|---|---|
| seed | Lista 4 usuarios con sus `_id` `6500…01` a `6500…04` |
| categorías | 8 categorías ordenadas por nombre |
| ruta inexistente | 404 `{"error":{"code":"NOT_FOUND",…}}` |
| JSON mal formado | 400 `VALIDATION_ERROR` "El cuerpo de la petición no es JSON válido." |
| Compass → `innovation_hub` | 8 colecciones; `users` muestra `passwordHash` empezando con `$2b$12$` |
| Detener el servicio MongoDB y `npm run dev` | "No se pudo conectar a MongoDB…" y el proceso termina |

```powershell
cd ..
git add servidor
git commit -m "Agregar seed con los datos del Avance 1, catálogos y arranque del servidor" --trailer "Seccion: Servidor" --trailer "Cambio: Datos de prueba deterministas y CORS explícito"
git push
```

**Explicar en voz alta:** el orden de los middleware en `app.js` y por qué el manejador de errores va último; qué hace `select: false`; por qué `validar` deja los datos en `req.validado`; qué garantiza el índice parcial de solicitudes.

---

# DÍA 7 · domingo 1 nov · Iniciativas y equipo en el servidor

Capas: **ruta** (verbo, middleware) → **controlador** (HTTP: lee la petición, elige el código) → **servicio** (reglas y permisos, sin `req`/`res`) → **repositorio** (consultas a MongoDB).

### 7.1 Reglas, límites y validadores comunes

```powershell
Copy-Item cliente\src\mock\reglas.js servidor\src\services\reglas.js
```

El archivo funciona igual en el servidor porque compara ids con `String()`. Cambiar solo el comentario de la primera línea por `// Reglas de negocio puras: las mismas que usó el servidor simulado del cliente.`

**`servidor/src/validators/limites.js`** (mismos valores que `cliente/src/utils/validacion.js`)

```js
export const LIMITES = {
  title: [5, 80], summary: [10, 160], description: [20, 2000], problem: [10, 1000],
  beneficiaries: [5, 500], message: [15, 300], name: [5, 80], objective: [10, 300], bio: [0, 500],
};
export const REGEX_ETIQUETA = /^[\p{L}\p{N}][\p{L}\p{N}\- ]{1,29}$/u;
export const REGEX_DISPONIBILIDAD = /^\d{1,2}\s?h\/semana$/i;
export const REGEX_GITHUB = /^https:\/\/github\.com\/[\w.-]+\/[\w.-]+\/?$/;
export const REGEX_FECHA = /^\d{4}-\d{2}-\d{2}$/;
```

**`servidor/src/validators/comunes.js`**

```js
import { z } from 'zod';
import { LIMITES } from './limites.js';

export const objectId = z.string().regex(/^[a-f\d]{24}$/i, 'Identificador inválido.');

export const texto = (campo) => {
  const [min, max] = LIMITES[campo];
  return z.string({ required_error: 'Este campo es obligatorio.', invalid_type_error: 'Debe ser texto.' })
    .trim()
    .min(min, `Debe tener al menos ${min} caracteres.`)
    .max(max, `No debe superar los ${max} caracteres.`);
};

export const opcion = (valores) => z.enum(valores, { errorMap: () => ({ message: 'Elegí una opción válida.' }) });

export const sinRepetidos = (lista) => new Set(lista).size === lista.length;

// page ≥ 1; size ≥ 1, tope 50; sort se valida contra la lista blanca en el repositorio.
export const paginacion = {
  page: z.coerce.number().int('Debe ser un número entero.').min(1, 'Debe ser 1 o mayor.').default(1),
  size: z.coerce.number().int('Debe ser un número entero.').min(1, 'Debe ser 1 o mayor.').default(12).transform((n) => Math.min(n, 50)),
  sort: z.string().max(30).optional(),
};
```

**`servidor/src/utils/paginacion.js`**

```js
// Orden con lista blanca; _id desempata para que la paginación sea estable.
export function orden(sort, permitidos, porDefecto) {
  const valor = sort && permitidos.includes(sort.replace(/^-/, '')) ? sort : porDefecto;
  return { [valor.replace(/^-/, '')]: valor.startsWith('-') ? -1 : 1, _id: 1 };
}

// Nunca descarga la colección completa: cuenta y trae solo la página pedida.
export async function paginar(modelo, filtro, { page, size, sort }, permitidos, porDefecto) {
  const [items, totalItems] = await Promise.all([
    modelo.find(filtro).collation({ locale: 'es' }).sort(orden(sort, permitidos, porDefecto)).skip((page - 1) * size).limit(size).lean(),
    modelo.countDocuments(filtro),
  ]);
  return { items, page, size, totalItems, totalPages: Math.max(1, Math.ceil(totalItems / size)) };
}

export const escaparRegex = (texto) => texto.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
```

**`servidor/src/validators/iniciativas.schemas.js`**

```js
import { z } from 'zod';
import { TIPOS, VISIBILIDADES, ESTADOS_INICIATIVA } from '../models/enums.js';
import { REGEX_ETIQUETA } from './limites.js';
import { objectId, opcion, paginacion, sinRepetidos, texto } from './comunes.js';

export const cuerpoIniciativa = z.object({
  title: texto('title'),
  type: opcion(TIPOS),
  summary: texto('summary'),
  description: texto('description'),
  problem: texto('problem'),
  beneficiaries: texto('beneficiaries'),
  categoryId: z.string({ required_error: 'Elegí una categoría.' }).min(1, 'Elegí una categoría.'),
  requiredSkillIds: z.array(z.string(), { required_error: 'Agregá al menos 1.' })
    .min(1, 'Agregá al menos 1.').max(8, 'Podés agregar como máximo 8.')
    .refine(sinRepetidos, 'No repitas competencias.'),
  estimatedMembers: z.number({ required_error: 'Este campo es obligatorio.', invalid_type_error: 'Debe ser un número entero.' })
    .int('Debe ser un número entero.').min(1, 'Debe estar entre 1 y 20.').max(20, 'Debe estar entre 1 y 20.'),
  visibility: opcion(VISIBILIDADES),
  tags: z.array(z.string().trim().regex(REGEX_ETIQUETA, 'Usá letras, números, espacios o guiones (2 a 30 caracteres).'))
    .max(5, 'Podés agregar como máximo 5.').refine(sinRepetidos, 'No repitas etiquetas.').default([]),
});

export const consultaIniciativas = z.object({
  ...paginacion,
  q: z.string().trim().max(100).optional(),
  type: opcion(TIPOS).optional(),
  category: z.string().max(40).optional(),
  skill: z.string().max(40).optional(),
  status: opcion(ESTADOS_INICIATIVA).optional(),
  hasSpots: z.enum(['true', 'false']).optional(),
  owner: z.union([z.literal('me'), objectId]).optional(),
  member: z.literal('me').optional(),
});

export const consultaMiembros = z.object({
  skill: z.string().max(40).optional(),
  role: z.string().max(40).optional(),
});
```

### 7.2 Serializadores (lo que sale de la API)

**`servidor/src/utils/serializadores.js`**

Mismo resultado que `cliente/src/mock/serializar.js`, ahora a partir de documentos de MongoDB y un contexto con mapas de búsqueda.

```js
import { nivelDeAcceso, esPropietario, integrantes, espaciosDisponibles } from '../services/reglas.js';

export const nombreCompleto = (u) => [u.firstName, u.lastName, u.secondLastName].filter(Boolean).join(' ');

function persona(ctx, id) {
  const u = ctx.usuarios.get(String(id));
  return { id: String(id), fullName: u ? nombreCompleto(u) : 'Usuario no disponible', career: u?.career ?? '' };
}
const competencia = (ctx, id) => ({ id, name: ctx.competencias.get(id)?.name ?? id });
const rol = (ctx, id) => ({ id, name: ctx.roles.get(id)?.name ?? id });

export function serializarUsuarioPublico(u, ctx) {
  return {
    id: String(u._id), firstName: u.firstName, lastName: u.lastName, secondLastName: u.secondLastName ?? null,
    fullName: nombreCompleto(u), memberType: u.memberType, career: u.career, bio: u.bio ?? '',
    skills: (u.skills ?? []).map((s) => ({ skillId: s.skillId, name: competencia(ctx, s.skillId).name, level: s.level })),
    interests: u.interests ?? [],
    availability: { hoursPerWeek: u.availability?.hoursPerWeek ?? 0, detail: u.availability?.detail ?? '' },
    portfolioUrl: u.portfolioUrl ?? null,
  };
}

// Nunca incluye passwordHash: el campo no existe en el objeto devuelto.
export const serializarUsuarioPropio = (u, ctx) => ({
  ...serializarUsuarioPublico(u, ctx), email: u.email, status: u.status, createdAt: u.createdAt, updatedAt: u.updatedAt,
});

export function serializarMiembro(m, ctx) {
  const p = persona(ctx, m.userId);
  return { userId: p.id, fullName: p.fullName, career: p.career, role: rol(ctx, m.roleId), joinedAt: m.joinedAt };
}

export function serializarIniciativa(ini, ctx, usuario, { detalle = false } = {}) {
  const categoria = ctx.categorias.get(ini.categoryId);
  const base = {
    id: String(ini._id),
    accessLevel: nivelDeAcceso(ini, usuario) === 'full' ? 'full' : 'summary',
    title: ini.title, type: ini.type, summary: ini.summary,
    category: categoria ? { id: categoria._id, name: categoria.name, icon: categoria.icon } : null,
    requiredSkills: ini.requiredSkillIds.map((id) => competencia(ctx, id)),
    visibility: ini.visibility,
  };
  if (base.accessLevel !== 'full') return base; // RN-03: nada más sale para una restringida ajena

  const proyecto = ctx.proyectos.get(String(ini._id));
  const duenio = persona(ctx, ini.ownerId);
  const completo = {
    ...base,
    tags: ini.tags, status: ini.status, estimatedMembers: ini.estimatedMembers,
    memberCount: integrantes(ini), availableSpots: espaciosDisponibles(ini),
    owner: { id: duenio.id, fullName: duenio.fullName },
    projectId: proyecto ? String(proyecto._id) : null,
    createdAt: ini.createdAt, updatedAt: ini.updatedAt,
  };
  if (detalle) {
    completo.description = ini.description;
    completo.problem = ini.problem;
    completo.beneficiaries = ini.beneficiaries;
    completo.members = ini.members.map((m) => serializarMiembro(m, ctx));
  }
  if (esPropietario(ini, usuario)) {
    completo.relations = { members: ini.members.length, requests: ctx.solicitudes.get(String(ini._id)) ?? 0, hasProject: Boolean(proyecto) };
  }
  return completo;
}

export function serializarSolicitud(s, ctx) {
  const ini = ctx.iniciativas.get(String(s.initiativeId));
  const solicitante = ctx.usuarios.get(String(s.userId));
  return {
    id: String(s._id),
    initiative: { id: String(s.initiativeId), title: ini?.title ?? 'Iniciativa eliminada' },
    applicant: solicitante
      ? { id: String(solicitante._id), fullName: nombreCompleto(solicitante), career: solicitante.career, skills: serializarUsuarioPublico(solicitante, ctx).skills }
      : { id: String(s.userId), fullName: 'Usuario no disponible', career: '', skills: [] },
    message: s.message, offeredSkill: competencia(ctx, s.offeredSkillId), role: rol(ctx, s.roleId),
    availability: s.availability, status: s.status,
    decision: s.decidedAt ? { comment: s.decisionComment ?? null, decidedAt: s.decidedAt } : null,
    createdAt: s.createdAt, updatedAt: s.updatedAt,
  };
}

export function serializarProyecto(p, ctx, { detalle = false } = {}) {
  const ini = ctx.iniciativas.get(String(p.initiativeId));
  const duenio = persona(ctx, ini.ownerId);
  const proyecto = {
    id: String(p._id), initiative: { id: String(ini._id), title: ini.title },
    name: p.name, objective: p.objective, description: p.description, technologies: p.technologies,
    startDate: p.startDate, estimatedEndDate: p.estimatedEndDate ?? null, status: p.status, repositoryUrl: p.repositoryUrl ?? null,
    owner: { id: duenio.id, fullName: duenio.fullName }, memberCount: integrantes(ini),
    createdAt: p.createdAt, updatedAt: p.updatedAt,
  };
  if (detalle) proyecto.members = ini.members.map((m) => serializarMiembro(m, ctx));
  return proyecto;
}
```

### 7.3 Repositorios

**`servidor/src/repositories/usuarios.repository.js`**

```js
import { User } from '../models/User.js';

const CAMPOS_RESUMEN = 'firstName lastName secondLastName career skills';

export async function mapaPorIds(ids) {
  const usuarios = await User.find({ _id: { $in: ids } }).select(CAMPOS_RESUMEN).lean();
  return new Map(usuarios.map((u) => [String(u._id), u]));
}

export const buscarActivoPorId = (id) => User.findOne({ _id: id, status: 'active' }).lean();
```

**`servidor/src/repositories/solicitudes.repository.js`** (el día 8 se completa)

```js
import { Request } from '../models/Request.js';

export const contarDeIniciativa = (initiativeId) => Request.countDocuments({ initiativeId });

export async function contarPorIniciativas(ids) {
  if (!ids.length) return new Map();
  const grupos = await Request.aggregate([{ $match: { initiativeId: { $in: ids } } }, { $group: { _id: '$initiativeId', total: { $sum: 1 } } }]);
  return new Map(grupos.map((g) => [String(g._id), g.total]));
}
```

**`servidor/src/repositories/proyectos.repository.js`** (el día 8 se completa)

```js
import { Project } from '../models/Project.js';

export const buscarPorIniciativa = (initiativeId) => Project.findOne({ initiativeId }).lean();

export async function mapaPorIniciativas(ids) {
  const proyectos = await Project.find({ initiativeId: { $in: ids } }).lean();
  return new Map(proyectos.map((p) => [String(p.initiativeId), p]));
}
```

**`servidor/src/repositories/iniciativas.repository.js`**

```js
import { Types } from 'mongoose';
import { Initiative } from '../models/Initiative.js';
import { paginar, escaparRegex } from '../utils/paginacion.js';

const ORDENES = ['createdAt', 'updatedAt', 'title'];

// Traduce los filtros del contrato a una consulta de MongoDB.
// La visibilidad se aplica en la base: un tercero nunca recibe privadas.
function filtroListado(q, usuario) {
  const uid = usuario ? new Types.ObjectId(usuario.id) : null;
  const condiciones = [
    uid ? { $or: [{ visibility: { $ne: 'private' } }, { ownerId: uid }, { 'members.userId': uid }] } : { visibility: { $ne: 'private' } },
  ];
  if (q.owner === 'me') condiciones.push({ ownerId: uid });
  else if (q.owner) condiciones.push({ ownerId: new Types.ObjectId(q.owner) });
  if (q.member === 'me') condiciones.push({ 'members.userId': uid });
  if (q.status) condiciones.push({ status: q.status });
  else if (q.owner !== 'me' && q.member !== 'me') condiciones.push({ status: { $nin: ['draft', 'archived'] } });
  if (q.type) condiciones.push({ type: q.type });
  if (q.category) condiciones.push({ categoryId: q.category });
  if (q.skill) condiciones.push({ requiredSkillIds: q.skill });
  if (q.hasSpots === 'true') condiciones.push({ $expr: { $gt: ['$estimatedMembers', { $add: [{ $size: '$members' }, 1] }] } });
  if (q.q) {
    const patron = new RegExp(escaparRegex(q.q), 'i');
    condiciones.push({ $or: [{ title: patron }, { summary: patron }, { tags: patron }] });
  }
  return { $and: condiciones };
}

export const listar = (q, usuario) => paginar(Initiative, filtroListado(q, usuario), q, ORDENES, '-createdAt');
export const buscarPorId = (id) => Initiative.findById(id).lean();

export async function crear(datos) {
  const documento = await Initiative.create(datos);
  return documento.toObject();
}

export const actualizar = (id, cambios) => Initiative.findByIdAndUpdate(id, cambios, { new: true, runValidators: true }).lean();
export const eliminar = (id) => Initiative.deleteOne({ _id: id });

export async function quitarMiembro(id, userId) {
  const resultado = await Initiative.updateOne(
    { _id: id, 'members.userId': userId },
    { $pull: { members: { userId } }, $currentDate: { updatedAt: true } }
  );
  if (resultado.modifiedCount) {
    await Initiative.updateOne({ _id: id, status: 'forming', members: { $size: 0 } }, { status: 'published' });
  }
  return resultado.modifiedCount > 0;
}
```

### 7.4 Servicios

**`servidor/src/services/contexto.service.js`**

Carga en paralelo todo lo que los serializadores necesitan, con una consulta por colección y no una por documento.

```js
import { mapasCatalogo } from '../repositories/catalogos.repository.js';
import * as usuariosRepo from '../repositories/usuarios.repository.js';
import * as solicitudesRepo from '../repositories/solicitudes.repository.js';
import * as proyectosRepo from '../repositories/proyectos.repository.js';
import { esPropietario } from './reglas.js';

export async function contextoIniciativas(iniciativas, usuario, personasExtra = []) {
  const ids = iniciativas.map((i) => i._id);
  const personas = new Set([...personasExtra.map(String), ...iniciativas.flatMap((i) => [String(i.ownerId), ...i.members.map((m) => String(m.userId))])]);
  const propias = iniciativas.filter((i) => esPropietario(i, usuario)).map((i) => i._id);
  const [catalogo, usuarios, proyectos, solicitudes] = await Promise.all([
    mapasCatalogo(),
    usuariosRepo.mapaPorIds([...personas]),
    proyectosRepo.mapaPorIniciativas(ids),
    solicitudesRepo.contarPorIniciativas(propias),
  ]);
  return { ...catalogo, usuarios, proyectos, solicitudes, iniciativas: new Map(iniciativas.map((i) => [String(i._id), i])) };
}
```

**`servidor/src/services/iniciativas.service.js`**

```js
import * as repo from '../repositories/iniciativas.repository.js';
import * as solicitudesRepo from '../repositories/solicitudes.repository.js';
import * as proyectosRepo from '../repositories/proyectos.repository.js';
import { mapasCatalogo } from '../repositories/catalogos.repository.js';
import { contextoIniciativas } from './contexto.service.js';
import { nivelDeAcceso, esPropietario, integrantes } from './reglas.js';
import { serializarIniciativa, serializarMiembro } from '../utils/serializadores.js';
import { ErrorApi } from '../utils/ErrorApi.js';

// Visibilidad verificada en el servidor antes de responder (RNF-05, RN-20).
export async function cargarVisible(id, usuario) {
  const ini = await repo.buscarPorId(id);
  const nivel = ini ? nivelDeAcceso(ini, usuario) : 'none';
  if (nivel === 'none') throw ErrorApi.noEncontrado('La iniciativa no existe.');
  if (nivel === 'login') throw ErrorApi.noAutenticado('Esta iniciativa es institucional: iniciá sesión para verla.');
  return { ini, nivel };
}

// RN-06 y RN-07: solo el propietario modifica, archiva o elimina.
export async function cargarPropia(id, usuario) {
  const { ini } = await cargarVisible(id, usuario);
  if (!esPropietario(ini, usuario)) throw ErrorApi.prohibido('Solo el propietario puede realizar esta acción.');
  return ini;
}

async function validarReferencias({ categoryId, requiredSkillIds }) {
  const { categorias, competencias } = await mapasCatalogo();
  const details = [];
  if (!categorias.has(categoryId)) details.push({ field: 'categoryId', message: 'La categoría no existe.' });
  if (requiredSkillIds.some((s) => !competencias.has(s))) details.push({ field: 'requiredSkillIds', message: 'Alguna competencia no existe.' });
  if (details.length) throw ErrorApi.validacion(details);
}

export async function detalle(ini, usuario) {
  const ctx = await contextoIniciativas([ini], usuario);
  return serializarIniciativa(ini, ctx, usuario, { detalle: true });
}

export async function listar(q, usuario) {
  if ((q.owner === 'me' || q.member === 'me') && !usuario) throw ErrorApi.noAutenticado();
  const pagina = await repo.listar(q, usuario);
  const ctx = await contextoIniciativas(pagina.items, usuario);
  return { ...pagina, items: pagina.items.map((i) => serializarIniciativa(i, ctx, usuario)) };
}

export async function obtener(id, usuario) {
  const { ini } = await cargarVisible(id, usuario);
  return detalle(ini, usuario);
}

export async function crear(datos, usuario) {
  await validarReferencias(datos);
  // RN-02 y RN-03: el propietario es siempre el usuario autenticado, nunca un dato del cuerpo.
  const ini = await repo.crear({ ...datos, ownerId: usuario.id, members: [], status: 'published' });
  return detalle(ini, usuario);
}

export async function actualizar(id, datos, usuario) {
  const ini = await cargarPropia(id, usuario);
  if (ini.status === 'archived') throw ErrorApi.conflicto('ARCHIVED', 'Una iniciativa archivada no se puede modificar.');
  if (datos.estimatedMembers < integrantes(ini)) {
    throw ErrorApi.conflicto('PARTICIPANTS_BELOW_TEAM', `El equipo ya tiene ${integrantes(ini)} integrantes, incluido el propietario.`);
  }
  await validarReferencias(datos);
  return detalle(await repo.actualizar(id, datos), usuario);
}

// RN-08 y RN-09: se elimina solo si no hay información relacionada.
export async function eliminar(id, usuario) {
  const ini = await cargarPropia(id, usuario);
  const [solicitudes, proyecto] = await Promise.all([solicitudesRepo.contarDeIniciativa(ini._id), proyectosRepo.buscarPorIniciativa(ini._id)]);
  if (ini.members.length > 0 || solicitudes > 0 || proyecto) {
    throw ErrorApi.conflicto('HAS_RELATED_DATA', 'La iniciativa tiene equipo, solicitudes o proyecto: archivala en lugar de eliminarla.');
  }
  await repo.eliminar(id);
}

export async function archivar(id, usuario) {
  const ini = await cargarPropia(id, usuario);
  if (ini.status === 'archived') throw ErrorApi.conflicto('ARCHIVED', 'La iniciativa ya está archivada.');
  return detalle(await repo.actualizar(id, { status: 'archived' }), usuario);
}

export async function listarMiembros(id, q, usuario) {
  const { ini, nivel } = await cargarVisible(id, usuario);
  if (nivel !== 'full') throw ErrorApi.prohibido('No tenés acceso al equipo de esta iniciativa.');
  const ctx = await contextoIniciativas([ini], usuario);
  let miembros = ini.members;
  if (q.role) miembros = miembros.filter((m) => m.roleId === q.role);
  if (q.skill) miembros = miembros.filter((m) => ctx.usuarios.get(String(m.userId))?.skills?.some((s) => s.skillId === q.skill));
  return miembros.map((m) => serializarMiembro(m, ctx));
}

export async function retirarMiembro(id, userId, usuario) {
  const ini = await cargarPropia(id, usuario);
  if (String(ini.ownerId) === userId) throw ErrorApi.conflicto('CANNOT_REMOVE_OWNER', 'El propietario no puede retirarse del equipo.');
  const retirado = await repo.quitarMiembro(id, userId);
  if (!retirado) throw ErrorApi.noEncontrado('Esa persona no es miembro del equipo.');
}
```

### 7.5 Controlador y rutas

**`servidor/src/controllers/iniciativas.controller.js`**

```js
import * as servicio from '../services/iniciativas.service.js';

export async function listar(req, res) {
  res.json(await servicio.listar(req.validado.query, req.usuario));
}
export async function obtener(req, res) {
  res.json(await servicio.obtener(req.params.id, req.usuario));
}
export async function crear(req, res) {
  const ini = await servicio.crear(req.validado.body, req.usuario);
  res.status(201).location(`/api/initiatives/${ini.id}`).json(ini);
}
export async function actualizar(req, res) {
  res.json(await servicio.actualizar(req.params.id, req.validado.body, req.usuario));
}
export async function eliminar(req, res) {
  await servicio.eliminar(req.params.id, req.usuario);
  res.status(204).end();
}
export async function archivar(req, res) {
  res.json(await servicio.archivar(req.params.id, req.usuario));
}
export async function miembros(req, res) {
  res.json(await servicio.listarMiembros(req.params.id, req.validado.query, req.usuario));
}
export async function retirarMiembro(req, res) {
  await servicio.retirarMiembro(req.params.id, req.params.userId, req.usuario);
  res.status(204).end();
}
```

**`servidor/src/routes/iniciativas.routes.js`**

```js
import { Router } from 'express';
import * as c from '../controllers/iniciativas.controller.js';
import { requerirAuth } from '../middleware/autenticacion.js';
import { validar } from '../middleware/validar.js';
import { consultaIniciativas, consultaMiembros, cuerpoIniciativa } from '../validators/iniciativas.schemas.js';

const router = Router();
// requerirAuth va antes que validar: sin sesión se responde 401 aunque el cuerpo sea inválido.
router.get('/initiatives', validar({ query: consultaIniciativas }), c.listar);
router.post('/initiatives', requerirAuth, validar({ body: cuerpoIniciativa }), c.crear);
router.get('/initiatives/:id', c.obtener);
router.put('/initiatives/:id', requerirAuth, validar({ body: cuerpoIniciativa }), c.actualizar);
router.delete('/initiatives/:id', requerirAuth, c.eliminar);
router.post('/initiatives/:id/archive', requerirAuth, c.archivar);
router.get('/initiatives/:id/members', validar({ query: consultaMiembros }), c.miembros);
router.delete('/initiatives/:id/members/:userId', requerirAuth, c.retirarMiembro);
export default router;
```

En **`servidor/src/routes/index.js`** agregar `import iniciativas from './iniciativas.routes.js';` y `router.use(iniciativas);` debajo de `router.use(catalogos);`.

### 7.6 Verificar

Con el servidor en marcha (`npm run dev`) y `npm run seed` recién ejecutado:

```powershell
$ana = @{ 'X-Dev-User' = '650000000000000000000001' }
$luis = @{ 'X-Dev-User' = '650000000000000000000002' }
Invoke-RestMethod "http://localhost:3000/api/initiatives?size=3&sort=title" | ConvertTo-Json -Depth 5
Invoke-RestMethod "http://localhost:3000/api/initiatives/660000000000000000000005" | ConvertTo-Json -Depth 5
curl.exe -i http://localhost:3000/api/initiatives/660000000000000000000007
curl.exe -i http://localhost:3000/api/initiatives/660000000000000000000003
curl.exe -i "http://localhost:3000/api/initiatives?size=abc"
curl.exe -i -X DELETE http://localhost:3000/api/initiatives/660000000000000000000001 -H "X-Dev-User: 650000000000000000000001"
curl.exe -i -X DELETE http://localhost:3000/api/initiatives/660000000000000000000004 -H "X-Dev-User: 650000000000000000000002"
curl.exe -i -X DELETE http://localhost:3000/api/initiatives/660000000000000000000004 -H "X-Dev-User: 650000000000000000000001"
```

| Prueba | Esperado |
|---|---|
| Listado `size=3&sort=title` | 3 ítems en orden alfabético, `totalItems: 7`, `totalPages: 3` |
| Restringida (…05) sin usuario | Solo `id, accessLevel: "summary", title, type, summary, category, requiredSkills, visibility` |
| Privada (…07) sin usuario | 404 `NOT_FOUND` |
| Institucional (…03) sin usuario | 401 `UNAUTHORIZED` |
| `size=abc` | 400 con `details[0].field = "size"` |
| Ana elimina la suya con equipo (…01) | 409 `HAS_RELATED_DATA` |
| Luis elimina la de Ana (…04) | 403 `FORBIDDEN` |
| Ana elimina la suya sin relaciones (…04) | 204 |

Cambiar `cliente/.env` a `VITE_USE_MOCK=false` y abrir el catálogo en `http://localhost:5173`: debe verse igual que con el simulado. Volver a `true` hasta el día 9.

```powershell
git add servidor
git commit -m "Implementar iniciativas y equipo en el servidor con visibilidad y propiedad" --trailer "Seccion: Servidor" --trailer "Cambio: Filtros, orden y paginación en MongoDB; RN-03, RN-06 a RN-09"
git push
```

**Explicar en voz alta:** el recorrido de `GET /initiatives/{id}` por las cuatro capas; por qué el propietario sale de `req.usuario` y no del cuerpo; cómo se arma el filtro de visibilidad en MongoDB; por qué `paginar` usa `countDocuments` + `skip/limit`.

---

# DÍA 8 · lunes 2 nov · Solicitudes y proyectos en el servidor

### 8.1 Validadores

**`servidor/src/validators/solicitudes.schemas.js`**

```js
import { z } from 'zod';
import { ESTADOS_SOLICITUD } from '../models/enums.js';
import { REGEX_DISPONIBILIDAD, REGEX_FECHA } from './limites.js';
import { objectId, opcion, paginacion, texto } from './comunes.js';

export const cuerpoSolicitud = z.object({
  message: texto('message'),
  offeredSkillId: z.string({ required_error: 'Elegí una competencia.' }).min(1, 'Elegí una competencia.'),
  roleId: z.string({ required_error: 'Elegí un rol.' }).min(1, 'Elegí un rol.'),
  availability: z.string({ required_error: 'Este campo es obligatorio.' }).trim()
    .regex(REGEX_DISPONIBILIDAD, 'Usá el formato "N h/semana", por ejemplo 4 h/semana.'),
});

export const cuerpoDecision = z.object({
  decision: z.enum(['accepted', 'rejected'], { errorMap: () => ({ message: 'La decisión debe ser accepted o rejected.' }) }),
  comment: z.string().trim().max(300, 'No debe superar los 300 caracteres.').optional(),
});

const fecha = z.string().regex(REGEX_FECHA, 'Usá el formato AAAA-MM-DD.');

export const consultaMias = z.object({ ...paginacion, status: opcion(ESTADOS_SOLICITUD).optional() });

export const consultaRecibidas = z.object({
  ...paginacion,
  initiativeId: objectId.optional(),
  status: opcion(ESTADOS_SOLICITUD).optional(),
  skill: z.string().max(40).optional(),
  role: z.string().max(40).optional(),
  from: fecha.optional(),
  to: fecha.optional(),
}).refine((q) => !q.from || !q.to || q.from <= q.to, { message: 'La fecha inicial no puede ser posterior a la final.', path: ['from'] });
```

**`servidor/src/validators/proyectos.schemas.js`**

```js
import { z } from 'zod';
import { ESTADOS_PROYECTO } from '../models/enums.js';
import { REGEX_FECHA, REGEX_GITHUB } from './limites.js';
import { opcion, paginacion, sinRepetidos, texto } from './comunes.js';

export const cuerpoProyecto = z.object({
  name: texto('name'),
  objective: texto('objective'),
  description: texto('description'),
  technologies: z.array(z.string().trim().min(1).max(40, 'Cada tecnología admite hasta 40 caracteres.'))
    .max(10, 'Podés agregar como máximo 10.').refine(sinRepetidos, 'No repitas tecnologías.').default([]),
  startDate: z.string({ required_error: 'Este campo es obligatorio.' }).regex(REGEX_FECHA, 'Usá el formato AAAA-MM-DD.'),
  estimatedEndDate: z.string().regex(REGEX_FECHA, 'Usá el formato AAAA-MM-DD.').nullable().default(null),
  repositoryUrl: z.string().trim().regex(REGEX_GITHUB, 'Usá el formato https://github.com/usuario/repositorio.').nullable().default(null),
}).refine((p) => !p.estimatedEndDate || p.estimatedEndDate >= p.startDate, {
  message: 'Debe ser igual o posterior a la fecha de inicio.', path: ['estimatedEndDate'],
});

export const consultaProyectos = z.object({ ...paginacion, status: opcion(ESTADOS_PROYECTO).optional() });
```

### 8.2 Repositorios

**`servidor/src/repositories/solicitudes.repository.js`** (reemplazar completo)

```js
import { Types } from 'mongoose';
import { Request } from '../models/Request.js';
import { paginar } from '../utils/paginacion.js';

const ORDENES = ['createdAt', 'updatedAt'];

export const contarDeIniciativa = (initiativeId) => Request.countDocuments({ initiativeId });

export async function contarPorIniciativas(ids) {
  if (!ids.length) return new Map();
  const grupos = await Request.aggregate([{ $match: { initiativeId: { $in: ids } } }, { $group: { _id: '$initiativeId', total: { $sum: 1 } } }]);
  return new Map(grupos.map((g) => [String(g._id), g.total]));
}

export const contarPendientes = (initiativeId, userId) => Request.countDocuments({ initiativeId, userId, status: 'pending' });
export const buscarPorId = (id) => Request.findById(id).lean();

export async function crear(datos) {
  const documento = await Request.create(datos);
  return documento.toObject();
}

export const listarDeUsuario = (userId, q) => {
  const filtro = { userId: new Types.ObjectId(userId) };
  if (q.status) filtro.status = q.status;
  return paginar(Request, filtro, q, ORDENES, '-createdAt');
};

export const listarRecibidas = (idsIniciativas, q) => {
  const filtro = { initiativeId: { $in: idsIniciativas } };
  if (q.initiativeId) filtro.initiativeId = { $in: idsIniciativas.filter((id) => String(id) === q.initiativeId) };
  if (q.status) filtro.status = q.status;
  if (q.skill) filtro.offeredSkillId = q.skill;
  if (q.role) filtro.roleId = q.role;
  if (q.from || q.to) {
    filtro.createdAt = {};
    if (q.from) filtro.createdAt.$gte = new Date(`${q.from}T00:00:00.000Z`);
    if (q.to) filtro.createdAt.$lte = new Date(`${q.to}T23:59:59.999Z`);
  }
  return paginar(Request, filtro, q, ORDENES, '-createdAt');
};

// Cambio de estado condicionado a que siga pendiente: si otra petición ya la resolvió, no modifica nada.
export const actualizarSiPendiente = (id, cambios) =>
  Request.findOneAndUpdate({ _id: id, status: 'pending' }, cambios, { new: true, runValidators: true }).lean();

export const revertirAPendiente = (id) =>
  Request.updateOne({ _id: id }, { status: 'pending', decisionComment: null, decidedAt: null });
```

**`servidor/src/repositories/proyectos.repository.js`** (reemplazar completo)

```js
import { Project } from '../models/Project.js';
import { paginar } from '../utils/paginacion.js';

export const buscarPorIniciativa = (initiativeId) => Project.findOne({ initiativeId }).lean();
export const buscarPorId = (id) => Project.findById(id).lean();

export async function mapaPorIniciativas(ids) {
  const proyectos = await Project.find({ initiativeId: { $in: ids } }).lean();
  return new Map(proyectos.map((p) => [String(p.initiativeId), p]));
}

export async function crear(datos) {
  const documento = await Project.create(datos);
  return documento.toObject();
}

export const listarDeIniciativas = (ids, q) => {
  const filtro = { initiativeId: { $in: ids } };
  if (q.status) filtro.status = q.status;
  return paginar(Project, filtro, q, ['createdAt', 'name', 'startDate'], '-createdAt');
};
```

En **`servidor/src/repositories/iniciativas.repository.js`** agregar al final:

```js
// Visibles con acceso completo para el usuario: públicas, institucionales (con sesión) y las propias o de su equipo.
export function idsConAccesoCompleto(usuario) {
  const uid = new Types.ObjectId(usuario.id);
  return Initiative.distinct('_id', {
    $or: [{ visibility: { $in: ['public', 'institutional'] } }, { ownerId: uid }, { 'members.userId': uid }],
  });
}

export const idsDePropietario = (userId) => Initiative.distinct('_id', { ownerId: new Types.ObjectId(userId) });
export const buscarVarias = (ids) => Initiative.find({ _id: { $in: ids } }).lean();

// RN-14 y RN-15 en una sola operación atómica: solo agrega si sigue abierta, hay cupo y no es miembro.
export function agregarMiembroSiHayCupo(id, miembro) {
  return Initiative.findOneAndUpdate(
    {
      _id: id,
      status: { $in: ['published', 'forming'] },
      'members.userId': { $ne: miembro.userId },
      $expr: { $gt: ['$estimatedMembers', { $add: [{ $size: '$members' }, 1] }] },
    },
    { $push: { members: miembro }, $set: { status: 'forming' } },
    { new: true }
  ).lean();
}

export const marcarComoProyecto = (id) =>
  Initiative.findOneAndUpdate({ _id: id, status: { $in: ['published', 'forming'] } }, { status: 'project' }, { new: true }).lean();
```

### 8.3 Servicios

**`servidor/src/services/solicitudes.service.js`**

```js
import * as repo from '../repositories/solicitudes.repository.js';
import * as iniciativasRepo from '../repositories/iniciativas.repository.js';
import * as usuariosRepo from '../repositories/usuarios.repository.js';
import { mapasCatalogo } from '../repositories/catalogos.repository.js';
import { cargarVisible } from './iniciativas.service.js';
import { competenciasOfrecibles, esPropietario, verificarSolicitud } from './reglas.js';
import { serializarSolicitud } from '../utils/serializadores.js';
import { ErrorApi } from '../utils/ErrorApi.js';

async function contextoSolicitudes(solicitudes) {
  const idsIni = [...new Set(solicitudes.map((s) => String(s.initiativeId)))];
  const idsUsuarios = [...new Set(solicitudes.map((s) => String(s.userId)))];
  const [catalogo, iniciativas, usuarios] = await Promise.all([
    mapasCatalogo(), iniciativasRepo.buscarVarias(idsIni), usuariosRepo.mapaPorIds(idsUsuarios),
  ]);
  return { ...catalogo, usuarios, iniciativas: new Map(iniciativas.map((i) => [String(i._id), i])) };
}

const serializarVarias = async (lista) => {
  const ctx = await contextoSolicitudes(lista);
  return lista.map((s) => serializarSolicitud(s, ctx));
};
const serializarUna = async (s) => (await serializarVarias([s]))[0];

// La competencia debe ser requerida por la iniciativa y estar en el perfil; el rol debe existir.
async function validarContenido(cuerpo, ini, usuario) {
  const { roles } = await mapasCatalogo();
  const details = [];
  if (!competenciasOfrecibles(ini, usuario).includes(cuerpo.offeredSkillId)) {
    details.push({ field: 'offeredSkillId', message: 'Elegí una competencia que la iniciativa requiera y que tengas en tu perfil.' });
  }
  if (!roles.has(cuerpo.roleId)) details.push({ field: 'roleId', message: 'Elegí un rol válido.' });
  if (details.length) throw ErrorApi.validacion(details);
}

export async function crear(idIniciativa, cuerpo, usuario) {
  const { ini } = await cargarVisible(idIniciativa, usuario);
  const pendientes = await repo.contarPendientes(ini._id, usuario.id);
  const regla = verificarSolicitud(ini, usuario, pendientes);
  if (regla) throw ErrorApi.conflicto(regla.code, regla.message);
  await validarContenido(cuerpo, ini, usuario);
  const solicitud = await repo.crear({ ...cuerpo, initiativeId: ini._id, userId: usuario.id, status: 'pending' });
  return serializarUna(solicitud);
}

export async function mias(q, usuario) {
  const pagina = await repo.listarDeUsuario(usuario.id, q);
  return { ...pagina, items: await serializarVarias(pagina.items) };
}

export async function recibidas(q, usuario) {
  const propias = await iniciativasRepo.idsDePropietario(usuario.id);
  const pagina = await repo.listarRecibidas(propias, q);
  return { ...pagina, items: await serializarVarias(pagina.items) };
}

async function cargar(id, usuario) {
  const solicitud = await repo.buscarPorId(id);
  if (!solicitud) throw ErrorApi.noEncontrado('La solicitud no existe.');
  const ini = await iniciativasRepo.buscarPorId(solicitud.initiativeId);
  return { solicitud, ini, esSolicitante: String(solicitud.userId) === usuario.id, esDueno: Boolean(ini) && esPropietario(ini, usuario) };
}

export async function obtener(id, usuario) {
  const { solicitud, esSolicitante, esDueno } = await cargar(id, usuario);
  if (!esSolicitante && !esDueno) throw ErrorApi.prohibido('No tenés acceso a esta solicitud.');
  return serializarUna(solicitud);
}

async function cargarPropiaPendiente(id, usuario) {
  const datos = await cargar(id, usuario);
  if (!datos.esSolicitante) throw ErrorApi.prohibido('Solo quien envió la solicitud puede modificarla.');
  if (datos.solicitud.status !== 'pending') throw ErrorApi.conflicto('NOT_PENDING', 'Solo las solicitudes pendientes pueden modificarse.'); // RN-12
  return datos;
}

export async function actualizar(id, cuerpo, usuario) {
  const { ini } = await cargarPropiaPendiente(id, usuario);
  await validarContenido(cuerpo, ini, usuario);
  const actualizada = await repo.actualizarSiPendiente(id, cuerpo);
  if (!actualizada) throw ErrorApi.conflicto('NOT_PENDING', 'Solo las solicitudes pendientes pueden modificarse.');
  return serializarUna(actualizada);
}

export async function cancelar(id, usuario) {
  await cargarPropiaPendiente(id, usuario); // RN-13
  const cancelada = await repo.actualizarSiPendiente(id, { status: 'cancelled' });
  if (!cancelada) throw ErrorApi.conflicto('NOT_PENDING', 'Solo las solicitudes pendientes pueden cancelarse.');
  return serializarUna(cancelada);
}

export async function decidir(id, { decision, comment }, usuario) {
  const { solicitud, ini, esDueno } = await cargar(id, usuario);
  if (!esDueno) throw ErrorApi.prohibido('Solo el propietario de la iniciativa puede resolver solicitudes.');
  if (solicitud.status !== 'pending') throw ErrorApi.conflicto('NOT_PENDING', 'La solicitud ya fue resuelta o cancelada.');

  const resuelta = await repo.actualizarSiPendiente(id, { status: decision, decisionComment: comment || null, decidedAt: new Date() });
  if (!resuelta) throw ErrorApi.conflicto('NOT_PENDING', 'La solicitud ya fue resuelta o cancelada.');

  if (decision === 'accepted') {
    // RN-14: aceptar incorpora automáticamente al equipo, con control de cupo atómico.
    const actualizada = await iniciativasRepo.agregarMiembroSiHayCupo(ini._id, { userId: solicitud.userId, roleId: solicitud.roleId, joinedAt: new Date() });
    if (!actualizada) {
      await repo.revertirAPendiente(id); // sin transacciones: compensación manual
      const cerrada = !['published', 'forming'].includes(ini.status);
      throw cerrada
        ? ErrorApi.conflicto('INITIATIVE_CLOSED', 'La iniciativa ya no recibe integrantes.')
        : ErrorApi.conflicto('NO_SPOTS', 'La iniciativa ya completó los espacios previstos.');
    }
  }
  return serializarUna(resuelta);
}
```

**`servidor/src/services/proyectos.service.js`**

```js
import * as repo from '../repositories/proyectos.repository.js';
import * as iniciativasRepo from '../repositories/iniciativas.repository.js';
import { cargarPropia } from './iniciativas.service.js';
import { contextoIniciativas } from './contexto.service.js';
import { nivelDeAcceso, verificarConversion } from './reglas.js';
import { serializarProyecto } from '../utils/serializadores.js';
import { ErrorApi } from '../utils/ErrorApi.js';

// RN-09 del Avance 2 (RN-17 del documento general) verificada en el servidor.
export async function convertir(idIniciativa, cuerpo, usuario) {
  const ini = await cargarPropia(idIniciativa, usuario);
  const regla = verificarConversion(ini);
  if (regla) throw ErrorApi.conflicto(regla.code, regla.message);
  const proyecto = await repo.crear({ ...cuerpo, initiativeId: ini._id, status: 'planning' }); // índice único: RN-18
  const actualizada = await iniciativasRepo.marcarComoProyecto(ini._id);
  const ctx = await contextoIniciativas([actualizada ?? ini], usuario);
  return serializarProyecto(proyecto, ctx, { detalle: true });
}

export async function listar(q, usuario) {
  const ids = await iniciativasRepo.idsConAccesoCompleto(usuario);
  const pagina = await repo.listarDeIniciativas(ids, q);
  const iniciativas = await iniciativasRepo.buscarVarias(pagina.items.map((p) => p.initiativeId));
  const ctx = await contextoIniciativas(iniciativas, usuario);
  return { ...pagina, items: pagina.items.map((p) => serializarProyecto(p, ctx)) };
}

export async function obtener(id, usuario) {
  const proyecto = await repo.buscarPorId(id);
  const ini = proyecto ? await iniciativasRepo.buscarPorId(proyecto.initiativeId) : null;
  // El proyecto hereda la visibilidad de su iniciativa de origen.
  if (!ini || nivelDeAcceso(ini, usuario) !== 'full') throw ErrorApi.noEncontrado('El proyecto no existe o no tenés acceso.');
  const ctx = await contextoIniciativas([ini], usuario);
  return serializarProyecto(proyecto, ctx, { detalle: true });
}
```

### 8.4 Controladores y rutas

**`servidor/src/controllers/solicitudes.controller.js`**

```js
import * as servicio from '../services/solicitudes.service.js';

export async function crear(req, res) {
  const solicitud = await servicio.crear(req.params.id, req.validado.body, req.usuario);
  res.status(201).location(`/api/requests/${solicitud.id}`).json(solicitud);
}
export async function mias(req, res) { res.json(await servicio.mias(req.validado.query, req.usuario)); }
export async function recibidas(req, res) { res.json(await servicio.recibidas(req.validado.query, req.usuario)); }
export async function obtener(req, res) { res.json(await servicio.obtener(req.params.id, req.usuario)); }
export async function actualizar(req, res) { res.json(await servicio.actualizar(req.params.id, req.validado.body, req.usuario)); }
export async function cancelar(req, res) { res.json(await servicio.cancelar(req.params.id, req.usuario)); }
export async function decidir(req, res) { res.json(await servicio.decidir(req.params.id, req.validado.body, req.usuario)); }
```

**`servidor/src/controllers/proyectos.controller.js`**

```js
import * as servicio from '../services/proyectos.service.js';

export async function convertir(req, res) {
  const proyecto = await servicio.convertir(req.params.id, req.validado.body, req.usuario);
  res.status(201).location(`/api/projects/${proyecto.id}`).json(proyecto);
}
export async function listar(req, res) { res.json(await servicio.listar(req.validado.query, req.usuario)); }
export async function obtener(req, res) { res.json(await servicio.obtener(req.params.id, req.usuario)); }
```

**`servidor/src/routes/solicitudes.routes.js`**

```js
import { Router } from 'express';
import * as c from '../controllers/solicitudes.controller.js';
import { requerirAuth } from '../middleware/autenticacion.js';
import { validar } from '../middleware/validar.js';
import { consultaMias, consultaRecibidas, cuerpoDecision, cuerpoSolicitud } from '../validators/solicitudes.schemas.js';

// Todo el recurso requiere sesión; requerirAuth va en cada ruta para no afectar rutas de otros routers.
const router = Router();
router.post('/initiatives/:id/requests', requerirAuth, validar({ body: cuerpoSolicitud }), c.crear);
router.get('/requests/mine', requerirAuth, validar({ query: consultaMias }), c.mias);
router.get('/requests/received', requerirAuth, validar({ query: consultaRecibidas }), c.recibidas);
router.get('/requests/:id', requerirAuth, c.obtener);
router.put('/requests/:id', requerirAuth, validar({ body: cuerpoSolicitud }), c.actualizar);
router.post('/requests/:id/cancel', requerirAuth, c.cancelar);
router.post('/requests/:id/decision', requerirAuth, validar({ body: cuerpoDecision }), c.decidir);
export default router;
```

**`servidor/src/routes/proyectos.routes.js`**

```js
import { Router } from 'express';
import * as c from '../controllers/proyectos.controller.js';
import { requerirAuth } from '../middleware/autenticacion.js';
import { validar } from '../middleware/validar.js';
import { consultaProyectos, cuerpoProyecto } from '../validators/proyectos.schemas.js';

const router = Router();
router.post('/initiatives/:id/project', requerirAuth, validar({ body: cuerpoProyecto }), c.convertir);
router.get('/projects', requerirAuth, validar({ query: consultaProyectos }), c.listar);
router.get('/projects/:id', requerirAuth, c.obtener);
export default router;
```

**`servidor/src/routes/index.js`** (reemplazar completo)

```js
import { Router } from 'express';
import { config } from '../config/env.js';
import catalogos from './catalogos.routes.js';
import iniciativas from './iniciativas.routes.js';
import solicitudes from './solicitudes.routes.js';
import proyectos from './proyectos.routes.js';
import dev from './dev.routes.js';

const router = Router();
router.use(catalogos);
if (config.authDev) router.use('/dev', dev);
router.use(iniciativas);
router.use(solicitudes);
router.use(proyectos);
export default router;
```

### 8.5 Verificar

`npm run seed` y luego, con el servidor en marcha:

```powershell
$m = @{ 'X-Dev-User' = '650000000000000000000003'; 'Content-Type' = 'application/json' }   # María
$l = @{ 'X-Dev-User' = '650000000000000000000002'; 'Content-Type' = 'application/json' }   # Luis
$cuerpo = '{"message":"Puedo coordinar las actividades del espacio.","offeredSkillId":"comp-gestion","roleId":"rol-coordinacion","availability":"3 h/semana"}'
$s = Invoke-RestMethod -Method Post http://localhost:3000/api/initiatives/660000000000000000000006/requests -Headers $m -Body $cuerpo
curl.exe -i -X POST http://localhost:3000/api/initiatives/660000000000000000000006/requests -H "X-Dev-User: 650000000000000000000003" -H "Content-Type: application/json" -d $cuerpo
Invoke-RestMethod -Method Post "http://localhost:3000/api/requests/$($s.id)/decision" -Headers $l -Body '{"decision":"accepted"}'
(Invoke-RestMethod http://localhost:3000/api/initiatives/660000000000000000000006 -Headers $l).members
```

| Prueba | Esperado |
|---|---|
| María solicita en coworking | 201 con `status: "pending"` |
| María repite | 409 `DUPLICATE_PENDING` |
| María ofrece `comp-javascript` | 400 `offeredSkillId` |
| Diego solicita en "Monitoreo" (proyecto) | 409 `INITIATIVE_CLOSED` |
| Luis acepta | 200 `accepted`; María aparece en `members` |
| Luis acepta otra vez | 409 `NOT_PENDING` |
| Ana pide `GET /requests/<id de María>` | 403 |
| Ana convierte "Plataforma de voluntariado" con `startDate` 2026-11-10 y fin 2026-11-01 | 400 `estimatedEndDate` |
| Ana convierte con fechas válidas | 201; la iniciativa queda en `project` |
| Ana convierte de nuevo | 409 `ALREADY_PROJECT` |
| `GET /projects` como Diego | Solo proyectos de iniciativas que ve completas |

```powershell
git add servidor
git commit -m "Implementar solicitudes, decisión con incorporación al equipo y proyectos" --trailer "Seccion: Servidor" --trailer "Cambio: RN-10 a RN-18 verificadas en el servidor con actualizaciones condicionales"
git push
```

**Explicar en voz alta:** por qué `actualizarSiPendiente` filtra por `status: 'pending'`; cómo `agregarMiembroSiHayCupo` evita superar el cupo con dos aceptaciones simultáneas; qué hace la compensación cuando falla; por qué el proyecto hereda la visibilidad.

---

# DÍA 9 · martes 3 nov · Cliente contra Express y colección de pruebas

### 9.1 Conectar el cliente a la API y borrar el simulado

**`cliente/.env`** y **`cliente/.env.example`**: quitar la línea `VITE_USE_MOCK=true`. Quedan:

```
VITE_API_URL=http://localhost:3000/api
VITE_GITHUB_API_URL=https://api.github.com
```

**`cliente/src/api/cliente.js`**: borrar la constante `USAR_MOCK` y el bloque completo:

```js
  if (USAR_MOCK) {
    const { atenderMock } = await import('../mock/servidorMock.js');
    return atenderMock(metodo, url, cuerpo, cabecerasAuth());
  }
```

```powershell
Remove-Item -Recurse cliente\src\mock, cliente\public\mock
```

Reiniciar `npm run dev` del cliente (Vite solo lee `.env` al arrancar). En la consola del navegador: `localStorage.clear()` y recargar.

Repetir la tabla de verificación del día 4 con el servidor real. Todo debe comportarse igual; si algo difiere, el servidor es la referencia y se corrige el cliente.

```powershell
git add -A cliente
git commit -m "Conectar el cliente a la API Express y eliminar el servidor simulado" --trailer "Seccion: Integración" --trailer "Cambio: Toda la persistencia pasa a MongoDB"
```

### 9.2 Colección Postman generada por código

Escribir 50 peticiones a mano en JSON es propenso a errores. Un script corto genera la colección y el entorno; ambos archivos resultantes se versionan y se pueden abrir en Postman.

**`pruebas/generar-coleccion.mjs`**

```js
// Genera innovation-hub.postman_collection.json y innovation-hub.postman_environment.json.
// Uso: node pruebas/generar-coleccion.mjs
import { writeFileSync } from 'node:fs';

const MODO_AUTH = 'dev'; // el día 10 cambia a 'jwt'

const ID = {
  ana: '650000000000000000000001', luis: '650000000000000000000002', maria: '650000000000000000000003', diego: '650000000000000000000004',
  voluntariado: '660000000000000000000001', energia: '660000000000000000000002', tutorias: '660000000000000000000003',
  reciclaje: '660000000000000000000004', soporte: '660000000000000000000005', coworking: '660000000000000000000006',
  bienestar: '660000000000000000000007', festival: '660000000000000000000008',
  solicitudVoluntariado: '670000000000000000000003', proyectoEnergia: '680000000000000000000001',
  inexistente: '6a0000000000000000000099',
};
const CONTRASENA_PRUEBA = 'Prueba123!';
const CORREOS = { ana: 'ana.mora@ucenfotec.ac.cr', luis: 'luis.perez@ucenfotec.ac.cr', maria: 'maria.solis@ucenfotec.ac.cr', diego: 'diego.vargas@ucenfotec.ac.cr' };

function cabeceras(quien) {
  const lista = [{ key: 'Content-Type', value: 'application/json' }];
  if (!quien) return lista;
  lista.push(MODO_AUTH === 'jwt'
    ? { key: 'Authorization', value: `Bearer {{token_${quien}}}` }
    : { key: 'X-Dev-User', value: ID[quien] });
  return lista;
}

function caso(nombre, metodo, ruta, { quien = null, cuerpo, estado, codigo, extra = [] } = {}) {
  const pruebas = [`pm.test('Responde ${estado}', () => pm.response.to.have.status(${estado}));`];
  if (codigo) pruebas.push(`pm.test('Código de error ${codigo}', () => pm.expect(pm.response.json().error.code).to.eql('${codigo}'));`);
  pruebas.push(...extra);
  return {
    name: nombre,
    event: [{ listen: 'test', script: { type: 'text/javascript', exec: pruebas } }],
    request: {
      method: metodo,
      header: cabeceras(quien),
      url: `{{baseUrl}}${ruta}`,
      ...(cuerpo !== undefined && { body: { mode: 'raw', raw: typeof cuerpo === 'string' ? cuerpo : JSON.stringify(cuerpo, null, 2) } }),
    },
  };
}

const guardar = (variable, expresion = 'pm.response.json().id') => `pm.collectionVariables.set('${variable}', ${expresion});`;
const carpeta = (name, item) => ({ name, item });

const iniciativaNueva = {
  title: 'Huerta comunitaria en el campus', type: 'idea',
  summary: 'Cultivar hortalizas con estudiantes y vecinos del campus.',
  description: 'Proponemos habilitar un espacio de huerta con turnos de cuidado y talleres abiertos a la comunidad.',
  problem: 'No existen espacios verdes productivos en el campus.', beneficiaries: 'Estudiantes y vecinos.',
  categoryId: 'cat-sostenibilidad', requiredSkillIds: ['comp-gestion'], estimatedMembers: 4, visibility: 'public', tags: ['huerta', 'comunidad'],
};
const solicitudMaria = { message: 'Puedo coordinar las actividades del espacio.', offeredSkillId: 'comp-gestion', roleId: 'rol-coordinacion', availability: '3 h/semana' };
const proyecto = {
  name: 'Plataforma de voluntariado', objective: 'Centralizar convocatorias de voluntariado del campus.',
  description: 'Aplicación web para publicar convocatorias, inscribirse y certificar horas de voluntariado.',
  technologies: ['React', 'Express'], startDate: '2026-11-10', estimatedEndDate: '2027-03-01', repositoryUrl: null,
};

const carpetas = [];

if (MODO_AUTH === 'jwt') {
  carpetas.push(carpeta('00 Preparación', Object.keys(CORREOS).map((quien) => caso(`Login ${quien}`, 'POST', '/auth/login', {
    cuerpo: { email: CORREOS[quien], password: CONTRASENA_PRUEBA }, estado: 200,
    extra: [guardar(`token_${quien}`, 'pm.response.json().token')],
  }))));

  carpetas.push(carpeta('01 Autenticación', [
    caso('Registro correcto', 'POST', '/auth/register', {
      cuerpo: { firstName: 'Sofía', lastName: 'Quesada', email: 'sofia.quesada@ucenfotec.ac.cr', password: 'Clave2026x', confirmPassword: 'Clave2026x', memberType: 'student', career: 'Diseño', acceptTerms: true },
      estado: 201, extra: ["pm.test('No devuelve contraseña', () => pm.expect(pm.response.json()).to.not.have.any.keys('password', 'passwordHash'));"],
    }),
    caso('Registro con datos inválidos', 'POST', '/auth/register', { cuerpo: { firstName: 'S', email: 'no-es-correo', password: '123', confirmPassword: '456', acceptTerms: false }, estado: 400, codigo: 'VALIDATION_ERROR' }),
    caso('Registro con correo existente', 'POST', '/auth/register', {
      cuerpo: { firstName: 'Otra', lastName: 'Persona', email: CORREOS.ana, password: 'Clave2026x', confirmPassword: 'Clave2026x', memberType: 'student', career: 'Diseño', acceptTerms: true },
      estado: 409, codigo: 'EMAIL_TAKEN',
    }),
    caso('Login con contraseña incorrecta', 'POST', '/auth/login', { cuerpo: { email: CORREOS.ana, password: 'incorrecta1' }, estado: 401, codigo: 'INVALID_CREDENTIALS' }),
    caso('Login de la cuenta nueva', 'POST', '/auth/login', { cuerpo: { email: 'sofia.quesada@ucenfotec.ac.cr', password: 'Clave2026x' }, estado: 200, extra: [guardar('token_sofia', 'pm.response.json().token')] }),
    caso('Perfil propio', 'GET', '/auth/me', { quien: 'sofia', estado: 200 }),
    caso('Perfil propio sin token', 'GET', '/auth/me', { estado: 401, codigo: 'UNAUTHORIZED' }),
    caso('Logout', 'POST', '/auth/logout', { quien: 'sofia', estado: 204 }),
    caso('Token revocado tras logout', 'GET', '/auth/me', { quien: 'sofia', estado: 401, codigo: 'UNAUTHORIZED' }),
  ]));

  carpetas.push(carpeta('02 Usuarios', [
    caso('Buscar por competencia y disponibilidad', 'GET', '/users?skill=comp-gestion&available=true', { quien: 'ana', estado: 200 }),
    caso('Buscar con tamaño inválido', 'GET', '/users?size=abc', { quien: 'ana', estado: 400, codigo: 'VALIDATION_ERROR' }),
    caso('Perfil inexistente', 'GET', `/users/${ID.inexistente}`, { quien: 'ana', estado: 404, codigo: 'NOT_FOUND' }),
    caso('Buscar sin token', 'GET', '/users', { estado: 401, codigo: 'UNAUTHORIZED' }),
    caso('Modificar perfil', 'PUT', '/users/me', {
      quien: 'diego', estado: 200,
      cuerpo: { firstName: 'Diego', lastName: 'Vargas', secondLastName: null, career: 'Administración', bio: 'Marketing para emprendimientos.', skills: [{ skillId: 'comp-marketing', level: 'advanced' }], interests: ['emprendimiento'], availability: { hoursPerWeek: 2, detail: 'viernes' }, portfolioUrl: null },
    }),
    caso('Modificar el correo desde el perfil', 'PUT', '/users/me', {
      quien: 'diego', estado: 400, codigo: 'VALIDATION_ERROR',
      cuerpo: { firstName: 'Diego', lastName: 'Vargas', career: 'Administración', email: 'otro@ucenfotec.ac.cr' },
    }),
  ]));
}

carpetas.push(carpeta('03 Catálogos', [
  caso('Categorías', 'GET', '/categories', { estado: 200, extra: ["pm.test('Ocho categorías', () => pm.expect(pm.response.json()).to.have.length(8));"] }),
  caso('Competencias', 'GET', '/skills', { estado: 200 }),
  caso('Roles', 'GET', '/roles', { estado: 200 }),
  caso('Ruta inexistente', 'GET', '/no-existe', { estado: 404, codigo: 'NOT_FOUND' }),
]));

carpetas.push(carpeta('04 Iniciativas', [
  caso('Listar con filtros combinados', 'GET', '/initiatives?type=challenge&category=cat-sostenibilidad&sort=title', {
    estado: 200, extra: ["pm.test('Sobre de paginación', () => pm.expect(pm.response.json()).to.have.all.keys('items', 'page', 'size', 'totalItems', 'totalPages'));"],
  }),
  caso('Tamaño de página limitado a 50', 'GET', '/initiatives?size=1000', { estado: 200, extra: ["pm.test('size = 50', () => pm.expect(pm.response.json().size).to.eql(50));"] }),
  caso('Listar con tipo inválido', 'GET', '/initiatives?type=cualquiera', { estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Mis iniciativas sin token', 'GET', '/initiatives?owner=me', { estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Restringida vista por un tercero', 'GET', `/initiatives/${ID.soporte}`, {
    quien: 'ana', estado: 200,
    extra: ["pm.test('Solo el resumen', () => pm.expect(Object.keys(pm.response.json()).sort()).to.eql(['accessLevel','category','id','requiredSkills','summary','title','type','visibility']));"],
  }),
  caso('Privada vista por un tercero', 'GET', `/initiatives/${ID.bienestar}`, { quien: 'diego', estado: 404, codigo: 'NOT_FOUND' }),
  caso('Institucional sin sesión', 'GET', `/initiatives/${ID.tutorias}`, { estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Detalle inexistente', 'GET', `/initiatives/${ID.inexistente}`, { estado: 404, codigo: 'NOT_FOUND' }),
  caso('Crear', 'POST', '/initiatives', { quien: 'ana', cuerpo: iniciativaNueva, estado: 201, extra: [guardar('nuevaIniciativa'), "pm.test('Location', () => pm.response.to.have.header('Location'));"] }),
  caso('Crear con datos inválidos', 'POST', '/initiatives', { quien: 'ana', cuerpo: { ...iniciativaNueva, title: 'abc', estimatedMembers: 50, visibility: 'secreta' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Crear sin sesión', 'POST', '/initiatives', { cuerpo: iniciativaNueva, estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Modificar', 'PUT', '/initiatives/{{nuevaIniciativa}}', { quien: 'ana', cuerpo: { ...iniciativaNueva, estimatedMembers: 5 }, estado: 200 }),
  caso('Modificar iniciativa ajena', 'PUT', '/initiatives/{{nuevaIniciativa}}', { quien: 'luis', cuerpo: iniciativaNueva, estado: 403, codigo: 'FORBIDDEN' }),
  caso('Modificar inexistente', 'PUT', `/initiatives/${ID.inexistente}`, { quien: 'ana', cuerpo: iniciativaNueva, estado: 404, codigo: 'NOT_FOUND' }),
  caso('Integrantes menores que el equipo', 'PUT', `/initiatives/${ID.voluntariado}`, {
    quien: 'ana', estado: 409, codigo: 'PARTICIPANTS_BELOW_TEAM',
    cuerpo: { ...iniciativaNueva, title: 'Plataforma de voluntariado universitario', estimatedMembers: 1 },
  }),
  caso('Eliminar con información relacionada', 'DELETE', `/initiatives/${ID.voluntariado}`, { quien: 'ana', estado: 409, codigo: 'HAS_RELATED_DATA' }),
  caso('Eliminar iniciativa ajena', 'DELETE', '/initiatives/{{nuevaIniciativa}}', { quien: 'luis', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Archivar', 'POST', '/initiatives/{{nuevaIniciativa}}/archive', { quien: 'ana', estado: 200, extra: ["pm.test('Archivada', () => pm.expect(pm.response.json().status).to.eql('archived'));"] }),
  caso('Modificar archivada', 'PUT', '/initiatives/{{nuevaIniciativa}}', { quien: 'ana', cuerpo: iniciativaNueva, estado: 409, codigo: 'ARCHIVED' }),
  caso('Eliminar sin relaciones', 'DELETE', '/initiatives/{{nuevaIniciativa}}', { quien: 'ana', estado: 204 }),
  caso('Eliminar inexistente', 'DELETE', `/initiatives/${ID.inexistente}`, { quien: 'ana', estado: 404, codigo: 'NOT_FOUND' }),
]));

carpetas.push(carpeta('05 Equipo', [
  caso('Listar miembros', 'GET', `/initiatives/${ID.voluntariado}/members`, { quien: 'ana', estado: 200 }),
  caso('Miembros de una restringida ajena', 'GET', `/initiatives/${ID.soporte}/members`, { quien: 'ana', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Retirar al propietario', 'DELETE', `/initiatives/${ID.voluntariado}/members/${ID.ana}`, { quien: 'ana', estado: 409, codigo: 'CANNOT_REMOVE_OWNER' }),
  caso('Retirar sin ser propietario', 'DELETE', `/initiatives/${ID.coworking}/members/${ID.diego}`, { quien: 'ana', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Retirar a quien no es miembro', 'DELETE', `/initiatives/${ID.voluntariado}/members/${ID.diego}`, { quien: 'ana', estado: 404, codigo: 'NOT_FOUND' }),
  caso('Retirar miembro', 'DELETE', `/initiatives/${ID.coworking}/members/${ID.diego}`, { quien: 'luis', estado: 204 }),
]));

carpetas.push(carpeta('06 Solicitudes', [
  caso('Crear solicitud', 'POST', `/initiatives/${ID.coworking}/requests`, { quien: 'maria', cuerpo: solicitudMaria, estado: 201, extra: [guardar('nuevaSolicitud')] }),
  caso('Solicitud duplicada pendiente', 'POST', `/initiatives/${ID.coworking}/requests`, { quien: 'maria', cuerpo: solicitudMaria, estado: 409, codigo: 'DUPLICATE_PENDING' }),
  caso('Competencia que no tiene', 'POST', `/initiatives/${ID.festival}/requests`, { quien: 'luis', cuerpo: { ...solicitudMaria, offeredSkillId: 'comp-redes' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Solicitar en la propia', 'POST', `/initiatives/${ID.voluntariado}/requests`, { quien: 'ana', cuerpo: solicitudMaria, estado: 409, codigo: 'SELF_REQUEST' }),
  caso('Solicitar en un proyecto', 'POST', `/initiatives/${ID.energia}/requests`, { quien: 'diego', cuerpo: solicitudMaria, estado: 409, codigo: 'INITIATIVE_CLOSED' }),
  caso('Solicitar en inexistente', 'POST', `/initiatives/${ID.inexistente}/requests`, { quien: 'maria', cuerpo: solicitudMaria, estado: 404, codigo: 'NOT_FOUND' }),
  caso('Solicitar sin sesión', 'POST', `/initiatives/${ID.coworking}/requests`, { cuerpo: solicitudMaria, estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Mis solicitudes', 'GET', '/requests/mine?status=pending', { quien: 'maria', estado: 200 }),
  caso('Recibidas con filtros', 'GET', `/requests/received?initiativeId=${ID.coworking}&status=pending`, { quien: 'luis', estado: 200 }),
  caso('Recibidas con fechas invertidas', 'GET', '/requests/received?from=2026-12-01&to=2026-01-01', { quien: 'luis', estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Ver solicitud ajena', 'GET', '/requests/{{nuevaSolicitud}}', { quien: 'ana', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Modificar pendiente', 'PUT', '/requests/{{nuevaSolicitud}}', { quien: 'maria', cuerpo: { ...solicitudMaria, availability: '4 h/semana' }, estado: 200 }),
  caso('Resolver sin ser propietario', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'maria', cuerpo: { decision: 'accepted' }, estado: 403, codigo: 'FORBIDDEN' }),
  caso('Decisión inválida', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'luis', cuerpo: { decision: 'tal vez' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Aceptar e incorporar', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'luis', cuerpo: { decision: 'accepted', comment: 'Bienvenida' }, estado: 200 }),
  caso('Resolver de nuevo', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'luis', cuerpo: { decision: 'rejected' }, estado: 409, codigo: 'NOT_PENDING' }),
  caso('Cancelar ya resuelta', 'POST', '/requests/{{nuevaSolicitud}}/cancel', { quien: 'maria', estado: 409, codigo: 'NOT_PENDING' }),
  caso('Cancelar pendiente', 'POST', `/requests/${ID.solicitudVoluntariado}/cancel`, { quien: 'maria', estado: 200 }),
  caso('Cancelar inexistente', 'POST', `/requests/${ID.inexistente}/cancel`, { quien: 'maria', estado: 404, codigo: 'NOT_FOUND' }),
]));

carpetas.push(carpeta('07 Proyectos', [
  caso('Convertir con datos inválidos', 'POST', `/initiatives/${ID.voluntariado}/project`, { quien: 'ana', cuerpo: { ...proyecto, name: '', estimatedEndDate: '2026-01-01' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Convertir sin ser propietario', 'POST', `/initiatives/${ID.voluntariado}/project`, { quien: 'luis', cuerpo: proyecto, estado: 403, codigo: 'FORBIDDEN' }),
  caso('Convertir sin integrantes', 'POST', `/initiatives/${ID.festival}/project`, { quien: 'diego', cuerpo: proyecto, estado: 409, codigo: 'NO_MEMBERS' }),
  caso('Convertir', 'POST', `/initiatives/${ID.voluntariado}/project`, { quien: 'ana', cuerpo: proyecto, estado: 201, extra: [guardar('nuevoProyecto')] }),
  caso('Convertir de nuevo', 'POST', `/initiatives/${ID.voluntariado}/project`, { quien: 'ana', cuerpo: proyecto, estado: 409, codigo: 'ALREADY_PROJECT' }),
  caso('Listar proyectos', 'GET', '/projects?sort=name', { quien: 'luis', estado: 200 }),
  caso('Detalle de proyecto', 'GET', '/projects/{{nuevoProyecto}}', { quien: 'luis', estado: 200, extra: ["pm.test('Conserva la iniciativa de origen', () => pm.expect(pm.response.json().initiative.id).to.eql('660000000000000000000001'));"] }),
  caso('Proyecto inexistente', 'GET', `/projects/${ID.inexistente}`, { quien: 'luis', estado: 404, codigo: 'NOT_FOUND' }),
  caso('Proyectos sin sesión', 'GET', '/projects', { estado: 401, codigo: 'UNAUTHORIZED' }),
]));

const coleccion = {
  info: {
    name: 'Innovation Hub API',
    description: 'Casos correctos, inválidos, inexistentes y no autorizados por recurso. Ejecutar después de npm run seed.',
    schema: 'https://schema.getpostman.com/json/collection/v2.1.0/collection.json',
  },
  event: [{
    listen: 'test',
    script: { type: 'text/javascript', exec: [
      "pm.test('Sin datos sensibles en la respuesta', () => {",
      "  const texto = pm.response.text();",
      "  // '\"password\":' detecta un valor filtrado; '\"field\":\"password\"' en un error de validación es legítimo.",
      "  ['passwordHash', '\"password\":', 'mongodb://', 'stack'].forEach((s) => pm.expect(texto).to.not.include(s));",
      '});',
      "if (pm.response.code >= 400) pm.test('Formato de error uniforme', () => pm.expect(pm.response.json().error).to.have.all.keys('code', 'message', 'details'));",
    ] },
  }],
  variable: [{ key: 'nuevaIniciativa', value: '' }, { key: 'nuevaSolicitud', value: '' }, { key: 'nuevoProyecto', value: '' }],
  item: carpetas,
};

const entorno = { name: 'Innovation Hub local', values: [{ key: 'baseUrl', value: 'http://localhost:3000/api', enabled: true }] };

const destino = new URL('.', import.meta.url);
writeFileSync(new URL('innovation-hub.postman_collection.json', destino), JSON.stringify(coleccion, null, 2));
writeFileSync(new URL('innovation-hub.postman_environment.json', destino), JSON.stringify(entorno, null, 2));
console.log(`Colección generada: ${carpetas.reduce((n, c) => n + c.item.length, 0)} peticiones en ${carpetas.length} carpetas.`);
```

### 9.3 Verificar

```powershell
node pruebas\generar-coleccion.mjs
cd servidor
npm run test:api
cd ..
```

Newman debe terminar con 0 fallos. Si falla una prueba, leer el mensaje: indica la petición, el código esperado y el recibido; la corrección va en la capa que produjo el código equivocado (validador, servicio o repositorio). Abrir los dos JSON en Postman (Import) para verlos de forma visual.

```powershell
git add pruebas servidor
git commit -m "Agregar colección Postman generada con casos por recurso" --trailer "Seccion: Pruebas" --trailer "Cambio: Correcto, inválido, inexistente y no autorizado; ejecución con Newman"
git push
```

**Explicar en voz alta:** cómo encadena la colección los `id` creados entre peticiones; qué revisa el script de nivel colección; por qué la colección siempre corre después de `npm run seed`.

---

# DÍA 10 · miércoles 4 nov · Autenticación, usuarios y perfil

**Cómo funciona, en cuatro pasos.**
1. **Registro:** la contraseña se transforma con **bcrypt** (12 rondas). bcrypt agrega una sal aleatoria y es lento a propósito, así que un hash robado no se puede revertir ni probar millones de veces por segundo. SHA-256 sería rápido y por eso inadecuado.
2. **Login:** se compara la contraseña con el hash. Si coincide, el servidor firma un **JWT**: un texto con `sub` (id del usuario), `jti` (id único del token) y `exp` (vencimiento a 2 h), firmado con `JWT_SECRET`. Nadie puede alterarlo sin invalidar la firma.
3. **Cada petición:** el cliente envía `Authorization: Bearer <token>`. El middleware `identificar` verifica la firma y el vencimiento, revisa que el `jti` no esté revocado y carga al usuario en `req.usuario`.
4. **Logout:** el `jti` se guarda en `revokedtokens` hasta que el token vence. Desde ese momento el mismo token responde 401, aunque su firma siga siendo válida. MongoDB borra la entrada sola gracias al índice TTL.

### 10.1 Servidor

**`servidor/src/repositories/usuarios.repository.js`** (reemplazar completo)

```js
import { User } from '../models/User.js';
import { paginar, escaparRegex } from '../utils/paginacion.js';

const CAMPOS_RESUMEN = 'firstName lastName secondLastName career skills';

export async function mapaPorIds(ids) {
  const usuarios = await User.find({ _id: { $in: ids } }).select(CAMPOS_RESUMEN).lean();
  return new Map(usuarios.map((u) => [String(u._id), u]));
}

export const buscarActivoPorId = (id) => User.findOne({ _id: id, status: 'active' }).lean();
export const existeCorreo = (email) => User.exists({ email });
// Única consulta que trae el hash; solo la usa el login.
export const buscarConHashPorCorreo = (email) => User.findOne({ email }).select('+passwordHash').lean();

export async function crear(datos) {
  const documento = await User.create(datos);
  const { passwordHash, ...sinHash } = documento.toObject();
  return sinHash;
}

export const actualizar = (id, cambios) => User.findByIdAndUpdate(id, cambios, { new: true, runValidators: true }).lean();

export function buscar(q) {
  const condiciones = [{ status: 'active' }];
  // Cada palabra debe aparecer en alguno de los nombres: "ana mora" encuentra a Ana Mora Jiménez.
  for (const palabra of (q.q ?? '').split(/\s+/).filter(Boolean)) {
    const patron = new RegExp(escaparRegex(palabra), 'i');
    condiciones.push({ $or: [{ firstName: patron }, { lastName: patron }, { secondLastName: patron }] });
  }
  if (q.skill) condiciones.push({ 'skills.skillId': q.skill });
  if (q.career) condiciones.push({ career: new RegExp(escaparRegex(q.career), 'i') });
  if (q.available === 'true') condiciones.push({ 'availability.hoursPerWeek': { $gt: 0 } });
  return paginar(User, { $and: condiciones }, q, ['firstName', 'lastName', 'createdAt'], 'firstName');
}
```

**`servidor/src/repositories/tokens.repository.js`**

```js
import { RevokedToken } from '../models/RevokedToken.js';

export const revocar = (jti, expiresAt) => RevokedToken.updateOne({ jti }, { jti, expiresAt }, { upsert: true });
export const estaRevocado = async (jti) => Boolean(await RevokedToken.exists({ jti }));
```

**`servidor/src/validators/auth.schemas.js`**

```js
import { z } from 'zod';
import { TIPOS_MIEMBRO } from '../models/enums.js';
import { opcion } from './comunes.js';

const obligatorio = { required_error: 'Este campo es obligatorio.', invalid_type_error: 'Debe ser texto.' };
export const nombre = z.string(obligatorio).trim().min(2, 'Debe tener al menos 2 caracteres.').max(60, 'No debe superar los 60 caracteres.');
const correo = z.string(obligatorio).trim().toLowerCase().max(120, 'No debe superar los 120 caracteres.').email('Ingresá un correo válido.');

export const cuerpoRegistro = z.object({
  firstName: nombre,
  lastName: nombre,
  secondLastName: nombre.nullable().optional(),
  email: correo,
  password: z.string(obligatorio).min(8, 'Debe tener al menos 8 caracteres.').max(72, 'No debe superar los 72 caracteres.')
    .regex(/[A-Za-zÁÉÍÓÚáéíóúÑñ]/, 'Debe incluir al menos una letra.').regex(/\d/, 'Debe incluir al menos un número.'),
  confirmPassword: z.string(obligatorio),
  memberType: opcion(TIPOS_MIEMBRO),
  career: nombre,
  acceptTerms: z.literal(true, { errorMap: () => ({ message: 'Debés aceptar los términos de uso.' }) }),
}).refine((d) => d.password === d.confirmPassword, { message: 'Las contraseñas no coinciden.', path: ['confirmPassword'] });

export const cuerpoLogin = z.object({
  email: correo,
  password: z.string(obligatorio).min(1, 'Este campo es obligatorio.'),
});
```

**`servidor/src/validators/usuarios.schemas.js`**

```js
import { z } from 'zod';
import { NIVELES } from '../models/enums.js';
import { opcion, paginacion, sinRepetidos } from './comunes.js';
import { nombre } from './auth.schemas.js';

export const cuerpoPerfil = z.object({
  firstName: nombre,
  lastName: nombre,
  secondLastName: nombre.nullable().optional(),
  career: nombre,
  bio: z.string().trim().max(500, 'No debe superar los 500 caracteres.').optional(),
  skills: z.array(z.object({ skillId: z.string().min(1), level: opcion(NIVELES) }))
    .max(15, 'Podés registrar como máximo 15.')
    .refine((lista) => sinRepetidos(lista.map((s) => s.skillId)), 'No repitas competencias.').optional(),
  interests: z.array(z.string().trim().min(2, 'Cada interés necesita al menos 2 caracteres.').max(30)).max(10, 'Podés agregar como máximo 10.').optional(),
  availability: z.object({
    hoursPerWeek: z.number({ invalid_type_error: 'Debe ser un número entero.' }).int().min(0, 'Entre 0 y 40.').max(40, 'Entre 0 y 40.'),
    detail: z.string().trim().max(100).default(''),
  }).optional(),
  portfolioUrl: z.string().trim().max(200).regex(/^https?:\/\/\S+$/, 'Ingresá una URL que empiece con http:// o https://.').nullable().optional(),
  email: z.string().optional(), // se acepta solo para rechazarlo con un mensaje claro si cambia
});

export const consultaUsuarios = z.object({
  ...paginacion,
  q: z.string().trim().max(60).optional(),
  skill: z.string().max(40).optional(),
  career: z.string().trim().max(60).optional(),
  available: z.enum(['true', 'false']).optional(),
});
```

**`servidor/src/services/auth.service.js`**

```js
import { randomUUID } from 'node:crypto';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';
import { config } from '../config/env.js';
import * as usuariosRepo from '../repositories/usuarios.repository.js';
import * as tokensRepo from '../repositories/tokens.repository.js';
import { mapasCatalogo } from '../repositories/catalogos.repository.js';
import { serializarUsuarioPropio } from '../utils/serializadores.js';
import { ErrorApi } from '../utils/ErrorApi.js';

// Si el correo no existe se compara igual contra un hash falso,
// para que la respuesta tarde lo mismo y no revele qué correos están registrados.
const HASH_FALSO = bcrypt.hashSync('no-existe-este-usuario', config.rondasBcrypt);

export async function registrar(datos) {
  if (await usuariosRepo.existeCorreo(datos.email)) throw ErrorApi.conflicto('EMAIL_TAKEN', 'Ya existe una cuenta con ese correo.');
  const passwordHash = await bcrypt.hash(datos.password, config.rondasBcrypt);
  const usuario = await usuariosRepo.crear({
    firstName: datos.firstName, lastName: datos.lastName, secondLastName: datos.secondLastName ?? null,
    email: datos.email, passwordHash, memberType: datos.memberType, career: datos.career, acceptedTermsAt: new Date(),
  });
  return serializarUsuarioPropio(usuario, await mapasCatalogo());
}

export async function iniciarSesion({ email, password }) {
  const usuario = await usuariosRepo.buscarConHashPorCorreo(email);
  const coincide = await bcrypt.compare(password, usuario?.passwordHash ?? HASH_FALSO);
  if (!usuario || !coincide || usuario.status !== 'active') {
    throw new ErrorApi(401, 'INVALID_CREDENTIALS', 'Correo o contraseña incorrectos.');
  }
  const token = jwt.sign({ sub: String(usuario._id) }, config.jwtSecreto, {
    algorithm: 'HS256', expiresIn: config.jwtExpira, jwtid: randomUUID(),
  });
  const { exp } = jwt.decode(token);
  const { passwordHash, ...sinHash } = usuario;
  return {
    token, tokenType: 'Bearer', expiresAt: new Date(exp * 1000).toISOString(),
    user: serializarUsuarioPropio(sinHash, await mapasCatalogo()),
  };
}

export async function cerrarSesion({ jti, exp }) {
  await tokensRepo.revocar(jti, new Date(exp * 1000));
}
```

**`servidor/src/services/usuarios.service.js`**

```js
import * as repo from '../repositories/usuarios.repository.js';
import { mapasCatalogo } from '../repositories/catalogos.repository.js';
import { serializarUsuarioPropio, serializarUsuarioPublico } from '../utils/serializadores.js';
import { ErrorApi } from '../utils/ErrorApi.js';

export async function perfilPropio(usuario) {
  return serializarUsuarioPropio(usuario, await mapasCatalogo());
}

export async function buscar(q) {
  const [pagina, ctx] = await Promise.all([repo.buscar(q), mapasCatalogo()]);
  return { ...pagina, items: pagina.items.map((u) => serializarUsuarioPublico(u, ctx)) };
}

export async function obtener(id) {
  const usuario = await repo.buscarActivoPorId(id);
  if (!usuario) throw ErrorApi.noEncontrado('La persona no existe.');
  return serializarUsuarioPublico(usuario, await mapasCatalogo());
}

export async function actualizarPerfil(usuario, { email, ...cambios }) {
  // El correo es el identificador de la cuenta: no se modifica desde el perfil.
  if (email !== undefined && email.trim().toLowerCase() !== usuario.email) {
    throw ErrorApi.validacion([{ field: 'email', message: 'El correo no se puede modificar desde el perfil.' }]);
  }
  const ctx = await mapasCatalogo();
  if (cambios.skills?.some((s) => !ctx.competencias.has(s.skillId))) {
    throw ErrorApi.validacion([{ field: 'skills', message: 'Alguna competencia no existe.' }]);
  }
  return serializarUsuarioPropio(await repo.actualizar(usuario.id, cambios), ctx);
}
```

**`servidor/src/middleware/autenticacion.js`** (reemplazar completo; las rutas no cambian)

```js
import jwt from 'jsonwebtoken';
import { config } from '../config/env.js';
import * as usuariosRepo from '../repositories/usuarios.repository.js';
import * as tokensRepo from '../repositories/tokens.repository.js';
import { ErrorApi } from '../utils/ErrorApi.js';

// Corre en toda petición a /api. Sin encabezado: visitante. Con encabezado inválido: 401.
export async function identificar(req, _res, next) {
  req.usuario = null;
  const cabecera = req.get('Authorization');
  if (!cabecera) return next();

  const [esquema, token] = cabecera.split(' ');
  if (esquema !== 'Bearer' || !token) throw ErrorApi.noAutenticado('El encabezado debe tener la forma "Authorization: Bearer <token>".');

  let carga;
  try {
    carga = jwt.verify(token, config.jwtSecreto, { algorithms: ['HS256'] });
  } catch (error) {
    throw ErrorApi.noAutenticado(error.name === 'TokenExpiredError' ? 'La sesión venció. Iniciá sesión de nuevo.' : 'El token no es válido.');
  }
  if (await tokensRepo.estaRevocado(carga.jti)) throw ErrorApi.noAutenticado('La sesión fue cerrada. Iniciá sesión de nuevo.');

  const usuario = await usuariosRepo.buscarActivoPorId(carga.sub);
  if (!usuario) throw ErrorApi.noAutenticado('La cuenta no existe o está inactiva.');

  req.usuario = { ...usuario, id: String(usuario._id) };
  req.token = { jti: carga.jti, exp: carga.exp };
  return next();
}

export function requerirAuth(req, _res, next) {
  if (!req.usuario) throw ErrorApi.noAutenticado();
  next();
}
```

**`servidor/src/middleware/limiteLogin.js`**

```js
import rateLimit from 'express-rate-limit';

// Solo cuentan los intentos fallidos: frena la fuerza bruta sin molestar a quien entra bien.
export const limiteLogin = rateLimit({
  windowMs: 15 * 60 * 1000,
  limit: 20,
  skipSuccessfulRequests: true,
  standardHeaders: 'draft-7',
  legacyHeaders: false,
  handler: (_req, res) => res.status(429).json({
    error: { code: 'TOO_MANY_REQUESTS', message: 'Demasiados intentos fallidos. Esperá unos minutos.', details: [] },
  }),
});
```

**`servidor/src/controllers/auth.controller.js`**

```js
import * as servicio from '../services/auth.service.js';
import * as usuarios from '../services/usuarios.service.js';

export async function registrar(req, res) {
  const usuario = await servicio.registrar(req.validado.body);
  res.status(201).location(`/api/users/${usuario.id}`).json(usuario);
}
export async function login(req, res) {
  res.json(await servicio.iniciarSesion(req.validado.body));
}
export async function logout(req, res) {
  await servicio.cerrarSesion(req.token);
  res.status(204).end();
}
export async function me(req, res) {
  res.json(await usuarios.perfilPropio(req.usuario));
}
```

**`servidor/src/controllers/usuarios.controller.js`**

```js
import * as servicio from '../services/usuarios.service.js';

export async function buscar(req, res) { res.json(await servicio.buscar(req.validado.query)); }
export async function obtener(req, res) { res.json(await servicio.obtener(req.params.id)); }
export async function actualizar(req, res) { res.json(await servicio.actualizarPerfil(req.usuario, req.validado.body)); }
```

**`servidor/src/routes/auth.routes.js`**

```js
import { Router } from 'express';
import * as c from '../controllers/auth.controller.js';
import { requerirAuth } from '../middleware/autenticacion.js';
import { limiteLogin } from '../middleware/limiteLogin.js';
import { validar } from '../middleware/validar.js';
import { cuerpoLogin, cuerpoRegistro } from '../validators/auth.schemas.js';

const router = Router();
router.post('/auth/register', validar({ body: cuerpoRegistro }), c.registrar);
router.post('/auth/login', limiteLogin, validar({ body: cuerpoLogin }), c.login);
router.post('/auth/logout', requerirAuth, c.logout);
router.get('/auth/me', requerirAuth, c.me);
export default router;
```

**`servidor/src/routes/usuarios.routes.js`**

```js
import { Router } from 'express';
import * as c from '../controllers/usuarios.controller.js';
import { requerirAuth } from '../middleware/autenticacion.js';
import { validar } from '../middleware/validar.js';
import { consultaUsuarios, cuerpoPerfil } from '../validators/usuarios.schemas.js';

const router = Router();
router.get('/users', requerirAuth, validar({ query: consultaUsuarios }), c.buscar);
router.put('/users/me', requerirAuth, validar({ body: cuerpoPerfil }), c.actualizar);
router.get('/users/:id', requerirAuth, c.obtener);
export default router;
```

**`servidor/src/routes/index.js`** (reemplazar completo; desaparece la ruta de desarrollo)

```js
import { Router } from 'express';
import auth from './auth.routes.js';
import usuarios from './usuarios.routes.js';
import catalogos from './catalogos.routes.js';
import iniciativas from './iniciativas.routes.js';
import solicitudes from './solicitudes.routes.js';
import proyectos from './proyectos.routes.js';

const router = Router();
router.use(auth);
router.use(usuarios);
router.use(catalogos);
router.use(iniciativas);
router.use(solicitudes);
router.use(proyectos);
export default router;
```

**Quitar todo lo temporal del servidor:**

```powershell
Remove-Item servidor\src\routes\dev.routes.js
```

- `servidor/src/config/env.js`: borrar la línea `authDev` y su comentario.
- `servidor/src/app.js`: en `allowedHeaders` dejar solo `['Content-Type', 'Authorization']`.
- `servidor/.env` y `servidor/.env.example`: borrar `DEV_AUTH=true`.

```powershell
git add -A servidor
git commit -m "Implementar registro, login y logout con bcrypt, JWT y revocación" --trailer "Seccion: Seguridad" --trailer "Cambio: Autenticación real; se elimina el usuario de desarrollo"
```

### 10.2 Cliente: token, contexto de sesión y rutas protegidas

```powershell
Remove-Item cliente\src\api\dev.js, cliente\src\components\layout\SelectorSesion.jsx
```

**`cliente/src/api/sesion.js`** (reemplazar completo)

```js
// El token vive en sessionStorage: se pierde al cerrar la pestaña.
// Riesgo conocido: un script inyectado (XSS) podría leerlo. Mitigación: React escapa todo
// el contenido dinámico y la aplicación nunca usa dangerouslySetInnerHTML.
const CLAVE = 'ih.token';

export const obtenerToken = () => sessionStorage.getItem(CLAVE);
export const guardarToken = (token) => sessionStorage.setItem(CLAVE, token);
export const borrarToken = () => sessionStorage.removeItem(CLAVE);

export function cabecerasAuth() {
  const token = obtenerToken();
  return token ? { Authorization: `Bearer ${token}` } : {};
}
```

**`cliente/src/api/cliente.js`**: cambiar el import a `import { cabecerasAuth, obtenerToken, borrarToken } from './sesion.js';` y, dentro de `if (!respuesta.ok) {`, agregar como primeras líneas:

```js
    // Token vencido o revocado: se descarta y se avisa a la aplicación.
    if (respuesta.status === 401 && obtenerToken()) {
      borrarToken();
      window.dispatchEvent(new Event('ih:sesion-expirada'));
    }
```

**`cliente/src/api/auth.js`**

```js
import { solicitar } from './cliente.js';

export const registrar = (datos) => solicitar('POST', '/auth/register', { cuerpo: datos });
export const iniciarSesion = (credenciales) => solicitar('POST', '/auth/login', { cuerpo: credenciales });
export const cerrarSesion = () => solicitar('POST', '/auth/logout');
```

**`cliente/src/context/SesionContext.jsx`** (reemplazar completo; las pantallas siguen usando `useSesion()` igual)

```jsx
import { createContext, useCallback, useContext, useEffect, useState } from 'react';
import * as apiAuth from '../api/auth.js';
import { obtenerPerfilPropio } from '../api/usuarios.js';
import { borrarToken, guardarToken, obtenerToken } from '../api/sesion.js';
import { useToast } from './ToastContext.jsx';

const SesionContext = createContext(null);

export function SesionProvider({ children }) {
  const mostrarToast = useToast();
  const [usuario, setUsuario] = useState(null);
  const [cargandoSesion, setCargandoSesion] = useState(() => Boolean(obtenerToken()));

  // Al abrir la aplicación con un token guardado, se recupera el perfil.
  useEffect(() => {
    if (!obtenerToken()) return undefined;
    const control = new AbortController();
    obtenerPerfilPropio(control.signal)
      .then(setUsuario)
      .catch((error) => { if (error.name !== 'AbortError') { borrarToken(); setUsuario(null); } })
      .finally(() => { if (!control.signal.aborted) setCargandoSesion(false); });
    return () => control.abort();
  }, []);

  // cliente.js emite este evento cuando el servidor responde 401 a una petición con token.
  useEffect(() => {
    const alExpirar = () => {
      setUsuario(null);
      mostrarToast('Tu sesión terminó. Iniciá sesión de nuevo.', 'warning');
    };
    window.addEventListener('ih:sesion-expirada', alExpirar);
    return () => window.removeEventListener('ih:sesion-expirada', alExpirar);
  }, [mostrarToast]);

  const iniciarSesion = useCallback(async (credenciales) => {
    const respuesta = await apiAuth.iniciarSesion(credenciales);
    guardarToken(respuesta.token);
    setUsuario(respuesta.user);
    return respuesta.user;
  }, []);

  const registrar = useCallback(async (datos) => {
    await apiAuth.registrar(datos);
    return iniciarSesion({ email: datos.email, password: datos.password });
  }, [iniciarSesion]);

  const cerrarSesion = useCallback(async () => {
    try {
      await apiAuth.cerrarSesion(); // el servidor revoca el token
    } finally {
      borrarToken();
      setUsuario(null);
    }
  }, []);

  const recargarUsuario = useCallback(() => obtenerPerfilPropio().then(setUsuario), []);

  return (
    <SesionContext.Provider value={{ usuario, cargandoSesion, iniciarSesion, registrar, cerrarSesion, recargarUsuario }}>
      {children}
    </SesionContext.Provider>
  );
}

export const useSesion = () => useContext(SesionContext);
```

**`cliente/src/components/layout/RutaProtegida.jsx`** (reemplazar completo)

```jsx
import { Navigate, useLocation } from 'react-router-dom';
import { useSesion } from '../../context/SesionContext.jsx';
import EstadoCarga from '../ui/EstadoCarga.jsx';

// Ocultar botones no protege nada: el servidor valida cada petición.
// Este componente solo evita mostrar pantallas que de todos modos responderían 401.
export default function RutaProtegida({ children }) {
  const { usuario, cargandoSesion } = useSesion();
  const location = useLocation();
  if (cargandoSesion) return <EstadoCarga texto="Verificando la sesión…" />;
  if (!usuario) return <Navigate to="/login" replace state={{ desde: location }} />;
  return children;
}
```

**`cliente/src/components/layout/ControlesSesion.jsx`** (reemplazar completo)

```jsx
import { Link, NavLink, useNavigate } from 'react-router-dom';
import { useSesion } from '../../context/SesionContext.jsx';
import { useToast } from '../../context/ToastContext.jsx';

export default function ControlesSesion() {
  const { usuario, cerrarSesion } = useSesion();
  const navigate = useNavigate();
  const mostrarToast = useToast();

  if (!usuario) {
    return (
      <>
        <NavLink className="nav-link" to="/login"><i className="bi bi-box-arrow-in-right" aria-hidden="true"></i> Iniciar sesión</NavLink>
        <Link className="btn btn-primary btn-sm" to="/registro">Crear cuenta</Link>
      </>
    );
  }

  async function salir() {
    await cerrarSesion();
    mostrarToast('Sesión cerrada.', 'secondary');
    navigate('/');
  }

  return (
    <>
      <NavLink className="nav-link" to="/usuarios"><i className="bi bi-people" aria-hidden="true"></i> Personas</NavLink>
      <NavLink className="nav-link" to="/solicitudes"><i className="bi bi-send" aria-hidden="true"></i> Solicitudes</NavLink>
      <NavLink className="nav-link" to="/perfil"><i className="bi bi-person-circle" aria-hidden="true"></i> {usuario.firstName}</NavLink>
      <Link className="btn btn-primary btn-sm" to="/iniciativas/nueva"><i className="bi bi-plus-circle-fill" aria-hidden="true"></i> Publicar</Link>
      <button type="button" className="btn btn-outline-secondary btn-sm" onClick={salir}>
        <i className="bi bi-box-arrow-right" aria-hidden="true"></i> Salir
      </button>
    </>
  );
}
```

### 10.3 Login y registro

**`cliente/src/components/ui/CampoCasilla.jsx`**

```jsx
export default function CampoCasilla({ id, etiqueta, valor, onChange, error, className = 'mb-3' }) {
  return (
    <div className={className}>
      <div className="form-check">
        <input
          id={id} type="checkbox" className={`form-check-input${error ? ' is-invalid' : ''}`} checked={Boolean(valor)}
          onChange={(e) => onChange(e.target.checked)} aria-invalid={Boolean(error)} aria-describedby={error ? `${id}-error` : undefined}
        />
        <label className="form-check-label" htmlFor={id}>{etiqueta}</label>
        {error && <div id={`${id}-error`} className="invalid-feedback d-block"><i className="bi bi-exclamation-triangle-fill" aria-hidden="true"></i> {error}</div>}
      </div>
    </div>
  );
}
```

**`cliente/src/features/auth/LoginPage.jsx`**

```jsx
import { useState } from 'react';
import { Link, Navigate, useLocation, useNavigate } from 'react-router-dom';
import CampoTexto from '../../components/ui/CampoTexto.jsx';
import { useFormulario } from '../../hooks/useFormulario.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useSesion } from '../../context/SesionContext.jsx';
import { REGEX_CORREO, combinar, formato, requerido } from '../../utils/validacion.js';

export default function LoginPage() {
  useTituloPagina('Iniciar sesión');
  const { usuario, iniciarSesion } = useSesion();
  const navigate = useNavigate();
  const location = useLocation();
  const destino = location.state?.desde?.pathname ?? '/iniciativas';
  const [errorGeneral, setErrorGeneral] = useState(null);
  const [procesando, setProcesando] = useState(false);
  const form = useFormulario(
    { email: '', password: '' },
    { email: combinar(requerido, formato(REGEX_CORREO, 'Ingresá un correo válido.')), password: requerido }
  );

  if (usuario) return <Navigate to={destino} replace />;

  async function enviar(e) {
    e.preventDefault();
    setErrorGeneral(null);
    if (!form.validarTodo()) return;
    setProcesando(true);
    try {
      await iniciarSesion({ email: form.valores.email.trim(), password: form.valores.password });
      navigate(destino, { replace: true });
    } catch (error) {
      setErrorGeneral(error.message);
      setProcesando(false);
    }
  }

  return (
    <div className="container py-5" style={{ maxWidth: '28rem' }}>
      <h1 className="h3 mb-4 text-center">Iniciar sesión</h1>
      <form className="formulario-tarjeta" noValidate onSubmit={enviar}>
        {errorGeneral && <div className="alert alert-danger d-flex gap-2" role="alert"><i className="bi bi-x-circle-fill" aria-hidden="true"></i> {errorGeneral}</div>}
        <CampoTexto etiqueta="Correo" requerido tipo="email" autoComplete="email" {...form.campo('email')} />
        <CampoTexto etiqueta="Contraseña" requerido tipo="password" autoComplete="current-password" {...form.campo('password')} />
        <button type="submit" className="btn btn-primary w-100" disabled={procesando}>
          {procesando && <span className="spinner-border spinner-border-sm me-1" aria-hidden="true"></span>} Entrar
        </button>
        <p className="small text-center mt-3 mb-0">¿No tenés cuenta? <Link to="/registro">Creala acá</Link></p>
      </form>
    </div>
  );
}
```

**`cliente/src/features/auth/RegistroPage.jsx`**

```jsx
import { useState } from 'react';
import { Link, Navigate, useNavigate } from 'react-router-dom';
import CampoTexto from '../../components/ui/CampoTexto.jsx';
import CampoSelect from '../../components/ui/CampoSelect.jsx';
import CampoCasilla from '../../components/ui/CampoCasilla.jsx';
import AccionesFormulario from '../../components/ui/AccionesFormulario.jsx';
import { useFormulario } from '../../hooks/useFormulario.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useSesion } from '../../context/SesionContext.jsx';
import { useToast } from '../../context/ToastContext.jsx';
import { opciones, TIPOS_MIEMBRO } from '../../utils/etiquetas.js';
import { REGEX_CORREO, combinar, formato, longitud, opcional, requerido, seleccionValida } from '../../utils/validacion.js';

const nombre = combinar(requerido, longitud(2, 60));

export default function RegistroPage() {
  useTituloPagina('Crear cuenta');
  const { usuario, registrar } = useSesion();
  const navigate = useNavigate();
  const mostrarToast = useToast();
  const [procesando, setProcesando] = useState(false);
  const form = useFormulario(
    { firstName: '', lastName: '', secondLastName: '', email: '', password: '', confirmPassword: '', memberType: '', career: '', acceptTerms: false },
    {
      firstName: nombre,
      lastName: nombre,
      secondLastName: opcional(longitud(2, 60)),
      email: combinar(requerido, formato(REGEX_CORREO, 'Ingresá un correo válido.')),
      password: combinar(requerido, longitud(8, 72), formato(/[A-Za-zÁÉÍÓÚáéíóúÑñ]/, 'Debe incluir al menos una letra.'), formato(/\d/, 'Debe incluir al menos un número.')),
      // Regla entre campos: la confirmación debe coincidir.
      confirmPassword: (v, todos) => (!v ? 'Confirmá la contraseña.' : v !== todos.password ? 'Las contraseñas no coinciden.' : null),
      memberType: seleccionValida(Object.keys(TIPOS_MIEMBRO)),
      career: nombre,
      acceptTerms: (v) => (v ? null : 'Debés aceptar los términos de uso.'),
    }
  );

  // Con sesión abierta no tiene sentido registrarse. Va al mismo destino que tras registrarse,
  // porque el Layout vuelve a montar esta página cuando cambia el usuario.
  if (usuario) return <Navigate to="/perfil/editar" replace />;

  async function enviar(e) {
    e.preventDefault();
    if (!form.validarTodo()) return;
    const v = form.valores;
    setProcesando(true);
    try {
      await registrar({
        firstName: v.firstName.trim(), lastName: v.lastName.trim(), secondLastName: v.secondLastName.trim() || null,
        email: v.email.trim(), password: v.password, confirmPassword: v.confirmPassword,
        memberType: v.memberType, career: v.career.trim(), acceptTerms: v.acceptTerms,
      });
      mostrarToast('Cuenta creada. Completá tu perfil con tus competencias.');
      navigate('/perfil/editar', { replace: true });
    } catch (error) {
      if (error.code === 'EMAIL_TAKEN') form.erroresDelServidor([{ field: 'email', message: error.message }]);
      else if (error.status === 400) form.erroresDelServidor(error.details);
      mostrarToast(error.message, 'danger');
      setProcesando(false);
    }
  }

  return (
    <div className="container py-4" style={{ maxWidth: '44rem' }}>
      <h1 className="h3 mb-1">Crear cuenta</h1>
      <p className="text-body-secondary mb-4">Los campos con <span className="text-danger" aria-hidden="true">*</span><span className="visually-hidden">asterisco</span> son obligatorios.</p>
      <form className="formulario-tarjeta" noValidate onSubmit={enviar}>
        <div className="row g-3">
          <CampoTexto className="col-md-4" etiqueta="Nombre" requerido autoComplete="given-name" {...form.campo('firstName')} />
          <CampoTexto className="col-md-4" etiqueta="Primer apellido" requerido autoComplete="family-name" {...form.campo('lastName')} />
          <CampoTexto className="col-md-4" etiqueta="Segundo apellido" ayuda="Opcional" {...form.campo('secondLastName')} />
          <CampoTexto className="col-12" etiqueta="Correo" requerido tipo="email" autoComplete="email" ayuda="Será tu identificador; no se podrá cambiar." {...form.campo('email')} />
          <CampoTexto className="col-md-6" etiqueta="Contraseña" requerido tipo="password" autoComplete="new-password" min={8} max={72} ayuda="Al menos una letra y un número." {...form.campo('password')} />
          <CampoTexto className="col-md-6" etiqueta="Confirmar contraseña" requerido tipo="password" autoComplete="new-password" {...form.campo('confirmPassword')} />
          <CampoSelect className="col-md-6" etiqueta="Tipo de miembro" requerido vacio="Elegí…" opciones={opciones(TIPOS_MIEMBRO)} {...form.campo('memberType')} />
          <CampoTexto className="col-md-6" etiqueta="Carrera o área profesional" requerido {...form.campo('career')} />
          <CampoCasilla className="col-12" etiqueta="Acepto los términos de uso de Innovation Hub" {...form.campo('acceptTerms')} />
        </div>
        <AccionesFormulario textoGuardar="Crear cuenta" procesando={procesando} onCancelar={() => navigate('/')} />
        <p className="small mt-3 mb-0">¿Ya tenés cuenta? <Link to="/login">Iniciá sesión</Link></p>
      </form>
    </div>
  );
}
```

### 10.4 Editar perfil y gestión de competencias

**`cliente/src/features/usuarios/EditorCompetencias.jsx`**

Registrar, modificar el nivel y eliminar competencias del perfil (RF-A-COM-01 a 04, puntos extra). Quitar pide confirmación.

```jsx
import { useState } from 'react';
import ModalConfirmacion from '../../components/ui/ModalConfirmacion.jsx';
import { NIVELES, opciones } from '../../utils/etiquetas.js';

export default function EditorCompetencias({ id = 'skills', valores, onChange, catalogo, error }) {
  const [nueva, setNueva] = useState({ skillId: '', level: 'basic' });
  const [aviso, setAviso] = useState(null);
  const [porQuitar, setPorQuitar] = useState(null);
  const nombre = (skillId) => catalogo.find((c) => c.id === skillId)?.name ?? skillId;
  const niveles = opciones(NIVELES);

  function agregar() {
    if (!nueva.skillId) return setAviso('Elegí una competencia.');
    if (valores.some((s) => s.skillId === nueva.skillId)) return setAviso('Esa competencia ya está en tu perfil.');
    onChange([...valores, nueva]);
    setNueva({ skillId: '', level: 'basic' });
    setAviso(null);
    return undefined;
  }

  const cambiarNivel = (skillId, level) => onChange(valores.map((s) => (s.skillId === skillId ? { ...s, level } : s)));
  const mensaje = aviso ?? error;

  return (
    <fieldset className="mb-3">
      <legend className="form-label fs-6">Competencias</legend>
      <div className="row g-2 align-items-end">
        <div className="col-sm-6">
          <label className="form-label small" htmlFor={id}>Competencia</label>
          <select id={id} className="form-select" value={nueva.skillId} onChange={(e) => { setNueva({ ...nueva, skillId: e.target.value }); setAviso(null); }}>
            <option value="">Elegí una competencia…</option>
            {catalogo.map((c) => <option key={c.id} value={c.id} disabled={valores.some((s) => s.skillId === c.id)}>{c.name}</option>)}
          </select>
        </div>
        <div className="col-sm-4">
          <label className="form-label small" htmlFor={`${id}-nivel`}>Nivel</label>
          <select id={`${id}-nivel`} className="form-select" value={nueva.level} onChange={(e) => setNueva({ ...nueva, level: e.target.value })}>
            {niveles.map((n) => <option key={n.valor} value={n.valor}>{n.texto}</option>)}
          </select>
        </div>
        <div className="col-sm-2 d-grid">
          <button type="button" className="btn btn-outline-primary" onClick={agregar}><i className="bi bi-plus-lg" aria-hidden="true"></i> Agregar</button>
        </div>
      </div>
      {mensaje && <div className="invalid-feedback d-block"><i className="bi bi-exclamation-triangle-fill" aria-hidden="true"></i> {mensaje}</div>}

      <ul className="list-unstyled mt-3 mb-0">
        {valores.map((s) => (
          <li key={s.skillId} className="fila-lista">
            <span className="fw-semibold">{nombre(s.skillId)}</span>
            <span className="d-flex gap-2 align-items-center">
              <label className="visually-hidden" htmlFor={`nivel-${s.skillId}`}>Nivel de {nombre(s.skillId)}</label>
              <select id={`nivel-${s.skillId}`} className="form-select form-select-sm" value={s.level} onChange={(e) => cambiarNivel(s.skillId, e.target.value)}>
                {niveles.map((n) => <option key={n.valor} value={n.valor}>{n.texto}</option>)}
              </select>
              <button type="button" className="btn btn-sm btn-outline-danger" aria-label={`Quitar ${nombre(s.skillId)}`} onClick={() => setPorQuitar(s.skillId)}>
                <i className="bi bi-trash3" aria-hidden="true"></i>
              </button>
            </span>
          </li>
        ))}
        {valores.length === 0 && <li className="small text-body-secondary">Todavía no registraste competencias.</li>}
      </ul>

      <ModalConfirmacion
        abierto={porQuitar !== null} titulo="Quitar competencia" textoConfirmar="Quitar"
        onConfirmar={() => { onChange(valores.filter((s) => s.skillId !== porQuitar)); setPorQuitar(null); }}
        onCancelar={() => setPorQuitar(null)}
      >
        <p className="mb-0">¿Quitar <strong>{porQuitar && nombre(porQuitar)}</strong> de tu perfil? La competencia sigue existiendo en el catálogo.</p>
      </ModalConfirmacion>
    </fieldset>
  );
}
```

**`cliente/src/features/usuarios/EditarPerfilPage.jsx`**

```jsx
import { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import CampoTexto from '../../components/ui/CampoTexto.jsx';
import SelectorChips from '../../components/ui/SelectorChips.jsx';
import AccionesFormulario from '../../components/ui/AccionesFormulario.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EditorCompetencias from './EditorCompetencias.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useFormulario } from '../../hooks/useFormulario.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { useSesion } from '../../context/SesionContext.jsx';
import { useToast } from '../../context/ToastContext.jsx';
import { actualizarPerfil, obtenerPerfilPropio } from '../../api/usuarios.js';
import { listarCompetencias } from '../../api/catalogos.js';
import { LIMITES, REGEX_URL, cantidad, combinar, formato, longitud, opcional, rango, requerido } from '../../utils/validacion.js';

const nombre = combinar(requerido, longitud(2, 60));

export default function EditarPerfilPage() {
  useTituloPagina('Editar perfil');
  const navigate = useNavigate();
  const mostrarToast = useToast();
  const { recargarUsuario } = useSesion();
  const carga = useConsulta((signal) => Promise.all([obtenerPerfilPropio(signal), listarCompetencias(signal)]), []);
  const [perfil, catalogo] = carga.datos ?? [null, []];
  const [procesando, setProcesando] = useState(false);

  const form = useFormulario(
    { firstName: '', lastName: '', secondLastName: '', career: '', bio: '', skills: [], interests: [], hoursPerWeek: '0', detail: '', portfolioUrl: '' },
    {
      firstName: nombre, lastName: nombre, secondLastName: opcional(longitud(2, 60)), career: nombre,
      bio: longitud(0, LIMITES.bio[1]),
      skills: cantidad(0, 15), interests: cantidad(0, 10),
      hoursPerWeek: rango(0, 40), detail: longitud(0, 100),
      portfolioUrl: opcional(formato(REGEX_URL, 'Ingresá una URL que empiece con http:// o https://.')),
    }
  );

  useEffect(() => {
    if (!perfil) return;
    form.reiniciar({
      firstName: perfil.firstName, lastName: perfil.lastName, secondLastName: perfil.secondLastName ?? '', career: perfil.career,
      bio: perfil.bio, skills: perfil.skills.map(({ skillId, level }) => ({ skillId, level })), interests: perfil.interests,
      hoursPerWeek: String(perfil.availability.hoursPerWeek), detail: perfil.availability.detail, portfolioUrl: perfil.portfolioUrl ?? '',
    });
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [perfil]);

  async function enviar(e) {
    e.preventDefault();
    if (!form.validarTodo()) return;
    const v = form.valores;
    setProcesando(true);
    try {
      await actualizarPerfil({
        firstName: v.firstName.trim(), lastName: v.lastName.trim(), secondLastName: v.secondLastName.trim() || null,
        career: v.career.trim(), bio: v.bio.trim(), skills: v.skills, interests: v.interests,
        availability: { hoursPerWeek: Number(v.hoursPerWeek), detail: v.detail.trim() },
        portfolioUrl: v.portfolioUrl.trim() || null,
      });
      await recargarUsuario();
      mostrarToast('Perfil actualizado.');
      navigate('/perfil');
    } catch (error) {
      if (error.status === 400) form.erroresDelServidor(error.details);
      mostrarToast(error.message, 'danger');
      setProcesando(false);
    }
  }

  return (
    <div className="container py-4" style={{ maxWidth: '48rem' }}>
      <h1 className="h3 mb-4">Editar perfil</h1>
      {carga.cargando && <EstadoCarga texto="Cargando tu perfil…" />}
      {carga.error && <EstadoError error={carga.error} onReintentar={carga.recargar} />}
      {perfil && (
        <form className="formulario-tarjeta" noValidate onSubmit={enviar}>
          <div className="row g-3">
            <CampoTexto className="col-md-4" etiqueta="Nombre" requerido {...form.campo('firstName')} />
            <CampoTexto className="col-md-4" etiqueta="Primer apellido" requerido {...form.campo('lastName')} />
            <CampoTexto className="col-md-4" etiqueta="Segundo apellido" ayuda="Opcional" {...form.campo('secondLastName')} />
            {/* El correo identifica la cuenta: se muestra pero no se edita. */}
            <CampoTexto className="col-md-6" id="email" etiqueta="Correo" valor={perfil.email} onChange={() => {}} readOnly ayuda="No se puede modificar desde el perfil." />
            <CampoTexto className="col-md-6" etiqueta="Carrera o área profesional" requerido {...form.campo('career')} />
            <CampoTexto className="col-12" etiqueta="Descripción personal" multilinea max={LIMITES.bio[1]} {...form.campo('bio')} />
          </div>
          <EditorCompetencias valores={form.valores.skills} catalogo={catalogo} error={form.errores.skills} onChange={(lista) => form.cambiar('skills', lista)} />
          <SelectorChips id="interests" etiqueta="Intereses" libre max={10} placeholder="comunidad" valores={form.valores.interests}
            error={form.errores.interests} onChange={(lista) => form.cambiar('interests', lista)} ayuda="Escribí un interés y presioná Enter"
            validarNuevo={(v) => (v.length < 2 || v.length > 30 ? 'Entre 2 y 30 caracteres.' : null)} />
          <div className="row g-3">
            <CampoTexto className="col-sm-4" etiqueta="Horas por semana" tipo="number" inputMode="numeric" ayuda="0 si por ahora no estás disponible" {...form.campo('hoursPerWeek')} />
            <CampoTexto className="col-sm-8" etiqueta="Detalle de disponibilidad" max={100} placeholder="tardes y fines de semana" {...form.campo('detail')} />
          </div>
          <CampoTexto etiqueta="Portafolio" tipo="url" placeholder="https://github.com/usuario" ayuda="Opcional" {...form.campo('portfolioUrl')} />
          <AccionesFormulario textoGuardar="Guardar perfil" procesando={procesando} onCancelar={() => navigate('/perfil')} />
        </form>
      )}
    </div>
  );
}
```

### 10.5 Búsqueda de personas

**`cliente/src/hooks/useValorDiferido.js`**

```js
import { useEffect, useState } from 'react';

// Devuelve el valor recién cuando dejó de cambiar durante `ms` milisegundos.
export function useValorDiferido(valor, ms = 350) {
  const [diferido, setDiferido] = useState(valor);
  useEffect(() => {
    const temporizador = setTimeout(() => setDiferido(valor), ms);
    return () => clearTimeout(temporizador);
  }, [valor, ms]);
  return diferido;
}
```

**`cliente/src/features/usuarios/BuscarUsuariosPage.jsx`**

```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import Avatar from '../../components/ui/Avatar.jsx';
import Chip from '../../components/ui/Chip.jsx';
import EstadoCarga from '../../components/ui/EstadoCarga.jsx';
import EstadoError from '../../components/ui/EstadoError.jsx';
import EstadoVacio from '../../components/ui/EstadoVacio.jsx';
import Paginacion from '../../components/ui/Paginacion.jsx';
import { useConsulta } from '../../hooks/useConsulta.js';
import { useValorDiferido } from '../../hooks/useValorDiferido.js';
import { useTituloPagina } from '../../hooks/useTituloPagina.js';
import { buscarUsuarios } from '../../api/usuarios.js';
import { listarCompetencias } from '../../api/catalogos.js';

export default function BuscarUsuariosPage() {
  useTituloPagina('Personas');
  const [nombre, setNombre] = useState('');
  const [carrera, setCarrera] = useState('');
  const [competencia, setCompetencia] = useState('');
  const [disponibles, setDisponibles] = useState(false);
  const [pagina, setPagina] = useState(1);
  const q = useValorDiferido(nombre);
  const career = useValorDiferido(carrera);

  const catalogo = useConsulta((signal) => listarCompetencias(signal), []);
  const { cargando, error, datos, recargar } = useConsulta(
    (signal) => buscarUsuarios({ q, career, skill: competencia, available: disponibles ? 'true' : '', page: pagina, size: 12 }, signal),
    [q, career, competencia, disponibles, pagina]
  );
  const conPagina1 = (fijar) => (valor) => { fijar(valor); setPagina(1); };

  return (
    <div className="container py-4">
      <h1 className="h3 mb-1">Personas</h1>
      <p className="text-body-secondary mb-4">Encontrá con quién colaborar según su carrera, competencias y disponibilidad.</p>
      <form className="formulario-tarjeta row g-3 mb-4" role="search" aria-label="Buscar personas" onSubmit={(e) => e.preventDefault()}>
        <div className="col-12 col-md-4">
          <label className="form-label" htmlFor="p-nombre">Nombre</label>
          <input id="p-nombre" type="search" className="form-control" value={nombre} onChange={(e) => conPagina1(setNombre)(e.target.value)} />
        </div>
        <div className="col-12 col-md-3">
          <label className="form-label" htmlFor="p-carrera">Carrera o área</label>
          <input id="p-carrera" className="form-control" value={carrera} onChange={(e) => conPagina1(setCarrera)(e.target.value)} />
        </div>
        <div className="col-12 col-md-3">
          <label className="form-label" htmlFor="p-competencia">Competencia</label>
          <select id="p-competencia" className="form-select" value={competencia} onChange={(e) => conPagina1(setCompetencia)(e.target.value)}>
            <option value="">Todas</option>
            {(catalogo.datos ?? []).map((c) => <option key={c.id} value={c.id}>{c.name}</option>)}
          </select>
        </div>
        <div className="col-12 col-md-2 d-flex align-items-end">
          <div className="form-check">
            <input id="p-disponibles" type="checkbox" className="form-check-input" checked={disponibles} onChange={(e) => conPagina1(setDisponibles)(e.target.checked)} />
            <label className="form-check-label" htmlFor="p-disponibles">Con disponibilidad</label>
          </div>
        </div>
      </form>

      {cargando && <EstadoCarga texto="Buscando personas…" />}
      {error && <EstadoError error={error} onReintentar={recargar} />}
      {datos && !cargando && datos.items.length === 0 && <EstadoVacio titulo="No encontramos personas con esos criterios" />}
      {datos && !cargando && datos.items.length > 0 && (
        <>
          <div className="row row-cols-1 row-cols-md-2 row-cols-xl-3 g-4">
            {datos.items.map((p) => (
              <div className="col" key={p.id}>
                <article className="card h-100">
                  <div className="card-body">
                    <div className="d-flex align-items-center gap-2 mb-2">
                      <Avatar nombre={p.fullName} />
                      <div>
                        <h2 className="h6 mb-0"><Link to={`/usuarios/${p.id}`}>{p.fullName}</Link></h2>
                        <span className="small text-body-secondary">{p.career}</span>
                      </div>
                    </div>
                    <p className="small mb-2">
                      <i className="bi bi-clock" aria-hidden="true"></i> {p.availability.hoursPerWeek > 0 ? `${p.availability.hoursPerWeek} h/semana` : 'Sin disponibilidad'}
                    </p>
                    <ul className="lista-chips">{p.skills.slice(0, 3).map((s) => <li key={s.skillId}><Chip nivel={s.level}>{s.name}</Chip></li>)}</ul>
                  </div>
                </article>
              </div>
            ))}
          </div>
          <Paginacion pagina={datos.page} totalPaginas={datos.totalPages} onCambiar={setPagina} />
        </>
      )}
    </div>
  );
}
```

### 10.6 Ajustes en pantallas existentes

- **`PerfilPage.jsx`**: agregar `import { Link } from 'react-router-dom';` (junto a `useParams`) y reemplazar `<TarjetaPerfil persona={persona} propio={propio} />` por:

```jsx
<TarjetaPerfil persona={persona} propio={propio}>
  {propio && <Link className="btn btn-outline-primary w-100 mt-3" to="/perfil/editar"><i className="bi bi-pencil" aria-hidden="true"></i> Editar perfil</Link>}
</TarjetaPerfil>
```

- **`DetalleCompleto.jsx`**, en `AccionesVisitante`, reemplazar la línea `if (!usuario) return <p …>Iniciá sesión…</p>;` por:

```jsx
  if (!usuario) return <Link className="btn btn-outline-primary" to="/login" state={{ desde: { pathname: `/iniciativas/${ini.id}` } }}>Iniciá sesión para solicitar</Link>;
```

- **`DetalleRestringido.jsx`**: después del bloque `{usuario && (…)}` agregar:

```jsx
        {!usuario && <Link className="btn btn-outline-primary" to="/login" state={{ desde: { pathname: `/iniciativas/${ini.id}` } }}>Iniciá sesión para solicitar participación</Link>}
```

- **`DetallePage.jsx`**: en el caso `error?.status === 401`, cambiar `accion={volver}` por:

```jsx
accion={<Link className="btn btn-primary" to="/login" state={{ desde: { pathname: `/iniciativas/${id}` } }}>Iniciar sesión</Link>}
```

- **`FormularioSolicitudPage.jsx`**: en el `EstadoVacio` de "No tenés competencias…", cambiar `to="/perfil"` por `to="/perfil/editar"` y el texto por "Editar mi perfil".

- **`App.jsx`**: agregar los imports y rutas:

```jsx
import LoginPage from './features/auth/LoginPage.jsx';
import RegistroPage from './features/auth/RegistroPage.jsx';
import EditarPerfilPage from './features/usuarios/EditarPerfilPage.jsx';
import BuscarUsuariosPage from './features/usuarios/BuscarUsuariosPage.jsx';
// ...
        <Route path="login" element={<LoginPage />} />
        <Route path="registro" element={<RegistroPage />} />
        <Route path="perfil/editar" element={protegida(<EditarPerfilPage />)} />
        <Route path="usuarios" element={protegida(<BuscarUsuariosPage />)} />
```

### 10.7 Colección con autenticación

En **`pruebas/generar-coleccion.mjs`** cambiar `const MODO_AUTH = 'dev';` por `const MODO_AUTH = 'jwt';`.

```powershell
node pruebas\generar-coleccion.mjs
cd servidor
npm run test:api
cd ..
```

### 10.8 Verificar

| Prueba | Esperado |
|---|---|
| Búsqueda en el código | `Get-ChildItem -Recurse cliente\src, servidor\src -File \| Select-String "X-Dev-User\|DEV_AUTH\|dev/users"` no encuentra nada |
| Newman | 0 fallos, incluidas las carpetas de autenticación y usuarios |
| Abrir `/solicitudes` sin sesión | Redirige a `/login`; después de entrar vuelve a `/solicitudes` |
| Login con contraseña errónea | Alerta "Correo o contraseña incorrectos." |
| Registro con contraseñas distintas | Error en "Confirmar contraseña"; desaparece al corregir |
| Registro con `ana.mora@ucenfotec.ac.cr` | Error bajo Correo "Ya existe una cuenta con ese correo." |
| Registro correcto | Entra directo a "Editar perfil" |
| Editar perfil | Correo de solo lectura; agregar, cambiar nivel y quitar (con confirmación) competencias |
| Personas → competencia "Gestión" + "Con disponibilidad" | Ana y María |
| Salir | Toast "Sesión cerrada."; en DevTools → Network, el `POST /auth/logout` responde 204 |
| Copiar el token antes de salir y usarlo en Postman después | 401 "La sesión fue cerrada…" |
| Compass → `revokedtokens` | Un documento con `jti` y `expiresAt` |

```powershell
git add -A cliente pruebas
git commit -m "Agregar login, registro, edición de perfil y búsqueda de personas en el cliente" --trailer "Seccion: Cliente" --trailer "Cambio: Sesión con token Bearer, rutas protegidas y gestión de competencias"
git push
```

**Explicar en voz alta:** por qué bcrypt y no SHA-256; qué contiene el JWT y por qué no se puede alterar; cómo el logout invalida un token que todavía no venció; por qué ocultar un botón no reemplaza la verificación en el servidor; qué hace el hash falso en el login.

---

# DÍA 11 · jueves 5 nov · Auditoría, documentación y README

### 11.1 Auditoría de seguridad: `docs/seguridad.md`

````markdown
# Innovation Hub · Medidas de seguridad

| Medida | Dónde |
|---|---|
| Contraseñas con bcrypt (12 rondas, sal por usuario) | `servidor/src/services/auth.service.js` |
| `passwordHash` con `select: false` y serializadores con campos explícitos | `models/User.js`, `utils/serializadores.js` |
| JWT HS256 con vencimiento de 2 h y `jti` | `auth.service.js` |
| Logout con lista de revocación e índice TTL | `models/RevokedToken.js`, `middleware/autenticacion.js` |
| Comparación contra hash falso cuando el correo no existe | `auth.service.js` |
| Límite de 20 logins fallidos por IP cada 15 min | `middleware/limiteLogin.js` |
| CORS solo para `CLIENT_ORIGIN` | `app.js` |
| Encabezados de seguridad con helmet; sin `x-powered-by` | `app.js` |
| Cuerpo JSON limitado a 100 KB | `app.js` |
| Validación zod de params, query y body en cada ruta; campos desconocidos descartados | `validators/`, `middleware/validar.js` |
| Errores con formato único, sin trazas ni mensajes de MongoDB | `middleware/manejadorErrores.js` |
| Secretos en `.env`, fuera del repositorio | `.gitignore`, `.env.example` |

## Autorización por punto de acceso

| Punto de acceso | Verificación en el servidor |
|---|---|
| `GET /initiatives` | Filtro de visibilidad en la consulta; privadas solo para propietario o miembros |
| `GET /initiatives/{id}` | `cargarVisible`: 404 privada ajena, 401 institucional sin sesión, proyección `summary` para restringidas |
| `POST /initiatives` | Sesión; `ownerId` tomado del token, nunca del cuerpo |
| `PUT`, `DELETE`, `archive` | `cargarPropia`: 403 si no es el propietario |
| `GET …/members` | Acceso `full` |
| `DELETE …/members/{userId}` | Propietario; no puede retirarse a sí mismo |
| `POST …/requests` | Sesión, visibilidad, reglas en orden fijo, competencia en la intersección |
| `GET /requests/mine` | Filtra por el usuario del token |
| `GET /requests/received` | Solo iniciativas cuyo `ownerId` es el usuario del token |
| `GET`, `PUT /requests/{id}`, `cancel` | Solicitante (y propietario para leer) |
| `POST /requests/{id}/decision` | Propietario de la iniciativa de la solicitud |
| `POST …/project` | Propietario y RN-17/RN-18 |
| `GET /projects`, `/projects/{id}` | Acceso `full` a la iniciativa de origen |
| `GET /users`, `/users/{id}` | Sesión; solo campos públicos |
| `PUT /users/me` | Siempre el usuario del token; el correo no cambia |

## Limitaciones conocidas

- El token se guarda en `sessionStorage`; un XSS podría leerlo. React escapa el contenido y no se usa `dangerouslySetInnerHTML`.
- MongoDB como servidor único no tiene transacciones; la aceptación de solicitudes usa actualizaciones condicionales y compensación (ver `modelo-datos.md`).
- La búsqueda por texto usa expresiones regulares; con muchos datos convendría un índice de búsqueda dedicado.
````

Recorrer la tabla y comprobar cada fila con la colección o con `curl.exe`. Cualquier fila que falle se corrige hoy.

```powershell
git add docs\seguridad.md
git commit -m "Documentar medidas de seguridad y autorización por punto de acceso" --trailer "Seccion: Documentación" --trailer "Cambio: Auditoría de seguridad del servidor"
```

### 11.2 Barrido de estados de interfaz

Para cada pantalla, provocar los tres estados: **carga** (DevTools → Network → Throttling "Slow 3G"), **error** (detener el servidor) y **vacío** (filtro sin resultados o usuario sin datos).

| Pantalla | Carga | Error | Vacío |
|---|---|---|---|
| Portada (cifras) | ✓ | ✓ | n/a |
| Catálogo | ✓ | ✓ | ✓ |
| Detalle de iniciativa | ✓ | ✓ | 404 |
| Formulario de iniciativa | ✓ | ✓ | n/a |
| Perfil (dos pestañas) | ✓ | ✓ | ✓ |
| Editar perfil | ✓ | ✓ | n/a |
| Personas | ✓ | ✓ | ✓ |
| Solicitar participación | ✓ | ✓ | sin competencias compatibles |
| Mis solicitudes | ✓ | ✓ | ✓ |
| Recibidas | ✓ | ✓ | ✓ |
| Convertir en proyecto | ✓ | ✓ | n/a |
| Proyectos | ✓ | ✓ | ✓ |
| Detalle de proyecto y GitHub | ✓ | ✓ | sin repositorio |

Además: navegar todo con teclado (Tab, Enter, Escape en modales, flechas en pestañas) y revisar cada pantalla a 390 px, 768 px y 1280 px.

### 11.3 Calidad del cliente

```powershell
cd cliente
npm run lint
npm run build
cd ..
```

Corregir todo error de `lint` (las advertencias de `exhaustive-deps` ya marcadas con comentario son intencionales). `build` debe terminar sin errores.

```powershell
git add cliente
git commit -m "Corregir observaciones de lint y verificar el build de producción" --trailer "Seccion: Cliente" --trailer "Cambio: Revisión de estados de interfaz, teclado y tamaños de pantalla"
```

### 11.4 README completo

**`README.md`** (reemplazar completo). Entre las dos marcas va vacío a propósito: al ejecutar `bash herramientas/tabla-commits.sh` el script lo llena con el historial real, incluidos los mensajes antiguos tal como fueron escritos. La nota sobre esos mensajes queda debajo de la marca final, fuera de la zona que el script reescribe.

````markdown
# Innovation Hub

Proyecto del curso SOFT-12 — Programación web avanzada · Universidad CENFOTEC

**Estudiante:** Jose Ricardo Barrantes Sáenz · **Sección:** SCV2 · **Periodo:** III cuatrimestre 2026
**Docente:** Álvaro Cordero Peña

## Descripción

Aplicación web para publicar ideas, necesidades y retos, declarar las competencias que cada iniciativa
requiere, solicitar participación, conformar equipos interdisciplinarios y convertir iniciativas en
proyectos con su repositorio de GitHub.

| Avance | Tecnología | Carpeta |
|---|---|---|
| 1 — Prototipo | HTML, Sass + Bootstrap, JavaScript, JSON | `avance1/` |
| 2 — Cliente-servidor | React (Vite) + Node.js/Express + MongoDB | `cliente/`, `servidor/` |
| Final | React + ASP.NET Core + EF Core | pendiente |

## Arquitectura

```
cliente/ (React + Vite)  ──HTTP/JSON──▶  servidor/ (Express)  ──Mongoose──▶  MongoDB
                         ──HTTPS──────▶  API pública de GitHub
```

El cliente nunca accede a la base de datos. Toda regla de negocio, permiso y validación definitiva
ocurre en el servidor.

## Estructura

```
innovation-hub/
├── docs/            api-contract.md · modelo-datos.md · seguridad.md
├── avance1/         prototipo del Avance 1 (sin cambios)
├── cliente/         src/api · components · context · features · hooks · styles · utils
├── servidor/        src/config · controllers · db · middleware · models · repositories · routes · seed · services · utils · validators
├── pruebas/         colección y entorno de Postman + generador
└── herramientas/    tabla de commits
```

## Requisitos (Windows)

- Node.js 20 o superior.
- MongoDB Community Server 8 instalado como servicio, y mongosh.
- Git.

## Puesta en marcha

### 1. Base de datos

Verificar que el servicio esté activo:

```powershell
Get-Service MongoDB
mongosh "mongodb://127.0.0.1:27017" --eval "db.runCommand({ ping: 1 })"
```

Si está detenido: `Start-Service MongoDB` (PowerShell como administrador).

### 2. Servidor

```powershell
cd servidor
npm install
Copy-Item .env.example .env
```

Editar `servidor/.env`:

| Variable | Valor |
|---|---|
| `PORT` | `3000` |
| `NODE_ENV` | `development` |
| `MONGODB_URI` | `mongodb://127.0.0.1:27017/innovation_hub` |
| `CLIENT_ORIGIN` | `http://localhost:5173` |
| `JWT_SECRET` | cadena aleatoria: `node -e "console.log(require('crypto').randomBytes(48).toString('hex'))"` |
| `JWT_EXPIRES_IN` | `2h` |
| `BCRYPT_ROUNDS` | `12` |

```powershell
npm run seed     # borra y carga los datos de prueba
npm run dev      # http://localhost:3000/api
```

### 3. Cliente

En otra terminal:

```powershell
cd cliente
npm install
Copy-Item .env.example .env   # VITE_API_URL y VITE_GITHUB_API_URL
npm run dev                   # http://localhost:5173
```

### Usuarios de prueba

Contraseña de todos: `Prueba123!` (definida en `servidor/src/seed/seed.js`).

| Usuario | Correo | Para probar |
|---|---|---|
| Ana Mora | ana.mora@ucenfotec.ac.cr | propietaria con equipo; miembro de una privada |
| Luis Pérez | luis.perez@ucenfotec.ac.cr | propietario que recibe solicitudes |
| María Solís | maria.solis@ucenfotec.ac.cr | solicitante; propietaria de una privada |
| Diego Vargas | diego.vargas@ucenfotec.ac.cr | propietario de una restringida |

### 4. Pruebas de la API

Con el servidor en marcha:

```powershell
cd servidor
npm run test:api   # recarga el seed y ejecuta la colección con Newman
```

Para abrirla en Postman: Import → `pruebas/innovation-hub.postman_collection.json` y
`pruebas/innovation-hub.postman_environment.json`. Si se modifica, regenerarla con
`node pruebas/generar-coleccion.mjs`.

### Avance 1

Abrir la raíz del repositorio en VS Code → clic derecho sobre `avance1/index.html` →
**Open with Live Server** → `http://127.0.0.1:5500/avance1/index.html`.
Para recompilar sus estilos: `npm install` y `npm run css` en la raíz.

## Documentación

- [Contrato de la API](docs/api-contract.md) — congelado desde la etiqueta `entrega-2`.
- [Modelo de datos](docs/modelo-datos.md) — embebido frente a referencia y equivalencia en SQL.
- [Seguridad](docs/seguridad.md) — medidas y autorización por punto de acceso.

## Decisiones de diseño del Avance 2

1. **Conversión primero.** El Avance 1 se migró a React contra un servidor simulado con el mismo
   contrato; luego se reemplazó por Express cambiando una variable, sin rehacer pantallas.
2. **Capas en el servidor.** Rutas → controladores → servicios → repositorios. Las reglas de negocio
   son funciones puras en `services/reglas.js`.
3. **Contrato pensado para .NET.** Recursos en inglés, enums en minúscula, ids opacos, errores con
   formato único, acciones de estado como subrecursos `POST`.
4. **Visibilidad en el servidor.** Las restringidas ajenas salen con proyección `summary`; las privadas
   ajenas responden 404.
5. **Equipo embebido, solicitudes referenciadas.** Ver `docs/modelo-datos.md`.
6. **JWT con revocación.** Cerrar sesión invalida el token aunque no haya vencido.
7. **Correcciones del Avance 1.** RN-03 estricta, visibilidad sin valor por defecto, contadores en todos
   los campos con límite, errores que se limpian al corregir, etiquetas y competencias como chips,
   eliminar desde la lista, competencias de la solicitud en intersección y roles de catálogo.

## Resumen de commits

<!-- INICIO TABLA COMMITS -->
<!-- FIN TABLA COMMITS -->

> Los mensajes de los commits anteriores al Avance 2 conservan su redacción original sin tildes:
> corregirlos exigiría reescribir el historial y cambiaría los identificadores ya entregados.
> Desde el Avance 2 los mensajes se escriben con ortografía completa.
````

```powershell
bash herramientas/tabla-commits.sh
git add README.md
git commit -m "Reescribir el README con instrucciones de base de datos, servidor, cliente y pruebas" --trailer "Seccion: Documentación" --trailer "Cambio: README del Avance 2"
git push
```

`bash` está disponible en Git Bash; desde PowerShell funciona si Git está en el PATH. Si no, abrir Git Bash en la carpeta del repositorio y ejecutar el mismo comando.

---

# DÍA 12 · viernes 6 nov · Preparación de la defensa

Hoy no se escribe código nuevo en `main`. Desactivar Copilot, Claude y cualquier autocompletado con IA en VS Code y trabajar como en la defensa.

### 12.1 Recorrido completo, ensayado dos veces en voz alta

**Crear una solicitud:** `FormularioSolicitudPage.enviar` → `useFormulario.validarTodo` → `api/iniciativas.crearSolicitud` → `api/cliente.solicitar` (agrega `Authorization`) → `POST /api/initiatives/:id/requests` → `app.js` (helmet, cors, json) → `identificar` (JWT y revocación) → `solicitudes.routes.js` → `requerirAuth` → `validar(cuerpoSolicitud)` → `solicitudes.controller.crear` → `solicitudes.service.crear` (`cargarVisible`, `verificarSolicitud`, `validarContenido`) → `solicitudes.repository.crear` → MongoDB (índice parcial único) → `serializarSolicitud` → 201 + `Location` → toast y navegación al detalle.

Repetir con: listar el catálogo con filtros, aceptar una solicitud y convertir en proyecto.

### 12.2 Modificaciones en caliente

```powershell
git checkout -b practica-defensa
```

Cada una en menos de 10 minutos, probada con la colección o en la interfaz. Al terminar, `git checkout main` y `git branch -D practica-defensa`.

| # | Pedido probable | Dónde se interviene |
|---|---|---|
| 1 | "El título no puede ser todo mayúsculas" con 400 | `validators/iniciativas.schemas.js`: `.refine((t) => t !== t.toUpperCase(), 'No uses solo mayúsculas.')` en `title`; misma regla en `utils/validacion.js` del cliente |
| 2 | Ordenar iniciativas por cupo disponible | Agregar `estimatedMembers` a `ORDENES` en `iniciativas.repository.js`, la opción en `FiltrosIniciativas` y la fila en el contrato |
| 3 | Las institucionales muestran resumen a visitantes en vez de 401 | `reglas.js`: `if (ini.visibility === 'institutional') return usuario ? 'full' : 'summary';` |
| 4 | Estado vacío en la lista de miembros | `ListaEquipo.jsx`: ya existe; practicar uno nuevo, por ejemplo tecnologías vacías en `ProyectosPage` |
| 5 | Máximo 3 solicitudes pendientes por usuario, 409 | `solicitudes.repository`: `contarPendientesDeUsuario`; `solicitudes.service.crear`: lanzar `ErrorApi.conflicto('TOO_MANY_PENDING', …)` |
| 6 | Filtro de proyectos por tecnología | `proyectos.schemas.js` (`technology`), `proyectos.repository.listarDeIniciativas` (`technologies: q.technology`) |

### 12.3 Preguntas de la rúbrica (criterio 18), con el archivo para mostrar

| Tema | Respuesta corta | Mostrar |
|---|---|---|
| Inmutabilidad | Se crea un objeto o arreglo nuevo (`...`, `map`, `filter`); React compara referencias para saber qué volver a dibujar | `useFormulario.cambiar`, `SelectorChips.quitar` |
| `key` | Siempre el `id`; con índices React reutilizaría el nodo equivocado al quitar un elemento | cualquier `.map` |
| `useEffect` | Dependencias: cuándo repetir; limpieza: cancelar la petición o el temporizador anterior | `useConsulta`, `FiltrosIniciativas` |
| Estado elevado | Los filtros viven en el padre (URL) y bajan por props | `CatalogoPage` |
| Enrutamiento | Rutas anidadas, `useParams`, ruta `*`, `RutaProtegida` | `App.jsx`, `DetallePage` |
| Códigos HTTP | Tabla del contrato, sección 1.3 | `docs/api-contract.md`, `ErrorApi.js` |
| Hashing | bcrypt: lento y con sal; SHA es rápido y facilita la fuerza bruta | `auth.service.js` |
| Autorización | El servidor verifica propiedad y visibilidad aunque el botón no aparezca | `cargarPropia`, `cargarVisible` |
| Embebido vs referencia | Tabla de `modelo-datos.md` | `Initiative.js`, `Request.js` |
| CORS | El navegador bloquea respuestas a otros orígenes; el servidor declara cuál acepta | `app.js` |
| Paginación | `countDocuments` + `skip/limit`, tope 50 | `utils/paginacion.js` |
| Errores de GitHub | 404 inexistente o privado, 403/429 con límite, 5xx o red, respuesta inválida | `api/github.js` |
| Pruebas | Cuatro tipos de caso por recurso, script de colección | `pruebas/generar-coleccion.mjs` |

### 12.4 Decisiones a defender (criterio 19)

- Por qué 404 para privadas y no 403.
- Por qué `DELETE` responde 409 en vez de archivar en silencio.
- Por qué el equipo embebido y las solicitudes en colección aparte, y qué tablas serán en SQL.
- Por qué JWT en encabezado y no cookie, pensando en ASP.NET Core.
- Qué cambia en diciembre: transacciones en lugar de compensación, enums como `varchar` o tablas, `revokedtokens` como tabla.

```powershell
git commit --allow-empty -m "Ensayo de defensa: recorridos y modificaciones en caliente practicadas" --trailer "Seccion: Defensa" --trailer "Cambio: Sin cambios de código"
git push
```

---

# DÍA 13 · sábado 7 nov · Entrega

1. **Clon limpio**, en otra carpeta, siguiendo solo el README:

```powershell
cd $env:USERPROFILE\Desktop
git clone https://github.com/<tu-usuario>/innovation-hub.git prueba-entrega
cd prueba-entrega
```

Ejecutar las secciones 1 a 4 del README tal como están escritas. Si algún paso no funciona, corregir el README en el repositorio original.

2. **Flujo completo en la interfaz:** registro → editar perfil con competencias → catálogo con filtros y páginas → publicar → solicitar como otra persona → aceptar → ver equipo → retirar a alguien → convertir en proyecto con repositorio → ver datos de GitHub → cerrar sesión.

3. **Revisiones finales:**

```powershell
git status                       # sin cambios pendientes
git ls-files | Select-String "\.env$"   # no debe aparecer ningún .env
git log --oneline | Measure-Object       # cantidad de commits
```

4. **Etiqueta y envío:**

```powershell
git tag -a entrega-2 -m "Avance 2: React, Express y MongoDB"
git push origin main --tags
git ls-remote --tags origin      # deben aparecer entrega-1 y entrega-2
```

5. Crear `entrega-2.txt` con la URL del repositorio y subirlo a Moodle antes de las 11:55 p. m.

6. Para la defensa: dejar MongoDB con el seed cargado, el servidor y el cliente listos para arrancar, y la colección abierta en Postman.

---

## Verificación de punta a punta

| # | Comprobación | Cómo |
|---|---|---|
| 1 | Cada observación del Avance 1 corregida | Tabla "Correcciones del Avance 1" al inicio de este plan, fila por fila en la interfaz |
| 2 | Contrato y servidor coinciden | `npm run test:api` con 0 fallos |
| 3 | Ninguna respuesta expone datos sensibles | Prueba de nivel colección en Newman |
| 4 | Restringida ajena solo con resumen | Caso "Restringida vista por un tercero" |
| 5 | Logout invalida el token | Caso "Token revocado tras logout" |
| 6 | Paginación nunca descarga todo | Caso "Tamaño de página limitado a 50" |
| 7 | CORS | Desde la consola de una pestaña en otro sitio, `fetch('http://localhost:3000/api/categories')` falla por CORS |
| 8 | Estados de interfaz | Tabla del día 11 |
| 9 | Sin código temporal | Búsqueda del día 10 sin resultados; `src/mock` ya no existe |
| 10 | Historial | Al menos 30 commits del Avance 2 con mensajes descriptivos; etiquetas `entrega-1` y `entrega-2` en el remoto |
