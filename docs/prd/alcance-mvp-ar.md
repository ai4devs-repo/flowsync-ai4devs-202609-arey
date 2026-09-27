# FlowSync — Alcance del MVP

## 1. Problema

Los equipos remotos pequeños pierden tiempo y foco por una sola pregunta que se repite constantemente: **"¿en qué estás?"**. Hoy esa pregunta se resuelve de dos formas, ambas caras:

- **La daily de sincronización.** De los 15 minutos, la mitad se va en la ronda de "en qué está cada uno". La daily no desaparece —la parte de bloqueos se sigue necesitando y este MVP no la toca— pero esa ronda de estado es pura fricción: información que podría verse sin reunión.
- **La interrupción por chat.** Entre daily y daily, cuando alguien necesita saber si un módulo está libre, pregunta por Slack. Eso cuesta doble: el tiempo de quien pregunta y el de quien es interrumpido a mitad de otra cosa.

El costo no es hipotético: dos personas del equipo tocaron el mismo módulo la misma semana porque una empezó sin que la otra lo supiera. Dos días de trabajo perdidos por no tener, en un vistazo, la respuesta a "¿esto ya lo está haciendo alguien?".

Quien paga el precio de esta falta de visibilidad son los pares —no un manager pidiendo reportes hacia arriba, que aquí no existe—: el que interrumpe, el que es interrumpido, y los dos que descubren tarde que iban detrás de lo mismo.

## 2. Usuarios

- **Perfil:** equipos remotos pequeños, de 3 a 10 personas, con roles planos. En el MVP todos ven y editan lo mismo; no hay jerarquía de permisos.
- **Caso de estudio de referencia (no un cliente real):** un equipo de producto SaaS de 6 personas repartidas en 3 husos horarios. Hoy usan un gestor de tareas pesado más una daily de 15 minutos por videollamada, y sienten el costo descrito arriba.
- **Fuera de este perfil:** organizaciones con más de un equipo, o personas que participan en varios equipos a la vez. FlowSync en este MVP asume **un espacio único compartido**, sin entidad "equipo" — es un supuesto explícito, no un olvido, y limita a quién le sirve el producto tal como está planteado.

## 3. Propuesta de valor

**Ver el estado del equipo sin preguntar y sin reunión, actualizado por quien hace el trabajo, en el momento en que le sirve a él mismo.**

Se sostiene sobre tres ideas, no negociables entre sí:

- **Frescura de la tarea, no vigilancia de la persona.** El estado que se ve es el de la tarea ("en curso", "hecha"...), nunca quién está conectado o activo ahora mismo. Eso último es vigilancia y queda fuera a propósito.
- **Resumen que espera, no aviso que interrumpe.** El caso de uso es "llego por la mañana, o vuelvo de una reunión, y veo qué se movió". Sin notificaciones push: la herramienta se consulta, no avisa.
- **Dos clics, cero fricción, beneficio inmediato para quien escribe.** Actualizar el estado no puede costar más que la interrupción que reemplaza. Y quien lo teclea no lo hace por altruismo: esa misma lista es su cola de trabajo — la mira para decidir qué toma a continuación — y de paso deja de recibir preguntas sobre cómo va. Si el beneficio fuera solo para los demás, no se sostendría.

La decisión que esto cambia, concretamente: no empezar algo que otro ya está tocando, y elegir lo siguiente sabiendo qué está libre. Es el único motivo por el que vale la pena el esfuerzo de mantener el estado al día.

**Riesgo #1, asumido explícitamente:** si la información se queda vieja, el producto pierde el sentido. No hay red de seguridad de proceso (nadie está obligado a actualizar) — la única mitigación es que actualizar sea tan barato que no actualizar sea la opción rara. Este es el punto a validar en la primera semana de uso real, no un detalle secundario.

**Criterio de éxito (una semana de uso real):** el equipo cancela la ronda de "en qué estás" de la daily y nadie pide que vuelva. Si la siguen haciendo igual que antes, el MVP no funcionó, sin importar cuánto se haya usado la app.

## 4. Alcance

FlowSync MVP es una vertical fina y completa de punta a punta — una sola capability bien terminada, no un andamiaje amplio a medias. Sustituye al gestor de tareas actual del equipo (no convive con él: la doble actualización es como muere esta categoría de producto).

**Incluido:**

- **Autenticación y espacio compartido** (ya existe en el repo: signup, login, logout, perfil). Todas las personas autenticadas comparten el mismo espacio único de tareas.
- **Gestión de tareas propia**, sin flujos de configuración ni campos obligatorios más allá de lo mínimo:
  - Crear una tarea con **título**, **estado** y **fecha de vencimiento**.
  - **Autoasignación únicamente**: cada persona crea y gestiona las tareas de las que es responsable. No existe la acción de crear o reasignar una tarea a otra persona — evita que la herramienta se convierta en un canal de reparto de trabajo, que es justo la fricción de gestión que se quiere evitar.
  - Cambiar el estado de una tarea propia en dos clics, sin pasos intermedios.
