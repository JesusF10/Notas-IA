# Inferencia Probabilística

## Introducción

La inferencia probabilística es el proceso de **calcular alguna cantidad a partir de una distribución de probabilidad conjunta** . Se le describe como la "verdadera lógica para este mundo" y el "cálculo de probabilidades, que toma en cuenta la magnitud de la probabilidad que está, o debería estar, en la mente de un hombre razonable" . Un ejemplo de su aplicación práctica es que Google, según un ingeniero senior, utiliza el **filtrado bayesiano** (una forma de inferencia probabilística) de manera tan fundamental como otras compañías usan una sentencia `if` .

En general, en la inferencia probabilística, las variables se dividen en tres grupos :
*   **Query (Q o X)**: Las variables sobre las que se desea realizar la consulta .
*   **Evidence (E)**: Las variables cuyos valores se conocen .
*   **Hidden (H o Y)**: Las variables que no son ni de consulta ni de evidencia .

Los ejemplos clásicos para ilustrar la inferencia incluyen la **Red de Alarma** (que relaciona robo, terremoto, alarma y las llamadas de John y Mary)  y el **Dominio del Tráfico** (con variables como Lluvia, Tráfico y Llegar Tarde a Clase) .

## Métodos de Inferencia Exacta

Cuando se busca un resultado preciso, se utilizan métodos de inferencia exacta.

### Inferencia por Enumeración

Este método es conceptualmente directo si se tiene **tiempo ilimitado** . La receta básica es :
1.  Declarar las probabilidades incondicionales necesarias .
2.  Enumerar todas las probabilidades atómicas necesarias .
3.  Calcular la **suma de los productos** .

Por ejemplo, para calcular P(+b, +j, +m) en la Red de Alarma, se sumaría sobre todas las combinaciones de E y A, expandiendo la probabilidad conjunta según la estructura de la red bayesiana .

La principal desventaja de la inferencia por enumeración es su **lentitud** . Esto se debe a que **se une (calcula) toda la distribución conjunta** antes de sumar (marginalizar) las variables ocultas . Esto lleva a **repetir mucho trabajo** . Para la Red de Alarma, podría implicar el cálculo de un número muy grande de filas (aproximadamente 10^16) .

### Eliminación de Variables (Variable Elimination - VE)

La Eliminación de Variables es una **optimización** de la inferencia por enumeración . La idea clave es **intercalar la unión y la marginalización** . Aunque sigue siendo un problema NP-hard, suele ser **mucho más rápida** que la inferencia por enumeración . Requiere un álgebra específica para combinar "factores" (arrays multidimensionales) .

Los **factores** son representaciones de distribuciones de probabilidad o familias de condicionales . Cuando se escribe P(Y1...YN | X1...XM), generalmente se refiere a un factor, que es un array multidimensional con valores P(y1...yN | x1...xM) . Las variables con valores asignados se representan como dimensiones faltantes (seleccionadas) en el array .

Las **operaciones** básicas en Eliminación de Variables son:
1.  **Unir Factores (Join Factors)**: Similar a una unión de base de datos . Se toman todos los factores que mencionan una variable en común y se crea un nuevo factor sobre la unión de todas las variables involucradas . El cálculo para cada entrada es el **producto punto a punto** . Por ejemplo, unir P(R) y P(T|R) en el Dominio del Tráfico produce un factor P(R,T) .
2.  **Eliminar (Eliminate)**: También llamada marginalización . Se toma un factor y se **suma sobre una variable**, reduciendo su tamaño. Es una operación de proyección . Por ejemplo, sumar sobre R en P(R,T) produce P(T) .

El **proceso general** de Eliminación de Variables es :
*   Comenzar con los **factores iniciales**, que son las CPTs (Tablas de Probabilidad Condicional) locales de cada nodo .
*   Si hay evidencia, los valores conocidos se **seleccionan** en los factores iniciales .
*   Mientras haya **variables ocultas** (que no son Q ni evidencia) :
    *   Elegir una variable oculta H .
    *   Unir todos los factores que mencionan a H .
    *   Eliminar (sumar) H del factor resultante .
*   Unir todos los factores restantes . Si había evidencia, este resultado es una distribución conjunta seleccionada de las variables de consulta y evidencia (ej. P(L, +r) para P(L | +r)) .
*   **Normalizar** el resultado final para obtener la probabilidad condicional de la consulta dada la evidencia P(Q | E) .

## Métodos de Inferencia Aproximada

Para problemas donde la inferencia exacta es demasiado lenta, se pueden usar métodos de **inferencia aproximada**, a menudo basados en **muestreo (sampling)** . La idea básica es :
*   Tomar N muestras de una distribución de muestreo S .
*   Calcular una probabilidad posterior aproximada .
*   Confiar en que esto converge a la verdadera probabilidad P a medida que N aumenta .

El muestreo es útil tanto para el aprendizaje (cuando no se conoce la distribución) como para la inferencia (cuando obtener una muestra es más rápido que el cálculo exacto) .

### Muestreo Prior (Prior Sampling)

Este método genera muestras directamente de la red bayesiana, siguiendo las probabilidades definidas en las CPTs . El proceso genera muestras con la probabilidad conjunta de la red bayesiana . Si N es el número total de muestras y N(e) es el número de muestras de un evento 'e', entonces N(e)/N se aproxima a P(e) . Este procedimiento es consistente . Permite estimar cualquier probabilidad contando las ocurrencias en las muestras y normalizando . Es rápido, ya que se pueden usar menos muestras si hay menos tiempo, aunque con menor precisión .

### Muestreo por Rechazo (Rejection Sampling)

Si se quiere calcular una probabilidad condicional como P(C | +s), se pueden generar muestras usando el muestreo prior, pero **ignorando (rechazando) las muestras que no coinciden con la evidencia** (donde S no es +s) . Solo se cuentan las muestras que cumplen la condición de la evidencia . Este método también es consistente para probabilidades condicionales .
El problema principal es que **si la evidencia es poco probable, se rechazan muchas muestras**, lo que lo hace ineficiente . No aprovecha la evidencia durante el proceso de muestreo .

### Muestreo por Ponderación de Verosimilitud (Likelihood Weighting)

Para abordar el problema del rechazo, se **fijan las variables de evidencia y se muestrean las demás** . Sin embargo, esto hace que la distribución de muestreo no sea consistente . La solución es **ponderar cada muestra por la probabilidad de la evidencia dados sus padres** .

Con esta ponderación, la distribución de muestreo ponderada es consistente . El muestreo por ponderación de verosimilitud es bueno porque toma la evidencia en cuenta al generar la muestra, influyendo en las variables "aguas abajo" (downstream) . Por ejemplo, el valor de W (WetGrass) será elegido basándose en los valores de S (Sprinkler) y R (Rain) si estos son evidencia . Sin embargo, no resuelve todos los problemas: **la evidencia influye en las variables aguas abajo, pero no en las variables "aguas arriba" (upstream)** (ej. C - Cloudy no se vuelve más probable para coincidir con la evidencia) .

### Cadenas de Markov Monte Carlo (MCMC)

Una idea alternativa es crear muestras que son similares a la anterior, en lugar de muestrear desde cero . El procedimiento implica **remuestrear una variable a la vez, condicionada a todas las demás variables, manteniendo la evidencia fija** . Las muestras generadas no son independientes, pero los promedios de las muestras siguen siendo estimadores consistentes . La ventaja es que **tanto las variables aguas arriba como las aguas abajo condicionan sobre la evidencia** .
