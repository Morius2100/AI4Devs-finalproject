> Detalla en esta sección los prompts principales utilizados durante la creación del proyecto, que justifiquen el uso de asistentes de código en todas las fases del ciclo de vida del desarrollo. Esperamos un máximo de 3 por sección, principalmente los de creación inicial o los de corrección o adición de funcionalidades que consideres más relevantes.
> Puedes añadir adicionalmente la conversación completa como link o archivo adjunto si así lo consideras

**Asistente utilizado:** Claude Code, con OpenSpec 1.4.1 para Spec Driven Development.

**Modelos:** Claude Opus 5.5, Claude Sonnet 5.5 y Claude Haiku 5.5, cambiados con `/model` según la sesión; el registro automático (`Prompts.md` del repositorio del proyecto) guarda cada cambio. Los commits del repositorio del proyecto declaran el modelo con `Co-Authored-By`: 16 de Sonnet 5.5, 3 de Haiku 5.5 y 2 de Opus 5.5. No se atribuye cada resultado a un modelo concreto.

**Contexto del asistente:** [`CLAUDE.md`](CLAUDE.md) (stack, capas, política de visibilidad única y regla de que solo `VoteService` escribe votos), más las specs y las tareas de cada cambio de OpenSpec.

**Nota:** la documentación se generó con un único prompt inicial detallado (Prompt 1 de la sección 1), que definía todos los entregables. Un segundo prompt adaptó esa documentación a esta plantilla. En cada sección se indica qué prompt (o qué parte del Prompt 1) produjo el contenido y qué generó el asistente a partir de él. Las secciones 1 a 6 corresponden a la fase de análisis y diseño; la implementación, las pruebas, el despliegue y las Pull Requests están en las secciones 7 y 8.

## Índice

