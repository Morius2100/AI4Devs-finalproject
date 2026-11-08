> Detalla en esta sección los prompts principales utilizados durante la creación del proyecto, que justifiquen el uso de asistentes de código en todas las fases del ciclo de vida del desarrollo. Esperamos un máximo de 3 por sección, principalmente los de creación inicial o los de corrección o adición de funcionalidades que consideres más relevantes.
> Puedes añadir adicionalmente la conversación completa como link o archivo adjunto si así lo consideras

**Asistente utilizado:** Claude Code (modelo Claude Opus 5.5), con OpenSpec 1.4.1 para Spec Driven Development.

**Nota:** la documentación se generó con un único prompt inicial detallado (Prompt 1 de la sección 1), que definía todos los entregables. Un segundo prompt adaptó esa documentación a esta plantilla. En cada sección se indica qué prompt (o qué parte del Prompt 1) produjo el contenido y qué generó el asistente a partir de él. Las secciones que dependen de la implementación (Pull Requests) quedan pendientes.

## Índice

1. [Descripción general del producto](#1-descripción-general-del-producto)
2. [Arquitectura del sistema](#2-arquitectura-del-sistema)
3. [Modelo de datos](#3-modelo-de-datos)
4. [Especificación de la API](#4-especificación-de-la-api)
5. [Historias de usuario](#5-historias-de-usuario)
6. [Tickets de trabajo](#6-tickets-de-trabajo)
7. [Pull requests](#7-pull-requests)

---

## 1. Descripción general del producto

**Prompt 1:** _(prompt inicial completo; genera toda la documentación técnica y el plan SDD)_

```
Documentación técnica y planificación del proyecto "Opina"
1. Objetivo
Crea la documentación técnica necesaria para la creación del siguiente proyecto.
La documentación debe servir también como base para la creación de la aplicación mediante Spec Driven Development (SDD).
No añadas funcionalidades o requisitos que no estén especificados a continuación.
2. Entregables
La documentación técnica debe incluir los siguientes apartados:
2.1. Ficha del proyecto
Incluir:
* Nombre del proyecto.
* Descripción.
* URL del repositorio o ZIP.
2.2. Descripción general del producto
Definir:
* Objetivo del producto.
* Características principales.
* Funcionalidades principales.
2.3. Arquitectura del sistema y modelo de datos
Documentar:
* Arquitectura del sistema.
* Componentes principales.
* Relación entre los diferentes componentes.
* Modelo de datos necesario para la aplicación.
2.4. Historias de usuario y tickets de trabajo
Crear:
* Historias de usuario necesarias para desarrollar la aplicación.
* Tickets de trabajo derivados de las historias de usuario.
2.5. Stack tecnológico elegido
Especificar el stack tecnológico seleccionado para desarrollar la aplicación.
3. Desglose de tareas para el desarrollo
Además de la documentación anterior, crea un desglose completo de tareas para la creación de la aplicación.
Este desglose debe estar estructurado de forma que pueda ejecutarse mediante Spec Driven Development (SDD).
Las tareas deben derivarse de las funcionalidades e historias de usuario definidas anteriormente.
4. Descripción de la aplicación
4.1. Definición
Opina es una aplicación web y red social basada en un sistema de temas sobre los que los usuarios pueden votar.
La aplicación pretende proporcionar a los usuarios diferentes temas sobre los que opinar.
La experiencia busca funcionar como una especie de foro, pero de una forma más simplificada y dinámica.
5. Flujo principal de la aplicación
5.1. Pantalla principal
Cuando el usuario entra en la aplicación:
* Verá una serie de temas sobre los que puede opinar.
* Los temas aparecerán mediante una lista scrolleable.
* En la parte superior habrá un buscador.
* Mientras el usuario no realice ninguna búsqueda, se mostrarán las mayores tendencias.
5.2. Acceso a un tema
Cuando el usuario pulse sobre una de las propuestas/temas:
* Entrará dentro del tema.
* Se mostrará el título del tema.
* En caso de que el tema tenga una imagen adjunta, esta aparecerá en la parte superior.
* La imagen ocupará la mitad superior del móvil.
* En la parte inferior aparecerán dos botones:
   * Un botón de pulgar arriba de color verde.
   * Un botón de pulgar abajo de color rojo.
5.3. Proceso de votación
Cuando el usuario pulse uno de los botones de votación:
1. Se le pedirá una justificación del voto.
2. La justificación será opcional, en caso de que el usuario quiera proporcionarla.
3. Una vez finalizado el proceso, el usuario emitirá su voto.
4. Después de votar, el usuario podrá visualizar:
   * El total de votos positivos.
   * El total de votos negativos.
   * Las opiniones de los usuarios que hayan comentado.
Los dos botones/cuadrados iniciales se transformarán en una lista de opiniones de los usuarios que hayan comentado.
Las opiniones se mostrarán:
* En verde si el usuario realizó un voto positivo.
* En rojo si el usuario realizó un voto negativo.
6. Creación de temas
Desde el menú principal, los usuarios podrán crear nuevos temas para que el resto de usuarios pueda votarlos.
Los temas creados por los usuarios podrán ser:
* Públicos.
* Privados.
6.1. Temas públicos
Los temas públicos podrán ser accesibles para los usuarios de la aplicación según el funcionamiento general definido anteriormente.
6.2. Temas privados
Los temas privados tendrán las siguientes características:
* Solo la lista de amigos del usuario que creó el tema podrá acceder a esa votación.
* Los amigos del usuario recibirán una notificación indicando que un amigo ha creado una nueva propuesta.
7. Resultado esperado
Genera una documentación técnica completa del proyecto Opina, utilizando exclusivamente la información proporcionada en este documento.
La documentación debe:
1. Definir el producto y sus funcionalidades.
2. Establecer la arquitectura del sistema.
3. Definir el modelo de datos.
4. Crear las historias de usuario.
5. Crear los tickets de trabajo.
6. Definir el stack tecnológico.
7. Crear un desglose de tareas para desarrollar la aplicación.
8. Organizar dicho desglose para que pueda ejecutarse mediante Spec Driven Development (SDD).
No añadas funcionalidades, requisitos o características que no estén contemplados en la descripción proporcionada.
```

_Resultado:_ `README.md` (ficha) y `docs/01-producto.md`: objetivo, 9 características, 6 funcionalidades, diagrama del flujo principal, **supuestos habilitantes** (identificación de usuarios, relación de amistad, definición de tendencia y de opinión), lo que queda fuera de alcance y las cuestiones abiertas. La restricción "no añadas funcionalidades" llevó al asistente a separar explícitamente los supuestos mínimos imprescindibles de las funcionalidades de producto.

**Prompt 2:** _(adaptación de la documentación a esta plantilla)_

```
Con los documentos que acabas de crear, rellena las plantillas readme y prompts que hay en la carpeta AI4Devs-finalproject
```

_Resultado:_ este `prompts.md` y el `readme.md` de la entrega, incluidas las secciones 1.3 (recorrido de UX y esquema de la pantalla de tema) y 1.4 (instrucciones de instalación según el stack definido).

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

---

## 2. Arquitectura del Sistema

### **2.1. Diagrama de arquitectura:**

**Prompt 1:** _(fragmento del Prompt 1 de la sección 1)_

```
2.3. Arquitectura del sistema y modelo de datos
Documentar:
* Arquitectura del sistema.
* Componentes principales.
* Relación entre los diferentes componentes.
* Modelo de datos necesario para la aplicación.
```

_Resultado:_ `docs/02-arquitectura.md` §2.1: monolito modular por capas sobre Next.js, con el diagrama Mermaid de cliente, middleware, páginas, API, servicios, Prisma, PostgreSQL y S3, y la justificación frente a SPA + API separada.

**Prompt 2:** _(Prompt 2 de la sección 1)_: adaptó el diagrama a la plantilla, añadiendo las tecnologías en cada nodo y la lista de beneficios y sacrificios.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

### **2.2. Descripción de componentes principales:**

**Prompt 1:** _(fragmento del Prompt 1 de la sección 1)_

```
* Componentes principales.
* Relación entre los diferentes componentes.
```

_Resultado:_ `docs/02-arquitectura.md` §2.3–2.4: tabla de componentes de frontend con la historia de usuario asociada, tabla de servicios de dominio (`TopicService`, `VoteService`, `VisibilityPolicy`, `FriendshipService`, `NotificationService`, `ImageStorage`), diagrama de relaciones y diagramas de secuencia de "votar un tema" y "crear un tema privado".

**Prompt 2:**

_[No se usaron más prompts para esta sección]_

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

### **2.3. Descripción de alto nivel del proyecto y estructura de ficheros**

**Prompt 1:** _(fragmento del Prompt 1 de la sección 1)_

```
Este desglose debe estar estructurado de forma que pueda ejecutarse mediante Spec Driven Development (SDD).
```

_Resultado:_ estructura de carpetas `src/app` · `src/server/{services,db,storage,auth}` · `src/lib/validation` · `prisma/` · `tests/` (`docs/02-arquitectura.md` §2.7). Además, el asistente inicializó OpenSpec en el proyecto (`openspec init --tools claude`) y creó la carpeta `openspec/` con `config.yaml` y los 6 cambios.

**Prompt 2:**

_[No se usaron más prompts para esta sección]_

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

### **2.4. Infraestructura y despliegue**

**Prompt 1:** _(Prompt 1 de la sección 1)_: generó el esquema de despliegue (entorno local con Docker Compose, CI en GitHub Actions y producción con contenedor Next.js, PostgreSQL gestionado y bucket S3) y las variables de entorno (`docs/02-arquitectura.md` §2.8).

**Prompt 2:** _(Prompt 2 de la sección 1)_: amplió el apartado con el diagrama de CI/CD, el proceso de despliegue paso a paso y la estrategia de rollback.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

### **2.5. Seguridad**

**Prompt 1:** _(Prompt 1 de la sección 1)_: definió las decisiones transversales de autenticación (Auth.js, cookie `httpOnly`), autorización centralizada en `VisibilityPolicy` con `404` para temas privados, validación con Zod y límites de subida de imágenes (`docs/02-arquitectura.md` §2.6).

**Prompt 2:** _(Prompt 2 de la sección 1)_: las reunió en la tabla de prácticas de seguridad de la plantilla, con un ejemplo de petición denegada.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

### **2.6. Tests**

**Prompt 1:** _(Prompt 1 de la sección 1)_: la regla de OpenSpec _"Specs should be testable - each scenario is a potential test case"_ llevó a definir una tarea de pruebas al final de cada grupo de tareas y un test por escenario (`openspec/config.yaml` y `tasks.md` de cada cambio).

**Prompt 2:** _(Prompt 2 de la sección 1)_: resumió la estrategia de pruebas por niveles (unitarios, integración y e2e) con ejemplos concretos sacados de las specs.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

---

### 3. Modelo de Datos

**Prompt 1:** _(fragmento del Prompt 1 de la sección 1)_

```
* Modelo de datos necesario para la aplicación.
```

_Resultado:_ `docs/03-modelo-datos.md`: entidades `User`, `Friendship`, `Topic`, `Vote` y `Notification`; diagrama ER en Mermaid; esquema Prisma completo; SQL adicional (`pg_trgm`, CHECKs); consultas clave con sus índices; y datos semilla.

**Prompt 2:** _(Prompt 2 de la sección 1)_: reescribió el diagrama ER con tipos, PK/FK, UNIQUE, NOT NULL y reglas en cada atributo, como pide la plantilla.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

---

### 4. Especificación de la API

**Prompt 1:** _(Prompt 1 de la sección 1)_: generó el contrato de los 8 endpoints con entradas, respuestas, errores e historia asociada, y los tipos TypeScript de respuesta (`docs/02-arquitectura.md` §2.5).

**Prompt 2:** _(Prompt 2 de la sección 1)_: convirtió los 3 endpoints principales (`GET /topics`, `GET /topics/{id}`, `POST /topics/{id}/votes`) a OpenAPI 3.0 con un ejemplo de petición y respuesta.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

---

### 5. Historias de Usuario

**Prompt 1:** _(fragmento del Prompt 1 de la sección 1)_

```
2.4. Historias de usuario y tickets de trabajo
Crear:
* Historias de usuario necesarias para desarrollar la aplicación.
```

_Resultado:_ `docs/04-historias-usuario.md`: 13 historias (HU-00 a HU-12) en 6 épicas, con formato _Como / quiero / para_, criterios de aceptación numerados, prioridad y estimación (49 puntos). Cada criterio se tradujo después a escenarios `WHEN/THEN` en las specs de OpenSpec.

**Prompt 2:** _(Prompt 2 de la sección 1)_: seleccionó las 3 historias principales (HU-01, HU-04 + HU-05 y HU-12) y las amplió con un escenario de ejemplo, dependencias y trazabilidad con tickets y specs.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

---

### 6. Tickets de Trabajo

**Prompt 1:** _(fragmento del Prompt 1 de la sección 1)_

```
* Tickets de trabajo derivados de las historias de usuario.
...
3. Desglose de tareas para el desarrollo
Además de la documentación anterior, crea un desglose completo de tareas para la creación de la aplicación.
Este desglose debe estar estructurado de forma que pueda ejecutarse mediante Spec Driven Development (SDD).
Las tareas deben derivarse de las funcionalidades e historias de usuario definidas anteriormente.
```

_Resultado:_

- `docs/05-tickets.md`: 32 tickets (SETUP / DB / BE / FE / QA) con historias, dependencias, estimación (70 puntos) y criterios.
- `openspec/changes/fase-1…fase-6`: un cambio por incremento, cada uno con `proposal.md`, `design.md`, `specs/<capacidad>/spec.md` y `tasks.md` (99 tareas), validados con `openspec validate --all --strict`.
- `docs/07-desglose-tareas-sdd.md`: orden de ejecución, grafo de dependencias y matriz de trazabilidad (requisito → historia → spec → ticket).

**Prompt 2:** _(Prompt 2 de la sección 1)_: desarrolló en detalle un ticket de cada tipo pedido por la plantilla: base de datos (TKT-003), backend (TKT-019) y frontend (TKT-021), con tareas técnicas, criterios de aceptación, pruebas y definición de hecho.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

---

### 7. Pull Requests

**Prompt 1:**

_[PENDIENTE: la implementación aún no ha comenzado. Se añadirán los prompts de ejecución, por ejemplo `/opsx:apply fase-1-plataforma-base`]_

**Prompt 2:**

_[PENDIENTE]_

**Prompt 3:**

_[PENDIENTE]_
