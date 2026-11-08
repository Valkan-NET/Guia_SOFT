# Todos los cambios pendientes, en orden (desde el zip del 7 de noviembre)

Parto del último zip que subiste, que ya tiene hechos los pasos 1 y 2 del plan: "Ver como" eliminado y la
colección solo con JWT. Este documento **reemplaza** a los otros como lista de trabajo. `Plan-final-entrega.md`,
`Simplificacion-segun-practicas.md` y `Validacion-sin-zod.md` quedan solo como explicación.

**Ya lo probé:** apliqué los pasos 1 a 4 en una copia de tu zip.
- Newman da **78 peticiones, 258 aserciones y 0 fallos**.
- El lint da **0 errores** y las 3 advertencias conocidas.
- El **build** compila.

| Hoy, sábado 7 | Mañana, domingo 8 |
|---|---|
| Pasos 1 a 8: código, documentación y revisión (unas 3 a 4 horas) | Pasos 10 a 13: clon limpio, etiqueta y Moodle |
| Paso 9: ensayo de la defensa | Paso 14: dejar todo listo |

**No hacer:** el antiguo Paso 4 (`SelectorChips`). La confirmación al quitar una competencia es una
corrección del profesor.

---

## Paso 0 · Antes de empezar

En PowerShell, en la raíz del repo:

```powershell
git status          # debe decir "nothing to commit"; si no, haz commit o descarta antes
git pull
node -v             # tiene que ser v20.6 o más nuevo (para --env-file)
```

- Si `node -v` da menos de 20.6, **salta el Paso 2**. Todo lo demás funciona igual.
- **Cómo probar después de cada paso de código:**
  - Terminal 1, en `servidor`: `npm start`. Espera a ver `API escuchando`.
  - Terminal 2, en `servidor`: `npm run test:api`. Tiene que dar 78 peticiones y 0 fallos.
  - Usa `npm start` y no `npm run dev`, para que un reinicio no tumbe la prueba.
  - Detén la terminal 1 con `Ctrl+C` antes de editar el siguiente paso.
- Si después de un commit `git status` muestra `README.md` modificado, es tu script de la tabla de commits.
  Inclúyelo en el commit siguiente.

---

## Paso 1 · Contrato: miembros sin filtros y login sin límite de intentos

**1.1 `servidor/src/routes/iniciativas.routes.js`**

Línea 5, el import queda así (sin `consultaMiembros`):

```js
import { consultaIniciativas, cuerpoIniciativa } from '../validators/iniciativas.schemas.js';
```

Línea 15, la ruta de miembros queda así:

```js
router.get('/initiatives/:id/members', c.miembros);
```

**1.2 `servidor/src/controllers/iniciativas.controller.js`**, función `miembros`:

```js
export async function miembros(req, res) {
  res.json(await servicio.listarMiembros(req.params.id, req.usuario));
}
```

**1.3 `servidor/src/services/iniciativas.service.js`**: reemplaza la función `listarMiembros` completa por:

```js
export async function listarMiembros(id, usuario) {
  const { ini, nivel } = await cargarVisible(id, usuario);
  if (nivel !== 'full') throw ErrorApi.prohibido('No tenés acceso al equipo de esta iniciativa.');
  const ctx = await contextoIniciativas([ini], usuario);
  return ini.members.map((m) => serializarMiembro(m, ctx));
}
```

**1.4 Quitar el límite de login**

```powershell
Remove-Item servidor\src\middleware\limiteLogin.js
cd servidor
npm uninstall express-rate-limit
cd ..
```

En `servidor/src/routes/auth.routes.js`, borra esta línea:

```js
import { limiteLogin } from '../middleware/limiteLogin.js';
```

y deja la ruta de login así:

```js
router.post('/auth/login', validar({ body: cuerpoLogin }), c.login);
```

**1.5 `docs/api-contract.md`**
- Tabla 1.3 (línea 69 aprox.): borra la fila `| 429 | TOO_MANY_REQUESTS | … |`.
- Fila `POST /auth/login` (línea 197 aprox.): al final, `400, 401 INVALID_CREDENTIALS` (sin `, 429`).
- Fila `GET /initiatives/{id}/members` (línea 232 aprox.): en la columna de parámetros, `—` en lugar
  de `` `skill`, `role` ``.

**1.6 Probar** (78 / 0) **y hacer el commit**

```powershell
git add -A servidor docs\api-contract.md
git commit -m "Ajustar el contrato al alcance del Avance 2: sin filtros de miembros ni límite de login" --trailer "Seccion: Contrato" --trailer "Cambio: GET members sin parámetros; se elimina el 429 y express-rate-limit"
```

---

## Paso 2 · Cargar el `.env` con `--env-file`, como en las prácticas

**2.1 `servidor/src/config/env.js`**: borra la línea 1:

```js
import 'dotenv/config';
```

**2.2 `servidor/package.json`**: deja `scripts` exactamente así:

```json
  "scripts": {
    "dev": "node --env-file=.env --watch src/server.js",
    "start": "node --env-file=.env src/server.js",
    "seed": "node --env-file=.env src/seed/seed.js",
    "test:api": "node --env-file=.env src/seed/seed.js && newman run ../pruebas/innovation-hub.postman_collection.json -e ../pruebas/innovation-hub.postman_environment.json"
  },
```

