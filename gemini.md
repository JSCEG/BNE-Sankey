# Ingeniería de Contexto para el Proyecto Gemini Sankey

La ingeniería de contexto es la práctica de diseñar sistemas que proporcionan a los modelos de IA, como Gemini, la información precisa, las herramientas y la comprensión que necesitan para realizar una tarea de manera efectiva. A diferencia de la ingeniería de prompts, que se centra en instrucciones únicas, la ingeniería de contexto gestiona todo el ecosistema de información que rodea a la IA, incluyendo el historial de conversación, datos externos y salidas de herramientas, para asegurar que la IA tenga todo el contexto relevante para responder eficazmente a través de múltiples interacciones y tareas.

Este documento describe los pasos para aplicar la ingeniería de contexto al modificar o implementar partes de este proyecto, asegurando que Gemini tenga la información necesaria para asistir de manera óptima.

## Pasos para la Ingeniería de Contexto:

### 1. Entender el Requerimiento del Usuario
Antes de cualquier acción, asegúrate de comprender completamente la solicitud del usuario. Esto incluye:
- **Claridad:** ¿Qué se espera lograr?
- **Alcance:** ¿Qué partes del proyecto se verán afectadas?
- **Restricciones:** ¿Hay alguna limitación o requisito específico?

### 2. Recopilar Contexto Relevante del Proyecto
Para que Gemini pueda trabajar eficazmente, necesita acceso a la información pertinente del proyecto. Esto puede incluir:
- **Estructura del Proyecto:** Utiliza `list_directory` y `glob` para entender la organización de archivos y carpetas.
- **Archivos de Configuración:** Lee `requirements.md`, `design.md`, o cualquier otro archivo de configuración relevante con `read_file`.
- **Código Existente:** Si la tarea implica modificar código, lee los archivos de código fuente relevantes con `read_file` o `read_many_files` para entender la lógica actual, las convenciones de codificación y las dependencias.
- **Documentación:** Revisa cualquier documentación existente que pueda proporcionar información sobre el diseño o la funcionalidad.
- **Pruebas:** Identifica y lee los archivos de prueba (`tests/`) para entender cómo se verifica la funcionalidad y para asegurar que cualquier cambio no rompa las pruebas existentes.

### 3. Analizar y Planificar la Solución
Con el contexto recopilado, formula un plan detallado. Esto puede implicar:
- **Identificación de Patrones:** Observa los patrones de diseño y las convenciones de codificación existentes en el proyecto para asegurar la coherencia.
- **Diseño de la Solución:** Define cómo se abordará el requerimiento, considerando la integración con el código existente.
- **Pasos Detallados:** Desglosa la tarea en pasos más pequeños y manejables.
- **Verificación:** Piensa en cómo se verificará la solución (ej. pruebas unitarias, pruebas de integración, linting).

#### Criterios Específicos del Proyecto:
- **Estructura de Nodos:** Cada nodo padre tiene la misma estructura de nodos hijos.
- **Identificación de Nodos Hijos:** Cada nodo hijo tiene un color y un `id_hijo` únicos.
- **Reutilización de Nodos Hijos:** Los nodos hijos se repiten en cada nodo padre con diferentes valores.
- **Orden de Listado:** Se utiliza un `id_orden` para listar los nodos hijos en un orden específico.

#### Relaciones y Flujos de Nodos (Detectados en `index.html` y `datos_energia_completo.json`):

*   **Nodos Padre Principales:**
    *   `Producción`
    *   `Importación`
    *   `Variación de Inventarios`
    *   `Oferta Total (Hub)`: Actúa como un nodo centralizador para los energéticos primarios.
    *   `Oferta Interna Bruta`
    *   `Coquizadoras y Hornos`
    *   `Refinerías y Despuntadoras`
    *   `Plantas de Gas y Fraccionadoras`
    *   `Centrales Eléctricas`
    *   `Exportación`
    *   `Energía No Aprovechada`
    *   `Consumo Propio del Sector`

*   **Flujos Actuales:**
    *   `Producción` -> `Oferta Total (Hub)` (por cada energético primario)
    *   `Importación` -> `Oferta Total (Hub)` (por cada energético primario)
    *   `Variación de Inventarios` -> `Oferta Total (Hub)` (por cada energético primario, manejando valores positivos y negativos)
    *   `Oferta Total (Hub)` -> `Oferta Interna Bruta` (por cada energético primario)
    *   `Oferta Interna Bruta` -> `Coquizadoras y Hornos` (para Carbón mineral)
    *   `Oferta Interna Bruta` -> `Refinerías y Despuntadoras` (para Petróleo crudo)
    *   `Oferta Interna Bruta` -> `Plantas de Gas y Fraccionadoras` (para Gas natural y Condensados)
    *   `Oferta Interna Bruta` -> `Centrales Eléctricas` (para energéticos primarios)
    *   `Oferta Total (Hub)` -> `Exportación` (por cada energético primario)
    *   `Oferta Total (Hub)` -> `Energía No Aprovechada` (por cada energético primario)
    *   `Oferta Total (Hub)` -> `Consumo Propio del Sector` (por cada energético primario)

*   **Reglas de Colores:**
    *   Los colores de los nodos padre y los enlaces se toman directamente de la propiedad `color` definida en cada "Nodo Padre" y "Nodo Hijo" en `datos_energia_completo.json`.
    *   Se utiliza `typeof color === 'string' ? color : '#888'` para asegurar que el color sea un string válido, usando un gris por defecto si no lo es.

*   **Patrones Detectados:**
    *   Los "Nodos Hijo" se identifican por `Nodo Hijo` (nombre), `tipo` (Energía Primaria/Secundaria), `id_hijo` y `color`.
    *   Los valores de flujo se obtienen por año (ej. `2010`, `2011`).
    *   El `id_hijo` se utiliza para ordenar los nodos hijo dentro de cada nodo padre.
    *   Los valores negativos en `Variación de Inventarios` se manejan con `Math.abs()` para el valor del enlace, pero se suman/restan al `primaryEnergyTotals` según su signo.

### 4. Implementar los Cambios
Utiliza las herramientas disponibles para aplicar los cambios según el plan:
- **Modificación de Archivos:** Usa `replace` para cambios específicos en el contenido de los archivos o `write_file` para crear nuevos archivos o sobrescribir contenido.
- **Ejecución de Comandos:** Utiliza `run_shell_command` para tareas como instalar dependencias, ejecutar scripts de construcción o formatear código.

### 5. Verificar y Validar
Después de implementar los cambios, es crucial verificar que todo funcione como se espera y que se adhiera a los estándares del proyecto:
- **Ejecutar Pruebas:** Si existen, ejecuta las pruebas del proyecto para asegurar que los cambios no introdujeron regresiones.
- **Linting y Formateo:** Ejecuta las herramientas de linting y formateo del proyecto para mantener la calidad del código y la coherencia del estilo.
- **Construcción del Proyecto:** Si aplica, construye el proyecto para asegurar que no haya errores de compilación.
- **Revisión Manual:** Realiza una revisión manual de los cambios para detectar cualquier problema que las pruebas automatizadas no puedan capturar.

### 6. Iterar y Refinar
Si la verificación revela problemas o si el usuario solicita ajustes, repite los pasos anteriores, utilizando el feedback para refinar la solución. Mantén un registro de las interacciones y los cambios para mantener el contexto actualizado.

Al seguir estos pasos, se busca maximizar la eficiencia y precisión de la asistencia de Gemini en el desarrollo del proyecto Gemini Sankey.