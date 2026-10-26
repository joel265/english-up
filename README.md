# English Up — Plataforma Web Interactiva de Gamificación en Inglés (SENA ADSO)

Plataforma web multijugador en tiempo real desarrollada para el evento institucional **English Up** del **SENA (Análisis y Desarrollo de Software — ADSO)**. Permite a los aprendices conectarse desde sus dispositivos móviles mediante un código PIN de 6 dígitos para competir en rondas sincrónicas de inglés (niveles MCER A2–B1) proyectadas en auditorio, integrando cuatro mecánicas interactivas, retroalimentación pedagógica por ruleta y clasificación en vivo.

---

<p align="center">
  <img src="docs/assets/sena-adso-team-pixelart.webp" alt="Equipo SENA ADSO - English Up" width="100%">
</p>

## 🤖 Creación del Proyecto & Uso Responsable de la Inteligencia Artificial

> *"La inteligencia artificial no reemplaza la creatividad ni el criterio humano: los potencia. English Up fue desarrollado utilizando la IA responsablemente como tutor pedagógico, mentor técnico y guía paso a paso en la construcción de la aplicación web."*

En el marco de la formación profesional integral del **SENA**, los aprendices de **Análisis y Desarrollo de Software (ADSO)** adoptaron un enfoque vanguardista y ético: la **co-creación responsable asistida por Inteligencia Artificial**. La IA fungió como un catalizador del aprendizaje activo, guiando la construcción paso a paso de la aplicación web:

- **Tutoría Socrática y Guía Metodológica:** La IA actuó como un mentor permanente que explicó fundamentos de ingeniería de software, patrones arquitectónicos cliente-servidor, concurrencia en WebSockets y normalización de datos, fomentando en los aprendices la comprensión profunda del *porqué* detrás de cada decisión técnica.
- **Construcción Paso a Paso:**
  - **Arquitectura Backend:** Modelado asistido de entidades relacionales, configuración de Spring Boot 3 con STOMP WebSockets, y algoritmos deterministas de puntuación por velocidad de respuesta.
  - **Mecánicas Pedagógicas y Gamificación:** Diseño e implementación de las cuatro modalidades interactivas (selección múltiple, ordenamiento sintáctico por bloques, entrada de texto y reconocimiento de voz mediante la Web Speech API).
  - **Frontend Táctil y Accesible:** Creación del sistema de diseño *Tactile Gamified Arcade*, renderizado de avatares interactivos, animación de ruleta de retroalimentación en Canvas 2D y adaptabilidad para dispositivos móviles y proyector.
- **Supervisión, Criterio y Verificación Humana:** Cada recomendación de la IA fue rigurosamente revisada, adaptada y validada por los aprendices a través de matrices de calidad, pruebas unitarias, suites End-to-End y simulaciones de carga concurrente. El liderazgo conceptual, las decisiones de arquitectura y la autoría intelectual residen enteramente en el equipo humano de aprendices.

---

## Características Principales

