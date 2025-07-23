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

#### Posiciones Iniciales de Nodos en `index.html` (Sección `updateSankey`)

Para asegurar una visualización clara y evitar solapamientos, se han definido las siguientes posiciones iniciales para los nodos clave en el diagrama de Sankey. Estas coordenadas `nodeX` (horizontal) y `nodeY` (vertical) se encuentran en la función `updateSankey` dentro del script de `index.html`:

```javascript
            labels.forEach((label, i) => {

                if (label === 'Importación de energéticos primarios') {
                    nodeX[i] = 0.01;
                    nodeY[i] = 0.1;
                } else if (label === 'Variación de inventarios de Energéticos primarios') {
                    nodeX[i] = 0.01;
                    nodeY[i] = 0.3;
                } else if (label === 'Producción') {
                    nodeX[i] = 0.01;
                    nodeY[i] = 0.7;
                } else if (label === 'Oferta Total (Hub)') {
                    nodeX[i] = 0.3;
                    nodeY[i] = 0.4;
                } else if (label === 'Oferta Interna Bruta') {
                    nodeX[i] = 0.4;
                    nodeY[i] = 0.1;
                } else if (label === 'Exportación') {
                    nodeX[i] = 0.4;
                    nodeY[i] = 0.7;
                } else if (label === 'Consumo Propio del Sector') {
                    nodeX[i] = 0.4;
                    nodeY[i] = 0.8;
                } else if (label === 'Energía No Aprovechada') {
                    nodeX[i] = 0.4;
                    nodeY[i] = 0.9;
                } else if (label === 'Refinerías y Despuntadoras') {
                    nodeX[i] = 0.5;
                    nodeY[i] = 0.2;
                } else if (label === 'Plantas de Gas y Fraccionadoras') {
                    nodeX[i] = 0.5;
                    nodeY[i] = 0.3;
                } else if (label === 'Coquizadoras y Hornos') {
                    nodeX[i] = 0.5;
                    nodeY[i] = 0.4;
                } else if (label === 'Centrales Eléctricas') {
                    nodeX[i] = 0.6;
                    nodeY[i] = 0.03;
                }
            });
```

#### Ajuste del Tamaño de Fuente de las Etiquetas de Nodos

Para mejorar la legibilidad y permitir la inclusión de valores, el tamaño de la fuente de las etiquetas de los nodos se ha ajustado a `6` en la configuración del nodo dentro de la función `updateSankey`:

```javascript
                node: {
                    // ... otras propiedades ...
                    font: { size: 6 } // Tamaño de fuente de las etiquetas de los nodos
                },
```

## Nodos Padre

A continuación se presenta la lista de nodos padre utilizada en el archivo datos_energia_completo.json, junto con su respectivo id_padre:

* Oferta Interna Bruta (id_padre: 1)
* Oferta Total (id_padre: 2)
* Producción (id_padre: 3)
* Importación (id_padre: 4)
* Variación de Inventarios (id_padre: 5)
* Exportación (id_padre: 6)
* Energía No Aprovechada (id_padre: 7)
* Total Transformación (id_padre: 8)
* Coquizadoras y Hornos (id_padre: 9)
* Refinerías y Despuntadoras (id_padre: 10)
* Plantas de Gas y Fraccionadoras (id_padre: 11)
* Centrales Eléctricas (id_padre: 12)
* Carboeléctrica (id_padre: 13)
* Térmica Convencional (id_padre: 14)
* Combustión Interna (id_padre: 15)
* Turbogás (id_padre: 16)
* Ciclo Combinado (id_padre: 17)
* Nucleoeléctrica (id_padre: 18)
* Cogeneración (id_padre: 19)
* Geotérmica (id_padre: 20)
* Eólica (id_padre: 21)
* Solar Fotovoltaica (id_padre: 22)
* Total Consumo del Sector (id_padre: 23)
* Consumo Propio del Sector (id_padre: 24)
* Diferencia Estadística (id_padre: 25)
* Pérdidas técnicas por transporte, transmisión y distribución (id_padre: 26)
* Pérdidas en transporte y transmisión por energético (id_padre: 27)
* Pérdidas en distribución por energético (id_padre: 28)
* Pérdidas no técnicas por energético (id_padre: 29)
* Consumo final total (id_padre: 30)
* Consumo final no energético (id_padre: 31)
* Otras ramas económicas (id_padre: 32)
* Consumo final energético (id_padre: 33)
* Agropecuario (id_padre: 34)
* Industrial (id_padre: 35)
* Comercial (id_padre: 36)
* Residencial (id_padre: 37)
* Público (id_padre: 38)
* Transporte (id_padre: 39)
* Producción bruta energía secundaria (id_padre: 40)


  ### **Energéticos Primarios**


  1. Carbón mineral (id_hijo: 1)
  2. Petróleo crudo (id_hijo: 2)
  3. Condensados (id_hijo: 3)
  4. Gas natural (id_hijo: 4)
  5. Energía Nuclear (id_hijo: 5)
  6. Energia Hidraúlica (id_hijo: 6)
  7. Geoenergía (id_hijo: 7)
  8. Energía solar (id_hijo: 8)
  9. Energía eólica (id_hijo: 9)
  10. Bagazo de caña (id_hijo: 10)
  11. Leña (id_hijo: 11)
  12. Biogás (id_hijo: 12)
  13. Otros energéticos (id_hijo: 20)
  14. Energía Hidráulica (id_hijo: 23)

  ---

  ### **Energéticos Secundarios**

  1. Coque de carbón (id_hijo: 13)
  2. Coque de petróleo (id_hijo: 14)
  3. Gas licuado de petróleo (id_hijo: 15)
  4. Gasolinas y naftas (id_hijo: 16)
  5. Querosenos (id_hijo: 17)
  6. Diesel (id_hijo: 18)
  7. Combustóleo (id_hijo: 19)
  8. Gas natural seco (id_hijo: 21)
  9. Energía eléctrica (id_hijo: 22)

#### Relaciones y Flujos de Nodos (Detectados en `index.html` y `datos_energia_completo.json`):

* **Nodos Padre Principales:**

  * `Producción`
  * `Importación`
  * `Variación de Inventarios`
  * `Oferta Total (Hub)`: Actúa como un nodo centralizador para los energéticos primarios.
  * `Oferta Interna Bruta`
  * `Coquizadoras y Hornos`
  * `Refinerías y Despuntadoras`
  * `Plantas de Gas y Fraccionadoras`
  * `Centrales Eléctricas`
  * `Exportación`
  * `Energía No Aprovechada`
  * `Consumo Propio del Sector`
* **Flujos Actuales:**

  * `Producción` -> `Oferta Total (Hub)` (por cada energético primario)
  * `Importación` -> `Oferta Total (Hub)` (por cada energético primario)
  * `Variación de Inventarios` -> `Oferta Total (Hub)` (por cada energético primario, manejando valores positivos y negativos)
  * `Oferta Total (Hub)` -> `Oferta Interna Bruta` (por cada energético primario)
  * `Oferta Interna Bruta` -> `Coquizadoras y Hornos` (para Carbón mineral)
  * `Oferta Interna Bruta` -> `Refinerías y Despuntadoras` (para Petróleo crudo)
  * `Oferta Interna Bruta` -> `Plantas de Gas y Fraccionadoras` (para Gas natural y Condensados)
  * `Oferta Interna Bruta` -> `Centrales Eléctricas` (para energéticos primarios)
  * `Refinerías y Despuntadoras` -> `Centrales Eléctricas` (para Diesel, Combustóleo y Gas natural seco)
  * `Plantas de Gas y Fraccionadoras` -> `Centrales Eléctricas` (para Gas natural seco)
  * `Coquizadoras y Hornos` -> `Centrales Eléctricas` (para Coque de carbón)
  * `Oferta Total (Hub)` -> `Exportación` (por cada energético primario)
  * `Oferta Total (Hub)` -> `Energía No Aprovechada` (por cada energético primario)
  * `Oferta Total (Hub)` -> `Consumo Propio del Sector` (por cada energético primario)
