# Plan final hasta la entrega del 8 de noviembre

Basado en el zip del 5 de noviembre (`innovation-hub-main.zip`), comparado con `Guia.md`, la consigna
del Avance 2 y la descripción general.

## 0. Dónde estás

Revisé el zip archivo por archivo y lo ejecuté contra un MongoDB de prueba:

| Revisión | Resultado |
|---|---|
| Días 0 a 10 de la guía | Aplicados. Login, registro, logout, perfil, búsqueda de personas y JWT funcionan |
| Newman con JWT (`npm run test:api`) | **78 peticiones, 258 aserciones, 0 fallos** |
| `npm run build` del cliente | Sin errores |
| Flujo en el navegador | Login → solicitar → aceptar → equipo → convertir en proyecto → datos de GitHub: todo funciona |

Lo que falta:

1. **Restos de "Ver como"**: `api/dev.js`, `SelectorSesion.jsx`, `authDev` en `env.js` y `app.js`, `DEV_AUTH` en `.env.example`, el estilo `.ver-como` y el modo `dev` del generador de la colección.
2. **Dos recortes del contrato** (filtros de miembros y el `429` del login), como se acordó.
3. **`SelectorChips` más simple.**
4. **Día 11**: `docs/seguridad.md`, el README del Avance 2, el barrido de estados y el lint.
5. **Días 12 y 13**: ensayo, etiqueta `entrega-2` y entrega en Moodle.

Apliqué los pasos 1 a 5 de este plan en una copia de tu zip y los probé. Con los cambios,
Newman sigue en **78 / 0**, el lint da **0 errores** y el build compila. Además recorrí en el
navegador el flujo completo como Luis y María. Si copias el código tal cual, el resultado será el mismo.

### Lo que decidí NO cambiar (y por qué)

| Propuesta anterior | Decisión | Razón |
|---|---|---|
| `EstadoConsulta` en lugar de `EstadoCarga` / `EstadoError` / `EstadoVacio` | **No** | Habría que tocar 14 pantallas en 3 días. El patrón actual es explícito: en cada pantalla se ven las tres condiciones, y eso es justo lo que pregunta el criterio 3 |
| Quitar `useValorDiferido` | **No** | Son 11 líneas y es tu mejor ejemplo de `useEffect` con limpieza (`clearTimeout`) para el criterio 3 |
| Quitar `CampoCasilla` | **No** | Son 14 líneas, accesibles y reutilizables |
| `populate` en lugar de `contexto.service.js` y unir controladores con rutas | **No** | Cambios grandes con riesgo de romper algo. El servidor ya cumple el nivel Excelente |

Terminal: **PowerShell**, desde la raíz del repo, salvo que se indique otra cosa. Antes de empezar:

```powershell
git pull
git status   # sin cambios pendientes
```

---

## Jueves 5 de noviembre

### Paso 1 · Quitar "Ver como" por completo

**1.1 Borrar los archivos temporales del cliente**

```powershell
Remove-Item cliente\src\api\dev.js, cliente\src\components\layout\SelectorSesion.jsx
```

**1.2 `servidor/src/config/env.js`**: borrar esta línea (es la última dentro de `Object.freeze`):

```js
  authDev: process.env.DEV_AUTH === 'true' && process.env.NODE_ENV !== 'production',
```

**1.3 `servidor/src/app.js`**, línea 19. Cambiar:

```js
    allowedHeaders: ['Content-Type', 'Authorization', ...(config.authDev ? ['X-Dev-User'] : [])],
```

por:

```js
    allowedHeaders: ['Content-Type', 'Authorization'],
```

**1.4 `servidor/.env.example` y tu `servidor/.env`**: borrar la línea `DEV_AUTH=true`.

**1.5 `cliente/src/styles/_navbar.scss`**: borrar el bloque completo `.ver-como { … }` (13 líneas,
desde `.ver-como {` hasta su `}` de cierre, incluido el `.form-select` interno).

**1.6 Verificar** (no debe encontrar nada):

```powershell
Get-ChildItem -Recurse cliente\src, servidor\src -File | Select-String "X-Dev-User|DEV_AUTH|dev/users|SelectorSesion|usuariosDev|authDev|ver-como"
```

**1.7 Commit**

