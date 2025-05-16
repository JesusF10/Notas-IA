# Procesos de Decisión de Markov (MDPs):

Los **Procesos de Decisión de Markov (MDPs)** son un **modelo matemático diseñado para la toma de decisiones en escenarios donde existe incertidumbre** . Fueron introducidos entre las décadas de 1950 y 1960, basándose en trabajos como el libro de Ronald Howard sobre Programación Dinámica y Procesos de Markov . El término 'Markov' hace referencia a Andrey Markov, y los MDPs son una **extensión de las Cadenas de Markov** que incorporan la posibilidad de tomar decisiones o acciones .

A diferencia de los problemas de búsqueda previos, donde las acciones eran determinísticas (una acción `a` en un estado `s` siempre llevaba a un único estado `Succ(s, a)`) , en los MDPs **las acciones son aleatorias** . Esto significa que al tomar una acción `a` en un estado `s`, se puede transitar a diferentes estados `s01`, `s02`, etc., con ciertas probabilidades asociadas . Esta aleatoriedad o incertidumbre es característica de muchos problemas del mundo real, como en robótica (posibles fallos de actuadores u obstáculos imprevistos), asignación de recursos (demanda de clientes desconocida) o agricultura (clima incierto) . El ejemplo del juego de dados ilustra esta incertidumbre, donde la acción "stay" puede resultar en diferentes resultados (continuar el juego o terminar) dependiendo de la tirada del dado .

Los **componentes principales de un MDP** son :

*   **Estados (States):** El conjunto de todas las posibles situaciones o configuraciones .
*   **Estado inicial (sstart):** El estado donde comienza el proceso .
*   **Acciones posibles (Actions(s)):** El conjunto de acciones que se pueden tomar desde un estado específico `s` .
*   **Probabilidades de transición (T(s, a, s0)):** La probabilidad de que, al tomar la acción `a` en el estado `s`, el siguiente estado sea `s0` . Para cualquier estado `s` y acción `a`, la suma de las probabilidades de transición a todos los posibles estados `s0` es igual a 1 . Los sucesores de un par `(s, a)` son los estados `s0` donde `T(s, a, s0) > 0` .
*   **Recompensa (Reward(s, a, s0)):** El valor numérico obtenido al transitar del estado `s` al estado `s0` como resultado de tomar la acción `a` .
*   **Estado final (IsEnd(s)):** Una indicación de si un estado marca el final del juego o proceso .
*   **Factor de descuento (γ):** Un valor entre 0 y 1 (ambos inclusive) que ajusta la importancia de las recompensas futuras en comparación con las inmediatas . Un `γ = 0` ignora las recompensas futuras ("vive el momento"), mientras que un `γ = 1` valora por igual todas las recompensas futuras ("guarda para el futuro") . El descuento afecta la forma en que se calcula la utilidad total de un camino .

Mientras que en los problemas de búsqueda la solución es un camino (una secuencia fija de acciones) , en los MDPs, la **solución es una política (π)** . Una política es un **mapeo que asigna a cada estado `s` la acción `a` que se debe tomar en ese estado** (`π(s) = a`) .

Seguir una política genera un camino que es inherentemente aleatorio debido a las probabilidades de transición . La **utilidad** de una política en un camino específico es la suma de las recompensas obtenidas a lo largo de ese camino, posiblemente afectadas por el factor de descuento `γ` . Como el camino es aleatorio, la utilidad es una variable aleatoria .

El **valor de una política (Vπ(s))** en un estado `s` se define como la **utilidad esperada** que se obtendría al comenzar en el estado `s` y seguir la política `π` . Relacionado con esto, el **Q-valor de una política (Qπ(s, a))** es la utilidad esperada de tomar la acción `a` en el estado `s` y *luego* seguir la política `π` . Existe una relación recursiva entre el valor y el Q-valor de una política :
`Vπ(s) = Qπ(s, π(s))` (si no es un estado final) .
`Qπ(s, a) = Σ s0 T(s0|s, a) ` (sumando sobre todos los posibles siguientes estados `s0`) .