- **Cuatro estados de tarea:** Pendiente, En curso, Bloqueada, Hecha. Se incluye "Bloqueada" como estado explícito de la tarea aunque la conversación sobre el motivo del bloqueo siga siendo verbal (en la parte de la daily que no desaparece) — el estado permite que el resto del equipo vea de un vistazo que algo está parado, sin tener que preguntar.
- **Una tarea "en curso" a la vez por persona.** Fuerza un foco único y hace que la señal "en qué está cada uno" sea literal y sin ambigüedad — es el corazón de la propuesta de valor, no un límite técnico incidental.
- **Vista de lista compartida**, filtrable por estado, para poder centrarse en lo pendiente o revisar rápidamente qué se movió. Pensada para consultarse varias veces al día (no solo al llegar por la mañana), acorde a un equipo en 3 husos horarios donde "empezar el día" ocurre en momentos distintos para cada persona.
- **Indicador visual de vencimiento pasado**, para detectar de un vistazo qué se retrasó — sin que dispare ninguna notificación (eso es explícitamente NO-alcance, ver más abajo).

## 5. NO-alcance

Cada exclusión responde a una razón concreta, no a "no dio tiempo":

- **Notificaciones push.** Contradicen la propuesta de valor: FlowSync es un resumen que se consulta, no un aviso que interrumpe. Añadir push reintroduce exactamente la interrupción que el producto existe para evitar.
- **Integración con Slack.** Mantenerse fuera evita que el estado de una tarea se derive o se sincronice con un canal externo; el estado vive y se edita solo en FlowSync. Es además la puerta de entrada más obvia hacia notificaciones, que ya se excluyen por la razón anterior.
- **Roles y permisos avanzados.** El equipo objetivo tiene una estructura plana y nadie reporta a nadie a estos efectos. Construir jerarquía de permisos sería resolver un problema que este usuario no tiene, a costa del tiempo que se le puede dar a la vertical fina.
- **Analítica y reporting.** No hay un manager que cobre valor de un dashboard: quien se beneficia son los pares, en el momento, mirando la lista. Un reporte histórico serviría a una audiencia (dirección) que este MVP no tiene.
- **Comentarios en tareas.** Abrir un hilo de conversación por tarea empieza a convertir la lista en un sustituto de chat/Jira, con el costo de mantenimiento y de "campos que llenar" que el producto rechaza a propósito. Si hace falta discutir algo, sigue siendo tema de la daily o de un mensaje directo.
- **Sprints, estimaciones, épicas, backlog priorizado.** Un equipo que necesite planear a ese nivel no es el usuario de este MVP; construir esto sería reconstruir Jira, exactamente lo que "menos rollo que Jira" busca evitar.
- **Asignar tareas a otra persona.** Se decidió limitar a autoasignación (ver bloque 4): la fuerza de la propuesta de valor está en que quien escribe el estado es quien cobra el beneficio inmediato de tenerlo actualizado. Permitir asignar a otros diluye ese incentivo y abre la puerta a que la herramienta se use para repartir trabajo, no para reflejarlo.
- **Derivar el estado de señales externas** (Git/PRs, CI, calendario). Es un producto distinto, con integraciones y OAuth de terceros, y contradice la premisa de que el estado lo teclea la persona en segundos porque es su cola de trabajo — no algo que se infiere por fuera.
- **Multi-equipo / pertenencia a varios equipos.** El MVP asume un espacio único compartido, sin entidad "equipo". Resolver aislamiento entre equipos es un problema de otro producto (o de una v2); aquí se anota como supuesto explícito de diseño, no como omisión.

**Los dos números**. Cuántas cosas propuso la IA meter dentro del alcance, y cuántas quedaron dentro después de tu recorte. Tal cual salieron, sin redondear ni explicar: Propuso 6 cosas. Deje las 6 cosas.

**Tres cosas que dejaste fuera, y por qué cada una**. El porqué tiene una forma concreta: qué hipótesis del producto no ayuda a validar. "No da tiempo" no vale, porque no es una decisión de producto: es una excusa de calendario, y mañana deja de ser cierta: La respuesta de la IA me parecio acertada.

**La exclusión de la que menos seguro estás, y qué tendría que pasar para que entrara**. Lo que interesa es qué dos cosas se contradecían: lo que te pedían contra lo que veías, lo barato contra lo que valida, lo que enamora contra lo que se puede sostener: NO estoy seguro respecto a la asignacion, lo deje como que cada persona asigna las tareas, pero si existe un lead, este podria asignar o re-asignar una tarea.