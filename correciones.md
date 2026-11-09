# Datos de prueba con temática Cenfotec (Leonardo, Donatello, Rafael y Miguel)

**Qué cambia:** el contenido del seed y la colección de Postman. El código de la aplicación no cambia, salvo un
texto de ejemplo y un comentario.

**Qué se mantiene igual, a propósito:**
- Los ids de MongoDB: personas, iniciativas, solicitudes y proyecto.
- Quién es dueño de qué, la visibilidad, los estados, los equipos, el cupo y las solicitudes.

Por eso todas las reglas siguen dando el mismo resultado, y las demostraciones de la guía funcionan con los nombres nuevos.

**Ya lo probé en una copia de tu último zip:**
- Los datos nuevos pasan los validadores reales, y cada solicitud ofrece una competencia de la intersección.
- `npm run seed` carga los 4 usuarios nuevos.
- Newman da **78 peticiones, 258 aserciones y 0 fallos**.
- El lint da 0 errores (las 3 advertencias de siempre) y el build compila.
- El catálogo se ve bien en el navegador.

## Importante: ¿ya creaste la etiqueta `entrega-2`?

- **Si ya la creaste y entregaste:** haz estos cambios igual en `main`, después de la etiqueta. La etiqueta sigue
  apuntando al commit entregado, que es lo que se evalúa, y en la defensa demuestras con los datos nuevos. Si el
  profesor pregunta, la respuesta es: "Después de la entrega solo cambié el contenido del seed y la colección; el
  código y el contrato son los mismos de la etiqueta."
- **Si todavía no la creaste:** hazlos antes de crear la etiqueta y sigue con los pasos 10 a 14 de `Pasos-finales.md`.

## Quién es quién

| Antes | Ahora | Correo | Tipo y carrera | Competencias |
|---|---|---|---|---|
| Ana Mora | **Leonardo Brenes Arias** | `leonardo.brenes@ucenfotec.ac.cr` | Estudiante · Ingeniería del Software | Desarrollo web (avanzado), Gestión ágil (intermedio) |
| Luis Pérez | **Donatello Quirós** | `donatello.quiros@ucenfotec.ac.cr` | Docente · Ciberseguridad | Análisis de datos, Redes y sistemas operativos |
| María Solís | **Rafael Montero Chaves** | `rafael.montero@ucenfotec.ac.cr` | Estudiante · Ingeniería en Ciberseguridad | Gestión ágil, Ciberseguridad (avanzado), Análisis de datos (básico) |
| Diego Vargas | **Miguel Solano** | `miguel.solano@ucenfotec.ac.cr` | Administrativo · Tecnologías de Información | Cloud y DevOps (avanzado), Diseño UX/UI (básico) |

La contraseña de todos sigue siendo `Prueba123!`.

| # | Antes | Ahora | Dueño | Visibilidad · estado | Para demostrar |
|---|---|---|---|---|---|
| 01 | Voluntariado | **Plataforma de mentorías de programación** | Leonardo (con Donatello) | pública · forming | Aceptar la solicitud pendiente de Rafael; archivar en vez de eliminar |
| 02 | Monitoreo de energía | **Monitoreo de la red de los laboratorios** | Donatello (con Leonardo y Rafael) | pública · project | Proyecto con GitHub |
| 03 | Tutorías | **Grupos de estudio para certificaciones de seguridad** | Rafael | institucional · published | 401 para visitantes |
| 04 | Reciclaje | **Galería web de proyectos de graduación** | Leonardo | pública · published | Eliminar (sin relaciones) |
| 05 | Soporte TI | **Reducir el tiempo de atención de la mesa de ayuda** | Miguel | restringida · published | Solo resumen para terceros |
| 06 | Coworking | **Laboratorio de práctica en la nube** | Donatello (con Miguel) | pública · forming | Solicitar como Rafael, retirar a Miguel, convertir |
| 07 | Bienestar | **Gestor de contraseñas para el grupo de investigación** | Rafael (con Leonardo) | privada · forming | 404 para terceros |
| 08 | Festival | **CTF interuniversitario de ciberseguridad** | Miguel | pública · published | Convertir sin miembros → 409 `NO_MEMBERS` |

**Categorías:**
- Desarrollo de software
- Ciberseguridad
- Sistemas e infraestructura
- Diseño y experiencia digital
- Educación y mentorías
- Emprendimiento tecnológico
- Impacto social
- Otro

**Competencias:**
- Desarrollo web
- Diseño UX/UI
- Diseño gráfico
- Análisis de datos
- Gestión ágil (Scrum)
- Redes y sistemas operativos
- Cloud y DevOps
- Ciberseguridad y hacking ético

---

## Paso 1 · Reemplazar los 7 archivos de `servidor/src/seed/datos/`

Abre cada archivo, selecciona todo (`Ctrl+A`) y pega el contenido completo de abajo.

### `servidor/src/seed/datos/categories.json`

```json
[
  { "id": "cat-software", "name": "Desarrollo de software", "icon": "bi-code-slash" },
  { "id": "cat-ciberseguridad", "name": "Ciberseguridad", "icon": "bi-shield-lock" },
  { "id": "cat-sistemas", "name": "Sistemas e infraestructura", "icon": "bi-hdd-network" },
  { "id": "cat-diseno", "name": "Diseño y experiencia digital", "icon": "bi-palette" },
  { "id": "cat-educacion", "name": "Educación y mentorías", "icon": "bi-mortarboard" },
  { "id": "cat-emprendimiento", "name": "Emprendimiento tecnológico", "icon": "bi-rocket-takeoff" },
  { "id": "cat-impacto-social", "name": "Impacto social", "icon": "bi-people" },
  { "id": "cat-otro", "name": "Otro", "icon": "bi-three-dots" }
]
```

