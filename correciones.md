# Correcciones antes de continuar con el DÍA 5

Resultado de comparar el repo (`innovation-hub-main.zip`) contra `Guia.md` hasta el punto 5.3.
Todo lo demás coincide con la guía; estas son las únicas diferencias que rompen la app.

| # | Archivo | Paso de la guía | Problema |
|---|---|---|---|
| 1 | `cliente/public/mock/request.json` | 1.1 | Falta la "s": el servidor simulado busca `requests.json` y toda la carga falla |
| 2 | `cliente/src/components/layout/Layout.jsx` | 1.6 | Quedó la versión del día 0: sin `ControlesSesion` ni `key` en el `<Outlet>` |
| 3 | `cliente/src/features/solicitudes/SolicitudesLayout.jsx` | 4.3 | Contiene una copia de `FormularioSolicitudPage` |
| 4 | `docs/api-contract.md` y `docs/modelo-datos.md` | 5.1 y 5.2 | No existen todavía |

Terminal: **PowerShell**, desde la raíz del repo.

---

## Paso 1 · Renombrar el archivo de solicitudes (paso 1.1)

```powershell
git mv cliente\public\mock\request.json cliente\public\mock\requests.json
```

## Paso 2 · Reemplazar `Layout.jsx` (paso 1.6)

Abrir `cliente/src/components/layout/Layout.jsx`, borrar todo y pegar:

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

## Paso 3 · Reemplazar `SolicitudesLayout.jsx` (paso 4.3)

Abrir `cliente/src/features/solicitudes/SolicitudesLayout.jsx`, borrar todo y pegar:

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

## Paso 4 · Revisar `cliente/.env`

No se versiona, por eso no viene en el zip. Si no existe:

```powershell
Copy-Item cliente\.env.example cliente\.env
```

Debe tener `VITE_USE_MOCK=true`.

## Paso 5 · Probar el cliente

```powershell
cd cliente
npm run dev
```

En el navegador: DevTools → Application → Local Storage → borrar lo del sitio y recargar
(así se vuelven a cargar los datos simulados).

- [ ] La portada carga y muestra las cifras.
- [ ] En el navbar aparece el selector "Ver como".
- [ ] Con un usuario elegido, `/solicitudes` muestra las pestañas **Enviadas** y **Recibidas**.
- [ ] `/solicitudes/recibidas` lista las solicitudes de las iniciativas propias.

```powershell
cd ..
```

## Paso 6 · Commits (cada uno con sus dos trailers)

No usar `git add -A`, para no versionar `cliente\.env`.

**Commit 1 · nombre de `requests.json`**

```powershell
git add cliente\public\mock\requests.json cliente\public\mock\request.json
git commit -m "Corregir el nombre del archivo de solicitudes simuladas" --trailer "Seccion: Cliente" --trailer "Cambio: request.json renombrado a requests.json para que cargue el servidor simulado"
```

**Commit 2 · `Layout.jsx`**

```powershell
git add cliente\src\components\layout\Layout.jsx
git commit -m "Mostrar los controles de sesión en el layout" --trailer "Seccion: Cliente" --trailer "Cambio: Layout con ControlesSesion y Outlet con key por usuario"
```

**Commit 3 · `SolicitudesLayout.jsx`**

```powershell
git add cliente\src\features\solicitudes\SolicitudesLayout.jsx
git commit -m "Restaurar el layout de solicitudes con pestañas" --trailer "Seccion: Cliente" --trailer "Cambio: SolicitudesLayout con NavLink y Outlet para enviadas y recibidas"
```

**Verificar los trailers y publicar**

```powershell
git log -3 --format="%s%n%(trailers)"
git push
```

Cada commit debe mostrar sus líneas `Seccion:` y `Cambio:`.

## Paso 7 · Completar el DÍA 5

1. **5.1** · Crear `docs\api-contract.md` con el bloque ` ````markdown ` de `Guia.md` (líneas 4351 a 4636)
   y hacer el commit que indica la guía:

   ```powershell
   git add docs\api-contract.md
   git commit -m "Documentar el contrato de la API" --trailer "Seccion: Contrato" --trailer "Cambio: Rutas, verbos, cuerpos, respuestas, errores y visibilidad"
   ```

2. **5.2** · Crear `docs\modelo-datos.md` con el bloque de `Guia.md` (líneas 4645 a 4704):

   ```powershell
   git add docs\modelo-datos.md
   git commit -m "Documentar el modelo de datos con decisiones de embebido y referencia" --trailer "Seccion: Modelo de datos" --trailer "Cambio: Colecciones, índices, atomicidad y equivalencia en SQL"
   ```

3. **5.3** · Instalar MongoDB Community Server 8 (como servicio, con Compass) y mongosh, y verificar:

   ```powershell
   Get-Service MongoDB
   mongosh "mongodb://127.0.0.1:27017" --eval "db.runCommand({ ping: 1 })"
   ```

   Debe mostrar `Running` y `{ ok: 1 }`. Al final:

   ```powershell
   git push
   ```
