# 🪗 Festival Vallenato Digital — Plataforma Modular de Micro Frontends

Bienvenido a la plataforma web oficial de la **59ª Edición del Festival de la Leyenda Vallenata (2026)** en Valledupar, Colombia. Este proyecto es una implementación de referencia de una **Arquitectura de Micro Frontends**, diseñada para que múltiples equipos de ingeniería puedan desarrollar, probar y desplegar capacidades de negocio de forma 100% independiente y desacoplada.

---

## 📌 Tabla de Contenidos

1. [¿Qué es este proyecto?](#-qué-es-este-proyecto)
2. [Arquitectura y Tecnologías](#-arquitectura-y-tecnologías)
3. [Requisitos Previos](#-requisitos-previos)
4. [Cómo Ejecutar el Proyecto](#-cómo-ejecutar-el-proyecto)
   - [Opción A: Ejecutar Todo el Sistema (1 Solo Comando)](#opción-a-ejecutar-todo-el-sistema-1-solo-comando)
   - [Opción B: Ejecutar Módulos de Forma Individual](#opción-b-ejecutar-módulos-de-forma-individual)
5. [Mapa de Puertos y Servicios](#-mapa-de-puertos-y-servicios)
6. [Cuentas Demo de Prueba (1-Click Login)](#-cuentas-demo-de-prueba-1-click-login)
7. [Guía de Demostración y Flujos Clave](#-guía-de-demostración-y-flujos-clave)
8. [Estructura del Repositorio](#-estructura-del-repositorio)
9. [Contratos y Comunicación entre Módulos](#-contratos-y-comunicación-entre-módulos)
10. [Compilación y Pruebas](#-compilación-y-pruebas)

---

## 📖 ¿Qué es este proyecto?

El festival enfrentaba limitaciones con un antiguo sitio monolítico que dificultaba la evolución ágil de sus servicios. Esta solución migra el sistema a **Micro Frontends**, dividiendo la plataforma en dominios autónomos:

- **Asistentes:** Consultan la programación, escuchan a los juglares, compran entradas y votan en tiempo real.
- **Concursantes:** Radican su inscripción oficial, seleccionan categoría folclórica y cargan documentos.
- **Organizadores y Jurados:** Monitorean eventos y resultados de las rondas de acordeón y piquería.
- **Patrocinadores:** Gestionan convenios comerciales mediante un portal legado en jQuery integrado sin necesidad de reescritura.

---

## 🛠️ Arquitectura y Tecnologías

| Módulo / Capa | Tecnología | Rol Arquitectónico |
| :--- | :--- | :--- |
| **App Shell (`festival-shell`)** | React 18 + React Router v6 | Orquestador Host, navegación unificada, Header reactivo, Footer y telemetría de errores. |
| **MF 1: Programación (`programacion-mf`)** | React 18 + SSG / SEO | Cartelera, noticias, artistas, galería cultural y microdatos Schema.org JSON-LD. |
| **MF 2: Boletería (`boleteria-mf`)** | React 18 (CSR Alta Concurrencia) | Localidades, carrito desacoplado, pasarela de pagos simulada y boletas con código QR. |
| **MF 3: Concursantes (`concursantes-mf`)** | **Vue 3** (Composition API) | Inscripción, carga documental y seguimiento con Stepper. Demuestra integración multi-framework. |
| **MF 4: Votación (`votacion-mf`)** | React 18 + **Server-Sent Events (SSE)** | Canciones en competencia, audio player y streaming de resultados en tiempo real. |
| **MF 5: Mi Cuenta (`cuenta-mf`)** | React 18 | Inicio de sesión, perfil de usuario, sincronización de JWT e historial de órdenes. |
| **MF 6: Patrocinadores (`legacy-sponsors`)** | **jQuery 3.7.1** (Sistema Legado) | Portal comercial 2014 integrado vía **Legacy Adapter Pattern** sin alterar su código. |
| **Backend API (`backend`)** | Node.js + Express + SSE Hub | Endpoints REST organizados por dominios y dataset semilla completo. |
| **Shared Contracts (`shared-contracts`)** | Vanilla JS / EventBus PubSub | Contratos de eventos tipados y tokens de diseño cultural. |

---

## 📋 Requisitos Previos

- **Node.js:** Versión 18.x o superior (Recomendado: Node.js v20 LTS).
- **Navegador Web Moderno:** Google Chrome, Microsoft Edge, Brave, Firefox o Safari.

---

## 🚀 Cómo Ejecutar el Proyecto

Abre una terminal (PowerShell, CMD o Bash) en la raíz del proyecto:
```bash
cd C:\Users\elmoj\.gemini\antigravity-ide\scratch\festival-vallenato-digital
```

### Opción A: Ejecutar Todo el Sistema (1 Solo Comando)

Para levantar el **Backend** y los **7 Micro Frontends** concurrentemente:

```bash
# 1. Instalar dependencias (solo la primera vez)
npm install

# 2. Iniciar todos los servicios simultáneamente
npm start
```
*(O alternativamente: `node scripts/start-all.js`)*

Una vez iniciados, abre tu navegador en:
👉 **[http://localhost:3000](http://localhost:3000)**

---

### Opción B: Ejecutar Módulos de Forma Individual

Cada Micro Frontend puede ser ejecutado, probado y modificado de manera aislada:

```bash
# Iniciar únicamente el Backend (REST & SSE):
npm run start:backend        # http://localhost:5000

# Iniciar únicamente el App Shell (Host):
npm run start:shell          # http://localhost:3000

# Iniciar únicamente Programación y Noticias:
npm run start:programacion   # http://localhost:3001

# Iniciar únicamente Boletería y Carrito:
npm run start:boleteria      # http://localhost:3002

# Iniciar únicamente el Portal de Concursantes (Vue 3):
npm run start:concursantes   # http://localhost:3003

# Iniciar únicamente Votación en Tiempo Real:
npm run start:votacion       # http://localhost:3004

# Iniciar únicamente Mi Cuenta & Autenticación:
npm run start:cuenta         # http://localhost:3005

# Iniciar únicamente el Portal Legado de Patrocinadores (jQuery):
npm run start:legacy         # http://localhost:3006
```

---

## 🌐 Mapa de Puertos y Servicios

| Servicio | Puerto | Descripción | URL Directa |
| :--- | :---: | :--- | :--- |
| **App Shell** | `:3000` | Plataforma Unificada y Enrutador | [http://localhost:3000](http://localhost:3000) |
| **Programación MF** | `:3001` | Remote de Agenda y Noticias (SEO) | [http://localhost:3001](http://localhost:3001) |
| **Boletería MF** | `:3002` | Remote de Entradas y Carrito | [http://localhost:3002](http://localhost:3002) |
| **Concursantes MF** | `:3003` | Remote en Vue 3 de Inscripciones | [http://localhost:3003](http://localhost:3003) |
| **Votación MF** | `:3004` | Remote de Votación en Streaming | [http://localhost:3004](http://localhost:3004) |
| **Cuenta MF** | `:3005` | Remote de Perfil y Autenticación | [http://localhost:3005](http://localhost:3005) |
| **Legacy Sponsors** | `:3006` | Portal Legado jQuery 3.7 | [http://localhost:3006](http://localhost:3006) |
| **Backend API** | `:5000` | API REST y Servidor SSE | [http://localhost:5000/api/health](http://localhost:5000/api/health) |

---

## 🔑 Cuentas Demo de Prueba (1-Click Login)

En la sección **Mi Cuenta** (`/cuenta`), dispones de botones para iniciar sesión al instante con 1 solo clic:

| Usuario | Correo Electrónico | Contraseña | Rol y Permisos |
| :--- | :--- | :--- | :--- |
| **Asistente / Aficionado** | `usuario@festival.test` | `user123` | Comprador de boletas, visualización de tickets QR y votación popular. |
| **Concursante Oficial** | `concursante@festival.test` | `contest123` | Aspirante registrado en Acordeón Profesional con ficha técnica activa. |
| **Administrador** | `admin@festival.test` | `admin123` | Administrador institucional de la Fundación Festival de la Leyenda Vallenata. |

---

## 🎭 Guía de Demostración y Flujos Clave

### 1. Flujo de Compra Desacoplado (Programación ➔ Boletería ➔ Carrito ➔ Pago)
1. Entra a **Programación** (`/programacion`).
2. En el evento *“Gran Noche de Inauguración”*, haz clic en **“🎟️ Comprar Boletas”**.
3. El módulo emite el evento `event:selected` y el App Shell te redirige fluidamente a **Boletería** con el evento preseleccionado.
4. Selecciona localidad *VIP Oro*, cantidad `2` y pulsa **“🛒 Agregar al Carrito”**.
5. Observa el **Header**: el contador del carrito se actualiza a `[2]` y calcula el total en COP inmediatamente gracias al evento `cart:updated`.
6. Ve a tu carrito, haz clic en **“💳 Proceder al Pago Seguro”**, completa la pasarela simulada y recibirás tu **Boleta Digital con Código QR**.

### 2. Demostración Multi-Framework (Vue 3 dentro de React)
1. Entra a **Concursantes** (`/concursantes`).
2. Verás una aplicación completa en **Vue 3** ejecutándose dentro del Shell de React mediante el componente `VueAdapter.js`.
3. Explora el directorio de 20 concursantes, filtra por categoría (*Acordeón Profesional, Aficionado, Canción Inédita, Piquería*) y consulta el **Stepper de Seguimiento**.

### 3. Votación en Vivo con Server-Sent Events (SSE)
1. Entra a **Votación** (`/votacion`).
2. Reproduce las muestras de audio de las canciones inéditas con el reproductor interactivo de onda sonora.
3. Vota por tu canción preferida. El sistema cuenta con protección antifraude por huella digital y actualiza la tabla de posiciones en streaming para todos los clientes conectados.

### 4. Sistema Legado jQuery (Legacy Adapter Pattern)
1. Entra a **Patrocinadores** (`/patrocinadores`).
2. Observa cómo el portal legado corporativo desarrollado en **jQuery 3.7 puro** se monta en un contenedor aislado (`LegacyAdapter.js`), permitiendo filtrado dinámico y modal AJAX sin interferir con el Virtual DOM de React.

---

## 📁 Estructura del Repositorio

```text
festival-vallenato-digital/
├── festival-shell/             # App Shell (React 18 Host Container)
│   ├── src/
│   │   ├── adapters/           # LegacyAdapter (jQuery) y VueAdapter (Vue 3)
│   │   ├── components/         # Header reactivo, Footer, ErrorBoundary
│   │   ├── observability/      # Módulo de telemetría y captura de errores
│   │   ├── styles/shell.css    # Tokens de diseño y estilos globales
│   │   └── App.js              # Enrutador principal y carga perezosa de remotes
│   └── webpack.config.js       # Configuración Module Federation Host
│
├── programacion-mf/            # MF 1: React + SSG/SEO Priority
│   ├── src/
│   │   ├── seo/HeadManager.js  # Inyector de OpenGraph y JSON-LD Schema.org
│   │   └── App.js              # Agenda, noticias, artistas y galería
│   └── webpack.config.js       # Expone './ProgramacionApp' en puerto 3001
│
├── boleteria-mf/               # MF 2: React + Carrito Desacoplado
│   ├── src/
│   │   ├── services/           # CartService desacoplado y almacenamiento local
│   │   └── App.js              # Localidades, carrito, pasarela y QR
│   └── webpack.config.js       # Expone './BoleteriaApp' en puerto 3002
│
├── concursantes-mf/            # MF 3: Vue 3 Composition API
│   ├── src/
│   │   ├── App.vue             # Formulario de inscripción, stepper y directorio
│   │   └── mount.js            # Función puente mount() para el App Shell
│   └── webpack.config.js       # Expone './mountConcursantes' en puerto 3003
│
├── votacion-mf/                # MF 4: React + Streaming SSE
│   ├── src/App.js              # Reproductor de audio, votos y leaderboard en vivo
│   └── webpack.config.js       # Expone './VotacionApp' en puerto 3004
│
├── cuenta-mf/                  # MF 5: React + Auth JWT Sync
│   ├── src/App.js              # Login, registro, demo users y compras
│   └── webpack.config.js       # Expone './CuentaApp' en puerto 3005
│
├── legacy-sponsors/            # MF 6: jQuery 3.7 Legacy Portal
│   ├── public/                 # legacy-sponsors.js, legacy.css, index.html
│   └── server.js               # Servidor estático en puerto 3006
│
├── backend/                    # Backend API REST y SSE
│   ├── src/
│   │   ├── data/seedData.js    # 10 eventos, 10 artistas, 20 concursantes, etc.
│   │   └── server.js           # Express API y Hub SSE en puerto 5000
│   └── package.json
│
├── shared-contracts/           # Bus de Eventos y Contratos Compartidos
│   └── src/index.js            # EventTypes, CrossMicroFrontendEventBus
│
├── docs/                       # Documentación Arquitectónica y Diagramas
│   ├── architecture/           # ARCHITECTURE.md, CONTRACTS.md
│   └── diagrams/               # Diagramas Mermaid (Sistema, Despliegue, Secuencia)
│
├── scripts/                    # Scripts de orquestación
│   ├── start-all.js            # Inicio concurrente de los 8 servicios
│   ├── build-all.js            # Compilación independiente por módulo
│   └── test-all.js             # Suite de pruebas de contratos y arquitectura
│
└── .github/workflows/          # Pipelines CI/CD independientes
    └── ci-cd-boleteria.yml     # Pipeline independiente para Boletería MF
```

---

## 📡 Contratos y Comunicación entre Módulos

La comunicación entre Micro Frontends se realiza mediante eventos desacoplados a través del **EventBus Global** (`shared-contracts`):

| Nombre del Evento | Módulo Emisor | Módulos Receptores | Payload Principal |
| :--- | :--- | :--- | :--- |
| `event:selected` | `programacion-mf` | `boleteria-mf`, `festival-shell` | `{ eventId, title, date, venue, priceFrom }` |
| `cart:updated` | `boleteria-mf` | `festival-shell` (Header Badge) | `{ items, itemsList, subtotal, total, currency: 'COP' }` |
| `purchase:completed` | `boleteria-mf` | `cuenta-mf`, `festival-shell` | `{ orderId, items, total, customer, timestamp }` |
| `user:authenticated` | `cuenta-mf` | Todos los Micro Frontends | `{ user, token, timestamp }` |
| `user:logout` | `cuenta-mf` | Todos los Micro Frontends | `{ timestamp }` |
| `vote:registered` | `votacion-mf` | `cuenta-mf`, `backend` | `{ songId, title, composer, totalVotes }` |
| `contestant:registered` | `concursantes-mf` | `festival-shell` | `{ contestantId, fullName, category }` |

---

## 🧪 Compilación y Pruebas

### Validar Contratos Arquitectónicos:
```bash
node scripts/test-all.js
```
*Ejecuta la suite de verificación de contratos y eventos con 100% de aserciones aprobadas.*

### Compilar Todos los Micro Frontends para Producción:
```bash
node scripts/build-all.js
```
*Genera los bundles optimizados y los manifiestos `remoteEntry.js` de cada módulo en sus respectivos directorios `dist/`.*

---

## 🏛️ Fundación Festival de la Leyenda Vallenata
*Valledupar, Cesar — Patrimonio Cultural Inmaterial de la Humanidad UNESCO.*  
*Plataforma desarrollada con Arquitectura de Micro Frontends en React 18, Vue 3, Webpack 5 Module Federation y jQuery Legacy Adapter.*