### `servidor/src/seed/datos/skills.json`

```json
[
  { "id": "comp-desarrollo-web", "name": "Desarrollo web (JavaScript y React)", "area": "Desarrollo" },
  { "id": "comp-diseno-ux", "name": "Diseño UX/UI", "area": "Diseño" },
  { "id": "comp-diseno-grafico", "name": "Diseño gráfico y multimedia", "area": "Diseño" },
  { "id": "comp-analisis-datos", "name": "Análisis de datos", "area": "Datos" },
  { "id": "comp-gestion-agil", "name": "Gestión ágil (Scrum)", "area": "Gestión" },
  { "id": "comp-redes", "name": "Redes y sistemas operativos", "area": "Infraestructura" },
  { "id": "comp-cloud", "name": "Cloud y DevOps", "area": "Infraestructura" },
  { "id": "comp-ciberseguridad", "name": "Ciberseguridad y hacking ético", "area": "Seguridad" }
]
```

### `servidor/src/seed/datos/roles.json`

```json
[
  { "id": "rol-frontend", "name": "Desarrollo frontend" },
  { "id": "rol-backend", "name": "Desarrollo backend" },
  { "id": "rol-ux", "name": "Diseño UX/UI" },
  { "id": "rol-datos", "name": "Análisis de datos" },
  { "id": "rol-coordinacion", "name": "Scrum Master / coordinación" },
  { "id": "rol-comunicacion", "name": "Documentación y difusión" },
  { "id": "rol-investigacion", "name": "Investigación" },
  { "id": "rol-infraestructura", "name": "Infraestructura y seguridad" }
]
```

### `servidor/src/seed/datos/users.json`

```json
[
  { "id": "650000000000000000000001", "firstName": "Leonardo", "lastName": "Brenes", "secondLastName": "Arias",
    "email": "leonardo.brenes@ucenfotec.ac.cr", "memberType": "student", "career": "Ingeniería del Software",
    "bio": "Cuarto cuatrimestre. Me interesa el desarrollo web y armar comunidades técnicas en Cenfotec.",
    "skills": [ { "skillId": "comp-desarrollo-web", "level": "advanced" }, { "skillId": "comp-gestion-agil", "level": "intermediate" } ],
    "interests": ["software", "mentorías"], "availability": { "hoursPerWeek": 6, "detail": "tardes y fines de semana" },
    "portfolioUrl": "https://github.com/leonardobrenes", "status": "active",
    "createdAt": "2026-09-01T14:00:00.000Z", "updatedAt": "2026-09-20T14:00:00.000Z" },

  { "id": "650000000000000000000002", "firstName": "Donatello", "lastName": "Quirós", "secondLastName": null,
    "email": "donatello.quiros@ucenfotec.ac.cr", "memberType": "teacher", "career": "Ciberseguridad",
    "bio": "Docente de redes y seguridad. Busco proyectos aplicados para mis cursos.",
    "skills": [ { "skillId": "comp-analisis-datos", "level": "advanced" }, { "skillId": "comp-redes", "level": "advanced" } ],
    "interests": ["redes", "monitoreo"], "availability": { "hoursPerWeek": 4, "detail": "martes y jueves" },
    "portfolioUrl": null, "status": "active",
    "createdAt": "2026-09-01T14:00:00.000Z", "updatedAt": "2026-09-01T14:00:00.000Z" },

  { "id": "650000000000000000000003", "firstName": "Rafael", "lastName": "Montero", "secondLastName": "Chaves",
    "email": "rafael.montero@ucenfotec.ac.cr", "memberType": "student", "career": "Ingeniería en Ciberseguridad",
    "bio": "Coordino el grupo estudiantil de seguridad informática.",
    "skills": [ { "skillId": "comp-gestion-agil", "level": "intermediate" }, { "skillId": "comp-ciberseguridad", "level": "advanced" }, { "skillId": "comp-analisis-datos", "level": "basic" } ],
    "interests": ["hacking ético", "certificaciones"], "availability": { "hoursPerWeek": 3, "detail": "mañanas" },
    "portfolioUrl": null, "status": "active",
    "createdAt": "2026-09-02T14:00:00.000Z", "updatedAt": "2026-09-02T14:00:00.000Z" },

  { "id": "650000000000000000000004", "firstName": "Miguel", "lastName": "Solano", "secondLastName": null,
    "email": "miguel.solano@ucenfotec.ac.cr", "memberType": "staff", "career": "Tecnologías de Información",
    "bio": "",
    "skills": [ { "skillId": "comp-cloud", "level": "advanced" }, { "skillId": "comp-diseno-ux", "level": "basic" } ],
    "interests": ["infraestructura"], "availability": { "hoursPerWeek": 0, "detail": "" },
    "portfolioUrl": "https://github.com/miguelsolano", "status": "active",
    "createdAt": "2026-09-03T14:00:00.000Z", "updatedAt": "2026-09-03T14:00:00.000Z" }
]
```

### `servidor/src/seed/datos/initiatives.json`