- **Salas en Vivo mediante PIN de 6 dígitos y Código QR Ampliable:** Creación instantánea de sesiones de juego con generación nativa de código QR dinámico (detección automática de URL pública de Cloudflare Tunnel o IP LAN) y modal de alta resolución a pantalla completa para facilitar el escaneo desde cualquier fila del auditorio.
- **Parámetros Configurables por el Moderador:** Ajuste en tiempo real de la cantidad de preguntas por partida (`1` a `24`), tiempos límite personalizados por mecánica (`MULTIPLE_CHOICE`, `WORD_BLOCKS`, `TEXT_INPUT`, `VOICE`), orden aleatorio barajado por sala y temporizador de preparación de 3 segundos antes de cada ronda.
- **Sincronización Bidireccional de Baja Latencia (< 200 ms) y Persistencia de Sesión:** Comunicación basada en **WebSockets (STOMP)** capaz de soportar 30–50+ aprendices concurrentes por sala sin condiciones de carrera, con sincronización completa de estado (`GET /api/rooms/{pin}/state`), reconexión automática ante recargas accidentales (`F5`), bloqueo de navegación durante rondas activas y botón de salida voluntaria del lobby.
- **Banco Oficial de 24 Preguntas en 6 Temáticas Bilingües:** Estructurado en *Pronouns*, *Verb To Be*, *Possessive Adjectives*, *Simple Present (Affirmative, Negative, Interrogative)*, *Present Continuous* y *WH Questions* (niveles MCER A2–B1).
- **Cuatro Mecánicas de Evaluación con Validación Inteligente:**
  - `MULTIPLE_CHOICE`: Selección múltiple de 4 opciones con retroalimentación táctil e indicadores visuales.
  - `WORD_BLOCKS`: Construcción sintáctica de oraciones mediante bloques interactivos (*Tap & Arrange*) con validación de cláusulas con sentido gramatical completo y posición flexible de adverbios de tiempo.
  - `TEXT_INPUT`: Escritura directa con expansión automática de contracciones (`don't` ↔ `do not`), tolerancia ortográfica inteligente (distancia Damerau-Levenshtein respetando grupos gramaticales excluyentes) y aceptación tanto de la palabra clave como de la oración completa.
  - `VOICE`: Reconocimiento de pronunciación en inglés (`en-US`) mediante **Web Speech API**, con transcripción en vivo, preservación de texto entre intentos, modo de edición manual por teclado y degradación elegante (*fallback* automático a texto) cuando el navegador o los permisos restringen el micrófono.
- **Retroalimentación Pedagógica Palabra por Palabra:** Corrector visual con subrayado de aciertos/errores y explicación gramatical detallada de la respuesta correcta al cierre de cada pregunta.
- **Algoritmo de Puntuación por Velocidad:** Cálculo determinista en servidor según el tiempo restante de respuesta:
  $$\text{Puntos} = \text{round}\left(1000 \times \left(0.5 + 0.5 \times \frac{t_{\text{restante}}}{t_{\text{total}}}\right)\right)$$
- **Ruleta de Retroalimentación Pedagógica:** Sorteo aleatorio en servidor entre los participantes de la ronda (con animación sincronizada en Canvas 2D tanto en el proyector como en los móviles de los aprendices) para fomentar la justificación oral en inglés.
- **Avatares Personalizados y Reacciones en Vivo:** Selector de avatares vectoriales (`EnglishDayAvatars`), efectos de sonido sintetizados (Web Audio API) y reacciones globales en tiempo real (`👋`, `🎉`) con control de cooldown.
- **Podio Olímpico, Certificación Bilingüe y Acreditación de +1 Punto Positivo:** Escenario final Top 3 con generación dinámica de **Diploma Oficial SENA ADSO en Canvas 2D (1600×1100 px)**, código único de verificación (`SENA-B1-ENGUP-2026-...`), acreditación de **+1 Punto Positivo en el Área de Inglés Técnico**, descarga directa universal en formato PNG (`Blob` `application/octet-stream`), visor en pantalla completa para móviles (*"Guardar en Fotos"*) y compartición inmediata por WhatsApp o portapapeles.
- **Panel de Moderador / Proyector Integrado:** Interfaz dedicada para el docente con minimización automática del panel PIN/QR durante la partida activa, controles de inicio, cierre de pregunta (Top 5 parcial y cierre automático al responder todos los aprendices), giro de ruleta, avance de ronda, reconexión por PIN y podio final con confeti.

---

## Arquitectura y Stack Tecnológico

| Capa | Tecnología | Propósito |
|---|---|---|
| **Backend** | Java 17+ / Spring Boot 3.5 | API REST síncrona, Broker STOMP WebSocket, validación pedagógica inteligente y puntuación |
| **Persistencia** | Spring Data JPA + Hibernate / MySQL 8.0+ | Almacenamiento relacional transaccional (con soporte H2 en memoria para ejecución autónoma y pruebas) |
| **Tiempo Real** | Spring WebSocket + STOMP Protocol | Difusión de eventos de sala (`lobby`, `status`, `question`, `results`, `roulette`, `podium`, `reactions`) |
| **Frontend** | HTML5 Semántico, Tailwind CSS + CSS3 (*Tactile Gamified Arcade*), JavaScript ES6+ | Clientes ligeros *Mobile-First* (`>= 48x48px` touch targets), Web Speech API, Web Audio API y Canvas 2D (Ruleta, QR y Diploma PNG) |
| **Infraestructura** | Cloudflare Tunnel (`cloudflared`) + Control Anti-Caché | Exposición segura mediante HTTPS / WSS (`englishup.jmj.agency`) con `Cache-Control: no-cache, must-revalidate` y versionado de recursos |