**2.3 Desinstalar `dotenv`**

```powershell
cd servidor
npm uninstall dotenv
cd ..
```

**2.4 Probar** (78 / 0) **y hacer el commit**

```powershell
git add servidor\src\config\env.js servidor\package.json servidor\package-lock.json
git commit -m "Cargar el .env con --env-file de Node en lugar de dotenv" --trailer "Seccion: Servidor" --trailer "Cambio: Se elimina dotenv; los scripts cargan el .env con la opción nativa de Node, como en las prácticas"
```

---

## Paso 3 · Validación a mano en lugar de zod (como `series.validacion.js` del lab)

Cada validador es una función que revisa campo por campo con `if` y devuelve `{ errores, datos }`:
- **`errores`**: un mensaje por campo, o `{}` si todo está bien, igual que `validarSerie`.
- **`datos`**: los campos copiados uno por uno y limpios.

**No cambian:**
- los nombres de los archivos ni de las funciones, así que las rutas no se tocan;
- los mensajes;
- el contrato;
- el cliente.

**3.1 Reemplaza completos los 6 archivos de `servidor/src/validators/`.** `limites.js` **no cambia**. En cada
archivo: `Ctrl+A` y pega el contenido.

#### `servidor/src/validators/comunes.js`

```js
// validators/comunes.js
// Revisiones que usan todos los validadores. Cada "revisar" devuelve el mensaje de error,
// o null si el valor está bien (igual que en series.validacion.js del lab).

import { LIMITES } from './limites.js';

export const OBLIGATORIO = 'Este campo es obligatorio.';
export const OPCION_INVALIDA = 'Elegí una opción válida.';
export const REGEX_OBJECT_ID = /^[a-f\d]{24}$/i;

export const falta = (valor) => valor === undefined || valor === null;
export const limpiar = (valor) => (typeof valor === 'string' ? valor.trim() : valor);
export const sinRepetidos = (lista) => new Set(lista).size === lista.length;

// Texto obligatorio entre min y max caracteres (sin contar espacios al inicio y al final).
export function revisarLargo(valor, min, max) {
  if (falta(valor)) return OBLIGATORIO;
  if (typeof valor !== 'string') return 'Debe ser texto.';
  const largo = valor.trim().length;
  if (largo < min) return `Debe tener al menos ${min} caracteres.`;
  if (largo > max) return `No debe superar los ${max} caracteres.`;
  return null;
}

// Campos con largo definido en limites.js: title, summary, message, name…
export const revisarTexto = (valor, campo) => revisarLargo(valor, LIMITES[campo][0], LIMITES[campo][1]);

// ── Query string (?page=2&type=idea…): todo llega como texto ──

// page ≥ 1 (1 por defecto); size ≥ 1 (12 por defecto, tope 50). sort se revisa en el repositorio.
export function revisarPaginacion(query, errores) {
  const page = query.page === undefined ? 1 : Number(query.page);
  const size = query.size === undefined ? 12 : Number(query.size);
  if (!Number.isInteger(page)) errores.page = 'Debe ser un número entero.';
  else if (page < 1) errores.page = 'Debe ser 1 o mayor.';
  if (!Number.isInteger(size)) errores.size = 'Debe ser un número entero.';
  else if (size < 1) errores.size = 'Debe ser 1 o mayor.';
  const sort = typeof query.sort === 'string' ? query.sort : undefined;
  return { page, size: Math.min(size, 50), sort };
}

// Filtro de texto opcional (?q=, ?skill=…). Devuelve el texto limpio o undefined.
export function revisarFiltro(query, campo, max, errores) {
  const valor = query[campo];
  if (valor === undefined) return undefined;
  if (typeof valor !== 'string' || valor.trim().length > max) {
    errores[campo] = `Debe ser un texto de hasta ${max} caracteres.`;
    return undefined;
  }
  return valor.trim();
}

// Filtro opcional con valores fijos (?type=idea, ?status=pending…).
export function revisarFiltroOpcion(query, campo, opciones, errores) {
  const valor = query[campo];
  if (valor === undefined) return undefined;
  if (!opciones.includes(valor)) {
    errores[campo] = OPCION_INVALIDA;
    return undefined;
  }
  return valor;
}
```

#### `servidor/src/validators/auth.schemas.js`