```json
[
  { "id": "660000000000000000000001", "title": "Plataforma de mentorías de programación", "type": "idea",
    "summary": "Conectar estudiantes avanzados de software con quienes empiezan a programar.",
    "description": "Hoy las mentorías se coordinan por chat y dependen de conocidos. Proponemos una plataforma para publicar horarios, emparejar mentores por tecnología y registrar las sesiones.",
    "problem": "Los estudiantes de primer ingreso no saben a quién pedir ayuda fuera de clase.",
    "beneficiaries": "Estudiantes de primer ingreso de las carreras de software.",
    "categoryId": "cat-educacion", "requiredSkillIds": ["comp-desarrollo-web", "comp-diseno-ux", "comp-gestion-agil"],
    "estimatedMembers": 4, "visibility": "public", "tags": ["mentorías", "programación"],
    "ownerId": "650000000000000000000001",
    "members": [ { "userId": "650000000000000000000002", "roleId": "rol-backend", "joinedAt": "2026-09-12T15:00:00.000Z" } ],
    "status": "forming", "createdAt": "2026-09-08T15:00:00.000Z", "updatedAt": "2026-09-20T15:00:00.000Z" },

  { "id": "660000000000000000000002", "title": "Monitoreo de la red de los laboratorios", "type": "challenge",
    "summary": "Tablero en tiempo real del tráfico y la disponibilidad de la red de los laboratorios.",
    "description": "Cuando la red de un laboratorio se satura o se cae, el área de TI se entera por los estudiantes. Se busca un sistema de monitoreo con alertas tempranas.",
    "problem": "Caídas y saturación de la red sin datos para prevenirlas.",
    "beneficiaries": "Estudiantes, docentes y el área de TI.",
    "categoryId": "cat-sistemas", "requiredSkillIds": ["comp-analisis-datos", "comp-redes", "comp-desarrollo-web"],
    "estimatedMembers": 3, "visibility": "public", "tags": ["redes", "monitoreo"],
    "ownerId": "650000000000000000000002",
    "members": [
      { "userId": "650000000000000000000001", "roleId": "rol-frontend", "joinedAt": "2026-09-11T15:00:00.000Z" },
      { "userId": "650000000000000000000003", "roleId": "rol-datos", "joinedAt": "2026-09-11T15:00:00.000Z" } ],
    "status": "project", "createdAt": "2026-08-30T15:00:00.000Z", "updatedAt": "2026-09-20T15:00:00.000Z" },

  { "id": "660000000000000000000003", "title": "Grupos de estudio para certificaciones de seguridad", "type": "need",
    "summary": "Preparación entre pares para Security+, CCNA y CEH.",
    "description": "Muchos estudiantes quieren certificarse, pero estudian solos y abandonan. Se busca organizar grupos semanales con simulacros y laboratorios prácticos.",
    "problem": "Baja tasa de aprobación de certificaciones por falta de acompañamiento.",
    "beneficiaries": "Estudiantes de Ciberseguridad y de Redes.",
    "categoryId": "cat-ciberseguridad", "requiredSkillIds": ["comp-gestion-agil", "comp-ciberseguridad"],
    "estimatedMembers": 5, "visibility": "institutional", "tags": ["certificaciones", "tutorías"],
    "ownerId": "650000000000000000000003", "members": [],
    "status": "published", "createdAt": "2026-09-10T15:00:00.000Z", "updatedAt": "2026-09-10T15:00:00.000Z" },

  { "id": "660000000000000000000004", "title": "Galería web de proyectos de graduación", "type": "idea",
    "summary": "Un sitio para mostrar los proyectos finales de todas las carreras de Cenfotec.",
    "description": "Los proyectos de graduación se presentan una vez y se pierden. Proponemos una galería pública con capturas, video y enlace al repositorio de cada proyecto.",
    "problem": "El trabajo de los estudiantes no queda visible para empresas ni para nuevas generaciones.",
    "beneficiaries": "Estudiantes que buscan empleo y futuros estudiantes.",
    "categoryId": "cat-diseno", "requiredSkillIds": ["comp-desarrollo-web", "comp-diseno-grafico"],
    "estimatedMembers": 3, "visibility": "public", "tags": ["portafolio", "graduación"],
    "ownerId": "650000000000000000000001", "members": [],
    "status": "published", "createdAt": "2026-09-14T15:00:00.000Z", "updatedAt": "2026-09-14T15:00:00.000Z" },

  { "id": "660000000000000000000005", "title": "Reducir el tiempo de atención de la mesa de ayuda", "type": "challenge",
    "summary": "Optimizar el proceso de tiquetes de soporte técnico del campus.",
    "description": "El tiempo promedio de resolución de un tiquete supera las 48 horas. Se busca analizar los tiquetes históricos para clasificar y priorizar mejor.",
    "problem": "Insatisfacción de estudiantes y docentes por demoras en soporte técnico.",
    "beneficiaries": "Personal administrativo, docentes y estudiantes.",
    "categoryId": "cat-sistemas", "requiredSkillIds": ["comp-analisis-datos"],
    "estimatedMembers": 2, "visibility": "restricted", "tags": ["soporte", "procesos"],
    "ownerId": "650000000000000000000004", "members": [],
    "status": "published", "createdAt": "2026-09-12T15:00:00.000Z", "updatedAt": "2026-09-18T15:00:00.000Z" },

  { "id": "660000000000000000000006", "title": "Laboratorio de práctica en la nube", "type": "need",
    "summary": "Un entorno compartido en la nube para practicar DevOps sin costo.",
    "description": "Los cursos de infraestructura necesitan servidores para practicar despliegues, pero cada estudiante paga su propia cuenta. Se busca un laboratorio común con cuotas y plantillas listas.",
    "problem": "Practicar en la nube es caro y cada curso arma su entorno desde cero.",
    "beneficiaries": "Estudiantes de Sistemas, Redes y Desarrollo de Software.",
    "categoryId": "cat-sistemas", "requiredSkillIds": ["comp-gestion-agil", "comp-cloud"],
    "estimatedMembers": 6, "visibility": "public", "tags": ["cloud", "devops"],
    "ownerId": "650000000000000000000002",
    "members": [ { "userId": "650000000000000000000004", "roleId": "rol-infraestructura", "joinedAt": "2026-09-13T15:00:00.000Z" } ],
    "status": "forming", "createdAt": "2026-09-05T15:00:00.000Z", "updatedAt": "2026-09-13T15:00:00.000Z" },

  { "id": "660000000000000000000007", "title": "Gestor de contraseñas para el grupo de investigación", "type": "idea",
    "summary": "Herramienta privada para compartir credenciales del laboratorio de forma segura.",
    "description": "Las credenciales de los equipos del laboratorio se comparten por chat. Se propone un gestor cifrado con permisos por persona y registro de accesos.",
    "problem": "Credenciales compartidas sin control ni trazabilidad.",
    "beneficiaries": "Integrantes del grupo de investigación en seguridad.",
    "categoryId": "cat-ciberseguridad", "requiredSkillIds": ["comp-diseno-ux", "comp-desarrollo-web"],
    "estimatedMembers": 3, "visibility": "private", "tags": ["seguridad", "contraseñas"],
    "ownerId": "650000000000000000000003",
    "members": [ { "userId": "650000000000000000000001", "roleId": "rol-frontend", "joinedAt": "2026-09-17T15:00:00.000Z" } ],
    "status": "forming", "createdAt": "2026-09-16T15:00:00.000Z", "updatedAt": "2026-09-19T15:00:00.000Z" },

  { "id": "660000000000000000000008", "title": "CTF interuniversitario de ciberseguridad", "type": "challenge",
    "summary": "Organizar una competencia Capture the Flag abierta a otras universidades.",
    "description": "Los estudiantes practican retos de seguridad por su cuenta. Se busca organizar un CTF anual con retos propios, tabla de posiciones e identidad visual del evento.",
    "problem": "No hay un espacio para que los estudiantes midan sus habilidades de seguridad con otras universidades.",
    "beneficiaries": "Estudiantes de Ciberseguridad y comunidad técnica universitaria.",
    "categoryId": "cat-ciberseguridad", "requiredSkillIds": ["comp-ciberseguridad", "comp-diseno-grafico", "comp-gestion-agil"],
    "estimatedMembers": 4, "visibility": "public", "tags": ["ctf", "competencia", "evento"],
    "ownerId": "650000000000000000000004", "members": [],
    "status": "published", "createdAt": "2026-09-21T15:00:00.000Z", "updatedAt": "2026-09-21T15:00:00.000Z" }
]
```

