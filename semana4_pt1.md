# Fundamentos de Búsquedas

Un problema de búsqueda consiste en:

- **Espacio de estados:** Conjunto de todas las configuraciones posibles del problema.
- **Modelo de transición:** Describe cómo pasar de un estado a otro mediante acciones. Esto se representa mediante una **función sucesor**, que define los posibles estados resultantes de una acción en un estado dado. Las **acciones** tienen **resultados**.
- **Estado inicial:** Estado en el que comienza la búsqueda.
- **Prueba de objetivo:** Condición que determina si un estado es una solución al problema.
- **Función de costo de ruta:** Asigna un costo a cada acción, permitiendo determinar el costo total de una secuencia de acciones.

El objetivo de la búsqueda es encontrar una **solución**, que es una **secuencia de acciones (un plan)** que transforma el estado inicial en un estado objetivo.

## Tipos de problemas de búsqueda

- **Totalmente observable y determinista:** El agente siempre sabe exactamente en qué estado se encuentra y cuál será el resultado de sus acciones.
- **Problema de un solo estado de creencia:** El agente tiene una única hipótesis sobre el estado actual.
- **No observable (sensorless o conforme):** El agente no tiene información sobre el estado inicial y no puede percibir el estado actual.
- **Parcialmente observable / no determinista (contingencia):** El agente solo tiene información parcial sobre el estado y las acciones pueden tener múltiples resultados posibles, requiriendo **intercalar búsqueda y ejecución**.
- **Espacio de estados desconocido (exploración):** El agente no conoce el espacio de estados por adelantado y debe **ejecutar primero** para descubrirlo.

## Modelar un problema de búsqueda

Modelar un problema de búsqueda implica definir estos componentes de forma formal para que un algoritmo de búsqueda pueda encontrar una solución. Se construye un **árbol de búsqueda** (what if tree) expandiendo los posibles planes y manteniendo una **frontera** de planes no expandidos. Es importante considerar las **consecuencias futuras de una acción** al buscar una secuencia de acciones.

## Gráfico del espacio de estados

Se menciona el **gráfico del espacio de estados**, que es una representación matemática del problema donde:

- Los **nodos** son los estados.
- Los **arcos** representan las transiciones entre estados.

Sin embargo, este gráfico puede ser muy grande o incluso infinito, por lo que no siempre se construye completo en memoria.

## Espacio de estados vs Árbol de búsqueda

Es fundamental distinguir entre:

- **Espacio de estados:** Una abstracción del mundo.
- **Árbol de búsqueda:** Representación de las posibles secuencias de acciones.

Un **nodo en el espacio de estados** representa un estado del mundo, mientras que un **nodo en el árbol de búsqueda** representa un camino hacia un estado. Un mismo estado puede ser alcanzado por múltiples nodos en el árbol de búsqueda.

# Búsqueda en Árbol

La búsqueda en árbol es un enfoque fundamental para resolver problemas de búsqueda. La idea principal es explorar las posibles secuencias de acciones a partir del estado inicial, construyendo un **árbol de "qué pasaría si..."**.

## Conceptos importantes en la búsqueda en árbol

- **Nodo raíz:** El árbol de búsqueda comienza con el **estado inicial** del problema como su nodo raíz.
- **Expansión:** Un nodo del árbol se **expande** al generar sus nodos hijos, que corresponden a los estados que se pueden alcanzar aplicando todas las acciones posibles al estado del nodo padre.
- **Frontera (o fringe):** Conjunto de todos los nodos que han sido generados pero aún no han sido expandidos. La frontera representa los posibles caminos o planes que aún se están considerando.
- **Estrategia de exploración:** Determina qué nodo de la frontera se expandirá a continuación. Diferentes estrategias dan lugar a distintos algoritmos de búsqueda, como:
    - **Búsqueda en amplitud (BFS)**
    - **Búsqueda en profundidad (DFS)**
    - **Búsqueda de costo uniforme (UCS)**
    - **Búsqueda A*** (A estrella)

- **Nodos:** Cada **nodo en el árbol de búsqueda** contiene:
    - Un **estado**.
    - Una **ruta** (secuencia de acciones) desde el estado inicial hasta ese estado.
    - Un **costo acumulado**, si estamos considerando costos de acciones.

    Es importante recordar que el mismo **estado** puede ser alcanzado por **múltiples nodos en el árbol de búsqueda**, a través de diferentes secuencias de acciones.

## Proceso general de la búsqueda en árbol