```powershell
git add -A cliente\src servidor\src servidor\.env.example
git commit -m "Eliminar el usuario de desarrollo Ver como del cliente y del servidor" --trailer "Seccion: Seguridad" --trailer "Cambio: Sin X-Dev-User, DEV_AUTH ni selector de usuario; solo autenticación con JWT"
```

### Paso 2 · Generador de la colección solo con JWT

Archivo: `pruebas/generar-coleccion.mjs`.

**2.1** Borrar la línea `const MODO_AUTH = 'jwt';` y la línea en blanco que le sigue.

**2.2** En la función `cabeceras`, reemplazar:

```js
  lista.push(MODO_AUTH === 'jwt'
    ? { key: 'Authorization', value: `Bearer {{token_${quien}}}` }
    : { key: 'X-Dev-User', value: ID[quien] });
```

por:

```js
  lista.push({ key: 'Authorization', value: `Bearer {{token_${quien}}}` });
```

**2.3** Buscar `if (MODO_AUTH === 'jwt') {` (antes de la carpeta `00 Preparación`). Borrar esa línea
y su `}` de cierre, que está justo antes de `carpetas.push(carpeta('03 Catálogos', [`. Las carpetas
`00`, `01` y `02` quedan sin condición. En VS Code: selecciona las líneas de esas tres carpetas y
presiona `Shift+Tab` una vez para quitar la sangría.

**2.4** Regenerar y probar. Terminal 1: `cd servidor` → `npm run dev`. Terminal 2:

```powershell
node pruebas\generar-coleccion.mjs   # "78 peticiones en 8 carpetas"
cd servidor
npm run test:api                     # requests 78 / 0, assertions 258 / 0
cd ..
```

**2.5 Commit**

```powershell
git add pruebas
git commit -m "Generar la colección de pruebas solo con autenticación JWT" --trailer "Seccion: Pruebas" --trailer "Cambio: Se elimina el modo de desarrollo X-Dev-User del generador"
```

### Paso 3 · Contrato: quitar filtros de miembros y el 429

**3.1 `servidor/src/validators/iniciativas.schemas.js`**: borrar al final:

```js
export const consultaMiembros = z.object({
  skill: z.string().max(40).optional(),
  role: z.string().max(40).optional(),
});
```

**3.2 `servidor/src/routes/iniciativas.routes.js`**:
- En el import, quitar `consultaMiembros, ` → `import { consultaIniciativas, cuerpoIniciativa } from '../validators/iniciativas.schemas.js';`
- Cambiar la ruta de miembros por:

```js
router.get('/initiatives/:id/members', c.miembros);
```

**3.3 `servidor/src/controllers/iniciativas.controller.js`**, función `miembros`:

```js
export async function miembros(req, res) {
  res.json(await servicio.listarMiembros(req.params.id, req.usuario));
}
```

**3.4 `servidor/src/services/iniciativas.service.js`**: reemplazar `listarMiembros` completa por:

```js
export async function listarMiembros(id, usuario) {
  const { ini, nivel } = await cargarVisible(id, usuario);
  if (nivel !== 'full') throw ErrorApi.prohibido('No tenés acceso al equipo de esta iniciativa.');
  const ctx = await contextoIniciativas([ini], usuario);
  return ini.members.map((m) => serializarMiembro(m, ctx));
}
```

**3.5 Quitar el límite de login**

```powershell
Remove-Item servidor\src\middleware\limiteLogin.js
cd servidor
npm uninstall express-rate-limit
cd ..
```

En `servidor/src/routes/auth.routes.js`, borrar `import { limiteLogin } from '../middleware/limiteLogin.js';`
y dejar la ruta así:

```js
router.post('/auth/login', validar({ body: cuerpoLogin }), c.login);
```

**3.6 `docs/api-contract.md`**
- Tabla 1.3: borrar la fila `| 429 | TOO_MANY_REQUESTS | … |`.
- Tabla 5.1, `POST /auth/login`: errores `400, 401 INVALID_CREDENTIALS` (sin `, 429`).
- Tabla 5.5, `GET /initiatives/{id}/members`: en la columna de parámetros, `—` en lugar de `` `skill`, `role` ``.

**3.7 Probar**: reiniciar `npm run dev` en la terminal 1 y en la terminal 2 `cd servidor` → `npm run test:api` → `cd ..`. Resultado: 78 / 0.

