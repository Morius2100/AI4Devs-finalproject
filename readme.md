## Índice

0. [Ficha del proyecto](#0-ficha-del-proyecto)
1. [Descripción general del producto](#1-descripción-general-del-producto)
2. [Arquitectura del sistema](#2-arquitectura-del-sistema)
3. [Modelo de datos](#3-modelo-de-datos)
4. [Especificación de la API](#4-especificación-de-la-api)
5. [Historias de usuario](#5-historias-de-usuario)
6. [Tickets de trabajo](#6-tickets-de-trabajo)
7. [Pull requests](#7-pull-requests)

---

## 0. Ficha del proyecto

### **0.1. Tu nombre completo:**

Jorge Rodríguez García

### **0.2. Nombre del proyecto:**

Opina

### **0.3. Descripción breve del proyecto:**

Opina es una aplicación web _mobile-first_ y red social basada en **temas sobre los que los usuarios votan**. Cada tema se vota con un pulgar arriba (verde) o un pulgar abajo (rojo), acompañado opcionalmente de una justificación. Tras votar, el usuario ve el total de votos positivos y negativos y las opiniones del resto de usuarios, coloreadas según su voto. Funciona como un foro, pero más simple y dinámico. Los usuarios pueden crear temas **públicos** (para toda la comunidad) o **privados** (solo para su lista de amigos, que recibe una notificación).

### **0.4. URL del proyecto:**

La aplicación está desplegada en Vercel y se puede probar en <https://ai4devs-finalproject-jrg.vercel.app/>. Para entrar, regístrate con un usuario de prueba propio desde la pantalla de acceso (no hace falta ninguna cuenta precargada; ver [0.6](#06-credenciales-de-prueba-y-pasos-de-verificación)). Comprobación rápida de que el servidor responde: <https://ai4devs-finalproject-jrg.vercel.app/api/health> devuelve `{"status":"ok"}`.

### 0.5. URL o archivo comprimido del repositorio

- **Repositorio de entrega (este):** [Morius2100/AI4Devs-finalproject](https://github.com/Morius2100/AI4Devs-finalproject/tree/feature/entrega-1-JRG), rama `feature/entrega-1-JRG`. Contiene este `readme.md`, el registro de prompts ([`prompts.md`](prompts.md)) y una copia de la documentación y del plan SDD.
- **Repositorio del proyecto (código completo):** [Morius2100/ai4devs-finalproject--JRG-](https://github.com/Morius2100/ai4devs-finalproject--JRG-). Contiene el código de la aplicación, las pruebas, la integración continua y la fuente de verdad de la documentación.

El proyecto se ha documentado siguiendo **Spec Driven Development** con [OpenSpec](https://github.com/Fission-AI/OpenSpec). La documentación completa está en [`docs/`](docs/README.md) y el plan ejecutable en [`openspec/changes/`](openspec/changes/), ambas en este mismo repositorio. La fuente de verdad es el [repositorio del proyecto](https://github.com/Morius2100/ai4devs-finalproject--JRG-).

**CI/CD:** el diagrama de la canalización (GitHub Actions, Vercel, Neon y R2), los entornos y los huecos pendientes están en [`docs/12-ci-cd.md`](docs/12-ci-cd.md).

**Uso de IA en el ciclo de desarrollo:** análisis, diseño, implementación, pruebas, despliegue y revisión de seguridad, con sus evidencias, los prompts reales y los errores corregidos, en [`docs/11-uso-de-ia.md`](docs/11-uso-de-ia.md) y [`prompts.md`](prompts.md). El contexto fijo del agente está en [`CLAUDE.md`](CLAUDE.md).

### 0.6. Credenciales de prueba y pasos de verificación

**Credenciales de prueba.** Solo se usan datos de prueba y el repositorio no contiene secretos reales: `.env*` está en `.gitignore`, solo se versiona `.env.example` con valores de relleno y el CI y el `Dockerfile` usan secretos ficticios.

| Entorno | Cómo acceder |
| --- | --- |
| **Despliegue** ([Vercel](https://ai4devs-finalproject-jrg.vercel.app/)) | Crea un usuario de prueba propio en `/registro` (nombre de usuario, email y contraseña de 8 a 72 caracteres). No se publican credenciales del despliegue, y los usuarios del _seed_ no se cargan en producción: el build de Vercel lo bloquea sin confirmación explícita y exige `SEED_PASSWORD`. Para probar amistades y temas privados crea dos usuarios (por ejemplo, uno en una ventana privada). |
| **Local**, tras `npm run prisma:seed` | `ana@opina.test`, `bruno@opina.test`, `carla@opina.test` y `dani@opina.test`, todos con la contraseña `password123`. Amistades `ana↔bruno` y `ana↔carla`; `dani` no es amigo de nadie. |
| **Servicios de Docker (solo local)** | PostgreSQL `opina` / `opina`; MinIO `opina` / `opina123` (consola en `http://localhost:9001`). Valores de desarrollo que no se usan fuera de tu máquina. |

**Pasos de verificación en el despliegue** (unos 10 minutos, con dos usuarios de prueba):

1. Abre `/api/health`: debe responder `{"status":"ok"}`.
2. Regístrate (HU-00): tras el registro entras en la pantalla principal. Cierra sesión desde el menú y vuelve a entrar con el email y la contraseña.
3. Pantalla principal (HU-01, HU-02): se ve la lista de tendencias y el buscador filtra por título; al vaciarlo vuelven las tendencias.
4. Crea un tema público con imagen (HU-08, HU-09): desde el menú, con un título y una imagen JPEG, PNG o WebP de hasta 4 MB. Encuéntralo con el buscador.
5. Vota con el segundo usuario (HU-04 a HU-07): abre el tema, pulsa 👍 o 👎 y escribe una justificación. Se muestran los totales y tu opinión en verde (👍) o rojo (👎). Al volver a entrar ya no hay botones: el voto es único.
6. Tema privado y notificación (HU-10 a HU-12): haz amigos a los dos usuarios (solicitud y aceptación en la pantalla de personas), crea un tema privado con el primero y comprueba que el segundo recibe la notificación y puede votarlo. Un tercer usuario que abra la misma URL ve «tema no encontrado».

**Verificación en local:** `npm run lint && npm run typecheck && npm test && npm run test:e2e` (ver §1.4, paso 6, y §2.6).

**Evidencia de funcionamiento:** aplicación desplegada ([URL](https://ai4devs-finalproject-jrg.vercel.app/)), código y documentación ([repositorio del proyecto](https://github.com/Morius2100/ai4devs-finalproject--JRG-)), ejecuciones del CI en [GitHub Actions](https://github.com/Morius2100/ai4devs-finalproject--JRG-/actions) y la APK de depuración que construye el job `apk` como artefacto.

---

## 1. Descripción general del producto

### **1.1. Objetivo:**

**Propósito.** Dar a los usuarios una forma rápida de opinar sobre temas y conocer la opinión de los demás, sin la carga de un foro tradicional. No hay hilos ni respuestas: cada tema se resuelve con un voto binario y, si el usuario quiere, una justificación breve.

**Qué soluciona.** En los foros y redes sociales, opinar sobre algo exige leer y escribir hilos largos, y saber qué piensa la mayoría es difícil. Opina reduce la interacción a un gesto (👍/👎) y resume el resultado al instante: los totales y los argumentos a favor (verde) y en contra (rojo).

**Para quién.**

- Usuarios que quieren descubrir temas en tendencia y posicionarse sobre ellos.
- Usuarios que quieren plantear una pregunta a toda la comunidad (tema público).
- Usuarios que quieren consultar solo a su círculo de amigos (tema privado con notificación).

### **1.2. Características y funcionalidades principales:**

| #     | Funcionalidad                           | Descripción                                                                                                                                                                                                                                         |
| ----- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| F-01  | **Pantalla principal**                  | Lista scrolleable de temas con buscador en la parte superior. Sin búsqueda activa se muestran las mayores tendencias (temas públicos ordenados por número total de votos). Al escribir se filtran los temas públicos cuyo título contiene el texto. |
| F-02  | **Acceso a un tema**                    | Al pulsar un tema se muestra su título. Si tiene imagen, esta ocupa la mitad superior de la pantalla del móvil. En la parte inferior aparecen dos botones: pulgar arriba verde y pulgar abajo rojo.                                                 |
| F-03  | **Votación con justificación opcional** | Al pulsar un botón se pide una justificación que se puede dejar vacía; al confirmar se emite el voto (uno por usuario y tema).                                                                                                                      |
| F-03b | **Resultados y opiniones**              | Tras votar, los dos botones se transforman en el total de votos positivos y negativos y en la lista de opiniones: **verde** si el autor votó positivo y **rojo** si votó negativo.                                                                  |
| F-04  | **Creación de temas**                   | Desde el menú principal: título, imagen opcional y visibilidad (pública o privada).                                                                                                                                                                 |
| F-05  | **Temas públicos**                      | Accesibles a todos los usuarios desde la lista de tendencias y la búsqueda.                                                                                                                                                                         |
| F-06  | **Temas privados y notificaciones**     | Solo la lista de amigos del creador puede acceder a la votación. Los amigos reciben una notificación de que un amigo ha creado una nueva propuesta.                                                                                                 |

**Funcionalidades must-have operativas (5):**

1. **F-01** Lista de tendencias y buscador (HU-01, HU-02). F-05 (temas públicos) se cubre aquí y en F-04.
2. **F-02 y F-03** Acceso a un tema y votación con justificación opcional (HU-03 a HU-05).
3. **F-03b** Resultados y opiniones coloreadas (HU-06, HU-07).
4. **F-04** Creación de temas con imagen opcional (HU-08 a HU-10).
5. **F-06** Temas privados con notificación a los amigos (HU-11, HU-12).

Cada una tiene sus tests: `tests/e2e/descubrimiento-temas.spec.ts`, `votacion.spec.ts`, `creacion-temas.spec.ts` y `temas-privados.spec.ts`, además de las pruebas unitarias y de integración (§2.6). Más allá de este alcance inicial se implementaron, siempre con su historia y su spec: personas y amistades con consentimiento, historias de amigos, perfil, borrar un tema propio (HU-23, en la PR #4, pendiente de fusionar) y la app Android.

**Supuestos habilitantes** (mínimos imprescindibles que el enunciado no detalla):

- **Identificación de usuarios:** registro e inicio de sesión, porque los votos, los temas y los amigos son de un usuario concreto.
- **Relación de amistad:** modelada en el sistema y **con consentimiento**: se solicita desde el perfil de la persona y la amistad, mutua, solo existe cuando la otra persona la acepta (una solicitud pendiente no da acceso a temas privados). Quitarla es directo. Se decidió así porque, sin consentimiento, cualquiera podía hacerse amigo de otro y leer sus temas privados ([revisión de seguridad](docs/09-revision-seguridad.md), hallazgo S-01).
- **Definición de "tendencia":** número total de votos, en orden descendente.
- **Definición de "opinión":** un voto con justificación.
- **Visibilidad en listados:** los temas privados no aparecen en la lista ni en la búsqueda.

### **1.3. Diseño y experiencia de usuario:**

La aplicación está implementada y desplegada (§0.4). No se incluyen capturas ni vídeo: la forma de verla funcionando es la [URL desplegada](https://ai4devs-finalproject-jrg.vercel.app/) siguiendo los pasos de verificación de §0.6. Los diseños de las pantallas de personas y de perfil están en [`docs/mockups/`](docs/mockups/).

Recorrido de la experiencia de usuario:

```mermaid
flowchart TD
    A[Entrar en Opina] --> L{¿Tiene sesión?}
    L -->|No| R[Login / Registro]
    R --> B
    L -->|Sí| B[Pantalla principal:<br/>buscador + lista de tendencias]
    B -->|Escribe en el buscador| C[Resultados de búsqueda]
    C --> D
    B -->|Pulsa un tema| D[Pantalla del tema:<br/>imagen 50% superior + título + 👍 / 👎]
    D -->|Pulsa 👍 o 👎| E[Diálogo de justificación opcional]
    E -->|Cancelar| D
    E -->|Votar| G[Totales 👍 / 👎 +<br/>opiniones verdes y rojas]
    B -->|Menú → Crear tema| H[Título, imagen opcional,<br/>Público / Privado]
    H -->|Público| D
    H -->|Privado| I[Notificación a los amigos]
    B -->|Menú → Notificaciones| N[Lista de notificaciones]
    N -->|Pulsa una| D
```

Esquema de la pantalla de un tema antes y después de votar:

```
┌──────────────────────┐        ┌──────────────────────┐
│                      │        │                      │
│   IMAGEN DEL TEMA    │        │   IMAGEN DEL TEMA    │
│   (50% de la altura) │        │   (50% de la altura) │
│                      │        │                      │
├──────────────────────┤        ├──────────────────────┤
│ ¿Pizza con piña?     │        │ ¿Pizza con piña?     │
│                      │        │ 👍 124     👎 87     │
│                      │  ───►  │ ┌──────────────────┐ │
│                      │ votar  │ │ ana: ¡Me encanta!│ │ ← verde
│                      │        │ └──────────────────┘ │
│┌─────────┐┌─────────┐│        │ ┌──────────────────┐ │
││   👍    ││   👎    ││        │ │ bruno: Jamás     │ │ ← rojo
││ (verde) ││ (rojo)  ││        │ └──────────────────┘ │
│└─────────┘└─────────┘│        │          …           │
└──────────────────────┘        └──────────────────────┘
```

### **1.4. Instrucciones de instalación:**

> Instrucciones según el stack y la estructura definidos en la documentación técnica ([`docs/06-stack.md`](docs/06-stack.md), [`docs/02-arquitectura.md`](docs/02-arquitectura.md)). Si solo quieres probar la aplicación, no hace falta instalar nada: está desplegada en <https://ai4devs-finalproject-jrg.vercel.app/>. Para el despliegue propio, consulta [`docs/08-despliegue-vercel.md`](docs/08-despliegue-vercel.md).

**Requisitos previos:** Node.js 22 LTS, npm, Docker y Docker Compose, Git.

1. **Clonar el repositorio e instalar dependencias**
   ```bash
   git clone https://github.com/Morius2100/ai4devs-finalproject--JRG-.git opina
   cd opina
   npm install
   ```
2. **Configurar variables de entorno:** copiar `.env.example` a `.env` y revisar los valores.
   ```bash
   cp .env.example .env
   ```
   | Variable                                    | Ejemplo local                                   |
   | ------------------------------------------- | ----------------------------------------------- |
   | `DATABASE_URL`                              | `postgresql://opina:opina@localhost:5433/opina?schema=public` |
   | `AUTH_SECRET`                               | cadena aleatoria (`npx auth secret`)            |
   | `S3_ENDPOINT`                               | `http://localhost:9000`                         |
   | `S3_REGION`                                 | `us-east-1`                                     |
   | `S3_BUCKET`                                 | `opina-images`                                  |
   | `S3_ACCESS_KEY_ID` / `S3_SECRET_ACCESS_KEY` | `opina` / `opina123`                     |
   | `S3_PUBLIC_URL`                             | `http://localhost:9000/opina-images`            |
3. **Levantar la infraestructura** (PostgreSQL 16 + MinIO con el bucket `opina-images`):
   ```bash
   docker compose up -d
   ```
4. **Aplicar migraciones y cargar datos semilla** (usuarios `ana`, `bruno`, `carla` y `dani`, con email `<nombre>@opina.test` y contraseña `password123`; amistades `ana↔bruno` y `ana↔carla`; 25 temas públicos y un tema privado):
   ```bash
   npm run prisma:migrate
   npm run prisma:seed
   ```
5. **Arrancar la aplicación** en `http://localhost:3000`:
   ```bash
   npm run dev
   ```
6. **Ejecutar las pruebas.** Las de integración usan la base `opina_test` (la crea Docker): copia `.env.example` a `.env.test`, cambia `opina` por `opina_test` en `DATABASE_URL` y activa `IMAGE_STORAGE="memory"`; después aplica las migraciones y el _seed_ a esa base. Los e2e usan `.env` y construyen y arrancan la aplicación solos.
   ```bash
   DATABASE_URL="postgresql://opina:opina@localhost:5433/opina_test?schema=public" npx prisma migrate deploy
   DATABASE_URL="postgresql://opina:opina@localhost:5433/opina_test?schema=public" npm run prisma:seed
   npm run lint && npm run typecheck && npm test
   npm run test:e2e
   ```

---

## 2. Arquitectura del Sistema

### **2.1. Diagrama de arquitectura:**

```mermaid
flowchart LR
    subgraph Cliente["Navegador (móvil / escritorio)"]
        UI["UI React 19<br/>Server + Client Components<br/>Tailwind · TanStack Query"]
    end

    subgraph App["Aplicación Next.js 15 (Node.js 22)"]
        direction TB
        MW["Middleware<br/>(protección de rutas · Auth.js)"]
        PG["Páginas App Router<br/>/, /temas/[id], /temas/nuevo,<br/>/notificaciones, /login, /registro"]
        API["Route Handlers /api/*<br/>(validación Zod)"]
        SVC["Servicios de dominio<br/>Auth · Topic · Vote · VisibilityPolicy ·<br/>Friendship · Notification · ImageStorage"]
        DAL["Prisma Client"]
        MW --> PG
        MW --> API
        PG --> SVC
        API --> SVC
        SVC --> DAL
    end

    DB[("PostgreSQL 16<br/>+ pg_trgm")]
    S3[("Almacenamiento S3<br/>(MinIO en local)")]

    UI -- "HTTPS (HTML / JSON)" --> MW
    DAL --> DB
    SVC -- "PutObject" --> S3
    UI -- "GET imagen" --> S3
```

**Patrón:** **monolito modular por capas** (presentación → API → servicios de dominio → acceso a datos) sobre Next.js full-stack.

**Por qué esta arquitectura.** El dominio es pequeño (usuarios, temas, votos, amistades y notificaciones) y lo lleva un único equipo, sin necesidad de escalar partes por separado. Un monolito full-stack permite que cada incremento del plan SDD recorra todas las capas en un solo cambio, con un único lenguaje (TypeScript) y un único despliegue.

**Beneficios:**

- **Simplicidad operativa:** una aplicación, una base de datos y un bucket.
- **Tipos de extremo a extremo:** Prisma genera los tipos y Zod valida en cliente y servidor con el mismo esquema.
- **Carga inicial rápida en móvil:** la primera página se renderiza en el servidor.
- **Una sola regla de acceso:** `VisibilityPolicy` es el único punto que decide quién puede ver un tema, y la usan el detalle, el voto y las opiniones.
- **Integridad garantizada por la base de datos:** restricción de voto único y transacciones.

**Sacrificios / déficits:**

- La API y la UI se escalan juntas; habría que separarlas si la carga creciera mucho.
- Queda acoplado al ecosistema de Next.js (Route Handlers, Middleware).
- Los contadores de votos desnormalizados añaden una regla: solo `VoteService` puede escribir votos.
- Las notificaciones se crean de forma síncrona en la transacción de creación del tema, lo que es adecuado para listas de amigos acotadas pero no para volúmenes masivos.

### **2.2. Descripción de componentes principales:**

| Componente                           | Tecnología                                                        | Responsabilidad                                                                                                                                                                                                                             |
| ------------------------------------ | ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Frontend (páginas y componentes)** | Next.js 15 App Router, React 19, Tailwind CSS 4, TanStack Query 5 | `HomePage` (`SearchBar` + `TopicList` con scroll infinito), `TopicPage` (`TopicImage` a `50dvh`, `VoteButtons`, `JustificationDialog`, `VoteResults`, `OpinionList`), `CreateTopicPage`, `NotificationsPage`, `MainMenu`, login y registro. |
| **Middleware**                       | Next.js Middleware + Auth.js                                      | Redirige a `/login` las páginas sin sesión y responde `401` a las llamadas `/api/*` sin sesión.                                                                                                                                             |
| **API**                              | Route Handlers + Zod                                              | Contrato HTTP: valida las entradas, delega en los servicios y convierte los errores de dominio en códigos HTTP con el formato `{ error: { code, message } }`.                                                                               |
| **AuthService**                      | Auth.js v5 (Credentials, JWT), bcryptjs                           | Registro, verificación de credenciales y usuario de la sesión.                                                                                                                                                                              |
| **TopicService**                     | TypeScript + Prisma                                               | Tendencias y búsqueda con paginación por cursor (keyset), detalle de un tema y creación de temas.                                                                                                                                           |
| **VisibilityPolicy**                 | TypeScript                                                        | Decide el acceso: un tema público es accesible para todos; uno privado, para su creador y los amigos del creador.                                                                                                                           |
| **VoteService**                      | Prisma (transacciones)                                            | Voto único, contadores coherentes y listado de opiniones.                                                                                                                                                                                   |
| **FriendshipService**                | Prisma (transacciones)                                            | Lista de amigos y solicitudes de amistad: solicitar, aceptar, rechazar, cancelar y quitar. Solo aceptar crea la amistad.                                                                                                                                                                                              |
| **NotificationService**              | Prisma                                                            | Crea una notificación por amigo al crear un tema privado y lista las notificaciones de cada usuario.                                                                                                                                        |
| **ImageStorage**                     | `@aws-sdk/client-s3`, `file-type`                                 | Valida y sube las imágenes (JPEG/PNG/WebP, máximo 4 MB) a un almacenamiento S3-compatible.                                                                                                                                                  |
| **Base de datos**                    | PostgreSQL 16 + `pg_trgm`, Prisma 6                               | Persistencia, restricciones de integridad y búsqueda por título.                                                                                                                                                                            |

### **2.3. Descripción de alto nivel del proyecto y estructura de ficheros**

```
opina/
├── docs/                         # Documentación técnica (producto, arquitectura, datos, HU, tickets, stack, SDD)
├── openspec/                     # Spec Driven Development
│   ├── config.yaml               # Contexto y reglas para generar artefactos
│   ├── specs/                    # Specs consolidadas (al archivar cada cambio)
│   └── changes/                  # 6 cambios: proposal · design · specs · tasks
│       ├── fase-1-plataforma-base/
│       ├── fase-2-listado-busqueda-temas/
│       ├── fase-3-detalle-tema/
│       ├── fase-4-votacion-opiniones/
│       ├── fase-5-creacion-temas/
│       └── fase-6-temas-privados-notificaciones/
├── prisma/
│   ├── schema.prisma             # Modelo de datos
│   ├── migrations/               # Migraciones versionadas (+ SQL de pg_trgm y CHECKs)
│   └── seed.ts                   # Datos de desarrollo
├── src/
│   ├── app/                      # Capa de presentación y API (App Router)
│   │   ├── (auth)/login, (auth)/registro
│   │   ├── page.tsx              # Pantalla principal
│   │   ├── temas/[id]/, temas/nuevo/, notificaciones/
│   │   └── api/                  # Route Handlers: register, auth, topics, votes, opinions, notifications
│   ├── components/               # Componentes de UI reutilizables
│   ├── lib/validation/           # Esquemas Zod compartidos cliente/servidor
│   ├── server/
│   │   ├── auth/                 # Configuración de Auth.js
│   │   ├── db/                   # Prisma Client
│   │   ├── services/             # Lógica de dominio
│   │   ├── storage/              # ImageStorage (S3 / en memoria para tests)
│   │   └── errors.ts             # Errores de dominio
│   └── middleware.ts
├── tests/{unit,integration,e2e}/
├── docker-compose.yml            # PostgreSQL + MinIO
└── .github/workflows/ci.yml
```

La estructura sigue el patrón de **capas** del apartado 2.1: [`src/app`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/src/app) es la presentación y la API, [`src/server/services`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/src/server/services) el dominio y [`src/server/db`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/src/server/db) el acceso a datos. El código de servidor vive en [`src/server`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/src/server) para que nunca se importe desde los componentes cliente. [`openspec/`](openspec/) guarda las especificaciones que dirigen la implementación.

**Enlaces a la estructura.** En este repositorio: [`docs/`](docs/README.md) y [`openspec/`](openspec/). En el [repositorio del proyecto](https://github.com/Morius2100/ai4devs-finalproject--JRG-): [`prisma/`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/prisma), [`src/`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/src), [`tests/`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/tests), [`docker-compose.yml`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/blob/main/docker-compose.yml) y [`.github/workflows/`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/.github/workflows).

### **2.4. Infraestructura y despliegue**

```mermaid
flowchart LR
    Dev["Desarrollador<br/>docker compose (PostgreSQL + MinIO)<br/>next dev"] -->|push / PR| GH["GitHub"]
    GH --> CI["GitHub Actions<br/>install → prisma migrate deploy →<br/>lint → typecheck → test → build → e2e"]
    CI -->|PR con ci en verde → merge a main| CD["Vercel<br/>npm run vercel-build<br/>(migraciones + next build)"]
    CD --> Prod

    subgraph Prod["Producción"]
        APP["Next.js en Vercel<br/>ai4devs-finalproject-jrg.vercel.app"]
        PDB[("PostgreSQL en Neon")]
        BKT[("Cloudflare R2")]
        APP --> PDB
        APP --> BKT
    end
```

**Proceso de despliegue:**

1. Los cambios se desarrollan en una rama (hoy `JRGA`) y se abre una Pull Request hacia `main`. Hasta el 9 de noviembre de 2026, las fases 1 a 6 y las mejoras posteriores se publicaron con `push` directo a `main`.
2. El CI (GitHub Actions) ejecuta en el job `ci` migraciones, _seed_, lint, typecheck, Vitest (unitarias e integración, con un servicio PostgreSQL), build y Playwright con axe-core. Los jobs `apk` (APK de depuración como artefacto) y `docker` (imagen y `/api/health`) son informativos. El _ruleset_ de `main` (`.github/rulesets/main.json`) exige PR y el check `ci`; el propietario comprobó que bloquea el `push` directo.
3. Al fusionar en `main`, Vercel despliega producción con `npm run vercel-build`: `prisma generate`, `prisma migrate deploy` (solo en producción) y `next build`. Las ramas y las PR reciben una vista previa sin migrar. Vercel no espera de forma explícita al CI: es un hueco conocido ([`docs/12-ci-cd.md`](docs/12-ci-cd.md) §12.5).
4. Las migraciones usan la conexión directa de Neon. El _seed_ de prueba no se carga en producción sin confirmación explícita (`scripts/seed-guard.mjs`).
5. La configuración se inyecta por variables de entorno (`DATABASE_URL`, `AUTH_SECRET`, `S3_*`), validadas con Zod al arrancar.
6. **Rollback:** en Vercel, _Promote to Production_ sobre un despliegue anterior. Las migraciones son aditivas, así que son compatibles con la versión previa.

La aplicación también se puede ejecutar en un contenedor (`Dockerfile` y `docker compose --profile app up -d --build`) y el CI genera la APK Android. Los entornos, los diagramas completos y los huecos pendientes están en [`docs/12-ci-cd.md`](docs/12-ci-cd.md).

### **2.5. Seguridad**

| Práctica                      | Detalle                                                                                                                                                                                                      |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Contraseñas con hash**      | bcrypt con coste 12. El `passwordHash` nunca se incluye en ninguna respuesta de la API.                                                                                                                      |
| **Sesión segura**             | JWT de Auth.js en una cookie `httpOnly`, `secure` y `sameSite=lax`, que protege frente a robo por XSS y la mayoría de ataques CSRF.                                                                          |
| **Autenticación obligatoria** | El middleware protege todas las rutas salvo `/login`, `/registro`, `/api/register` y `/api/auth/*`.                                                                                                          |
| **Mensaje de login genérico** | Siempre "Credenciales incorrectas", con el mismo tiempo de respuesta exista o no el email (se compara siempre con un hash de bcrypt).                                                                       |
| **Límite de intentos**        | Login web y de la app (10 por minuto por IP y email, 60 por IP), registro (20 por hora por IP) y solicitudes de amistad (30 por hora por usuario). Responde `429` con `Retry-After`. Contadores en PostgreSQL. |
| **Amistad con consentimiento** | Las solicitudes de amistad no dan acceso a temas privados hasta que la otra persona las acepta.                                                                                                              |
| **Imágenes privadas firmadas** | Las imágenes de temas privados se sirven con una URL firmada (HMAC) que caduca a los 30 minutos; sin firma válida `/api/media` responde `404`.                                                              |
| **Cabeceras HTTP**            | CSP (con `frame-ancestors 'none'`), `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy` y HSTS.                                                                                                |
| **Seed fuera de producción**  | El build de Vercel falla si se intenta cargar el seed de prueba en producción sin confirmarlo, y el seed exige una contraseña propia fuera de local.                                                          |
| **Contraseña acotada**        | Entre 8 y 72 bytes (límite real de bcrypt).                                                                                                                                                                  |
| **Autorización centralizada** | `VisibilityPolicy` decide el acceso en detalle, voto y opiniones. Un tema privado inaccesible responde **`404`** (no `403`) para no revelar que existe.                                                      |
| **Validación de entradas**    | Zod en todos los endpoints (títulos de 3 a 120 caracteres, justificaciones de hasta 500, enumerados cerrados, cursores validados).                                                                           |
| **Consultas parametrizadas**  | Prisma parametriza todas las consultas. En la búsqueda con `ILIKE` se escapan `%`, `_` y `\`.                                                                                                                |
| **Subida de imágenes segura** | Tamaño máximo de 4 MB (`413`; la petición completa debe caber en los 4,5 MB que admite una función de Vercel), tipo comprobado por _magic bytes_ (JPEG/PNG/WebP, `415`) y no solo por la extensión, y nombre de objeto aleatorio (`topics/<uuid>.<ext>`) que no depende del nombre original. |
| **Integridad de datos**       | `UNIQUE (topic_id, user_id)` impide votar dos veces incluso con peticiones concurrentes, y `CHECK` garantiza la coherencia de los contadores.                                                                |
| **Secretos fuera del código** | Variables de entorno validadas al arrancar; `.env` excluido del control de versiones.                                                                                                                        |
| **XSS**                       | React escapa todo el contenido generado por usuarios (títulos, justificaciones) y nunca se usa `dangerouslySetInnerHTML`.                                                                                    |

Estado de cada hallazgo y riesgos aceptados (sesiones de 30 días sin revocación): [`docs/09-revision-seguridad.md`](docs/09-revision-seguridad.md).

Ejemplo: un usuario que no es amigo intenta votar un tema privado.

```http
POST /api/topics/7f3c…/votes
{ "value": "UP" }

HTTP/1.1 404 Not Found
{ "error": { "code": "NOT_FOUND", "message": "Tema no encontrado" } }
```

### **2.6. Tests (pruebas)**

Estrategia: **cada escenario `#### Scenario` de las specs de OpenSpec tiene al menos un test** cuyo nombre lo cita. Hay tres niveles:

| Nivel           | Herramienta                   | Ejemplos                                                                                                                                                                                                                                                                                                                                                                                                               |
| --------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Unitarios**   | Vitest                        | `VisibilityPolicy` (público, privado propio, privado de un amigo, privado ajeno); codificación de cursores; validación de imágenes (PNG válido, 6 MB → `413`, PDF renombrado a `.jpg` → `415`).                                                                                                                                                                                                                        |
| **Integración** | Vitest + PostgreSQL de prueba | _Votos concurrentes del mismo usuario_: dos `castVote` simultáneos → solo uno se registra y `totalVotes` sube exactamente en 1. _Orden por tendencia_: temas con 10, 25 y 10 votos se devuelven como B, C, A. _Tema privado no listado_: no aparece en `GET /api/topics` aunque sea el más votado. _Crear un tema privado_: con 2 amigos se generan 2 notificaciones y, si falla una, no se crea el tema (atomicidad). |
| **End-to-end**  | Playwright (viewport 390×844) | Votar con justificación → los botones desaparecen y se muestra la opinión en verde. Comprobar que la imagen del tema mide exactamente la mitad de la altura. Flujo privado: `ana` crea un tema privado → `bruno` ve la notificación, entra y vota → `dani` recibe "tema no encontrado" en la misma URL.                                                                                                                |

**Cómo ejecutarlas** (preparación en §1.4, paso 6):

| Comando | Qué ejecuta |
| --- | --- |
| `npm run lint` · `npm run typecheck` | ESLint y `tsc --noEmit` |
| `npm test` | Vitest: unitarias (`tests/unit`) e integración (`tests/integration`, con PostgreSQL `opina_test`) |
| `npm run test:e2e` | Playwright (`tests/e2e`): flujos completos en Chromium y accesibilidad con axe-core |

**Trazabilidad:** cada criterio de aceptación (`CA-xx.y`) de las historias tiene un test cuyo título lo cita, o consta como no automatizable, con su motivo, en [`docs/10-trazabilidad-criterios.md`](docs/10-trazabilidad-criterios.md). Por ejemplo, `grep -rn "CA-07.3" tests` encuentra el test de ese criterio.

**Estado (11 de noviembre de 2026):**

- **Vitest:** 564 pruebas unitarias y de integración en verde en local el 9 de noviembre, ejecutadas dos veces seguidas (tarea T9). Desde entonces se añadieron las del borrado de temas (HU-23), que no entran en esa cifra.
- **Accesibilidad:** 16 situaciones con axe-core sin infracciones el 9 de noviembre (más el detalle del autor con el diálogo de borrado, en la PR #4).
- **CI en GitHub:** [Actions](https://github.com/Morius2100/ai4devs-finalproject--JRG-/actions). En la ejecución del 11 de noviembre pasan lint, typecheck, Vitest, build, `apk` y `docker`, pero el job `ci` falla en el paso «Instalar navegadores de Playwright» y los e2e no llegan a ejecutarse. Está pendiente de resolver.

---

## 3. Modelo de Datos

### **3.1. Diagrama del modelo de datos:**

```mermaid
erDiagram
    USER ||--o{ TOPIC : "crea"
    USER ||--o{ VOTE : "emite"
    TOPIC ||--o{ VOTE : "recibe"
    USER ||--o{ FRIENDSHIP : "tiene amigo (user_id)"
    USER ||--o{ FRIENDSHIP : "es amigo de (friend_id)"
    USER ||--o{ NOTIFICATION : "recibe (recipient_id)"
    USER ||--o{ NOTIFICATION : "provoca (actor_id)"
    TOPIC ||--o{ NOTIFICATION : "referencia"

    USER {
        uuid id PK
        varchar(30) username UK "NOT NULL, [a-z0-9_]{3,30}"
        varchar(254) email UK "NOT NULL, minúsculas"
        text password_hash "NOT NULL, bcrypt"
        timestamptz created_at "NOT NULL, default now()"
    }
    FRIENDSHIP {
        uuid user_id PK,FK "→ users.id, ON DELETE CASCADE"
        uuid friend_id PK,FK "→ users.id, ON DELETE CASCADE"
        timestamptz created_at "NOT NULL, default now()"
    }
    TOPIC {
        uuid id PK
        varchar(120) title "NOT NULL, 3-120"
        text image_url "NULL"
        enum visibility "NOT NULL, PUBLIC | PRIVATE"
        uuid author_id FK "→ users.id, NOT NULL"
        int positive_votes "NOT NULL, default 0, >= 0"
        int negative_votes "NOT NULL, default 0, >= 0"
        int total_votes "NOT NULL, default 0 = positive + negative"
        timestamptz created_at "NOT NULL, default now()"
    }
    VOTE {
        uuid id PK
        uuid topic_id FK "→ topics.id, NOT NULL, UK(topic_id,user_id)"
        uuid user_id FK "→ users.id, NOT NULL, UK(topic_id,user_id)"
        enum value "NOT NULL, UP | DOWN"
        varchar(500) justification "NULL"
        timestamptz created_at "NOT NULL, default now()"
    }
    NOTIFICATION {
        uuid id PK
        uuid recipient_id FK "→ users.id, NOT NULL"
        uuid actor_id FK "→ users.id, NOT NULL"
        uuid topic_id FK "→ topics.id, NOT NULL"
        enum type "NOT NULL, PRIVATE_TOPIC_CREATED"
        timestamptz created_at "NOT NULL, default now()"
    }
```

### **3.2. Descripción de entidades principales:**

#### `User` (tabla `users`): usuario de la aplicación

| Atributo        | Tipo         | Restricciones                       | Descripción                                                         |
| --------------- | ------------ | ----------------------------------- | ------------------------------------------------------------------- |
| `id`            | UUID         | PK                                  | Identificador.                                                      |
| `username`      | varchar(30)  | NOT NULL, UNIQUE, `[a-z0-9_]{3,30}` | Nombre que se muestra junto a cada opinión y en las notificaciones. |
| `email`         | varchar(254) | NOT NULL, UNIQUE                    | Normalizado a minúsculas; se usa para iniciar sesión.               |
| `password_hash` | text         | NOT NULL                            | Hash bcrypt (coste 12). Nunca se expone.                            |
| `created_at`    | timestamptz  | NOT NULL, `now()`                   | Fecha de alta.                                                      |

**Relaciones:** 1:N con `Topic` (autor), 1:N con `Vote`, N:M consigo mismo a través de `Friendship`, 1:N con `Notification` como destinatario y como actor.

#### `Friendship` (tabla `friendships`): lista de amigos

| Atributo     | Tipo        | Restricciones                                           | Descripción                |
| ------------ | ----------- | ------------------------------------------------------- | -------------------------- |
| `user_id`    | UUID        | PK compuesta, FK → `users.id` ON DELETE CASCADE         | Usuario dueño de la lista. |
| `friend_id`  | UUID        | PK compuesta, FK → `users.id` ON DELETE CASCADE, índice | Amigo.                     |
| `created_at` | timestamptz | NOT NULL                                                | Fecha de la relación.      |

**Reglas:** la relación es simétrica y se guarda en dos filas (A→B y B→A), de modo que "amigos de X" es una consulta directa. `CHECK (user_id <> friend_id)`. Solo contiene amistades **aceptadas**.

#### `FriendRequest` (tabla `friend_requests`): solicitud de amistad pendiente

| Atributo       | Tipo        | Restricciones                                           | Descripción                  |
| -------------- | ----------- | ------------------------------------------------------- | ---------------------------- |
| `requester_id` | UUID        | PK compuesta, FK → `users.id` ON DELETE CASCADE         | Quien envía la solicitud.    |
| `addressee_id` | UUID        | PK compuesta, FK → `users.id` ON DELETE CASCADE, índice | Quien la recibe.             |
| `created_at`   | timestamptz | NOT NULL                                                | Fecha de la solicitud.       |

**Reglas:** `CHECK (requester_id <> addressee_id)`. No concede ningún acceso. Al aceptarla se borra y se insertan las dos filas de `friendships` en una transacción. Además existe `rate_limits` (contadores del límite de intentos, sin relaciones ni datos personales: el email va como hash).

#### `Topic` (tabla `topics`): tema o propuesta que se vota

| Atributo         | Tipo              | Restricciones                                       | Descripción                                                           |
| ---------------- | ----------------- | --------------------------------------------------- | --------------------------------------------------------------------- |
| `id`             | UUID              | PK                                                  | Identificador.                                                        |
| `title`          | varchar(120)      | NOT NULL, 3–120 caracteres                          | Título del tema.                                                      |
| `image_url`      | text              | NULL                                                | URL pública de la imagen en S3. Si es nula, no se muestra imagen.     |
| `visibility`     | enum `Visibility` | NOT NULL (`PUBLIC`, `PRIVATE`)                      | Público: accesible para todos. Privado: solo el creador y sus amigos. |
| `author_id`      | UUID              | NOT NULL, FK → `users.id` ON DELETE CASCADE, índice | Creador.                                                              |
| `positive_votes` | int               | NOT NULL, default 0, ≥ 0                            | Total de 👍 (desnormalizado).                                         |
| `negative_votes` | int               | NOT NULL, default 0, ≥ 0                            | Total de 👎 (desnormalizado).                                         |
| `total_votes`    | int               | NOT NULL, default 0, `CHECK = positive + negative`  | Criterio de tendencia.                                                |
| `created_at`     | timestamptz       | NOT NULL                                            | Desempate en la ordenación.                                           |

**Índices:** `(visibility, total_votes DESC, created_at DESC, id)` para las tendencias, y GIN trigram sobre `lower(title)` para la búsqueda.

#### `Vote` (tabla `votes`): voto y, si tiene justificación, opinión

| Atributo        | Tipo             | Restricciones                                | Descripción                                                                         |
| --------------- | ---------------- | -------------------------------------------- | ----------------------------------------------------------------------------------- |
| `id`            | UUID             | PK                                           | Identificador.                                                                      |
| `topic_id`      | UUID             | NOT NULL, FK → `topics.id` ON DELETE CASCADE | Tema votado.                                                                        |
| `user_id`       | UUID             | NOT NULL, FK → `users.id` ON DELETE CASCADE  | Votante.                                                                            |
| `value`         | enum `VoteValue` | NOT NULL (`UP`, `DOWN`)                      | `UP` = 👍 verde, `DOWN` = 👎 rojo.                                                  |
| `justification` | varchar(500)     | NULL                                         | Justificación opcional. Una cadena vacía o solo con espacios se guarda como `NULL`. |
| `created_at`    | timestamptz      | NOT NULL                                     | Orden de las opiniones.                                                             |

**Restricciones:** `UNIQUE (topic_id, user_id)`, un voto por usuario y tema. Índice `(topic_id, created_at DESC)`.

#### `Notification` (tabla `notifications`): aviso de un nuevo tema privado

| Atributo       | Tipo                    | Restricciones                      | Descripción                |
| -------------- | ----------------------- | ---------------------------------- | -------------------------- |
| `id`           | UUID                    | PK                                 | Identificador.             |
| `recipient_id` | UUID                    | NOT NULL, FK → `users.id`          | Amigo que recibe el aviso. |
| `actor_id`     | UUID                    | NOT NULL, FK → `users.id`          | Creador del tema.          |
| `topic_id`     | UUID                    | NOT NULL, FK → `topics.id`         | Tema privado creado.       |
| `type`         | enum `NotificationType` | NOT NULL (`PRIVATE_TOPIC_CREATED`) | Tipo de notificación.      |
| `created_at`   | timestamptz             | NOT NULL                           | Orden de la lista.         |

**Índice:** `(recipient_id, created_at DESC)`.

---

## 4. Especificación de la API

Los tres endpoints principales, en formato OpenAPI 3.0. Todos requieren sesión (cookie de Auth.js o, en la app, `Authorization: Bearer`).

> El resto de la API (personas y perfiles, **solicitudes de amistad** `PUT/DELETE /api/users/{id}/friendship` y `/api/me/friend-requests`, notificaciones, historias, perfil propio, **borrado de un tema propio** `DELETE /api/topics/{id}` (solo el autor; `404` en cualquier otro caso) e imágenes) está en [`docs/02-arquitectura.md` §2.5](docs/02-arquitectura.md). Las rutas de inicio de sesión, token, registro y solicitudes de amistad pueden responder `429 Too Many Requests` con `Retry-After`.

```yaml
openapi: 3.0.3
info:
  title: Opina API
  version: 1.0.0
servers:
  - url: http://localhost:3000/api
components:
  securitySchemes:
    sessionCookie:
      type: apiKey
      in: cookie
      name: authjs.session-token
  schemas:
    Error:
      type: object
      properties:
        error:
          type: object
          properties:
            code: { type: string, example: NOT_FOUND }
            message: { type: string, example: Tema no encontrado }
    TopicSummary:
      type: object
      properties:
        id: { type: string, format: uuid }
        title: { type: string }
        imageUrl: { type: string, nullable: true }
        totalVotes: { type: integer }
        createdAt: { type: string, format: date-time }
    TopicDetail:
      type: object
      properties:
        id: { type: string, format: uuid }
        title: { type: string }
        imageUrl: { type: string, nullable: true }
        visibility: { type: string, enum: [PUBLIC, PRIVATE] }
        myVote: { type: string, enum: [UP, DOWN], nullable: true }
        results:
          description: Solo presente si myVote no es nulo
          type: object
          properties:
            positiveVotes: { type: integer }
            negativeVotes: { type: integer }
security:
  - sessionCookie: []
paths:
  /topics:
    get:
      summary: Tendencias o búsqueda de temas públicos
      description: Sin `q` devuelve las tendencias (total de votos desc., más recientes primero). Con `q` filtra por título sin distinguir mayúsculas.
      parameters:
        - {
            name: q,
            in: query,
            required: false,
            schema: { type: string, minLength: 1, maxLength: 100 },
          }
        - { name: cursor, in: query, required: false, schema: { type: string } }
      responses:
        "200":
          description: Página de 20 temas
          content:
            application/json:
              schema:
                type: object
                properties:
                  items: { type: array, items: { $ref: "#/components/schemas/TopicSummary" } }
                  nextCursor: { type: string, nullable: true }
        "400":
          {
            description: Parámetros inválidos,
            content: { application/json: { schema: { $ref: "#/components/schemas/Error" } } },
          }
        "401":
          {
            description: Sin sesión,
            content: { application/json: { schema: { $ref: "#/components/schemas/Error" } } },
          }
  /topics/{id}:
    get:
      summary: Detalle de un tema
      parameters:
        - { name: id, in: path, required: true, schema: { type: string, format: uuid } }
      responses:
        "200":
          {
            description: Tema,
            content: { application/json: { schema: { $ref: "#/components/schemas/TopicDetail" } } },
          }
        "404":
          {
            description: No existe o el usuario no tiene acceso (tema privado),
            content: { application/json: { schema: { $ref: "#/components/schemas/Error" } } },
          }
  /topics/{id}/votes:
    post:
      summary: Emitir voto con justificación opcional
      parameters:
        - { name: id, in: path, required: true, schema: { type: string, format: uuid } }
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [value]
              properties:
                value: { type: string, enum: [UP, DOWN] }
                justification: { type: string, maxLength: 500, nullable: true }
      responses:
        "201":
          description: Voto registrado; devuelve los totales actualizados
          content:
            application/json:
              schema:
                type: object
                properties:
                  positiveVotes: { type: integer }
                  negativeVotes: { type: integer }
                  myVote: { type: string, enum: [UP, DOWN] }
        "400":
          {
            description: Datos inválidos (p. ej. justificación > 500),
            content: { application/json: { schema: { $ref: "#/components/schemas/Error" } } },
          }
        "404":
          {
            description: Tema inexistente o sin acceso,
            content: { application/json: { schema: { $ref: "#/components/schemas/Error" } } },
          }
        "409":
          {
            description: El usuario ya había votado este tema,
            content: { application/json: { schema: { $ref: "#/components/schemas/Error" } } },
          }
```

**Ejemplo: votar un tema**

```http
POST /api/topics/2b1e9c1a-3f0d-4c55-9a8e-6a4f7f2d1c10/votes
Content-Type: application/json
Cookie: authjs.session-token=…

{ "value": "DOWN", "justification": "La piña no va en la pizza" }
```

```http
HTTP/1.1 201 Created
Content-Type: application/json

{ "positiveVotes": 124, "negativeVotes": 88, "myVote": "DOWN" }
```

---

## 5. Historias de Usuario

**Historia de Usuario 1: HU-01 · Ver las tendencias**

> **Como** usuario, **quiero** ver al entrar una lista scrolleable con los temas de mayor tendencia, **para** encontrar rápidamente temas sobre los que opinar.

- **Épica:** E2 · Descubrimiento de temas · **Prioridad:** Must · **Estimación:** 5 puntos
- **Criterios de aceptación:**
  - **CA-01.1** Dado que el usuario entra en la aplicación y no ha buscado nada, entonces ve una lista de temas públicos ordenados por número total de votos descendente y, a igualdad, del más reciente al más antiguo.
  - **CA-01.2** Cada elemento de la lista muestra el título del tema.
  - **CA-01.3** Cuando el usuario llega al final de la lista y hay más temas, se cargan los siguientes (scroll continuo, páginas de 20).
  - **CA-01.4** Los temas privados no aparecen en la lista.
  - **CA-01.5** Si no hay temas, se muestra un estado vacío.
  - **CA-01.6** Al pulsar un tema, el usuario navega a su pantalla.
- **Notas:** la definición de "tendencia" es un supuesto documentado y está aislada en `TopicService.listTrending`.
- **Tickets:** [TKT-011, TKT-012, TKT-013, TKT-015](docs/05-tickets.md) · **Spec:** [`descubrimiento-temas`](openspec/specs/descubrimiento-temas/spec.md)
- **En resumen:** el backend calcula las tendencias (solo temas públicos, por votos y fecha, en páginas de 20 con cursor) y la búsqueda, y las expone en `GET /api/topics` (TKT-011, TKT-012); el frontend pinta la lista con scroll continuo (TKT-013) y QA cubre orden, paginación, exclusión de privados y búsqueda (TKT-015). La spec `descubrimiento-temas` fija esas reglas.

**Historia de Usuario 2: HU-04 · Votar un tema** _(junto con HU-05 · Justificar el voto)_

> **Como** usuario, **quiero** votar a favor (pulgar arriba) o en contra (pulgar abajo) de un tema, justificando mi voto si lo deseo, **para** expresar mi opinión.

- **Épica:** E4 · Votación y opiniones · **Prioridad:** Must · **Estimación:** 5 + 3 puntos
- **Criterios de aceptación:**
  - **CA-04.1** Al pulsar uno de los dos botones se inicia el proceso de voto con el valor elegido.
  - **CA-05.1** Tras pulsar el botón se muestra una solicitud de justificación con un campo de texto.
  - **CA-05.2 / CA-05.3** Se puede confirmar el voto con el campo vacío o con hasta 500 caracteres.
  - **CA-05.4** Una justificación formada solo por espacios se trata como vacía.
  - **CA-04.2** Al confirmar, el voto queda registrado.
  - **CA-04.3** Solo se puede emitir un voto por tema; un segundo intento se rechaza.
  - **CA-04.4** Si el usuario cancela antes de confirmar, no se registra el voto y siguen visibles los dos botones.
- **Escenario de ejemplo:** _Dado_ un tema con 3 votos positivos y 1 negativo, _cuando_ el usuario pulsa 👎, escribe "No me convence" y confirma, _entonces_ los botones se sustituyen por 👍 3 · 👎 2 y su opinión aparece en rojo.
- **Tickets:** [TKT-019, TKT-021, TKT-023](docs/05-tickets.md) · **Spec:** [`votacion`](openspec/specs/votacion/spec.md)
- **En resumen:** el backend registra un único voto por usuario y tema en una transacción que actualiza los contadores y normaliza la justificación (TKT-019); el frontend muestra el diálogo de justificación accesible y gestiona el conflicto de voto repetido (TKT-021) y QA cubre voto único concurrente, contadores y flujo completo (TKT-023). La spec `votacion` fija esas reglas.

**Historia de Usuario 3: HU-12 · Notificar a los amigos de un nuevo tema privado**

> **Como** amigo de un creador, **quiero** recibir una notificación cuando cree un tema privado, **para** poder votarlo.

- **Épica:** E6 · Temas privados · **Prioridad:** Must · **Estimación:** 5 puntos
- **Criterios de aceptación:**
  - **CA-12.1** Cuando un usuario crea un tema privado, cada uno de sus amigos recibe una notificación indicando que un amigo ha creado una nueva propuesta.
  - **CA-12.2** La notificación muestra el nombre del amigo creador y el título del tema.
  - **CA-12.3** El menú principal da acceso a la pantalla de notificaciones, ordenadas de más reciente a más antigua.
  - **CA-12.4** Al pulsar una notificación, el usuario entra en el tema privado.
  - **CA-12.5** Crear un tema público no genera notificaciones.
  - **CA-12.6** Los usuarios que no son amigos del creador no reciben notificación.
- **Dependencias:** HU-10 (crear un tema privado) y HU-11 (acceso restringido a amigos).
- **Tickets:** [TKT-029, TKT-030, TKT-031, TKT-032](docs/05-tickets.md) · **Spec:** [`notificaciones`](openspec/specs/notificaciones/spec.md)
- **En resumen:** el backend crea una notificación por amigo en la misma transacción que el tema privado (TKT-029) y las lista de más reciente a más antigua en `GET /api/notifications` (TKT-030); el frontend muestra la pantalla de notificaciones enlazada al tema (TKT-031) y QA cubre la política de visibilidad y el flujo `ana` → `bruno` → `dani` (TKT-032). La spec `notificaciones` fija esas reglas.

---

## 6. Tickets de Trabajo

**Ticket 1: [DB] TKT-003 · Esquema Prisma y migración inicial**

| Campo      | Valor                                                 |
| ---------- | ----------------------------------------------------- |
| Tipo       | Base de datos                                         |
| Historias  | Todas (base de HU-00 a HU-12)                         |
| Cambio SDD | `fase-1-plataforma-base` (tareas 3.1–3.3)             |
| Depende de | TKT-002 (PostgreSQL en Docker y variables de entorno) |
| Estimación | 3 puntos                                              |

**Descripción.** Crear el modelo de datos completo de Opina con Prisma y la migración inicial, incluidas las restricciones e índices que Prisma no puede expresar.

**Tareas técnicas:**

1. Escribir [`prisma/schema.prisma`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/blob/main/prisma/schema.prisma) con los enums `Visibility`, `VoteValue` y `NotificationType` y los modelos `User`, `Friendship`, `Topic`, `Vote` y `Notification`, mapeados a tablas en snake_case (ver sección 3).
2. Declarar las claves e índices: `@@id([userId, friendId])` en `Friendship`, `@@unique([topicId, userId])` en `Vote`, `@@index([visibility, totalVotes(sort: Desc), createdAt(sort: Desc), id])` en `Topic`, `@@index([topicId, createdAt(sort: Desc)])` en `Vote` y `@@index([recipientId, createdAt(sort: Desc)])` en `Notification`.
3. Generar la migración con `npx prisma migrate dev --create-only --name init` y añadir el SQL manual:
   ```sql
   CREATE EXTENSION IF NOT EXISTS pg_trgm;
   CREATE INDEX topics_title_trgm_idx ON topics USING gin (lower(title) gin_trgm_ops);
   ALTER TABLE friendships ADD CONSTRAINT friendships_no_self CHECK (user_id <> friend_id);
   ALTER TABLE topics ADD CONSTRAINT topics_counters_ok
     CHECK (positive_votes >= 0 AND negative_votes >= 0 AND total_votes = positive_votes + negative_votes);
   ```
4. Crear el singleton de Prisma Client en [`src/server/db/client.ts`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/blob/main/src/server/db/client.ts), reutilizando la instancia en desarrollo para evitar el agotamiento de conexiones por _hot reload_.

**Criterios de aceptación:**

- `npx prisma migrate dev` aplica la migración sin errores sobre una base de datos vacía.
- Insertar dos votos del mismo usuario en el mismo tema falla por la restricción UNIQUE.
- Insertar una amistad de un usuario consigo mismo falla por `friendships_no_self`.
- Un `UPDATE` que deja `total_votes ≠ positive_votes + negative_votes` falla por `topics_counters_ok`.
- `EXPLAIN` de la consulta de tendencias usa el índice compuesto, y el de la búsqueda con `ILIKE` usa `topics_title_trgm_idx`.

**Pruebas:** test de integración que verifica las tres restricciones anteriores contra la base `opina_test`.
**Definición de hecho:** migración versionada en [`prisma/migrations/`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/tree/main/prisma/migrations), tipos generados, lint y typecheck en verde, CI en verde, revisión aprobada.

**Ticket 2: [Backend] TKT-019 · Emisión de voto**

| Campo      | Valor                                        |
| ---------- | -------------------------------------------- |
| Tipo       | Backend                                      |
| Historias  | HU-04, HU-05, HU-06                          |
| Cambio SDD | `fase-4-votacion-opiniones` (tareas 1.1–1.3) |
| Depende de | TKT-016 (VisibilityPolicy y detalle de tema) |
| Estimación | 3 puntos                                     |

**Descripción.** Implementar el registro del voto de un usuario sobre un tema, con justificación opcional, garantizando un único voto por usuario y tema y totales siempre coherentes.

**Tareas técnicas:**

1. Esquema Zod `castVoteSchema` en [`src/lib/validation/vote.ts`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/blob/main/src/lib/validation/vote.ts): `value: z.enum(["UP","DOWN"])`, `justification` opcional, con `trim`, un máximo de 500 caracteres y conversión de la cadena vacía a `null`.
2. `VoteService.castVote(topicId, userId, value, justification)` en [`src/server/services/vote-service.ts`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/blob/main/src/server/services/vote-service.ts), dentro de `prisma.$transaction`:
   - Cargar el tema. Si no existe o `VisibilityPolicy.canAccess` devuelve `false`, lanzar `NotFoundError`.
   - `vote.create(...)`. Si Prisma devuelve el error `P2002` (UNIQUE), lanzar `ConflictError("ALREADY_VOTED")`.
   - `topic.update` con `increment: 1` en `positiveVotes` o `negativeVotes` y en `totalVotes`.
   - Devolver `{ positiveVotes, negativeVotes, myVote }`.
3. Route Handler `POST /api/topics/[id]/votes`: `requireUser()`, validar el body, llamar al servicio y responder `201`. Los errores se convierten con `toHttpResponse` (`400`, `401`, `404`, `409`).

**Criterios de aceptación:**

- Un voto válido responde `201` con los totales incrementados en 1 en el valor correcto.
- Una justificación vacía o formada solo por espacios se guarda como `NULL`; una de 501 caracteres responde `400`.
- Un segundo voto del mismo usuario responde `409` y los totales no cambian.
- Dos peticiones concurrentes del mismo usuario producen un único voto y `totalVotes` aumenta exactamente en 1.
- Un tema inexistente, o privado y sin acceso, responde `404` y no se registra nada.

**Pruebas:** integración en `tests/integration/vote-service.test.ts` con un test por cada escenario de la spec `votacion` ("Primer voto", "Segundo voto", "Votos concurrentes del mismo usuario", "Votar un tema sin acceso", "Justificación solo con espacios", "Justificación demasiado larga").
**Definición de hecho:** escenarios en verde, sin escrituras en `votes` fuera de `VoteService`, CI en verde, revisión aprobada.

**Ticket 3: [Frontend] TKT-021 · Flujo de voto con justificación**

| Campo      | Valor                                                               |
| ---------- | ------------------------------------------------------------------- |
| Tipo       | Frontend                                                            |
| Historias  | HU-04, HU-05                                                        |
| Cambio SDD | `fase-4-votacion-opiniones` (tareas 3.1–3.3)                        |
| Depende de | TKT-017 (pantalla de tema con `VoteButtons`), TKT-019 (API de voto) |
| Estimación | 3 puntos                                                            |

**Descripción.** Conectar los botones 👍/👎 de la pantalla de tema con un diálogo que solicita la justificación opcional y emite el voto.

**Tareas técnicas:**

1. Componente `JustificationDialog` ([`src/components/topics/JustificationDialog.tsx`](https://github.com/Morius2100/ai4devs-finalproject--JRG-/blob/main/src/components/topics/JustificationDialog.tsx)):
   - Props: `open`, `value: "UP" | "DOWN"`, `onCancel`, `onConfirm(justification)`, `pending`, `error`.
   - `role="dialog"`, `aria-modal`, foco inicial en el `textarea`, foco atrapado dentro del diálogo, y `Esc` para cancelar.
   - `textarea` opcional con contador `n/500` y validación con el mismo `castVoteSchema`.
   - Botones "Cancelar" y "Votar". El color de "Votar" es verde (`UP`) o rojo (`DOWN`) según el voto elegido.
2. `VoteButtons.onSelect(value)` abre el diálogo con el valor elegido.
3. Mutación con TanStack Query (`useMutation`) a `POST /api/topics/:id/votes`:
   - Deshabilitar "Votar" mientras la petición está en curso.
   - `201` → cerrar el diálogo y pasar los totales a `TopicPage`, que sustituye `VoteButtons` por `VoteResults` (TKT-022).
   - `409` → invalidar la consulta del tema para mostrar los resultados existentes.
   - `400` → mostrar el error dentro del diálogo.

**Criterios de aceptación:**

- Pulsar 👍 abre el diálogo con "Votar" en verde, y pulsar 👎 lo abre con "Votar" en rojo.
- "Cancelar" o `Esc` cierran el diálogo sin llamar a la API, y los dos botones siguen visibles.
- Se puede votar con el campo vacío.
- Con más de 500 caracteres, "Votar" queda deshabilitado y se muestra el límite.
- Una doble pulsación en "Votar" produce una sola petición.
- Funciona en un viewport de 390×844 y solo con teclado.

**Pruebas:** tests de componente con Testing Library (cancelar, confirmar vacío, límite de 500, estado `pending`) y e2e con Playwright ("votar positivo con justificación → opinión verde").
**Definición de hecho:** escenarios "Inicio del proceso de voto", "Justificación opcional" y "Cancelar" cubiertos, sin errores de accesibilidad en `axe`, CI en verde, revisión aprobada.

---

## 7. Pull Requests

Las fases 1 a 6 y las mejoras posteriores (cambios archivados en [`openspec/changes/archive/`](openspec/changes/archive/)) se publicaron con `push` directo a `main` mientras no había protección. El 9 de noviembre de 2026 se pasó al flujo con Pull Request (ver §2.4). Estas son las PR del [repositorio del proyecto](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pulls?q=is%3Apr), todas desde la rama `JRGA` hacia `main`:

| PR | Título | Estado | Contenido |
| --- | --- | --- | --- |
| [#1](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pull/1) | `docs: CI en verde en GitHub y archivo de documentar-ci-cd (T10)` | Cerrada sin fusionar | Mismo título que la #3 |
| [#2](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pull/2) | `docs: CI en verde en GitHub y archivo de documentar-ci-cd (T10)` | Cerrada sin fusionar | Mismo título que la #3 |
| [#3](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pull/3) | `docs: CI en verde en GitHub y archivo de documentar-ci-cd (T10)` | **Fusionada** el 9 de noviembre de 2026 | CI de GitHub en verde, documentación CI/CD ([`docs/12-ci-cd.md`](docs/12-ci-cd.md)) y archivo del cambio `documentar-ci-cd` |
| [#4](https://github.com/Morius2100/ai4devs-finalproject--JRG-/pull/4) | `feat(topics): borrar un tema propio desde su detalle TKT-087 TKT-088 TKT-089` | **Abierta** | Cambio `borrar-temas-propios` (HU-23): `DELETE /api/topics/:id`, botón con papelera y diálogo de confirmación, pruebas de integración, de componentes, e2e y de accesibilidad (45 ficheros) |

Los prompts que originaron estos cambios están en [`prompts.md`](prompts.md), §7 y §8.