---

## Estructura del Proyecto

```text
english-up/
├── backend/                                  # Servidor Spring Boot (API REST + STOMP WebSocket)
│   ├── src/main/java/com/senadso/englishday/
│   │   ├── config/                           # Configuración CORS, recursos estáticos (no-cache), WebSocket y semilla automática
│   │   ├── controller/                       # Controladores REST (RoomController) y STOMP (GameWebSocketController)
│   │   ├── dto/                              # Objetos de transferencia de datos (incluye RoomSettingsDto y RoomSyncStateResponse)
│   │   ├── exception/                        # Manejo global de excepciones y códigos de error semánticos
│   │   ├── model/                            # Entidades JPA (Room, Player, Question, Answer, Score)
│   │   ├── repository/                       # Repositorios Spring Data JPA
│   │   └── service/                          # Lógica de salas, validación inteligente (AnswerService), puntajes y ruleta
│   ├── src/main/resources/                   # Perfiles de configuración (default, h2, prod)
│   └── src/test/java/                        # Suite de 52 pruebas unitarias, de integración y concurrencia
├── frontend/                                 # Aplicación Web Cliente (servida estáticamente por el Backend)
│   ├── index.html                            # Vista del Aprendiz (Mobile-First, Podio y Modal de Diploma Bilingüe)
│   ├── moderator.html                        # Panel de Control del Moderador, Proyector y Modal de QR Ampliado
│   ├── assets/                               # Fondos ilustrados en 2K (2752x1536), branding oficial y avatares
│   ├── css/                                  # Hojas de estilo del sistema de diseño Tactile Gamified Arcade
│   └── js/                                   # Clientes STOMP/REST (join.js, moderator.js), qrcode.js, roulette.js, audio.js y confetti.js
├── database/                                 # Persistencia y Analítica SQL
│   ├── schema.sql                            # Esquema DDL oficial para MySQL 8.0+
│   ├── questions_seed.sql                    # Semilla oficial de 24 preguntas (6 temáticas, niveles A2-B1)
│   ├── queries/                              # Consultas operativas y reportería analítica post-evento
│   └── schemas/                              # JSON Schema de validación del banco de preguntas
├── QA/                                       # Aseguramiento de Calidad y Pruebas Automatizadas
│   ├── MATRIZ_CASOS_DE_PRUEBA_Y_EJECUCION.md # Matriz y resultados de ejecución (CP-01 a CP-16)
│   ├── REPORTE_RESILIENCIA_AUDITORIA_Y_DISPOSITIVOS.md
│   ├── e2e_verification_suite.mjs            # Suite automatizada End-to-End (25 verificaciones REST + STOMP)
│   ├── load-test/                            # Scripts e informe de pruebas de carga y estrés (Grafana k6 / PowerShell)
│   └── banco-preguntas/                      # Insumos pedagógicos del área de bilingüismo
├── UIUX/                                     # Sistema de Diseño y Prototipos de Interfaz
│   ├── design-system/                        # Especificación visual (DESIGN.md)
│   ├── wireframes/                           # Estructura base de pantallas
│   ├── mobile-views/                         # Vistas móviles de alta fidelidad
│   ├── components/                           # Componentes interactivos por mecánica
│   └── projector-views/                      # Vistas de pantalla gigante / auditorio
├── scripts/                                  # Scripts de automatización (start-all.sh, start-tunnel.sh)
└── docs/                                     # Documentación de Arquitectura, Bitácora Técnica, Contrato API/WS y Despliegue
```

---

## Contrato de Comunicación (Resumen)