```js
// validators/auth.schemas.js
// Registro e inicio de sesión. Cada función devuelve { errores, datos }:
// errores trae un mensaje por campo ({} si todo está bien) y datos solo los campos permitidos, limpios.

import { TIPOS_MIEMBRO } from '../models/enums.js';
import { OBLIGATORIO, OPCION_INVALIDA, falta, limpiar, revisarLargo } from './comunes.js';

const REGEX_CORREO = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

// Nombre, apellidos y carrera: de 2 a 60 caracteres.
export const revisarNombre = (valor) => revisarLargo(valor, 2, 60);

function revisarCorreo(valor) {
  if (falta(valor)) return OBLIGATORIO;
  if (typeof valor !== 'string') return 'Debe ser texto.';
  if (valor.trim().length > 120) return 'No debe superar los 120 caracteres.';
  if (!REGEX_CORREO.test(valor.trim())) return 'Ingresá un correo válido.';
  return null;
}

// El correo se guarda y se busca siempre en minúsculas.
const limpiarCorreo = (valor) => (typeof valor === 'string' ? valor.trim().toLowerCase() : valor);

function revisarClave(clave) {
  if (falta(clave)) return OBLIGATORIO;
  if (typeof clave !== 'string') return 'Debe ser texto.';
  if (clave.length < 8) return 'Debe tener al menos 8 caracteres.';
  if (clave.length > 72) return 'No debe superar los 72 caracteres.';
  if (!/[A-Za-zÁÉÍÓÚáéíóúÑñ]/.test(clave)) return 'Debe incluir al menos una letra.';
  if (!/\d/.test(clave)) return 'Debe incluir al menos un número.';
  return null;
}

export function cuerpoRegistro(body) {
  const errores = {};

  for (const campo of ['firstName', 'lastName', 'career']) {
    const mensaje = revisarNombre(body[campo]);
    if (mensaje) errores[campo] = mensaje;
  }
  // El segundo apellido es opcional: si viene, cumple la misma regla que el primero.
  if (!falta(body.secondLastName)) {
    const mensaje = revisarNombre(body.secondLastName);
    if (mensaje) errores.secondLastName = mensaje;
  }

  const mensajeCorreo = revisarCorreo(body.email);
  if (mensajeCorreo) errores.email = mensajeCorreo;

  const mensajeClave = revisarClave(body.password);
  if (mensajeClave) errores.password = mensajeClave;
  if (typeof body.confirmPassword !== 'string') errores.confirmPassword = OBLIGATORIO;
  else if (!mensajeClave && body.confirmPassword !== body.password) errores.confirmPassword = 'Las contraseñas no coinciden.';

  if (!TIPOS_MIEMBRO.includes(body.memberType)) errores.memberType = OPCION_INVALIDA;
  if (body.acceptTerms !== true) errores.acceptTerms = 'Debés aceptar los términos de uso.';

  // confirmPassword y acceptTerms solo se revisan: no se guardan.
  const datos = {
    firstName: limpiar(body.firstName),
    lastName: limpiar(body.lastName),
    secondLastName: limpiar(body.secondLastName) ?? null,
    email: limpiarCorreo(body.email),
    password: body.password,
    memberType: body.memberType,
    career: limpiar(body.career),
  };
  return { errores, datos };
}

export function cuerpoLogin(body) {
  const errores = {};
  const mensajeCorreo = revisarCorreo(body.email);
  if (mensajeCorreo) errores.email = mensajeCorreo;
  if (typeof body.password !== 'string' || body.password === '') errores.password = OBLIGATORIO;

  return { errores, datos: { email: limpiarCorreo(body.email), password: body.password } };
}
```

#### `servidor/src/validators/iniciativas.schemas.js`

```js
// validators/iniciativas.schemas.js
// Cuerpo de POST y PUT /initiatives, y filtros de GET /initiatives.
// Cada función devuelve { errores, datos }.

import { TIPOS, VISIBILIDADES, ESTADOS_INICIATIVA } from '../models/enums.js';
import { REGEX_ETIQUETA } from './limites.js';
import {
  OBLIGATORIO, OPCION_INVALIDA, REGEX_OBJECT_ID, falta, limpiar, sinRepetidos,
  revisarTexto, revisarPaginacion, revisarFiltro, revisarFiltroOpcion,
} from './comunes.js';

const TEXTOS = ['title', 'summary', 'description', 'problem', 'beneficiaries'];

export function cuerpoIniciativa(body) {
  const errores = {};

  for (const campo of TEXTOS) {
    const mensaje = revisarTexto(body[campo], campo);
    if (mensaje) errores[campo] = mensaje;
  }

  if (!TIPOS.includes(body.type)) errores.type = OPCION_INVALIDA;
  if (!VISIBILIDADES.includes(body.visibility)) errores.visibility = OPCION_INVALIDA;
  if (typeof body.categoryId !== 'string' || body.categoryId === '') errores.categoryId = 'Elegí una categoría.';

  const competencias = body.requiredSkillIds;
  if (!Array.isArray(competencias) || competencias.length === 0) errores.requiredSkillIds = 'Agregá al menos 1.';
  else if (competencias.length > 8) errores.requiredSkillIds = 'Podés agregar como máximo 8.';
  else if (!competencias.every((c) => typeof c === 'string')) errores.requiredSkillIds = OPCION_INVALIDA;
  else if (!sinRepetidos(competencias)) errores.requiredSkillIds = 'No repitas competencias.';

  const integrantes = body.estimatedMembers;
  if (falta(integrantes)) errores.estimatedMembers = OBLIGATORIO;
  else if (!Number.isInteger(integrantes)) errores.estimatedMembers = 'Debe ser un número entero.';
  else if (integrantes < 1 || integrantes > 20) errores.estimatedMembers = 'Debe estar entre 1 y 20.';

  // Las etiquetas son opcionales: si no vienen, la lista queda vacía.
  const etiquetas = body.tags ?? [];
  if (!Array.isArray(etiquetas)) errores.tags = 'Debe ser una lista.';
  else if (etiquetas.length > 5) errores.tags = 'Podés agregar como máximo 5.';
  else if (!etiquetas.every((e) => typeof e === 'string' && REGEX_ETIQUETA.test(e.trim()))) {
    errores.tags = 'Usá letras, números, espacios o guiones (2 a 30 caracteres).';
  } else if (!sinRepetidos(etiquetas.map((e) => e.trim()))) errores.tags = 'No repitas etiquetas.';

  // Se copia campo por campo: ownerId, status o members del cliente nunca llegan al servicio.
  const datos = {
    title: limpiar(body.title),
    type: body.type,
    summary: limpiar(body.summary),
    description: limpiar(body.description),
    problem: limpiar(body.problem),
    beneficiaries: limpiar(body.beneficiaries),
    categoryId: body.categoryId,
    requiredSkillIds: competencias,
    estimatedMembers: integrantes,
    visibility: body.visibility,
    tags: Array.isArray(etiquetas) ? etiquetas.map(limpiar) : [],
  };
  return { errores, datos };
}

export function consultaIniciativas(query) {
  const errores = {};
  const datos = {
    ...revisarPaginacion(query, errores),
    q: revisarFiltro(query, 'q', 100, errores),
    type: revisarFiltroOpcion(query, 'type', TIPOS, errores),
    category: revisarFiltro(query, 'category', 40, errores),
    skill: revisarFiltro(query, 'skill', 40, errores),
    status: revisarFiltroOpcion(query, 'status', ESTADOS_INICIATIVA, errores),
    hasSpots: revisarFiltroOpcion(query, 'hasSpots', ['true', 'false'], errores),
    member: revisarFiltroOpcion(query, 'member', ['me'], errores),
  };
  // owner acepta "me" o el id de una persona.
  if (query.owner !== undefined) {
    if (query.owner === 'me' || (typeof query.owner === 'string' && REGEX_OBJECT_ID.test(query.owner))) datos.owner = query.owner;
    else errores.owner = 'Identificador inválido.';
  }
  return { errores, datos };
}
```

