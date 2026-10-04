## Purpose

Acceso y login. Permite que una persona cree una cuenta en FlowSync, acceda a ella con su email y contraseña y consulte su perfil.
Garantiza que solo quien tiene una sesión válida ve las pantallas privadas y que la sesión se mantiene o se cierra de forma predecible.

## Requirements

### Requirement: Registro de cuenta

El sistema SHALL permitir crear una cuenta desde la pantalla `/register` indicando nombre completo (opcional), email, contraseña y confirmación de contraseña. Al completarse el registro, el sistema MUST iniciar sesión automáticamente.

#### Scenario: Registro correcto

- **WHEN** una persona sin sesión rellena email, contraseña (entre 8 y 32 caracteres) y la misma contraseña en la confirmación, y pulsa "Crear cuenta"
- **THEN** el sistema crea la cuenta, inicia sesión y muestra la pantalla de perfil con los datos de la nueva cuenta

#### Scenario: Registro sin nombre

- **WHEN** una persona deja vacío el nombre completo y envía el resto de datos válidos
- **THEN** el sistema crea la cuenta igualmente y el perfil muestra "Sin nombre" como nombre

### Requirement: Validación del registro

El sistema SHALL rechazar el registro si los datos no son válidos y MUST mostrar el motivo en castellano junto al campo afectado. El sistema MUST NOT crear ninguna cuenta ni iniciar sesión cuando el registro es rechazado.

#### Scenario: Email ya registrado

- **WHEN** una persona intenta registrarse con un email que ya pertenece a otra cuenta
- **THEN** el sistema muestra bajo el campo email "Ese email ya está registrado. Inicia sesión en su lugar." y permanece en `/register`

#### Scenario: Email con formato inválido o demasiado largo

- **WHEN** una persona envía un email sin formato válido o de más de 254 caracteres
- **THEN** el sistema muestra un error bajo el campo email y no crea la cuenta

#### Scenario: Contraseña fuera de longitud

- **WHEN** una persona envía una contraseña de menos de 8 o de más de 32 caracteres
- **THEN** el sistema muestra un error bajo el campo contraseña indicando la longitud permitida y no crea la cuenta

#### Scenario: Las contraseñas no coinciden

- **WHEN** la contraseña y su confirmación son distintas
- **THEN** el sistema muestra "Las contraseñas no coinciden." bajo el campo de confirmación sin llegar a crear la cuenta

#### Scenario: Campo obligatorio vacío

- **WHEN** una persona envía el formulario sin email o sin contraseña
- **THEN** el sistema indica qué campo falta por rellenar y no crea la cuenta

### Requirement: Inicio de sesión

El sistema SHALL permitir iniciar sesión desde la pantalla `/login` con email y contraseña. Con credenciales correctas, el sistema MUST abrir una sesión y llevar a la persona a su perfil.

#### Scenario: Credenciales correctas

- **WHEN** una persona sin sesión introduce el email y la contraseña de una cuenta existente y pulsa "Entrar"
- **THEN** el sistema abre la sesión y muestra la pantalla de perfil

#### Scenario: Credenciales incorrectas

- **WHEN** una persona introduce un email inexistente o una contraseña errónea
- **THEN** el sistema muestra "El email o la contraseña no son correctos." sin indicar cuál de los dos falló y permanece en `/login`

#### Scenario: Datos mal formados

- **WHEN** una persona envía el formulario de login con un email de formato inválido o sin contraseña
- **THEN** el sistema muestra el error junto al campo afectado y no abre sesión

#### Scenario: Envío en curso

- **WHEN** una persona pulsa "Entrar" o "Crear cuenta" y la petición está en curso
- **THEN** el botón queda deshabilitado y muestra "Entrando…" o "Creando cuenta…" hasta recibir respuesta

### Requirement: Navegación entre registro e inicio de sesión

El sistema SHALL ofrecer en cada pantalla pública un enlace a la otra.

#### Scenario: Ir a registro desde login

- **WHEN** una persona sin cuenta está en `/login` y pulsa "Crea una"
- **THEN** el sistema muestra la pantalla `/register`

#### Scenario: Ir a login desde registro

- **WHEN** una persona con cuenta está en `/register` y pulsa "Inicia sesión"
- **THEN** el sistema muestra la pantalla `/login`

### Requirement: Consulta del perfil

El sistema SHALL mostrar en `/profile` los datos de la cuenta con sesión activa: iniciales en un avatar, nombre completo (o "Sin nombre"), email y fecha de alta como "Miembro desde".

#### Scenario: Perfil con nombre

- **WHEN** una persona con sesión y nombre "Ada Lovelace" abre `/profile`
- **THEN** el sistema muestra el avatar con las iniciales "AL", el nombre, el email y la fecha de alta en formato largo en castellano

#### Scenario: Iniciales sin nombre