### Endpoints REST Principales

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/rooms` | Crea una nueva sala en estado `WAITING_LOBBY`, genera un PIN único de 6 dígitos y aplica parámetros iniciales opcionales (`RoomSettingsDto`). |
| `GET` | `/api/rooms/{roomId}/settings` | Consulta los parámetros configurados de la sala (tiempos por mecánica, orden aleatorio y `questionCount`). |
| `PUT` | `/api/rooms/{roomId}/settings` | Actualiza en tiempo real los parámetros de tiempos, aleatoriedad y cantidad de preguntas de la sala. |
| `GET` | `/api/rooms/{pin}` | Obtiene el estado actual y metadatos básicos de una sala activa a partir de su PIN. |
| `GET` | `/api/rooms/{pin}/state` | Sincronización completa del estado de la sala (jugadores, pregunta activa, tiempo restante, últimos resultados, ruleta y podio) para persistencia y restauración de sesión. |
| `POST` | `/api/rooms/{pin}/join` | Registra un aprendiz con su avatar en la sala y notifica en tiempo real al tópico de lobby. |
| `DELETE` / `POST` | `/api/rooms/{roomId}/players/{playerId}[/leave]` | Permite que un aprendiz abandone voluntariamente la sala de espera y actualiza el lobby. |
| `POST` | `/api/rooms/{roomId}/start` | Inicia la partida (`QUESTION_IN_PROGRESS`) con al menos 1 jugador conectado y emite la primera pregunta. |
| `POST` | `/api/rooms/{roomId}/close-question` | Cierra la pregunta activa y publica la respuesta correcta con explicación pedagógica junto al Top 5. |
| `POST` | `/api/rooms/{roomId}/roulette` | Selecciona aleatoriamente a un aprendiz (`ROULETTE_TIME`) para la retroalimentación oral. |
| `POST` | `/api/rooms/{roomId}/next` | Avanza a la siguiente pregunta del mazo de la sala o finaliza la partida al completar `questionCount`. |
| `GET` | `/api/rooms/{roomId}/results` | Obtiene el podio final clasificado (Top 3) de la sala. |
| `POST` | `/api/rooms/{roomId}/finish` | Cierra la partida (`FINISHED`) y publica el podio final (Top 3) en `/topic/room/{roomId}/podium`. |

### Canales STOMP WebSocket (`/ws-englishup` / `/ws-englishday`)

| Dirección | Destino | Propósito |
|---|---|---|
| `SUBSCRIBE` | `/topic/room/{roomId}/lobby` | Lista actualizada de jugadores y avatares conectados en sala de espera. |
| `SEND` | `/app/room/{roomId}/lobby/sync` | Solicita la emisión inmediata del listado completo de jugadores en el lobby. |
| `SUBSCRIBE` | `/topic/room/{roomId}/status` | Cambios de estado de la sala (`WAITING_LOBBY`, `QUESTION_IN_PROGRESS`, `ROULETTE_TIME`, `FINISHED`). |
| `SUBSCRIBE` | `/topic/room/{roomId}/question` | Difusión de la pregunta activa, índice `X de N` y su temporizador (sin exponer `correctAnswer`). |
| `SEND` | `/app/room/{roomId}/answer` | Envío de respuesta del aprendiz (`{ playerId, questionId, value, clientTimestamp }`). |
| `SUBSCRIBE` | `/topic/room/{roomId}/results` | Respuesta correcta revelada, explicación pedagógica y clasificación parcial Top 5. |
| `SEND` | `/app/room/{roomId}/results` | Disparo por STOMP del cierre de pregunta y difusión de resultados parciales. |
| `SUBSCRIBE` | `/topic/room/{roomId}/roulette` | Aprendiz seleccionado por la ruleta y su respuesta registrada. |
| `SEND` | `/app/room/{roomId}/moderator/roulette` | Activación por STOMP del giro de la ruleta pedagógica. |
| `SEND` | `/app/room/{roomId}/moderator/next` | Avance por STOMP a la siguiente pregunta de la partida. |
| `SUBSCRIBE` | `/topic/room/{roomId}/podium` | Podio final Top 3 al cierre de la sesión, habilitando la descarga del diploma bilingüe. |
| `SEND` | `/app/room/{roomId}/moderator/finish` | Cierre por STOMP de la partida y publicación del podio final. |
| `SUBSCRIBE` | `/topic/room/{roomId}/reactions` | Efectos interactivos globales en tiempo real (saludos `👋` y lluvia de confeti `🎉`). |
| `SEND` | `/app/room/{roomId}/reaction` | Emisión de reacción interactiva con control de cooldown. |

---

## Requisitos Previos

- **Java JDK 17+**
- **MySQL 8.0+** *(opcional si se ejecuta con el perfil en memoria `h2`)*
- **Cloudflare Tunnel (`cloudflared`)** *(requerido únicamente en el evento en vivo para proveer HTTPS/WSS a dispositivos móviles)*

---

## Instalación y Puesta en Marcha

### 1. Inicializar la Base de Datos (Producción con MySQL)

```bash
mysql -u root -p < database/schema.sql
mysql -u root -p english_day < database/questions_seed.sql
```

### 2. Compilar y Ejecutar el Servidor

#### Modo Producción (MySQL)
```bash
cd backend
./mvnw clean package -DskipTests
java -jar -Dspring.profiles.active=prod -Dserver.port=8080 target/english-day-0.0.1-SNAPSHOT.jar
```
*(Variables de entorno soportadas: `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`, `PORT`).*

#### Modo Rápido / Autónomo (Base de datos H2 en memoria)
Si deseas ejecutar la plataforma de inmediato sin configurar MySQL externo:
```bash
cd backend
./mvnw spring-boot:run -Dspring-boot.run.arguments="--spring.profiles.active=h2"
```

#### Inicio Automatizado con un Solo Comando (Backend + Túnel Cloudflare)
El repositorio incluye scripts de automatización para levantar tanto el servidor como el túnel HTTPS:
```bash
# Levantar Backend + Cloudflare Tunnel simultáneamente
./scripts/start-all.sh