1. Inicializar la **frontera** con el nodo raíz, que representa el estado inicial.
2. Mientras la frontera no esté vacía:
    - Seleccionar un nodo de la frontera según la **estrategia de exploración**.
    - Si el nodo contiene un **estado objetivo**, se ha encontrado una **solución** (la ruta desde la raíz hasta este nodo).
    - Si no, **expandir** el nodo generando sus sucesores y agregarlos a la frontera.


# Búsqueda en Grafos

La **búsqueda en grafos** es una mejora clave sobre la **búsqueda en árboles**, diseñada para **evitar expandir repetidamente los mismos estados** cuando pueden ser alcanzados por diferentes caminos. Esta mejora es fundamental en espacios de estados donde es común que existan múltiples rutas hacia un mismo estado.

---

## Motivación

En una búsqueda en árbol, cada nodo lleva consigo la historia completa de su ruta, y **no hay memoria global sobre qué estados ya se visitaron**. Esto puede llevar a:
- **Repetición innecesaria de trabajo.**
- **Crecimiento exponencial del árbol**, explorando múltiples rutas hacia los mismos estados.

La búsqueda en grafos **resuelve este problema** mediante un conjunto adicional: el conjunto de **estados explorados** (a veces llamado "conjunto cerrado").

---

## Proceso General de Búsqueda en Grafos

1. Inicializar la **frontera** con el nodo raíz (el estado inicial).
2. Inicializar un **conjunto vacío de estados explorados**.
3. Mientras la frontera no esté vacía:
    - Seleccionar un nodo de la frontera (según la estrategia de búsqueda).
    - Sea `estado_actual` el estado contenido en este nodo.
    - Si `estado_actual` es el **estado objetivo**, reconstruir el camino como solución.
    - Si `estado_actual` ya está en el conjunto de **estados explorados**, ignorar el nodo y continuar.
    - Si no ha sido explorado:
        - Agregar `estado_actual` al conjunto de **estados explorados**.
        - **Expandir el nodo**, generando todos sus sucesores.
        - Agregar los sucesores a la **frontera** (si no han sido explorados o si se descubre un camino más barato, en búsquedas óptimas como UCS o A*).

---

## Comparación: Búsqueda en Árbol vs Búsqueda en Grafos

| Característica                  | Búsqueda en Árbol           | Búsqueda en Grafos              |
|----------------------------------|-----------------------------|---------------------------------|
| Estructura                      | Sin memoria global          | Usa un conjunto de explorados  |
| Estados duplicados              | **Sí**                      | **Evitados**                    |
| Eficiencia                       | Baja (explora estados repetidos) | Mayor (evita redundancia)  |
| Completitud                     | Depende del algoritmo       | Depende del algoritmo           |
| Optimalidad                     | Depende del algoritmo       | Depende del algoritmo y manejo de costo |

---

## Completitud y Optimalidad

### Completitud
- **Graph search** preserva la completitud siempre que el algoritmo base sea completo.
- Evitar ciclos y expansión repetida de estados mejora las chances de encontrar una solución en espacios grandes.

### Optimalidad
- En **BFS con costos uniformes**, graph search preserva la optimalidad porque siempre expande primero el nodo de menor profundidad.
- En **UCS y A***, la optimalidad se preserva **solo si**:
    - Al descubrir un mejor camino a un estado ya explorado, se actualiza el costo y el estado vuelve a considerarse para expansión (esta es la parte delicada de la implementación correcta).

---

## ¿Cuándo puede ser mejor usar Búsqueda en Árbol?

Aunque rara vez es preferible, hay casos donde **tree search** puede ser más sencilla o eficiente:

- **Espacios de estados muy pequeños.** Si no hay caminos redundantes, el costo de mantener el conjunto de explorados puede ser innecesario.
- **Problemas acíclicos.** Si el espacio de estados es un árbol natural (sin ciclos o múltiples rutas a un mismo estado), graph search no aporta beneficio.
- **Ambientes de memoria restringida.** Si no puedes permitirte guardar el historial de estados explorados, tree search puede ser la única opción.

---

## ¿Graph Search es casi siempre mejor?

Sí, **casi siempre** graph search es mejor, especialmente cuando:

- Hay **caminos redundantes** entre estados.
- Hay **ciclos**.
- El espacio de estados es grande.

En la práctica, la sobrecarga de mantener `explorados` es casi siempre compensada por el ahorro en nodos repetidos, **salvo en espacios extremadamente simples**.