#### `servidor/src/validators/usuarios.schemas.js`

```js
// validators/usuarios.schemas.js
// Edición del perfil propio (PUT /users/me) y búsqueda de personas (GET /users).
// Cada función devuelve { errores, datos }.

import { NIVELES } from '../models/enums.js';
import { limpiar, sinRepetidos, revisarPaginacion, revisarFiltro, revisarFiltroOpcion } from './comunes.js';
import { revisarNombre } from './auth.schemas.js';

const REGEX_URL = /^https?:\/\/\S+$/;

export function cuerpoPerfil(body) {
  const errores = {};
  const datos = {};

  // Obligatorios
  for (const campo of ['firstName', 'lastName', 'career']) {
    const mensaje = revisarNombre(body[campo]);
    if (mensaje) errores[campo] = mensaje;
    datos[campo] = limpiar(body[campo]);
  }

  // Opcionales: solo se revisan y se guardan si vienen en el cuerpo.
  if (body.secondLastName !== undefined) {
    if (body.secondLastName !== null) {
      const mensaje = revisarNombre(body.secondLastName);
      if (mensaje) errores.secondLastName = mensaje;
    }
    datos.secondLastName = limpiar(body.secondLastName);
  }

  if (body.bio !== undefined) {
    if (typeof body.bio !== 'string') errores.bio = 'Debe ser texto.';
    else if (body.bio.trim().length > 500) errores.bio = 'No debe superar los 500 caracteres.';
    datos.bio = limpiar(body.bio);
  }

  if (body.skills !== undefined) {
    const lista = body.skills;
    if (!Array.isArray(lista)) errores.skills = 'Debe ser una lista.';
    else if (lista.length > 15) errores.skills = 'Podés registrar como máximo 15.';
    else if (!lista.every((s) => typeof s?.skillId === 'string' && s.skillId !== '' && NIVELES.includes(s.level))) {
      errores.skills = 'Cada competencia necesita un nivel válido.';
    } else if (!sinRepetidos(lista.map((s) => s.skillId))) errores.skills = 'No repitas competencias.';
    else datos.skills = lista.map((s) => ({ skillId: s.skillId, level: s.level }));
  }

  if (body.interests !== undefined) {
    const lista = body.interests;
    if (!Array.isArray(lista)) errores.interests = 'Debe ser una lista.';
    else if (lista.length > 10) errores.interests = 'Podés agregar como máximo 10.';
    else if (!lista.every((i) => typeof i === 'string' && i.trim().length >= 2)) errores.interests = 'Cada interés necesita al menos 2 caracteres.';
    else if (!lista.every((i) => i.trim().length <= 30)) errores.interests = 'Cada interés admite hasta 30 caracteres.';
    else datos.interests = lista.map(limpiar);
  }

  if (body.availability !== undefined) {
    const horas = body.availability?.hoursPerWeek;
    const detalle = body.availability?.detail ?? '';
    if (!Number.isInteger(horas)) errores['availability.hoursPerWeek'] = 'Debe ser un número entero.';
    else if (horas < 0 || horas > 40) errores['availability.hoursPerWeek'] = 'Entre 0 y 40.';
    if (typeof detalle !== 'string' || detalle.trim().length > 100) errores['availability.detail'] = 'No debe superar los 100 caracteres.';
    datos.availability = { hoursPerWeek: horas, detail: limpiar(detalle) };
  }

  if (body.portfolioUrl !== undefined) {
    const url = limpiar(body.portfolioUrl);
    if (url !== null && (typeof url !== 'string' || url.length > 200 || !REGEX_URL.test(url))) {
      errores.portfolioUrl = 'Ingresá una URL que empiece con http:// o https://.';
    }
    datos.portfolioUrl = url;
  }

  // El correo se acepta solo para que el servicio lo rechace con un mensaje claro si cambia.
  if (typeof body.email === 'string') datos.email = body.email;

  return { errores, datos };
}

export function consultaUsuarios(query) {
  const errores = {};
  const datos = {
    ...revisarPaginacion(query, errores),
    q: revisarFiltro(query, 'q', 60, errores),
    skill: revisarFiltro(query, 'skill', 40, errores),
    career: revisarFiltro(query, 'career', 60, errores),
    available: revisarFiltroOpcion(query, 'available', ['true', 'false'], errores),
  };
  return { errores, datos };
}
```