### `servidor/src/seed/datos/requests.json`

```json
[
  { "id": "670000000000000000000001", "initiativeId": "660000000000000000000002", "userId": "650000000000000000000003",
    "message": "Puedo analizar los registros de tráfico y detectar patrones de saturación.", "offeredSkillId": "comp-analisis-datos",
    "roleId": "rol-datos", "availability": "6 h/semana", "status": "accepted",
    "decisionComment": "Bienvenido al equipo.", "decidedAt": "2026-09-11T15:00:00.000Z",
    "createdAt": "2026-09-10T15:00:00.000Z", "updatedAt": "2026-09-11T15:00:00.000Z" },
  { "id": "670000000000000000000002", "initiativeId": "660000000000000000000006", "userId": "650000000000000000000004",
    "message": "Administro servidores del campus y puedo preparar las plantillas de despliegue.", "offeredSkillId": "comp-cloud",
    "roleId": "rol-infraestructura", "availability": "4 h/semana", "status": "accepted",
    "decisionComment": null, "decidedAt": "2026-09-13T15:00:00.000Z",
    "createdAt": "2026-09-12T15:00:00.000Z", "updatedAt": "2026-09-13T15:00:00.000Z" },
  { "id": "670000000000000000000003", "initiativeId": "660000000000000000000001", "userId": "650000000000000000000003",
    "message": "Me gustaría coordinar los sprints y la asignación de mentores.", "offeredSkillId": "comp-gestion-agil",
    "roleId": "rol-coordinacion", "availability": "3 h/semana", "status": "pending",
    "decisionComment": null, "decidedAt": null,
    "createdAt": "2026-09-16T15:00:00.000Z", "updatedAt": "2026-09-16T15:00:00.000Z" },
  { "id": "670000000000000000000004", "initiativeId": "660000000000000000000002", "userId": "650000000000000000000001",
    "message": "Puedo construir el tablero web con React.", "offeredSkillId": "comp-desarrollo-web",
    "roleId": "rol-frontend", "availability": "5 h/semana", "status": "accepted",
    "decisionComment": null, "decidedAt": "2026-09-11T15:00:00.000Z",
    "createdAt": "2026-09-10T16:00:00.000Z", "updatedAt": "2026-09-11T15:00:00.000Z" },
  { "id": "670000000000000000000005", "initiativeId": "660000000000000000000006", "userId": "650000000000000000000003",
    "message": "Quisiera coordinar el uso del laboratorio entre los cursos.", "offeredSkillId": "comp-gestion-agil",
    "roleId": "rol-coordinacion", "availability": "3 h/semana", "status": "rejected",
    "decisionComment": "Por ahora ya cubrimos la coordinación.", "decidedAt": "2026-09-15T15:00:00.000Z",
    "createdAt": "2026-09-14T15:00:00.000Z", "updatedAt": "2026-09-15T15:00:00.000Z" }
]
```