* **Reglas de Colores:**

  * Los colores de los nodos padre y los enlaces se toman directamente de la propiedad `color` definida en cada "Nodo Padre" y "Nodo Hijo" en `datos_energia_completo.json`.
  * Se utiliza `typeof color === 'string' ? color : '#888'` para asegurar que el color sea un string válido, usando un gris por defecto si no lo es.
* **Patrones Detectados:**

  * Los "Nodos Hijo" se identifican por `Nodo Hijo` (nombre), `tipo` (Energía Primaria/Secundaria), `id_hijo` y `color`.
  * Los valores de flujo se obtienen por año (ej. `2010`, `2011`).
  * El `id_hijo` se utiliza para ordenar los nodos hijo dentro de cada nodo padre.
  * Los valores negativos en `Variación de Inventarios` se manejan con `Math.abs()` para el valor del enlace, pero se suman/restan al `primaryEnergyTotals` según su signo.

#### Creación de Flujos y Popups en `index.html`

Para asegurar la consistencia en la visualización de datos, es crucial seguir un patrón específico al agregar nuevos flujos al diagrama de Sankey. Cada flujo (o `link`) debe estar acompañado de un popup informativo que muestre el nombre del energético y su valor en petajoules (PJ).

A continuación, se describe el proceso para añadir un nuevo flujo en la función `updateSankey` de `index.html`:

1.  **Inicialización de Arrays:**
    Asegúrate de que los siguientes arrays estén inicializados al principio de la función `updateSankey`:

    ```javascript
    const source = [];
    const target = [];
    const value = [];
    const linkColors = [];
    const linkCustomdata = [];
    ```

2.  **Poblar los Arrays de Flujo:**
    Cuando proceses los datos para crear un nuevo flujo, debes añadir la información correspondiente a cada uno de los arrays. Es fundamental que el valor del flujo se trate con `Math.abs()` para asegurar que sea siempre positivo, y que el texto del popup se añada al array `linkCustomdata`.

    **Ejemplo de Implementación:**

    ```javascript
    // Suponiendo que 'child' es un nodo hijo con los datos del flujo
    const flowValue = child[year];
    const childName = child['Nodo Hijo'];
    const childColor = child.color;
    const parentIndex = nodeMap.get('Nombre del Nodo Padre');
    const childIndex = nodeMap.get(childName);

    if (flowValue !== undefined && flowValue !== 0) {
        source.push(parentIndex);
        target.push(childIndex);
        value.push(Math.abs(flowValue));
        linkColors.push(typeof childColor === 'string' ? childColor : '#888');
        linkCustomdata.push(`${childName}: ${Math.abs(flowValue).toLocaleString()} PJ`);
    }
    ```

3.  **Configuración del `link` en Plotly:**
    Finalmente, al definir el objeto `data` para el gráfico de Plotly, asegúrate de que la propiedad `link` esté configurada para utilizar los arrays que has poblado. El `hovertemplate` se encargará de mostrar el popup con la información de `linkCustomdata`.

    ```javascript
    const data = {
        type: "sankey",
        orientation: "h",
        node: {
            // ... propiedades del nodo
        },
        link: {
            source: source,
            target: target,
            value: value,
            color: linkColors,
            customdata: linkCustomdata,
            hovertemplate: '%{customdata}<extra></extra>'
        }
    };
    ```

Al seguir estos pasos, cualquier nuevo flujo que se añada al diagrama de Sankey tendrá un popup informativo coherente con el resto de la visualización.

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