# O levantar únicamente el túnel de Cloudflare
./scripts/start-tunnel.sh
```

### 3. Acceso a las Interfaces

Una vez iniciado el servidor en el puerto `8080`:
- **Vista del Aprendiz (Móvil / Navegador):** `http://localhost:8080/`
- **Panel del Moderador / Proyector:** `http://localhost:8080/moderator`

### 4. Exposición Segura HTTPS para el Evento (Cloudflare Tunnel)

Para permitir que los aprendices se conecten desde sus teléfonos móviles y utilicen el reconocimiento de voz por micrófono (que exige contexto seguro HTTPS):

**Opción A: Túnel administrado con Token (Recomendado para dominio propio, ej: `https://englishup.jmj.agency`)**
```bash
cloudflared tunnel run --token <TU_TOKEN_CLOUDFLARE>
```

**Opción B: Quick Tunnel temporal**
```bash
cloudflared tunnel --url http://localhost:8080
```

> **Nota sobre Caché en Cloudflare:** El servidor emite cabeceras `Cache-Control: no-cache, must-revalidate` y recursos con versionado (`?v=...`). Adicionalmente, en el panel de Cloudflare (*Caching → Cache Rules*), se recomienda configurar una regla con **Elegibilidad de caché: Omitir caché (Bypass cache)** para garantizar que todos los dispositivos carguen siempre la versión en vivo al instante.

Comparte la URL HTTPS generada (o amplía el código QR en pantalla gigante haciendo clic sobre él) con los aprendices en el auditorio.

---

## Ejecución de Pruebas Automatizadas

### Pruebas Unitarias, de Integración y Concurrencia (Backend)
```bash
cd backend
./mvnw test
```

### Suite End-to-End (Flujo Completo REST + STOMP WebSockets)
Con el servidor en ejecución en `http://localhost:8080`:
```bash
node QA/e2e_verification_suite.mjs
```

---

## Créditos y Reconocimientos

- **Equipo de Desarrollo:** Aprendices de la tecnología en **Análisis y Desarrollo de Software (ADSO)** del **Servicio Nacional de Aprendizaje (SENA)**.
- **Tutoría Metodológica & Asistencia Técnica:** Inteligencia Artificial (utilizada responsable y éticamente como tutor pedagógico, mentor técnico y guía paso a paso en la arquitectura, implementación y calidad del proyecto).
- **Iniciativa Institucional:** Jornada y evento de bilingüismo **English Up 2026** — Regional Distrito Capital.