### `servidor/src/seed/datos/projects.json`

```json
[
  { "id": "680000000000000000000001", "initiativeId": "660000000000000000000002",
    "name": "Monitoreo de la red de los laboratorios",
    "objective": "Detectar caídas y saturación de la red de los laboratorios en menos de 5 minutos.",
    "description": "Agentes ligeros en cada laboratorio, tablero web en tiempo real y alertas por correo cuando un enlace se satura o se cae.",
    "technologies": ["Node.js", "Prometheus", "Grafana"],
    "startDate": "2026-09-20", "estimatedEndDate": "2026-12-15",
    "repositoryUrl": "https://github.com/twbs/bootstrap", "status": "in_progress",
    "createdAt": "2026-09-20T15:00:00.000Z", "updatedAt": "2026-09-20T15:00:00.000Z" }
]
```

---

## Paso 2 · Reemplazar `pruebas/generar-coleccion.mjs`

Cambian los correos, los ids de competencias y categorías, y los textos de los cuerpos de prueba. Los
casos y lo que espera cada uno son los mismos 78 de antes. También cambian las claves internas: `ana` pasa
a `leonardo` y `voluntariado` a `mentorias`. Por eso en Newman ahora ves "Login leonardo".

```js
// Genera innovation-hub.postman_collection.json y innovation-hub.postman_environment.json.
// Uso: node pruebas/generar-coleccion.mjs
import { writeFileSync } from 'node:fs';

const ID = {
  leonardo: '650000000000000000000001', donatello: '650000000000000000000002', rafael: '650000000000000000000003', miguel: '650000000000000000000004',
  mentorias: '660000000000000000000001', monitoreo: '660000000000000000000002', certificaciones: '660000000000000000000003',
  galeria: '660000000000000000000004', mesaAyuda: '660000000000000000000005', nube: '660000000000000000000006',
  contrasenas: '660000000000000000000007', ctf: '660000000000000000000008',
  solicitudMentorias: '670000000000000000000003', proyectoMonitoreo: '680000000000000000000001',
  inexistente: '6a0000000000000000000099',
};
const CONTRASENA_PRUEBA = 'Prueba123!';
const CORREOS = { leonardo: 'leonardo.brenes@ucenfotec.ac.cr', donatello: 'donatello.quiros@ucenfotec.ac.cr', rafael: 'rafael.montero@ucenfotec.ac.cr', miguel: 'miguel.solano@ucenfotec.ac.cr' };

function cabeceras(quien) {
  const lista = [{ key: 'Content-Type', value: 'application/json' }];
  if (!quien) return lista;
  lista.push({ key: 'Authorization', value: `Bearer {{token_${quien}}}` });
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
  title: 'Comunidad de práctica de hacking ético', type: 'idea',
  summary: 'Sesiones semanales para practicar retos de seguridad en equipo.',
  description: 'Proponemos un espacio semanal con máquinas vulnerables controladas, guías y revisión de soluciones entre pares.',
  problem: 'Los estudiantes practican seguridad solos y sin retroalimentación.', beneficiaries: 'Estudiantes de Ciberseguridad.',
  categoryId: 'cat-ciberseguridad', requiredSkillIds: ['comp-gestion-agil'], estimatedMembers: 4, visibility: 'public', tags: ['hacking ético', 'comunidad'],
};
const solicitudRafael = { message: 'Puedo coordinar los turnos y las plantillas del laboratorio.', offeredSkillId: 'comp-gestion-agil', roleId: 'rol-coordinacion', availability: '3 h/semana' };
const proyecto = {
  name: 'Plataforma de mentorías', objective: 'Emparejar mentores y estudiantes por tecnología.',
  description: 'Aplicación web para publicar horarios de mentoría, reservar sesiones y registrar el avance.',
  technologies: ['React', 'Express'], startDate: '2026-11-10', estimatedEndDate: '2027-03-01', repositoryUrl: null,
};

const carpetas = [];

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
    cuerpo: { firstName: 'Otra', lastName: 'Persona', email: CORREOS.leonardo, password: 'Clave2026x', confirmPassword: 'Clave2026x', memberType: 'student', career: 'Diseño', acceptTerms: true },
    estado: 409, codigo: 'EMAIL_TAKEN',
  }),
  caso('Login con contraseña incorrecta', 'POST', '/auth/login', { cuerpo: { email: CORREOS.leonardo, password: 'incorrecta1' }, estado: 401, codigo: 'INVALID_CREDENTIALS' }),
  caso('Login de la cuenta nueva', 'POST', '/auth/login', { cuerpo: { email: 'sofia.quesada@ucenfotec.ac.cr', password: 'Clave2026x' }, estado: 200, extra: [guardar('token_sofia', 'pm.response.json().token')] }),
  caso('Perfil propio', 'GET', '/auth/me', { quien: 'sofia', estado: 200 }),
  caso('Perfil propio sin token', 'GET', '/auth/me', { estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Logout', 'POST', '/auth/logout', { quien: 'sofia', estado: 204 }),
  caso('Token revocado tras logout', 'GET', '/auth/me', { quien: 'sofia', estado: 401, codigo: 'UNAUTHORIZED' }),
]));

carpetas.push(carpeta('02 Usuarios', [
  caso('Buscar por competencia y disponibilidad', 'GET', '/users?skill=comp-gestion&available=true', { quien: 'leonardo', estado: 200 }),
  caso('Buscar con tamaño inválido', 'GET', '/users?size=abc', { quien: 'leonardo', estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Perfil inexistente', 'GET', `/users/${ID.inexistente}`, { quien: 'leonardo', estado: 404, codigo: 'NOT_FOUND' }),
  caso('Buscar sin token', 'GET', '/users', { estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Modificar perfil', 'PUT', '/users/me', {
    quien: 'miguel', estado: 200,
    cuerpo: { firstName: 'Miguel', lastName: 'Solano', secondLastName: null, career: 'Tecnologías de Información', bio: 'Administro la infraestructura en la nube del campus.', skills: [{ skillId: 'comp-cloud', level: 'advanced' }], interests: ['infraestructura'], availability: { hoursPerWeek: 2, detail: 'viernes' }, portfolioUrl: null },
  }),
  caso('Modificar el correo desde el perfil', 'PUT', '/users/me', {
    quien: 'miguel', estado: 400, codigo: 'VALIDATION_ERROR',
    cuerpo: { firstName: 'Miguel', lastName: 'Solano', career: 'Tecnologías de Información', email: 'otro@ucenfotec.ac.cr' },
  }),
]));

carpetas.push(carpeta('03 Catálogos', [
  caso('Categorías', 'GET', '/categories', { estado: 200, extra: ["pm.test('Ocho categorías', () => pm.expect(pm.response.json()).to.have.length(8));"] }),
  caso('Competencias', 'GET', '/skills', { estado: 200 }),
  caso('Roles', 'GET', '/roles', { estado: 200 }),
  caso('Ruta inexistente', 'GET', '/no-existe', { estado: 404, codigo: 'NOT_FOUND' }),
]));

carpetas.push(carpeta('04 Iniciativas', [
  caso('Listar con filtros combinados', 'GET', '/initiatives?type=challenge&category=cat-ciberseguridad&sort=title', {
    estado: 200, extra: ["pm.test('Sobre de paginación', () => pm.expect(pm.response.json()).to.have.all.keys('items', 'page', 'size', 'totalItems', 'totalPages'));"],
  }),
  caso('Tamaño de página limitado a 50', 'GET', '/initiatives?size=1000', { estado: 200, extra: ["pm.test('size = 50', () => pm.expect(pm.response.json().size).to.eql(50));"] }),
  caso('Listar con tipo inválido', 'GET', '/initiatives?type=cualquiera', { estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Mis iniciativas sin token', 'GET', '/initiatives?owner=me', { estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Restringida vista por un tercero', 'GET', `/initiatives/${ID.mesaAyuda}`, {
    quien: 'leonardo', estado: 200,
    extra: ["pm.test('Solo el resumen', () => pm.expect(Object.keys(pm.response.json()).sort()).to.eql(['accessLevel','category','id','requiredSkills','summary','title','type','visibility']));"],
  }),
  caso('Privada vista por un tercero', 'GET', `/initiatives/${ID.contrasenas}`, { quien: 'miguel', estado: 404, codigo: 'NOT_FOUND' }),
  caso('Institucional sin sesión', 'GET', `/initiatives/${ID.certificaciones}`, { estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Detalle inexistente', 'GET', `/initiatives/${ID.inexistente}`, { estado: 404, codigo: 'NOT_FOUND' }),
  caso('Crear', 'POST', '/initiatives', { quien: 'leonardo', cuerpo: iniciativaNueva, estado: 201, extra: [guardar('nuevaIniciativa'), "pm.test('Location', () => pm.response.to.have.header('Location'));"] }),
  caso('Crear con datos inválidos', 'POST', '/initiatives', { quien: 'leonardo', cuerpo: { ...iniciativaNueva, title: 'abc', estimatedMembers: 50, visibility: 'secreta' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Crear sin sesión', 'POST', '/initiatives', { cuerpo: iniciativaNueva, estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Modificar', 'PUT', '/initiatives/{{nuevaIniciativa}}', { quien: 'leonardo', cuerpo: { ...iniciativaNueva, estimatedMembers: 5 }, estado: 200 }),
  caso('Modificar iniciativa ajena', 'PUT', '/initiatives/{{nuevaIniciativa}}', { quien: 'donatello', cuerpo: iniciativaNueva, estado: 403, codigo: 'FORBIDDEN' }),
  caso('Modificar inexistente', 'PUT', `/initiatives/${ID.inexistente}`, { quien: 'leonardo', cuerpo: iniciativaNueva, estado: 404, codigo: 'NOT_FOUND' }),
  caso('Integrantes menores que el equipo', 'PUT', `/initiatives/${ID.mentorias}`, {
    quien: 'leonardo', estado: 409, codigo: 'PARTICIPANTS_BELOW_TEAM',
    cuerpo: { ...iniciativaNueva, title: 'Plataforma de mentorías de programación', estimatedMembers: 1 },
  }),
  caso('Eliminar con información relacionada', 'DELETE', `/initiatives/${ID.mentorias}`, { quien: 'leonardo', estado: 409, codigo: 'HAS_RELATED_DATA' }),
  caso('Eliminar iniciativa ajena', 'DELETE', '/initiatives/{{nuevaIniciativa}}', { quien: 'donatello', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Archivar', 'POST', '/initiatives/{{nuevaIniciativa}}/archive', { quien: 'leonardo', estado: 200, extra: ["pm.test('Archivada', () => pm.expect(pm.response.json().status).to.eql('archived'));"] }),
  caso('Modificar archivada', 'PUT', '/initiatives/{{nuevaIniciativa}}', { quien: 'leonardo', cuerpo: iniciativaNueva, estado: 409, codigo: 'ARCHIVED' }),
  caso('Eliminar sin relaciones', 'DELETE', '/initiatives/{{nuevaIniciativa}}', { quien: 'leonardo', estado: 204 }),
  caso('Eliminar inexistente', 'DELETE', `/initiatives/${ID.inexistente}`, { quien: 'leonardo', estado: 404, codigo: 'NOT_FOUND' }),
]));

carpetas.push(carpeta('05 Equipo', [
  caso('Listar miembros', 'GET', `/initiatives/${ID.mentorias}/members`, { quien: 'leonardo', estado: 200 }),
  caso('Miembros de una restringida ajena', 'GET', `/initiatives/${ID.mesaAyuda}/members`, { quien: 'leonardo', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Retirar al propietario', 'DELETE', `/initiatives/${ID.mentorias}/members/${ID.leonardo}`, { quien: 'leonardo', estado: 409, codigo: 'CANNOT_REMOVE_OWNER' }),
  caso('Retirar sin ser propietario', 'DELETE', `/initiatives/${ID.nube}/members/${ID.miguel}`, { quien: 'leonardo', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Retirar a quien no es miembro', 'DELETE', `/initiatives/${ID.mentorias}/members/${ID.miguel}`, { quien: 'leonardo', estado: 404, codigo: 'NOT_FOUND' }),
  caso('Retirar miembro', 'DELETE', `/initiatives/${ID.nube}/members/${ID.miguel}`, { quien: 'donatello', estado: 204 }),
]));

carpetas.push(carpeta('06 Solicitudes', [
  caso('Crear solicitud', 'POST', `/initiatives/${ID.nube}/requests`, { quien: 'rafael', cuerpo: solicitudRafael, estado: 201, extra: [guardar('nuevaSolicitud')] }),
  caso('Solicitud duplicada pendiente', 'POST', `/initiatives/${ID.nube}/requests`, { quien: 'rafael', cuerpo: solicitudRafael, estado: 409, codigo: 'DUPLICATE_PENDING' }),
  caso('Competencia que no tiene', 'POST', `/initiatives/${ID.ctf}/requests`, { quien: 'donatello', cuerpo: { ...solicitudRafael, offeredSkillId: 'comp-redes' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Solicitar en la propia', 'POST', `/initiatives/${ID.mentorias}/requests`, { quien: 'leonardo', cuerpo: solicitudRafael, estado: 409, codigo: 'SELF_REQUEST' }),
  caso('Solicitar en un proyecto', 'POST', `/initiatives/${ID.monitoreo}/requests`, { quien: 'miguel', cuerpo: solicitudRafael, estado: 409, codigo: 'INITIATIVE_CLOSED' }),
  caso('Solicitar en inexistente', 'POST', `/initiatives/${ID.inexistente}/requests`, { quien: 'rafael', cuerpo: solicitudRafael, estado: 404, codigo: 'NOT_FOUND' }),
  caso('Solicitar sin sesión', 'POST', `/initiatives/${ID.nube}/requests`, { cuerpo: solicitudRafael, estado: 401, codigo: 'UNAUTHORIZED' }),
  caso('Mis solicitudes', 'GET', '/requests/mine?status=pending', { quien: 'rafael', estado: 200 }),
  caso('Recibidas con filtros', 'GET', `/requests/received?initiativeId=${ID.nube}&status=pending`, { quien: 'donatello', estado: 200 }),
  caso('Recibidas con fechas invertidas', 'GET', '/requests/received?from=2026-12-01&to=2026-01-01', { quien: 'donatello', estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Ver solicitud ajena', 'GET', '/requests/{{nuevaSolicitud}}', { quien: 'leonardo', estado: 403, codigo: 'FORBIDDEN' }),
  caso('Modificar pendiente', 'PUT', '/requests/{{nuevaSolicitud}}', { quien: 'rafael', cuerpo: { ...solicitudRafael, availability: '4 h/semana' }, estado: 200 }),
  caso('Resolver sin ser propietario', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'rafael', cuerpo: { decision: 'accepted' }, estado: 403, codigo: 'FORBIDDEN' }),
  caso('Decisión inválida', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'donatello', cuerpo: { decision: 'tal vez' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Aceptar e incorporar', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'donatello', cuerpo: { decision: 'accepted', comment: 'Bienvenida' }, estado: 200 }),
  caso('Resolver de nuevo', 'POST', '/requests/{{nuevaSolicitud}}/decision', { quien: 'donatello', cuerpo: { decision: 'rejected' }, estado: 409, codigo: 'NOT_PENDING' }),
  caso('Cancelar ya resuelta', 'POST', '/requests/{{nuevaSolicitud}}/cancel', { quien: 'rafael', estado: 409, codigo: 'NOT_PENDING' }),
  caso('Cancelar pendiente', 'POST', `/requests/${ID.solicitudMentorias}/cancel`, { quien: 'rafael', estado: 200 }),
  caso('Cancelar inexistente', 'POST', `/requests/${ID.inexistente}/cancel`, { quien: 'rafael', estado: 404, codigo: 'NOT_FOUND' }),
]));

carpetas.push(carpeta('07 Proyectos', [
  caso('Convertir con datos inválidos', 'POST', `/initiatives/${ID.mentorias}/project`, { quien: 'leonardo', cuerpo: { ...proyecto, name: '', estimatedEndDate: '2026-01-01' }, estado: 400, codigo: 'VALIDATION_ERROR' }),
  caso('Convertir sin ser propietario', 'POST', `/initiatives/${ID.mentorias}/project`, { quien: 'donatello', cuerpo: proyecto, estado: 403, codigo: 'FORBIDDEN' }),
  caso('Convertir sin integrantes', 'POST', `/initiatives/${ID.ctf}/project`, { quien: 'miguel', cuerpo: proyecto, estado: 409, codigo: 'NO_MEMBERS' }),
  caso('Convertir', 'POST', `/initiatives/${ID.mentorias}/project`, { quien: 'leonardo', cuerpo: proyecto, estado: 201, extra: [guardar('nuevoProyecto')] }),
  caso('Convertir de nuevo', 'POST', `/initiatives/${ID.mentorias}/project`, { quien: 'leonardo', cuerpo: proyecto, estado: 409, codigo: 'ALREADY_PROJECT' }),
  caso('Listar proyectos', 'GET', '/projects?sort=name', { quien: 'donatello', estado: 200 }),
  caso('Detalle de proyecto', 'GET', '/projects/{{nuevoProyecto}}', { quien: 'donatello', estado: 200, extra: ["pm.test('Conserva la iniciativa de origen', () => pm.expect(pm.response.json().initiative.id).to.eql('660000000000000000000001'));"] }),
  caso('Proyecto inexistente', 'GET', `/projects/${ID.inexistente}`, { quien: 'donatello', estado: 404, codigo: 'NOT_FOUND' }),
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

Luego regenera la colección:

```powershell
node pruebas\generar-coleccion.mjs     # "Colección generada: 78 peticiones en 8 carpetas."
```

---

## Paso 3 · Dos textos en el código

1. `cliente/src/features/iniciativas/FormularioIniciativaPage.jsx`, línea 145. El ejemplo de etiquetas pasa
   de `placeholder="voluntariado"` a:
   ```jsx
   placeholder="ciberseguridad"
   ```
2. `servidor/src/repositories/usuarios.repository.js`, línea 26. El comentario queda así:
   ```js
     // Cada palabra debe aparecer en alguno de los nombres: "leonardo brenes" encuentra a Leonardo Brenes Arias.
   ```

---

## Paso 4 · README: tabla de usuarios de prueba

En la sección **Usuarios de prueba**, reemplaza las cuatro filas por estas:

```markdown
| Leonardo Brenes | leonardo.brenes@ucenfotec.ac.cr | propietario con equipo; miembro de una privada |
| Donatello Quirós | donatello.quiros@ucenfotec.ac.cr | propietario que recibe solicitudes |
| Rafael Montero | rafael.montero@ucenfotec.ac.cr | solicitante; propietario de una privada |
| Miguel Solano | miguel.solano@ucenfotec.ac.cr | propietario de una restringida |
```

---

## Paso 5 · Cargar los datos nuevos en Atlas y probar

`npm run seed` **borra la base** y la vuelve a cargar, así que se pierden las iniciativas o usuarios que hayas
creado a mano. Dentro de `servidor`:

```powershell
npm run seed      # debe listar leonardo.brenes, donatello.quiros, rafael.montero y miguel.solano
```

Después:
1. En una terminal: `npm start`.
2. En otra, dentro de `servidor`: `npm run test:api`. Tiene que dar **78 peticiones y 0 fallos**.
3. En el navegador, con el cliente en `npm run dev`:
   - entra como `rafael.montero@ucenfotec.ac.cr`;
   - abre *Laboratorio de práctica en la nube* y elige "Solicitar participación";
   - solo debe aparecer la competencia "Gestión ágil (Scrum)", la única de la intersección.
4. Si en Postman tenías importada la colección vieja, vuelve a importar
   `pruebas/innovation-hub.postman_collection.json`.

---

## Paso 6 · Commit

```powershell
git add servidor\src\seed\datos pruebas cliente\src\features\iniciativas\FormularioIniciativaPage.jsx servidor\src\repositories\usuarios.repository.js README.md
git commit -m "Actualizar los datos de prueba con iniciativas y personas de Cenfotec" --trailer "Seccion: Datos" --trailer "Cambio: Seed y colección con temática de software, ciberseguridad, sistemas y diseño; mismos ids y relaciones"
git push
```

---

## Lo que ya actualicé por ti

- **`Guia-defensa.md`:** tabla de datos, recorridos, modificaciones en caliente (M2, M4 y M7) y la demostración de
  5 minutos, todo con los nombres y títulos nuevos. Ejemplos:
  - M7 ahora se prueba con `GET /api/projects?q=red`.
  - M4 se prueba como Rafael: ya tiene 1 pendiente en Mentorías; solicita en Mesa de ayuda y en Laboratorio en la
    nube, y la del CTF da 409.
- **`defensa-app/index.html`:** regenerada. Las 70 peticiones reales se volvieron a ejecutar con los datos nuevos,
  y la tabla del seed y los flujos usan los nombres nuevos. Los números de línea no cambiaron.