Para calcular el valor de una política específica (`Vπ`), se utiliza el **algoritmo de evaluación de políticas** . Este es un algoritmo **iterativo** . Comienza inicializando los valores estimados `V(0)π(s)` a cero para todos los estados . En cada iteración subsiguiente `t`, el valor estimado `V(t)π(s)` se actualiza utilizando la recurrencia, sumando sobre los posibles siguientes estados `s0`, ponderando por la probabilidad de transición `T(s0|s, π(s))` y añadiendo la recompensa inmediata `Reward(s, π(s), s0)` más el valor descontado estimado del siguiente estado `γV(t-1)π(s0)` . Las iteraciones continúan hasta que los valores convergen, es decir, no cambian significativamente entre iteraciones . El algoritmo necesita almacenar los valores de las dos últimas iteraciones . La complejidad temporal por iteración es O(S*S0), donde S es el número de estados y S0 es el número promedio de sucesores (estados `s0` con `T > 0`) para cada par (estado, acción) .

El **objetivo final en los MDPs es encontrar la política óptima (πopt)**, que es la política que maximiza la utilidad esperada . El **valor óptimo (Vopt(s))** es el valor máximo (utilidad esperada) que se puede alcanzar comenzando en el estado `s` y siguiendo la política óptima . El **Q-valor óptimo (Qopt(s, a))** es la utilidad esperada de tomar la acción `a` desde el estado `s` y luego seguir la política óptima . La relación entre ellos es :
`Vopt(s) = max a∈Actions(s) Qopt(s, a)` (si no es un estado final) .
`Qopt(s, a) = Σ s0 T(s, a, s0) ` .

Una vez que se conocen los Q-valores óptimos `Qopt(s, a)`, la **política óptima πopt(s)** se deriva fácilmente: en cada estado `s`, se elige la acción `a` que maximiza `Qopt(s, a)` .

Para calcular los valores óptimos (`Vopt` y `Qopt`) y, por lo tanto, la política óptima, se utiliza el **algoritmo de iteración de valor (Value Iteration)** . Este algoritmo, propuesto por Bellman en 1957 , también es **iterativo** . Se inicializa `V(0)opt(s)` arbitrariamente (por ejemplo, a 0) para todos los estados . En cada iteración `t`, el valor estimado `V(t)opt(s)` se actualiza utilizando la ecuación de Bellman para el valor óptimo: para cada estado `s`, se calcula el valor esperado de tomar cada acción `a` (que incluye la recompensa inmediata y el valor descontado estimado `V(t-1)opt(s0)` del siguiente estado `s0`) y se toma el **máximo** de estos valores sobre todas las acciones `a` posibles desde `s` . La iteración de valor **converge a la respuesta correcta** si el factor de descuento `γ < 1` o si el grafo del MDP es acíclico . Si `γ = 1` y hay ciclos con recompensa cero, podría no converger . La complejidad temporal por iteración es O(S*A*S0), donde S es el número de estados, A el número de acciones por estado y S0 el número promedio de sucesores .

En resumen:
*   Los MDPs modelan la toma de decisiones bajo incertidumbre .
*   La solución es una política que especifica la acción a tomar en cada estado .
*   El valor de una política es la utilidad esperada al seguirla .
*   La evaluación de políticas es un algoritmo iterativo para calcular el valor de una política dada .
*   El objetivo es encontrar la política óptima que maximiza la utilidad esperada .
*   La iteración de valor es un algoritmo iterativo para calcular los valores óptimos y derivar la política óptima .
*   Algoritmos como la evaluación de políticas y la iteración de valor, al igual que la programación dinámica en problemas de búsqueda, siguen la idea unificadora de **escribir una recurrencia y transformarla en un algoritmo iterativo** .