#### `servidor/src/validators/solicitudes.schemas.js`

```js
// validators/solicitudes.schemas.js
// Cuerpo de una solicitud, la decisión del propietario y los filtros de los dos listados.
// Cada función devuelve { errores, datos }.

import { ESTADOS_SOLICITUD } from '../models/enums.js';
import { REGEX_DISPONIBILIDAD, REGEX_FECHA } from './limites.js';
import { OBLIGATORIO, REGEX_OBJECT_ID, falta, limpiar, revisarTexto, revisarPaginacion, revisarFiltro, revisarFiltroOpcion } from './comunes.js';

export function cuerpoSolicitud(body) {
  const errores = {};

  const mensaje = revisarTexto(body.message, 'message');
  if (mensaje) errores.message = mensaje;
  if (typeof body.offeredSkillId !== 'string' || body.offeredSkillId === '') errores.offeredSkillId = 'Elegí una competencia.';
  if (typeof body.roleId !== 'string' || body.roleId === '') errores.roleId = 'Elegí un rol.';

  if (falta(body.availability)) errores.availability = OBLIGATORIO;
  else if (typeof body.availability !== 'string' || !REGEX_DISPONIBILIDAD.test(body.availability.trim())) {
    errores.availability = 'Usá el formato "N h/semana", por ejemplo 4 h/semana.';
  }

  const datos = {
    message: limpiar(body.message),
    offeredSkillId: body.offeredSkillId,
    roleId: body.roleId,
    availability: limpiar(body.availability),
  };
  return { errores, datos };
}

export function cuerpoDecision(body) {
  const errores = {};
  if (!['accepted', 'rejected'].includes(body.decision)) errores.decision = 'La decisión debe ser accepted o rejected.';

  const datos = { decision: body.decision };
  if (body.comment !== undefined) {
    if (typeof body.comment !== 'string' || body.comment.trim().length > 300) errores.comment = 'No debe superar los 300 caracteres.';
    datos.comment = limpiar(body.comment);
  }
  return { errores, datos };
}

export function consultaMias(query) {
  const errores = {};
  const datos = {
    ...revisarPaginacion(query, errores),
    status: revisarFiltroOpcion(query, 'status', ESTADOS_SOLICITUD, errores),
  };
  return { errores, datos };
}

// Fecha opcional en formato AAAA-MM-DD.
function revisarFecha(query, campo, errores) {
  const valor = query[campo];
  if (valor === undefined) return undefined;
  if (typeof valor !== 'string' || !REGEX_FECHA.test(valor)) {
    errores[campo] = 'Usá el formato AAAA-MM-DD.';
    return undefined;
  }
  return valor;
}

export function consultaRecibidas(query) {
  const errores = {};
  const datos = {
    ...revisarPaginacion(query, errores),
    status: revisarFiltroOpcion(query, 'status', ESTADOS_SOLICITUD, errores),
    skill: revisarFiltro(query, 'skill', 40, errores),
    role: revisarFiltro(query, 'role', 40, errores),
    from: revisarFecha(query, 'from', errores),
    to: revisarFecha(query, 'to', errores),
  };
  if (query.initiativeId !== undefined) {
    if (typeof query.initiativeId === 'string' && REGEX_OBJECT_ID.test(query.initiativeId)) datos.initiativeId = query.initiativeId;
    else errores.initiativeId = 'Identificador inválido.';
  }
  // Regla entre campos: solo si las dos fechas son válidas.
  if (datos.from && datos.to && datos.from > datos.to) errores.from = 'La fecha inicial no puede ser posterior a la final.';
  return { errores, datos };
}
```

#### `servidor/src/validators/proyectos.schemas.js`

