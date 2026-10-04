# Prompts

**Modelo:** Sonnet 5.5 xHigh
**Herramienta:** Claude Code

escribe una especificacion y dejala en "/docs/spec-viva-ar.md". La spec debe describir exclusivamente un requerimiento de cuentas y acceso: registro, inicio de sesion y perfil. La descripcion es exclusivamente de lo que esta implementado hoy en el codigo. La spec debe incluir front y back: pantallas, rutas y proteccion de rutas controladores, modelos, validadores y middleware. La spec debe tener el siguiente formato: comienza con un ##Purpose de dos lineas describiendo para que existe esta capability. Luego una seccion ##Requirements y colgando ###Requirement en los que el sistema SHALL hacer algo. Bajo cada ###Requirement un ###Scenario describiendo un **WHEN** y **THEN** la precondicion se incluye en **WHEN** no hay **GIVEN**. Se debe escribir en español, pero palabras como SHALL, MUST, WHEN, THEN van en ingles y en mayuscula sostenida. La especificacion exclusivamente contiene comportamiento observable desde fuera, NO es una especificacion TECNICA.
