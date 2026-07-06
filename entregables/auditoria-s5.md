## Parte A — Ajustes al backlog de S4

**Ajuste 1 — Añadir un spike de investigación técnica sobre la sincronización antes de dar sus historias por listas para desarrollo.**
Antes de considerar listas para desarrollo las historias de sincronización —US-12 (Sincronizar nueva tarea con fecha como evento), US-13 (Propagar cambios y eliminación al evento vinculado) y US-14 (Gestión de errores y reintentos de sincronización)—, incorporar un spike que valide el comportamiento real de la API de Google Calendar.
*Motivo:* el PRD identifica la sincronización como el mayor riesgo del MVP y recomienda validarla mediante un spike antes de descomponerla. Comprometer esas historias sin esa validación previa es prematuro.

**Ajuste 2 — Completar dos escenarios ausentes en las historias de sincronización.**
Incorporar a US-12 (Sincronizar nueva tarea con fecha como evento) y US-14 (Gestión de errores y reintentos de sincronización) dos escenarios no cubiertos: (a) garantizar la idempotencia, de modo que un reintento no genere el mismo evento dos veces; y (b) definir el tratamiento de los errores que no se resuelven reintentando, como un permiso de Google caducado o revocado, que exigen volver a solicitar autorización.
*Motivo:* ambos escenarios surgieron durante el ejercicio de poke-holes. Sin cubrirlos se producen eventos duplicados y no se alcanza el objetivo del PRD de mantener las sincronizaciones fallidas por debajo del 5%.

**Ajuste 3 — Añadir la historia de la pantalla de bienvenida (onboarding).**
Incorporar una historia para la pantalla de bienvenida que se muestra al usuario tras completar el registro.
*Motivo:* el PRD la especifica de forma explícita, pero no quedó reflejada en el backlog generado.

**Ajuste 4 — Retirar el bloqueo por intentos fallidos de inicio de sesión.**
Retirar de US-02 (Inicio de sesión) el bloqueo tras varios intentos fallidos, o indicar de forma explícita que queda fuera del MVP.
*Motivo:* no figura en el PRD; fue añadido por la IA e incrementa el alcance del MVP respecto a lo definido en el documento.

## Parte B. Auditoría de documentación del proyecto

### 1. README de proyecto
**Estado:** Completa
**Observación:** El README raíz permite arrancar de cero (requisitos, instalación, `.env.example`, generación de `APP_KEY`, migraciones y arranque), y los comandos coinciden con los `package.json`. El frontend no tiene README propio, aunque su arranque queda cubierto por el README raíz.

### 2. Descripción de la arquitectura general
**Estado:** Parcial
**Observación:** No existe un documento de arquitectura. La información está dispersa entre la tabla de stack de `CLAUDE.md` y el árbol de carpetas de `backend/README.md`, pero ninguna pieza explica el sistema en conjunto ni cómo se relacionan backend, frontend y OpenSpec.

### 3. Documentación de la API
**Estado:** Parcial
**Observación:** El README raíz incluye una tabla de endpoints y las specs de OpenSpec describen el comportamiento mediante escenarios. Sin embargo, no hay un contrato formal (OpenAPI o Swagger) ni ejemplos de payloads, por lo que sirve para orientarse pero no para integrarse sin leer el código.

### 4. Docstrings y comentarios en código
**Estado:** Parcial
**Observación:** Los controllers del backend incluyen un comentario breve al inicio de cada método con la ruta y su propósito, lo cual ayuda pero resulta limitado. El modelo `User` y el código del frontend no tienen comentarios explicativos. El proyecto tampoco cuenta con una capa de servicios, ya que la lógica reside directamente en los controllers.

### 5. Decisiones técnicas registradas (ADRs, Architecture Decision Records)
**Estado:** Inexistente
**Observación:** No existen ADRs ni documento equivalente. Decisiones relevantes como la autenticación por access tokens, la elección de SQLite, la capa de transformers o la ausencia de una capa de servicios no están justificadas en ningún sitio.

### 6. Guía operacional
**Estado:** Inexistente
**Observación:** No hay instrucciones de despliegue, runbooks ni documentación de resolución de incidencias. Solo se documenta el arranque en local, que corresponde a la puesta en marcha para desarrollo y no a la operación.

### 7. Convenciones de código
**Estado:** Parcial
**Observación:** `CLAUDE.md` documenta las convenciones del backend (lógica en controllers, validación con VineJS, salida a través de transformer, subpath imports) y el código las cumple. No obstante, solo cubren el backend, el frontend carece de convenciones y de configuración de linter, y la nota de `CLAUDE.md` sobre un `AGENTS.md` enlazado por symlink no se cumple, porque ese archivo no existe en el repositorio.

### 8. Especificación OpenSpec y su trazabilidad con el código
**Estado:** Parcial
**Observación:** La spec de `authentication` refleja fielmente el código, incluidos los códigos de estado, la forma `{ user, token }` y la revocación del token. La spec de `users`, en cambio, documenta `GET /api/v1/users/active` como implementado, e incluso afirma describir lo ya implementado, pero ese endpoint no existe en el código, ni como ruta ni como método. La especificación va por delante del código.

## Top 3 carencias
1. **Divergencia entre la especificación y el código.** La spec de `users` afirma documentar lo ya implementado, pero `GET /api/v1/users/active` no existe. Una spec que no coincide con el código no se puede usar como referencia fiable.
2. **Ausencia de documento de arquitectura.** Falta una visión de conjunto, con un diagrama, que explique cómo se relacionan backend, frontend y OpenSpec y cuál es el flujo de autenticación.
3. **Ausencia de ADRs.** Las decisiones técnicas clave no tienen justificación registrada, por lo que no se sabe por qué se eligieron ni qué alternativas se descartaron.

## Top 3 fortalezas
1. **Arranque reproducible desde cero.** El README raíz cubre requisitos, configuración de `.env`, migraciones y arranque de backend y frontend, con comandos que coinciden con los `package.json`.
2. **Especificación de autenticación fiel al código.** `authentication/spec.md` describe con precisión el comportamiento real, incluidos los códigos de estado y las formas de respuesta.
3. **Convenciones de backend claras y respetadas.** Están documentadas en `CLAUDE.md` y `backend/README.md`, y el código las aplica de forma consistente.

## Paso 7. Exploración de formatos de documentación
- **Diagrama C4:** aporta una vista visual y por niveles de zoom (contexto, contenedores, componentes, código) que permite entender la estructura del sistema y las relaciones entre sus componentes.
- **ADR (Architecture Decision Records):** como su nombre lo indica, registra las decisiones técnicas, su contexto y las alternativas descartadas, de modo que el porqué de esa decisión perdura en el tiempo.
- **Especificación OpenAPI:** es un documento escrito en YAML o JSON que define los endpoints, métodos, parámetros, respuestas y códigos de estado, mediante un formato que es legible por máquinas.