```js
// validators/proyectos.schemas.js
// Cuerpo para convertir una iniciativa en proyecto y filtros de GET /projects.
// Cada función devuelve { errores, datos }.

import { ESTADOS_PROYECTO } from '../models/enums.js';
import { REGEX_FECHA, REGEX_GITHUB } from './limites.js';
import { OBLIGATORIO, falta, limpiar, sinRepetidos, revisarTexto, revisarPaginacion, revisarFiltroOpcion } from './comunes.js';

export function cuerpoProyecto(body) {
  const errores = {};

  for (const campo of ['name', 'objective', 'description']) {
    const mensaje = revisarTexto(body[campo], campo);
    if (mensaje) errores[campo] = mensaje;
  }

  // Tecnologías: opcionales, hasta 10, sin repetir.
  const tecnologias = body.technologies ?? [];
  if (!Array.isArray(tecnologias)) errores.technologies = 'Debe ser una lista.';
  else if (tecnologias.length > 10) errores.technologies = 'Podés agregar como máximo 10.';
  else if (!tecnologias.every((t) => typeof t === 'string' && t.trim().length >= 1 && t.trim().length <= 40)) {
    errores.technologies = 'Cada tecnología admite hasta 40 caracteres.';
  } else if (!sinRepetidos(tecnologias.map((t) => t.trim()))) errores.technologies = 'No repitas tecnologías.';

  if (falta(body.startDate)) errores.startDate = OBLIGATORIO;
  else if (typeof body.startDate !== 'string' || !REGEX_FECHA.test(body.startDate)) errores.startDate = 'Usá el formato AAAA-MM-DD.';

  const fin = body.estimatedEndDate ?? null;
  if (fin !== null && (typeof fin !== 'string' || !REGEX_FECHA.test(fin))) errores.estimatedEndDate = 'Usá el formato AAAA-MM-DD.';
  // Regla entre campos: solo si las dos fechas son válidas.
  else if (fin !== null && !errores.startDate && fin < body.startDate) errores.estimatedEndDate = 'Debe ser igual o posterior a la fecha de inicio.';

  const repositorio = limpiar(body.repositoryUrl) ?? null;
  if (repositorio !== null && (typeof repositorio !== 'string' || !REGEX_GITHUB.test(repositorio))) {
    errores.repositoryUrl = 'Usá el formato https://github.com/usuario/repositorio.';
  }

  const datos = {
    name: limpiar(body.name),
    objective: limpiar(body.objective),
    description: limpiar(body.description),
    technologies: Array.isArray(tecnologias) ? tecnologias.map(limpiar) : [],
    startDate: body.startDate,
    estimatedEndDate: fin,
    repositoryUrl: repositorio,
  };
  return { errores, datos };
}

export function consultaProyectos(query) {
  const errores = {};
  const datos = {
    ...revisarPaginacion(query, errores),
    status: revisarFiltroOpcion(query, 'status', ESTADOS_PROYECTO, errores),
  };
  return { errores, datos };
}
```

**3.2 Reemplaza completo `servidor/src/middleware/validar.js`:**

```js
// middleware/validar.js
// Corre la función de validación de cada parte de la petición (query o body).
// Si alguna devolvió errores, responde 400 con un mensaje por campo; si no, deja los datos limpios
// en req.validado para el controlador.

import { ErrorApi } from '../utils/ErrorApi.js';

export const validar = (validadores) => (req, _res, next) => {
  req.validado = {};
  const errores = {};

  for (const parte of ['query', 'body']) {
    if (!validadores[parte]) continue;
    const resultado = validadores[parte](req[parte] ?? {});
    req.validado[parte] = resultado.datos;
    Object.assign(errores, resultado.errores);
  }

  if (Object.keys(errores).length > 0) {
    // De { title: 'Debe tener…' } al formato del contrato: [{ field: 'title', message: 'Debe tener…' }]
    const details = Object.entries(errores).map(([field, message]) => ({ field, message }));
    throw ErrorApi.validacion(details);
  }
  next();
};
```

**3.3 `servidor/src/middleware/manejadorErrores.js`**

- Borra la línea 2: `import { ZodError } from 'zod';`
- En `manejadorErrores`, borra la primera rama. **Antes:**

  ```js
    if (err instanceof ZodError) {
      error = ErrorApi.validacion(err.issues.map((i) => ({ field: i.path.join('.'), message: i.message })));
    } else if (err?.type === 'entity.parse.failed') {
  ```

  **Después:**

  ```js
    if (err?.type === 'entity.parse.failed') {
  ```

**3.4 Desinstalar zod y comprobar**

```powershell
cd servidor
npm uninstall zod
cd ..
git grep -n "zod" -- servidor/src servidor/package.json     # no debe mostrar nada
```

**3.5 Probar.**
- Corre la prueba de siempre: tiene que dar 78 / 0.
- En el navegador (con el cliente en `npm run dev`):
  - editar el perfil y guardar;
  - publicar una iniciativa válida;
  - en "Convertir en proyecto", poner la fecha de fin antes que la de inicio. Aparece el error bajo el campo.

**3.6 Commit**

```powershell
git add servidor\src\validators servidor\src\middleware\validar.js servidor\src\middleware\manejadorErrores.js servidor\package.json servidor\package-lock.json
git commit -m "Validar la entrada a mano en lugar de usar zod" --trailer "Seccion: Servidor" --trailer "Cambio: Validadores con funciones que devuelven errores por campo y datos copiados campo por campo, como en las prácticas"
```

---

## Paso 4 · Detalles de estilo

| Archivo | Cambio |
|---|---|
| `servidor/src/services/reglas.js`, línea 1 | Reemplaza `// Reglas de negocio puras. Luego se mueven a BD` por `// Reglas de negocio puras: no consultan la base ni dependen de Express.` |
| `cliente/src/App.jsx`, líneas 40 y 41 | Agrega 2 espacios al inicio de las dos rutas (`index` y `recibidas`) que están dentro de `solicitudes` |
| `cliente/src/features/iniciativas/DetalleCompleto.jsx`, línea 14 | Quita 2 espacios: `    return (` queda `  return (` |

