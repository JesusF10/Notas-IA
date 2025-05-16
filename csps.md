# Problemas de Satisfacción de Restricciones (CSPs)

## ¿Qué son los CSPs?

*   Los Problemas de Satisfacción de Restricciones (CSPs) son un **subconjunto especializado de problemas de búsqueda** .
*   A diferencia de los problemas de búsqueda estándar, donde el estado puede ser una "caja negra" y la prueba de objetivo cualquier función, en los CSPs:
    *   El **estado se define por variables** (Xi) con valores de un dominio (D) .
    *   La **prueba de objetivo es un conjunto de restricciones** que especifican combinaciones de valores permitidas para subconjuntos de variables .
*   Los CSPs son una **forma de representación formal** que permite algoritmos de propósito general más potentes que los algoritmos de búsqueda estándar .
*   Se **especializan en problemas de identificación**, donde el **objetivo en sí mismo es importante**, no la ruta para alcanzarlo .

## Componentes Clave de un CSP

*   **Variables**: Las entidades que necesitan un valor asignado .
*   **Dominios**: El conjunto de posibles valores que cada variable puede tomar .
*   **Restricciones**: Las reglas que limitan las combinaciones de valores que las variables pueden tomar .

## Ejemplos de CSPs

*   **Coloración de Mapas**: Variables son regiones, dominios son colores, restricción es que regiones adyacentes deben tener colores diferentes .
*   **Problema de las N-Reinas**: Colocar N reinas en un tablero N×N sin que se ataquen entre sí .
*   **Criptoaritmética**: Asignar dígitos a letras para que una ecuación aritmética sea válida .
*   **Sudoku**: Llenar una cuadrícula con dígitos cumpliendo restricciones de fila, columna y región .
*   **Algoritmo Waltz**: Interpretar dibujos lineales de poliedros como objetos 3D, donde las intersecciones son variables y las adyacentes imponen restricciones .

## Variedades de CSPs y Restricciones

*   **Variables Discretas**:
    *   Dominios finitos: O(d^n) asignaciones completas. Ej: Satisfacibilidad Booleana (NP-completo) .
    *   Dominios infinitos (enteros, cadenas): Ej: Programación de tareas .
*   **Variables Continuas**: Ej: Programación de observaciones del Telescopio Hubble. Las restricciones lineales son solubles .
*   **Variedades de Restricciones**:
    *   **Unarias**: Involucran una sola variable (equivalente a reducir dominios) .
    *   **Binarias**: Involucran pares de variables .
    *   **De Orden Superior**: Involucran 3 o más variables (Ej: restricciones de columna en criptoaritmética) .
    *   **Preferencias (Restricciones Blandas)**: Asignan un costo a las asignaciones; conducen a problemas de optimización con restricciones .

## CSPs en el Mundo Real

Los CSPs modelan muchos problemas prácticos, incluyendo :
*   Problemas de asignación (ej: quién enseña qué clase).
*   Problemas de horarios (ej: qué clase se ofrece cuándo y dónde).
*   Configuración de hardware.
*   Programación de transporte.
*   Diseño de circuitos.
*   Diagnóstico de fallas.

## Solución de CSPs

*   **Formulación de Búsqueda Estándar**: El estado es la asignación parcial, el sucesor asigna un valor a una variable no asignada, el objetivo es una asignación completa que satisface todas las restricciones .
*   **Búsqueda por Retroceso (Backtracking Search)**:
    *   Es el **algoritmo básico no informado** para resolver CSPs .
    *   Idea 1: Asignar **una variable a la vez** (las asignaciones son conmutativas) .
    *   Idea 2: **Verificar restricciones a medida que se avanza** ("prueba de objetivo incremental") . Considera solo valores que no entran en conflicto con asignaciones previas .
    *   Es una **búsqueda en profundidad (DFS) con estas mejoras** .
    *   Equivale a DFS + ordenamiento de variables + fallo al violar restricción .

## Mejora de la Búsqueda por Retroceso

Ideas de propósito general que mejoran significativamente la velocidad :
*   **Ordenamiento**: ¿Qué variable asignar después? ¿En qué orden probar sus valores? 
*   **Filtrado**: ¿Podemos detectar fallas inevitables tempranamente? 
*   **Estructura**: ¿Podemos explotar la estructura del problema (ej: grafos de restricción)? 

## Técnicas de Filtrado (Propagación de Restricciones)

Mantener seguimiento de los dominios para variables no asignadas y eliminar malas opciones .
*   **Verificación Adelantada (Forward Checking)**: Eliminar valores que violan una restricción cuando se añaden a la asignación existente . Propaga información de variables asignadas a no asignadas .
*   **Propagación de Restricciones**: Razonar de restricción a restricción .
*   **Consistencia de Arco (Arc Consistency)**: Asegurar que para cada arco X→Y (variable X a variable Y), para cada valor posible de X, existe *algún* valor en Y que no viola la restricción. Si X pierde un valor, los vecinos de X deben ser re-verificados . La consistencia de arco **detecta fallas antes** que la verificación adelantada . Puede ejecutarse como preprocesador o después de cada asignación .
*   **Limitaciones de la Consistencia de Arco**: Aunque detecta fallas tempranamente, no garantiza que la asignación restante tenga solución . Puede quedar una, múltiples o ninguna solución (y no saberlo) . La consistencia de arco **aún se ejecuta dentro de una búsqueda por retroceso** .

## Heurísticas de Ordenamiento

*   **Ordenamiento de Variables: Valores Restantes Mínimos (MRV)**: Elegir la variable con la **menor cantidad de valores legales restantes** en su dominio . Es una estrategia de "fallo rápido" . También llamada variable "más restringida" .
*   **Ordenamiento de Valores: Valor Menos Restrictivo (LCV)**: Dada una variable elegida, seleccionar el **valor que elimina la menor cantidad de valores** en las variables restantes . Requiere cierta computación para determinar .
*   Combinar estas ideas de ordenamiento hace que problemas grandes como el de 1000 reinas sean factibles .