1. [Descripción general del producto](#1-descripción-general-del-producto)
2. [Arquitectura del sistema](#2-arquitectura-del-sistema)
3. [Modelo de datos](#3-modelo-de-datos)
4. [Especificación de la API](#4-especificación-de-la-api)
5. [Historias de usuario](#5-historias-de-usuario)
6. [Tickets de trabajo](#6-tickets-de-trabajo)
7. [Pull requests](#7-pull-requests)
8. [Implementación y pruebas con IA (Revisión 1)](#8-implementación-y-pruebas-con-ia-revisión-1)

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

_(Prompt 8.8)_: `/opsx:apply T10 Despliegue, creando y documentando el diagrama CI/CD` y la propuesta «Proteger main…». Originaron el contenido de las [PR #1 a #3](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pulls?q=is%3Apr) (CI en verde, documentación CI/CD y archivo del cambio `documentar-ci-cd`).

**Prompt 2:**

_(Prompt 8.9)_: `/opsx:propose` para poder borrar temas propios. Originó el contenido de la [PR #4](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pull/4) (cambio `borrar-temas-propios`, HU-23), que sigue abierta.

**Prompt 3:**

_[No se usaron más prompts para esta sección]_

Hasta el 9 de noviembre de 2026 los cambios se publicaron con `push` directo a `main`, así que las fases 1 a 6 no tienen PR. El detalle está en el [README §7](readme.md).

---

## 8. Implementación y pruebas con IA (Revisión 1)

Prompts reales de la fase de implementación y pruebas, tal como se escribieron (el registro automático completo está en `Prompts.md` del [repositorio del proyecto](https://github.com/Morius2100/ai4devs-finalproject--JRG-)). Las respuestas cortas de confirmación («Sí, …») aprobaron la propuesta que hizo el asistente justo antes. Resumen por fases, errores corregidos y decisiones del humano: [`docs/11-uso-de-ia.md`](docs/11-uso-de-ia.md).

**Asistente utilizado:** Claude Code, con OpenSpec. Contexto fijo del agente en [`CLAUDE.md`](CLAUDE.md). Modelos usados: véase la cabecera de este documento.

**Los 5 prompts clave** son el 8.4 (seguridad), el 8.6 (contexto del agente), el 8.7 (tests desde los criterios), el 8.8 (CI/CD, protección de `main`, APK y Docker) y el 8.9 (borrar temas, HU-23), marcados con ★. El resto son de apoyo. Al final de la sección se explica dónde decidió el humano.

**Prompt 8.1 · Resumen de la Revisión 1 (sin tocar nada):**

> Dentro de docs he creado el documento Revision1. Busca las cosas que el equipo de LIDR comenta para cambiar y mejorar y crea un resumen sobre las tareas que tendriamos que realizar, por ahora no escribas nada de codigo ni modifiques el sistema, limitate a redactar las tareas que consideres oportunas.

**Prompt 8.2 · Specs para la documentación (T1 a T4):**

> `/opsx:propose` Comprueba que las tareas T1, T2, T3 y T4 no estan implementadas actualmente en el proyecto y en caso de no estarlas crea una spec por tareas no implementarla para realizar estas modificaciones sobre la documentacion del proyecto.

**Prompt 8.3 · Aplicar los cuatro cambios:**

> `/opsx:apply` Realiza la implementacion de los 4 Specs, teniendo en cuenta que ahora tenemos un repositorio Git enlazado a ai4devs-finalproject--JRG-. añade tambien donde sea necesario que tenemos la app desplegada en https://ai4devs-finalproject-jrg.vercel.app/

**Prompt 8.4 ★ · Amistades y seguridad (T6):**

> `/opsx:apply` T6, teniendo en cuenta que vamos a mantener el sistema de amistades, documenta esto para hacer y revisa si realmente existen patrones de seguridad adecuados para proteger la confidencialidad de los usuarios.

**Prompt 8.5 · Pruebas contra la base de datos:**

> Accede a la cuenta de Vercel para aplicar las pruebas sobre la bbDD

El asistente no accedió a Vercel: propuso una rama de Neon, Docker local o producción, y el humano eligió Docker local.

**Prompt 8.6 ★ · Contexto para el agente (T5):**

> Realiza T5 añadiendo a Claude.md las indicaciones de: el stack, las capas, la política de visibilidad, que es única, la regla de que solo el servicio de votos escribe votos. Segun lo que se definio en el proyecto.

**Prompt 8.7 ★ · Casos de test desde los criterios de las historias (T9):**

> `/opsx:apply` Implementa T9, creando los casos de test a partir de los criterios de las historias

**Prompt 8.8 ★ · CI/CD, protección de `main`, APK y Docker (T10):**

> `/opsx:apply` T10 Despliegue, creando y documentando el diagrama CI/CD

> `/opsx:propose` Crea una propuesta para: Proteger main: exigir el CI y hacer que Vercel espere a sus resultados y construir el APK y hacer que la aplicacion tenga dockerfile

Resultado: [`docs/12-ci-cd.md`](docs/12-ci-cd.md) con los diagramas y los huecos, y el cambio `endurecer-cicd-apk-docker` (_ruleset_ de `main`, job `apk` y `Dockerfile` con su job `docker`). Al documentarlo apareció un fallo que nadie había visto: el CI estaba en rojo desde el primer commit (el lock de npm 11 no pasaba `npm ci` con npm 10).

**Prompt 8.9 ★ · Borrar temas propios (HU-23):**

> `/opsx:propose` Realiza una modificación para poder borrar temas publicados. Solo se podrán borrar temas publicados por uno mismo y solo desde la pantalla del detalle del propio tema cuando se este dentro de uno. Para borrarlo se creará un botón con una papelera, guardando siempre el diseñor de la aplicación para la creación de este botón. En los temas que no creo el usuario que los está visualizando, no aparecerá el susodicho botón.

Resultado: el cambio `borrar-temas-propios` (HU-23, TKT-087 a TKT-089), que acaba en la [PR #4](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pull/4), con tests de integración, de componentes, e2e y de accesibilidad. Después, con Docker ya arrancado, el humano pidió: «Ahora que docker arranco continua con el borrado de temas propios».

**Dónde decidió el humano (criterio propio):**

- **Alcance:** qué tareas de la Revisión 1 se hacen y en qué orden. El borrado de temas (8.9) fue una petición propia, acotada a «solo los propios y solo desde el detalle».
- **Producto:** aceptar que la amistad exija solicitud y aceptación (cuestión abierta Q-04), tras el informe de seguridad del prompt 8.4.
- **Entorno de pruebas:** ante la petición de acceder a Vercel (8.5), el asistente propuso alternativas y el humano eligió PostgreSQL local, sin tocar producción.
- **CI/CD (8 de noviembre de 2026):** la excepción del propietario solo se puede usar mediante PR, se protege solo `main` (sin job de despliegue propio), el job `apk` es informativo y no se exige aprobación de otra persona.
- **Verificación del _ruleset_:** el humano comprobó que bloquea el `push` («El ruleset funciona, bloquea el PUSH…») y que para abrir PR había que trabajar desde otra rama; de ahí la rama `JRGA` y las PR del README §7.
- **Publicación:** los commits se hacen en local y el `push` se pide aparte («Push entrega»); el `push` al repositorio del proyecto puede desplegar en producción.

Dónde se equivocó la IA y cómo se detectó (consulta a la capa, `Revision1.txt` subido por error, contador de intentos entre ejecuciones, nombres accesibles): tabla en [`docs/11-uso-de-ia.md`](docs/11-uso-de-ia.md).

**Confirmaciones que aprobaron el trabajo propuesto:** «Sí, aceptar» (solicitud y aceptación de amistad), «Sí, implementar», «Sí, actualizar e2e», «Pasar al grupo 3», «Sí, grupo 4», «Sí, grupo 5», «Sí, sincronizar», «Push entrega», «Sí, accesibilidad» y «Sí, trazar uso de IA».