- **WHEN** una persona con sesión y sin nombre abre `/profile`
- **THEN** el avatar muestra las dos primeras letras de su email en mayúsculas

### Requirement: Protección de pantallas privadas

El sistema SHALL permitir el acceso a `/profile` únicamente con una sesión válida. Mientras se comprueba una sesión guardada, el sistema MUST mostrar una pantalla de carga y MUST NOT redirigir.

#### Scenario: Acceso sin sesión

- **WHEN** una persona sin sesión abre `/profile`
- **THEN** el sistema la redirige a `/login`

#### Scenario: Comprobación en curso

- **WHEN** una persona con sesión guardada recarga la página en `/profile`
- **THEN** el sistema muestra una pantalla de carga hasta validar la sesión y después muestra el perfil sin pasar por `/login`

### Requirement: Pantallas solo para personas sin sesión

El sistema SHALL impedir que una persona con sesión activa vea `/login` y `/register`.

#### Scenario: Login con sesión activa

- **WHEN** una persona con sesión activa abre `/login` o `/register`
- **THEN** el sistema la redirige a `/profile`

### Requirement: Rutas desconocidas

El sistema SHALL redirigir cualquier ruta no definida a `/profile`.

#### Scenario: Ruta inexistente con sesión

- **WHEN** una persona con sesión abre una ruta que no existe
- **THEN** el sistema muestra `/profile`

#### Scenario: Ruta inexistente sin sesión

- **WHEN** una persona sin sesión abre una ruta que no existe
- **THEN** el sistema la lleva a `/profile` y de ahí a `/login`

### Requirement: Persistencia de la sesión

El sistema SHALL mantener la sesión al recargar la página o volver a abrir el navegador mientras el servidor siga reconociéndola.

#### Scenario: Recarga con sesión válida

- **WHEN** una persona con sesión activa recarga la página
- **THEN** el sistema conserva la sesión y muestra sus datos sin pedir credenciales

#### Scenario: Sesión rechazada por el servidor

- **WHEN** una persona abre la aplicación con una sesión guardada que el servidor ya no reconoce
- **THEN** el sistema descarta la sesión, la lleva a `/login` y muestra "Tu sesión ha caducado. Vuelve a iniciar sesión."

#### Scenario: Servidor no disponible al restaurar la sesión

- **WHEN** una persona abre la aplicación con una sesión guardada y el servidor no responde
- **THEN** el sistema la lleva a `/login`, muestra el motivo del fallo y conserva la sesión guardada para poder recuperarla al recargar cuando el servidor vuelva

### Requirement: Cierre de sesión

El sistema SHALL permitir cerrar sesión desde `/profile` con el botón "Cerrar sesión" e invalidar la sesión en el servidor.

#### Scenario: Cierre correcto

- **WHEN** una persona con sesión pulsa "Cerrar sesión"
- **THEN** el sistema elimina la sesión, la lleva a `/login` y la sesión anterior deja de ser válida

#### Scenario: Cierre con el servidor caído

- **WHEN** una persona pulsa "Cerrar sesión" y el servidor no responde o ya no reconocía la sesión
- **THEN** el sistema cierra igualmente la sesión en el navegador y la lleva a `/login`

### Requirement: Acceso a la API con sesión

La API SHALL exigir una sesión válida para consultar el perfil (`GET /api/v1/account/profile`) y cerrar sesión (`POST /api/v1/account/logout`), y MUST permitir sin sesión el registro (`POST /api/v1/auth/signup`) y el inicio de sesión (`POST /api/v1/auth/login`). Todas las respuestas SHALL ser JSON, con los datos útiles dentro de `data`.

#### Scenario: Petición protegida sin sesión

- **WHEN** se llama a `/api/v1/account/profile` o `/api/v1/account/logout` sin credencial o con una credencial inválida
- **THEN** la API responde 401 en JSON

#### Scenario: Registro o login correcto por API

- **WHEN** se llama a `/api/v1/auth/signup` o `/api/v1/auth/login` con datos válidos
- **THEN** la API responde con `data.user` (id, fullName, email, createdAt, updatedAt, initials) y `data.token`, y MUST NOT incluir la contraseña

#### Scenario: Datos inválidos por API

- **WHEN** se llama a signup o login con datos que no cumplen las reglas
- **THEN** la API responde 422 con la lista de errores por campo; si las credenciales de login son erróneas, responde 400

#### Scenario: Consulta de perfil por API

- **WHEN** se llama a `/api/v1/account/profile` con una sesión válida
- **THEN** la API responde con `data` conteniendo los datos de la cuenta propietaria de la sesión

### Requirement: Contraseñas y emails

El sistema MUST NOT almacenar ni devolver contraseñas en claro y SHALL tratar cada email como único entre las cuentas.

#### Scenario: Cuenta creada

- **WHEN** se crea una cuenta
- **THEN** ninguna respuesta de la API contiene la contraseña y no puede existir otra cuenta con el mismo email