```powershell
git add servidor\src\services\reglas.js cliente\src\App.jsx cliente\src\features\iniciativas\DetalleCompleto.jsx
git commit -m "Corregir comentarios y sangría en reglas, rutas y detalle" --trailer "Seccion: Calidad" --trailer "Cambio: Comentario de reglas.js y rutas anidadas legibles"
git push
```

---

## Paso 5 · Crear `docs/seguridad.md`

Tu README ya enlaza este archivo, pero todavía no existe. Créalo con este contenido completo:

```markdown
# Innovation Hub · Medidas de seguridad

| Medida | Dónde |
|---|---|
| Contraseñas con bcrypt (12 rondas, sal por usuario) | `servidor/src/services/auth.service.js` |
| `passwordHash` con `select: false` y serializadores con campos explícitos | `models/User.js`, `utils/serializadores.js` |
| JWT HS256 con vencimiento de 2 h y `jti` | `auth.service.js` |
| Logout con lista de revocación e índice TTL | `models/RevokedToken.js`, `middleware/autenticacion.js` |
| Comparación contra hash falso cuando el correo no existe | `auth.service.js` |
| CORS solo para `CLIENT_ORIGIN` | `app.js` |
| Encabezados de seguridad con helmet; sin `x-powered-by` | `app.js` |
| Cuerpo JSON limitado a 100 KB | `app.js` |
| Validación a mano de query y body en cada ruta que recibe datos; los datos se copian campo por campo y lo desconocido se descarta | `validators/`, `middleware/validar.js` |
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
- La aceptación de solicitudes usa actualizaciones condicionales y compensación (ver `modelo-datos.md`). Funciona igual con MongoDB local y con Atlas; en SQL será una transacción.
- No hay límite de intentos de login. La comparación contra un hash falso evita revelar qué correos existen; un límite por IP queda como mejora para la entrega final.
- La búsqueda por texto usa expresiones regulares; con muchos datos convendría un índice de búsqueda dedicado.
```

```powershell
git add docs\seguridad.md
git commit -m "Documentar medidas de seguridad y autorización por punto de acceso" --trailer "Seccion: Documentación" --trailer "Cambio: Auditoría de seguridad del servidor"
```

---

## Paso 6 · `docs/modelo-datos.md`: atomicidad, porque usas Atlas

En la sección **Atomicidad**, reemplaza el primer párrafo, el que empieza con "MongoDB instalado como
servicio en Windows…", por:

```markdown
La aceptación de una solicitud toca dos documentos. En lugar de una transacción, se resuelve con
actualizaciones condicionales y compensación. Así funciona igual con MongoDB local (servidor único,
sin transacciones) y con Atlas:
```

Los pasos 1, 2 y 3 de esa sección, y la línea "En SQL este flujo será una transacción.", quedan igual.

```powershell
git add docs\modelo-datos.md
git commit -m "Aclarar la atomicidad para MongoDB local y Atlas" --trailer "Seccion: Modelo de datos" --trailer "Cambio: La compensación funciona en ambos entornos"
```

---

## Paso 7 · Ajustes al `README.md`

Tu README ya es el del Avance 2. Solo hay que hacerle estos cinco cambios:

**7.1 Estructura**: la carpeta `herramientas/` está en `.gitignore`, así que quien clone el repo no la va a
tener. Borra la línea `└── herramientas/    tabla de commits` y cambia la de `pruebas/` por:

```
└── pruebas/         colección y entorno de Postman + generador
```

**7.2 Requisitos (Windows)**: deja la lista así:

```markdown
- Node.js 20.6 o superior (los scripts cargan el `.env` con `--env-file`).
- MongoDB: Community Server 8 instalado como servicio (con mongosh) **o** un clúster gratuito de MongoDB Atlas.
- Git.
```

**7.3 1. Base de datos**: al final de la sección (después de "Si está detenido…"), agrega:

````markdown
**Con MongoDB Atlas:** crear un usuario de base de datos, agregar tu IP en *Network Access* y copiar
la cadena de *Connect → Drivers*. En `servidor/.env` va con el nombre de la base antes del `?`:

```
MONGODB_URI=mongodb+srv://<usuario>:<contraseña>@<cluster>.mongodb.net/innovation_hub?retryWrites=true&w=majority
```
````

**7.4 Tabla de variables**: en la fila `MONGODB_URI`, pon como valor:

```markdown
| `MONGODB_URI` | `mongodb://127.0.0.1:27017/innovation_hub` o la cadena de Atlas |
```

**7.5 Decisiones de diseño del Avance 2**: agrega al final de la lista:

```markdown
8. **Patrones de las prácticas de clase.** La validación del servidor está escrita a mano: cada función
   devuelve un mensaje por campo y copia solo los campos permitidos. El `.env` se carga con `--env-file`.
   Las dependencias del servidor son las de clase más las que exige el Avance 2: Mongoose, bcrypt,
   JWT, helmet y morgan.
9. **Alcance acotado a la consigna.** Sin límite de intentos de login ni filtros de miembros: no los
   pide el Avance 2 y quedan como mejora para la entrega final.