**3.8 Commit**

```powershell
git add -A servidor docs\api-contract.md
git commit -m "Ajustar el contrato al alcance del Avance 2: sin filtros de miembros ni límite de login" --trailer "Seccion: Contrato" --trailer "Cambio: GET members sin parámetros; se elimina el 429 y express-rate-limit"
```

### Paso 4 · `SelectorChips` más simple (mismo aspecto del Avance 1)

**4.1 `cliente/src/components/ui/SelectorChips.jsx`**: reemplazar completo por:

```jsx
import { useState } from 'react';

// Lista de valores con "Agregar" y chips para quitar. Con opciones es un select; con libre, texto.
export default function SelectorChips({
  id, etiqueta, valores, onChange, opciones = [], libre = false, max, requerido = false, error, ayuda,
  validarNuevo, placeholder, className = 'mb-3',
}) {
  const [entrada, setEntrada] = useState('');
  const [errorEntrada, setErrorEntrada] = useState(null);

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
    onChange([...valores, valor]); // arreglo nuevo: actualización inmutable
    setEntrada('');
    setErrorEntrada(null);
    return undefined;
  }

  function alTeclear(e) {
    if (e.key === 'Enter') {
      e.preventDefault(); // Enter no debe enviar el formulario completo
      agregar();
    }
  }

  const quitar = (valor) => onChange(valores.filter((v) => v !== valor));

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
              onClick={() => quitar(v)}
            ></button>
          </li>
        ))}
        {valores.length === 0 && <li className="small text-body-secondary">Todavía no agregaste ninguno.</li>}
      </ul>
    </div>
  );
}
```

Qué cambió: se quitaron el modal de confirmación al quitar un chip, el atajo con coma y el estado
`porQuitar`. Se mantienen `validarNuevo`, el máximo y los duplicados.

**4.2 `cliente/src/features/iniciativas/FormularioIniciativaPage.jsx`**
- En el `SelectorChips` de competencias, borrar `confirmarAlQuitar` (queda `requerido max={8}`).
- En el de etiquetas, cambiar la ayuda a `"Escribí una etiqueta y presioná Enter o Agregar"`.

**4.3 Probar en el navegador**: en Publicar iniciativa, agrega y quita competencias y etiquetas.
Presionar Enter en etiquetas agrega el chip y **no** envía el formulario. Lo mismo en Tecnologías
(Convertir en proyecto) y en Intereses (Editar perfil).

**4.4 Commit**

```powershell
git add cliente\src
git commit -m "Simplificar el selector de chips conservando el diseño del Avance 1" --trailer "Seccion: Cliente" --trailer "Cambio: SelectorChips sin modal al quitar ni atajo con coma"
```

### Paso 5 · Detalles que conviene corregir antes de la defensa

| Archivo | Cambio | Por qué |
|---|---|---|
| `servidor/src/services/reglas.js`, línea 1 | Reemplazar `// Reglas de negocio puras. Luego se mueven a BD` por `// Reglas de negocio puras: no consultan la base ni dependen de Express.` | El comentario actual es incorrecto y te lo pueden preguntar |
| `cliente/src/App.jsx`, líneas 40 y 41 | Agregar dos espacios de sangría a las rutas `index` y `recibidas` dentro de `solicitudes` | Que se vea que son rutas anidadas |
| `cliente/src/features/iniciativas/DetalleCompleto.jsx`, línea 14 | Quitar dos espacios a `    return (` para que quede `  return (` | Sangría |

```powershell
git add servidor\src\services\reglas.js cliente\src\App.jsx cliente\src\features\iniciativas\DetalleCompleto.jsx
git commit -m "Corregir comentarios y sangría en reglas, rutas y detalle" --trailer "Seccion: Calidad" --trailer "Cambio: Comentario de reglas.js y rutas anidadas legibles"
git push
```

---

## Viernes 6 de noviembre

### Paso 6 · `docs/seguridad.md` (guía 11.1, con dos ajustes)

Crear `docs/seguridad.md` con el bloque de la guía (`Guia.md`, líneas 7706 a 7747), con estos cambios:

1. **Borrar** la fila `| Límite de 20 logins fallidos por IP cada 15 min | middleware/limiteLogin.js |`.
2. En **Limitaciones conocidas**, agregar:

```markdown
- No hay límite de intentos de login. La comparación contra un hash falso evita revelar qué correos
  existen; un límite por IP queda como mejora para la entrega final.
```

3. Si usas Atlas, cambiar la línea de transacciones por:

```markdown
- La aceptación de solicitudes usa actualizaciones condicionales y compensación (ver `modelo-datos.md`).
  Funciona igual con MongoDB local y con Atlas; en SQL será una transacción.
```

```powershell
git add docs\seguridad.md
git commit -m "Documentar medidas de seguridad y autorización por punto de acceso" --trailer "Seccion: Documentación" --trailer "Cambio: Auditoría de seguridad del servidor"
```

### Paso 7 · README del Avance 2 (guía 11.4, con ajustes)

Hoy tu `README.md` todavía es el del Avance 1. Reemplázalo completo con el bloque de la guía
(`Guia.md`, líneas 7800 a 7955), con estos cambios:

1. **Requisitos**: cambiar la línea de MongoDB por:

```markdown
- MongoDB: Community Server 8 instalado como servicio **o** un clúster gratuito de MongoDB Atlas.
```

2. **1. Base de datos**: agregar al final:

````markdown
**Con MongoDB Atlas:** crear un usuario de base de datos, agregar tu IP en *Network Access* y copiar
la cadena de *Connect → Drivers*. En `servidor/.env` va con el nombre de la base antes del `?`:

```
MONGODB_URI=mongodb+srv://<usuario>:<contraseña>@<cluster>.mongodb.net/innovation_hub?retryWrites=true&w=majority
```
````

3. **Estructura**: si no tienes la carpeta `herramientas/` (está en `.gitignore`), borra la línea
`└── herramientas/    tabla de commits` y cambia el `├──` de `pruebas/` por `└──`.

4. **Resumen de commits**: si no tienes `herramientas/tabla-commits.sh`, reemplaza las dos marcas
`<!-- INICIO/FIN TABLA COMMITS -->` por esta línea:

```markdown
El historial completo está en `git log --oneline`; cada commit lleva los trailers `Seccion` y `Cambio`.
```

5. **Decisiones de diseño**: agregar un punto 8:

```markdown
8. **Alcance acotado a la consigna.** Sin límite de intentos de login ni filtros de miembros: no los
   pide el Avance 2 y quedan como mejora para la entrega final.
```

```powershell
git add README.md
git commit -m "Reescribir el README con instrucciones de base de datos, servidor, cliente y pruebas" --trailer "Seccion: Documentación" --trailer "Cambio: README del Avance 2"
git push
```

### Paso 8 · Barrido de estados, teclado y lint (guía 11.2 y 11.3)

1. Recorre la tabla de `Guia.md` 11.2 (líneas 7761 a 7775) provocando en cada pantalla:
   - **Carga**: DevTools → Network → *Slow 3G*.
   - **Error**: detener el servidor (Ctrl+C) y recargar.
   - **Vacío**: un filtro sin resultados.
2. Navega todo con el teclado: Tab, Enter, Escape para cerrar modales y flechas en las pestañas del perfil.
3. Revisa cada pantalla en 390 px, 768 px y 1280 px de ancho.
4. Lint y build:

```powershell
cd cliente
npm run lint    # 0 errores; quedan 3 advertencias conocidas (ver nota)
npm run build   # sin errores
cd ..
```

> **Las 3 advertencias del lint, por si te las preguntan.** Dos son `only-export-components` en
> `SesionContext.jsx` y `ToastContext.jsx`: exportan un componente y un hook, y solo afectan la
> recarga en caliente de Vite. La tercera es `set-state-in-effect` en `FiltrosIniciativas.jsx` línea 15:
> sincroniza el campo de texto cuando la URL cambia desde afuera, por ejemplo al presionar
> "Limpiar filtros". Las tres son intencionales.

```powershell
git add cliente
git commit -m "Corregir observaciones de lint y verificar el build de producción" --trailer "Seccion: Cliente" --trailer "Cambio: Revisión de estados de interfaz, teclado y tamaños de pantalla"
git push
```

### Paso 9 · Si usas Atlas: actualizar el modelo de datos

En `docs/modelo-datos.md`, sección **Atomicidad**, cambiar el primer párrafo por:

```markdown
La aceptación de una solicitud toca dos documentos. En lugar de una transacción, se resuelve con
actualizaciones condicionales y compensación. Así funciona igual con MongoDB local (servidor único,
sin transacciones) y con Atlas:
```

```powershell
git add docs\modelo-datos.md
git commit -m "Aclarar la atomicidad para MongoDB local y Atlas" --trailer "Seccion: Modelo de datos" --trailer "Cambio: La compensación funciona en ambos entornos"
```

---

## Sábado 7 de noviembre · Ensayo de la defensa

### Paso 10 · Ensayo sin IA

Desactiva Copilot, Claude y cualquier autocompletado en VS Code. Trabaja con `Guia-defensa.md`:

1. Explica en voz alta, dos veces, los **cuatro recorridos** de la Parte 4.
2. Practica las **modificaciones en caliente** de la Parte 12 en una rama aparte:

```powershell
git checkout -b practica-defensa
# … practicar …
git checkout main
git branch -D practica-defensa
```

3. Responde en voz alta las preguntas de la Parte 10 mostrando cada archivo.

```powershell
git commit --allow-empty -m "Ensayo de defensa: recorridos y modificaciones en caliente practicadas" --trailer "Seccion: Defensa" --trailer "Cambio: Sin cambios de código"
git push
```

---

## Domingo 8 de noviembre · Entrega

### Paso 11 · Clon limpio siguiendo solo el README

```powershell
cd $env:USERPROFILE\Desktop
git clone https://github.com/jose-ucenfotec/innovation-hub.git prueba-entrega
cd prueba-entrega
```

Ejecuta las secciones 1 a 4 del README tal como están escritas. Si algún paso falla, corrige el README
en el repositorio original, haz commit y push, y vuelve a clonar.

### Paso 12 · Flujo completo en la interfaz

Registro → editar perfil con competencias → catálogo con filtros y páginas → publicar → cerrar sesión →
solicitar como otra persona → aceptar como propietario → ver equipo → retirar a alguien → convertir en
proyecto con repositorio → ver datos de GitHub → cerrar sesión.

### Paso 13 · Revisiones finales

```powershell
git status                                    # sin cambios pendientes
git ls-files | Select-String "\.env$"         # no debe aparecer ningún .env
git log --oneline | Measure-Object            # al menos 30 commits en el bloque del Avance 2
```

### Paso 14 · Etiqueta y envío

```powershell
git tag -a entrega-2 -m "Avance 2: React, Express y MongoDB"
git push origin main --tags
git ls-remote --tags origin                   # deben aparecer entrega-1 y entrega-2
```

Crea `entrega-2.txt` con la URL del repositorio y súbelo a Moodle antes de las 11:55 p. m.

> Desde este momento **el contrato queda congelado**. Las modificaciones de la defensa se hacen en una rama aparte.

### Paso 15 · Dejar todo listo para la defensa

- MongoDB (local o Atlas) con `npm run seed` recién ejecutado.
- `servidor`: `npm run dev` funcionando.
- `cliente`: `npm run dev` funcionando.
- Postman con la colección y el entorno importados.
- VS Code abierto en la raíz, **sin asistentes de IA**.

---

## Resumen de commits de este plan

| # | Mensaje | Seccion |
|---|---|---|
| 1 | Eliminar el usuario de desarrollo Ver como del cliente y del servidor | Seguridad |
| 2 | Generar la colección de pruebas solo con autenticación JWT | Pruebas |
| 3 | Ajustar el contrato al alcance del Avance 2: sin filtros de miembros ni límite de login | Contrato |
| 4 | Simplificar el selector de chips conservando el diseño del Avance 1 | Cliente |
| 5 | Corregir comentarios y sangría en reglas, rutas y detalle | Calidad |
| 6 | Documentar medidas de seguridad y autorización por punto de acceso | Documentación |
| 7 | Reescribir el README con instrucciones de base de datos, servidor, cliente y pruebas | Documentación |
| 8 | Corregir observaciones de lint y verificar el build de producción | Cliente |
| 9 | Aclarar la atomicidad para MongoDB local y Atlas (solo si usas Atlas) | Modelo de datos |
| 10 | Ensayo de defensa: recorridos y modificaciones en caliente practicadas | Defensa |
