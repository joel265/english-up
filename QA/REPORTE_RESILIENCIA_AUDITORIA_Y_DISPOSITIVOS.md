# Informe de Pruebas de Resiliencia, Auditoría Pedagógica y Usabilidad Móvil
**Plataforma Web Interactiva de Gamificación — English Up (SENA ADSO)**  
**Subequipos:** QA & Contenido Pedagógico + Frontend (Desarrollo Web)  
**Tareas Cubiertas:**
* **QA Tarea 7:** Pruebas de resiliencia (caída de Wi-Fi, cierre de pestaña, ingreso de datos inválidos — Plan de Pruebas §3.4).
* **QA Tarea 8:** Auditoría ortográfica/gramatical del banco de inglés y pruebas de usabilidad en vivo (Actividad A7 / Hito H4).
* **Frontend Tarea 13:** Pruebas en dispositivos móviles y viewports responsivos (Chrome Android / Safari iOS — RNF-01, RNF-03, RNF-05).

---

## 1. Pruebas de Manejo de Excepciones y Resiliencia (QA Tarea 7 — Plan de Pruebas §3.4)

Se verificó el comportamiento del sistema ante condiciones adversas de red, sesiones interrumpidas y cargas malformadas:

| ID | Escenario de Resiliencia | Acción Simulada | Respuesta del Sistema (Frontend + Backend) | Resultado |
|---|---|---|---|---|
| **RES-01** | **Caída temporal de Wi-Fi o pérdida de WebSocket en mitad de partida** | Se cierra abruptamente la conexión WebSocket STOMP mientras el jugador está en `#lobbyCard` o `#questionCard`. | El listener `lobbySocket.addEventListener("close", ...)` detecta la desconexión y muestra el mensaje accesible (`role="status"`): *"Se perdió la conexión con la partida. Actualiza la página para volver a ingresar."* El jugador conserva su sesión en `sessionStorage` (`englishDayPlayerSession`). | ✅ APROBADO |
| **RES-02** | **Cierre o recarga accidental de la pestaña del Moderador (`F5`)** | El moderador recarga `/moderator.html` con una sala activa en curso. | El panel incluye el control **Reconectar PIN (`GET /api/rooms/{pin}`)** (Contrato §3 y Anexo A.4). Al ingresar el PIN de 6 dígitos, el moderador recupera el `roomId`, el `status` actual, el conteo de aprendices (`playerCount`) y vuelve a suscribirse a los 6 topics STOMP sin perder los puntajes persistidos en BD. | ✅ APROBADO |
| **RES-03** | **Envío duplicado de respuesta en la misma pregunta (Doble clic / Retraso de red)** | Un cliente envía dos tramas `SEND` a `/app/room/{roomId}/answer` para el mismo `(playerId, questionId)`. | En Frontend, `hasSubmittedAnswer = true` y `lockQuestionInputs()` deshabilitan los controles al primer envío. En Backend, `AnswerService#processAnswer` verifica `existsByPlayer_PlayerIdAndQuestion_QuestionId(playerId, questionId)` y omite el duplicado sin alterar el puntaje (Bitácora #7). | ✅ APROBADO |
| **RES-04** | **Ingreso de datos inválidos en registro (`POST /api/rooms/{pin}/join`)** | 1. PIN con menos de 6 dígitos o letras.<br>2. PIN inexistente (`000000`).<br>3. Nickname vacío o solo espacios.<br>4. Sala que ya no está en `WAITING_LOBBY`. | 1. Frontend filtra dígitos numéricos (`/^\d{6}$/`) y muestra error inmediato.<br>2. Backend responde `404 ROOM_NOT_FOUND` (`"PIN no encontrado"`).<br>3. Backend valida `@NotBlank` y responde `400 VALIDATION_ERROR`.<br>4. Backend responde `400 INVALID_ROOM_STATE` (`"La sala no está aceptando nuevos jugadores"`). | ✅ APROBADO |
| **RES-05** | **Fallo o bloqueo del micrófono en preguntas `VOICE` (Plan de Contingencia)** | El navegador móvil carece de `SpeechRecognition` o el usuario rechaza el permiso (`not-allowed` / `network`). | `join.js#activateVoiceTextFallback` captura el evento `onerror`, oculta el botón de micrófono, presenta la alerta del Contrato Anexo B.4 (*"No pudimos usar el micrófono. Escribe tu respuesta."*) y activa `#textInputUI` para que el aprendiz conteste por teclado con la misma validez y puntaje. | ✅ APROBADO |
| **RES-06** | **Seguridad anti-trampa en payload de pregunta (Contrato §1 regla 13 y §4.3)** | Inspección de tramas WebSocket en `/topic/room/{roomId}/question` y validación en cliente (`validateQuestion`). | `QuestionBroadcastResponse` excluye por diseño el campo `correctAnswer`. Además, `join.js#validateQuestion` rechaza cualquier payload que contenga `correctAnswer` o carezca de `questionId` UUID v4 válido. | ✅ APROBADO |
| **RES-07** | **Restauración automática de sesión completa ante recarga (`GET /api/rooms/{pin}/state`)** | Un aprendiz o moderador recarga la página durante una pregunta activa, resultados, ruleta o podio final. | El endpoint `/api/rooms/{pin}/state` retorna `RoomSyncStateResponse` con el estado exacto de la sala, pregunta actual, segundos restantes, respuesta enviada por el aprendiz, último resultado, ruleta y podio, restaurando la pantalla sin intervención manual. | ✅ APROBADO |
| **RES-08** | **Prevención de caché obsoleta en CDN Cloudflare y navegadores** | Diferentes computadores y móviles acceden al dominio público `https://englishup.jmj.agency` tras un despliegue. | `WebConfig.java` emite `Cache-Control: no-cache, must-revalidate`, los HTML incluyen meta tags anti-caché y los scripts usan versionado estricto (`?v=...`) junto con regla *Bypass Cache* en Cloudflare. | ✅ APROBADO |

---

## 2. Auditoría Ortográfica y Gramatical del Banco de Preguntas en Inglés (QA Tarea 8)

Se realizó la revisión lingüística y pedagógica completa de las **24 preguntas oficiales** cargadas en `database/questions_seed.sql` y `QuestionDataInitializer.java`, estructuradas de forma equilibrada en las **6 temáticas oficiales del área de bilingüismo**:

### 2.1 Resumen por Temática y Mecánica Pedagógica (24 Preguntas)
| Temática Gramatical Oficial | Nivel MCER | Mecánicas Cubiertas (`QuestionType`) | Cantidad | Ítems (`order`) | Tiempo Base (`timeLimitSeconds`) |
|---|---|---|:---:|:---:|:---:|
| **1. Simple Present (Affirmative, Negative, Interrogative)** | A2 – B1 | `MULTIPLE_CHOICE`, `TEXT_INPUT`, `WORD_BLOCKS` | 5 | #1, #3, #15, #16, #17 | 15 s / 20 s |
| **2. Verb To Be (`am`, `is`, `are`)** | A2 | `MULTIPLE_CHOICE`, `TEXT_INPUT`, `WORD_BLOCKS`, `VOICE` | 4 | #2, #4, #5, #10 | 15 s / 20 s / 25 s |
| **3. Pronouns (Subject & Object Pronouns)** | A2 | `MULTIPLE_CHOICE`, `TEXT_INPUT`, `WORD_BLOCKS`, `VOICE` | 4 | #6, #7, #8, #9 | 15 s / 20 s / 25 s |
| **4. Possessive Adjectives (`my`, `his`, `our`, etc.)** | A2 | `MULTIPLE_CHOICE`, `TEXT_INPUT`, `WORD_BLOCKS`, `VOICE` | 4 | #11, #12, #13, #14 | 15 s / 20 s / 25 s |
| **5. Present Continuous (Affirmative, Negative, Interrogative)** | A2 – B1 | `MULTIPLE_CHOICE`, `TEXT_INPUT`, `WORD_BLOCKS`, `VOICE` | 5 | #18, #19, #20, #21, #22 | 15 s / 20 s / 25 s |
| **6. WH Questions (`What`, `Where`, etc.)** | A2 | `MULTIPLE_CHOICE`, `VOICE` | 2 | #23, #24 | 15 s / 25 s |

### 2.2 Verificación de Reglas Lingüísticas y Técnicas (Contrato Anexo B.6 y Bitácora #12–#13)
1. **Ortografía estadounidense (`en-US`):** Todas las oraciones utilizan ortografía estándar estadounidense compatible con el motor `recognition.lang = "en-US"`.
2. **Ausencia de homófonos ambiguos en `VOICE`:** Las 6 frases de pronunciación (#5, #9, #14, #22, #24) están diseñadas sin homófonos ambiguos, garantizando transcripción determinista en Web Speech API.
3. **Soporte de variantes múltiples (`|`) y sentido gramatical:** En `TEXT_INPUT` y `WORD_BLOCKS`, `correctAnswer` admite tanto la respuesta corta como la oración completa o variantes con adverbios al inicio/final (ej. `I am happy today|I am happy|Today I am happy`).
4. **Puntuación, contracciones y corrector palabra por palabra:** Todas las contracciones negativas (`doesn't`, `don't`, `isn't`) están escapadas en SQL, son expandidas canónicamente por `AnswerService` y cuentan con retroalimentación pedagógica palabra por palabra al cierre de ronda.

---

## 3. Pruebas en Dispositivos Móviles y Usabilidad en Vivo (Frontend Tarea 13 & QA Tarea 8 — RNF-01, RNF-03, RNF-05)

Se validó el comportamiento responsivo, táctil y de compatibilidad en perfiles móviles (**Chrome Android — 360×800 px**, **Safari iOS — 390×844 px**, y pantalla pequeña **320×568 px**) así como en **Pantalla Proyector de Escritorio (1440×900 px)**:

| Criterio Evaluado | Requerimiento | Verificación Técnica Realizada | Estado |
|---|---|---|---|
| **Tamaño mínimo de objetivos táctiles (`>= 48x48 px`)** | RNF-01 | Botones de PIN (`.pin-digit`: `min-height: 3rem` / `48px`), opciones de selección múltiple (`.multiple-choice-option`: `min-height: 4.5rem` / `72px`), fichas de Word Blocks (`.word-block`: `min-height: 3rem` / `48px`), botón de envío (`.answer-submit-button`: `min-height: 3.25rem` / `52px`) y micrófono (`.voice-mic-button`: `88x88 px`). | ✅ CUMPLE |
| **Diseño Mobile-First sin desbordamiento horizontal (`320px - 480px`)** | RNF-01, RNF-03 | Contenedor `main` adaptable y grillas flexibles (`minmax(0, 1fr)`). Cero scroll horizontal en `320px`, `375px` y `412px`, incluyendo la vista de Podio Olímpico y el modal de Certificado. | ✅ CUMPLE |
| **Teclado móvil sin autocompletado tramposo en `TEXT_INPUT`** | RF-06, CP-06 | El campo `#textAnswer` declara explícitamente `autocomplete="off" autocapitalize="none" autocorrect="off" spellcheck="false" inputmode="text"`. | ✅ CUMPLE |
| **Teclado numérico directo en PIN de 6 dígitos** | RF-02, RNF-01 | Los 6 campos `.pin-digit` usan `inputmode="numeric" pattern="[0-9]" maxlength="1"` con avance automático al escribir, retroceso con `Backspace` y soporte de pegado (`paste`) de 6 dígitos. | ✅ CUMPLE |
| **Alto contraste, fondo 2K y QR ampliable a pantalla completa (Proyector)** | RNF-03 | Paleta *Tactile Gamified Arcade*, fondo ilustrado en resolución nativa 2K (2752×1536), tipografía **Rubik** (`700`–`900`) y modal de código QR ampliado (`360x360 px`) para escaneo desde cualquier distancia en el auditorio. | ✅ CUMPLE |
| **Compatibilidad Web Speech API + Modo Edición + Fallback HTTPS/WSS** | RNF-05, RF-07 | Soporte para `window.SpeechRecognition` (Chrome Android) y `window.webkitSpeechRecognition` (Safari iOS) sobre Cloudflare Tunnel (`https://` / `wss://`), preservación de transcripción al repetir, edición manual por teclado y contingencia automática. | ✅ CUMPLE |
| **Descarga universal de Diploma PNG y acreditación de Punto Positivo** | CU-06 | Generación síncrona en Canvas 2D (`1600x1100 px`) y descarga mediante `Blob` (`application/octet-stream` + `URL.createObjectURL`), compatible con Chrome, Safari macOS/iOS, Edge y Firefox, más visor directo *"Guardar en Fotos"* y compartición por WhatsApp. | ✅ CUMPLE |