```

```powershell
git add README.md
git commit -m "Actualizar el README con Atlas, Node 20.6 y las decisiones finales" --trailer "Seccion: Documentación" --trailer "Cambio: README del Avance 2 completo"
git push
```

---

## Paso 8 · Revisión de pantallas, lint y build

1. Recorre cada pantalla provocando sus tres estados. La tabla de pantallas es la de `Guia.md` 11.2, líneas 7761 a 7775:
   - **Carga**: DevTools → Network → *Slow 3G*.
   - **Error**: detén el servidor y recarga.
   - **Vacío**: usa un filtro sin resultados.
2. Navega con el teclado: Tab, Enter y Escape en los modales. Revisa también las pantallas a 390, 768 y 1280 px de ancho.
3. En el formulario de iniciativa, **agrega dos competencias y quita una: tiene que pedir confirmación**.
4. Lint y build:

```powershell
cd cliente
npm run lint     # 0 errores y las 3 advertencias conocidas
npm run build    # sin errores
cd ..
```

Si corregiste algo, haz commit:

```powershell
git add cliente
git commit -m "Revisar estados de interfaz, teclado y tamaños de pantalla" --trailer "Seccion: Cliente" --trailer "Cambio: Barrido final de estados, lint y build"
git push
```

Si no cambiaste nada, no hagas commit. Git no permite un commit sin cambios, salvo con `--allow-empty`.

---

## Paso 9 · Ensayo de la defensa (hoy en la tarde o noche)

Desactiva Copilot, Claude y el autocompletado. Usa `Guia-defensa.md`, que ya está actualizada sin zod:

1. **Recorridos en voz alta:** los cinco de la Parte 4, dos veces cada uno.
2. **Modificaciones en caliente** (Parte 12), en una rama aparte. Empieza por M1, M5 y M10, que son validaciones con el nuevo estilo `if`:

   ```powershell
   git checkout -b practica-defensa
   # … practicar …
   git checkout -- .
   git checkout main
   git branch -D practica-defensa
   ```

3. **Comparación con los labs:** lee la tabla de la sección 2 de `Simplificacion-segun-practicas.md`. Responde la pregunta "¿dónde está lo del lab X?".

---

## Domingo 8 · Entrega

### Paso 10 · Clon limpio siguiendo solo el README

```powershell
cd $env:USERPROFILE\Desktop
git clone https://github.com/jose-ucenfotec/innovation-hub.git prueba-entrega
cd prueba-entrega
```

1. Sigue las secciones 1 a 4 del README tal como están escritas. Para el `.env`, copia el tuyo de Atlas.
2. Al final, `npm run test:api` tiene que dar 78 / 0.
3. Si algo falla, corrige el README en tu repo original, haz commit y push, y vuelve a clonar.

### Paso 11 · Flujo completo en la interfaz

Recorre en este orden:
1. Registro y editar el perfil con competencias.
2. Catálogo con filtros y páginas, y publicar una iniciativa.
3. Cerrar sesión y solicitar participación como otra persona.
4. Aceptar la solicitud como propietario, ver el equipo y retirar a alguien.
5. Convertir en proyecto con repositorio y ver los datos de GitHub.
6. Cerrar sesión.

### Paso 12 · Revisiones finales

```powershell
git status                                    # sin cambios pendientes
git ls-files | Select-String "\.env$"         # no debe aparecer ningún .env
git log --oneline | Measure-Object            # 30 commits o más
```

### Paso 13 · Etiqueta y envío

```powershell
git tag -a entrega-2 -m "Avance 2: React, Express y MongoDB"
git push origin main --tags
git ls-remote --tags origin                   # deben aparecer entrega-1 y entrega-2
```

Crea `entrega-2.txt` con la URL del repositorio y súbelo a Moodle **antes de las 11:55 p. m.**

Desde ese momento el contrato queda congelado. Cualquier práctica posterior va en una rama aparte.

### Paso 14 · Dejar todo listo para la defensa

- Atlas con `npm run seed` recién ejecutado.
- El servidor corriendo con `npm start` y el cliente con `npm run dev`.
- Postman con la colección y el entorno importados.
- VS Code abierto en la raíz, **sin asistentes de IA**.

---

## Resumen de commits de hoy

| # | Mensaje | Seccion |
|---|---|---|
| 1 | Ajustar el contrato al alcance del Avance 2: sin filtros de miembros ni límite de login | Contrato |
| 2 | Cargar el .env con --env-file de Node en lugar de dotenv | Servidor |
| 3 | Validar la entrada a mano en lugar de usar zod | Servidor |
| 4 | Corregir comentarios y sangría en reglas, rutas y detalle | Calidad |
| 5 | Documentar medidas de seguridad y autorización por punto de acceso | Documentación |
| 6 | Aclarar la atomicidad para MongoDB local y Atlas | Modelo de datos |
| 7 | Actualizar el README con Atlas, Node 20.6 y las decisiones finales | Documentación |
| 8 | Revisar estados de interfaz, teclado y tamaños de pantalla (solo si hubo cambios) | Cliente |

Al terminar, el servidor queda con 7 dependencias: `express`, `cors`, `mongoose`, `bcryptjs`, `jsonwebtoken`,
`helmet` y `morgan`. Además está `newman`, solo para desarrollo.
