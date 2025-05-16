# Juegos Adversarios y Árboles de Juego

## Introducción y Estado del Arte

*   **Juegos Adversarios:** Un juego es un entorno de tarea con **más de un agente** .
*   Los juegos pueden clasificarse según varios ejes: **determinista o estocástico**, **información perfecta** (totalmente observable) o no, **número de jugadores** (dos, tres o más), **equipos o individuos**, **toma de turnos o simultáneo**, y **suma cero** .
*   Se buscan algoritmos para calcular una **estrategia (política)** que recomiende un movimiento desde cada estado posible .

### Estado del Arte en el Juego por Computadora:

*   **Damas (Checkers):** El primer jugador por computadora data de 1950 . En 1959, Samuel creó un programa autodidacta . En 1995, el primer campeón mundial de computadora venció al campeón humano . En 2007, las Damas fueron **resueltas** .
*   **Ajedrez (Chess):** Investigaciones comenzaron entre 1945-1960 con figuras como Zuse, Wiener, Shannon, Turing, Newell & Simon, McCarthy . Hubo mejoras graduales entre 1960-1996 . En **1997, Deep Blue derrotó al campeón humano Garry Kasparov** . En 2024, Stockfish tiene una calificación significativamente más alta que el campeón humano Magnus Carlsen .
*   **Go:** En 1968, el programa de Zobrist apenas jugaba partidas legales . De 1968 a 2005, se intentaron enfoques ad hoc a nivel de novato . De 2005 a 2014, la búsqueda **Monte Carlo tree search** llevó a un nivel de aficionado fuerte . En **2016-2017, AlphaGo derrotó a campeones mundiales humanos** . En 2022, un humano explotó una debilidad de una red neuronal para derrotar a los mejores programas de Go .
*   **Pacman:** Mencionado como ejemplo de juego .

## Tipos de Juegos

*   **Juegos Deterministas:** Una formalización incluye: **Estados (S)** (inicio en s0), **Jugadores (P)** (usualmente toman turnos), **Acciones (A)** (pueden depender del jugador/estado), **Función de Transición** (S x A → S), **Test Terminal** (S → {true, false}), **Utilidades Terminales** (S x P → R) . La solución para un jugador es una **política** (S → A) .
*   **Juegos de Suma Cero (Zero-Sum Games):** Los agentes tienen **utilidades opuestas** (valores en los resultados) . Es **pura competencia**: uno maximiza, el otro minimiza .
*   **Juegos de Suma General (General-Sum Games):** Los agentes tienen **utilidades independientes** . Son posibles la cooperación, la indiferencia, la competencia, las alianzas cambiantes, etc. .
*   **Juegos en Equipo:** Paga común para todos los miembros del equipo .

## Búsqueda Adversaria

*   Para juegos **deterministas, de suma cero**, como Tic-tac-toe, ajedrez o damas .
*   Un jugador **maximiza** el resultado y el otro **minimiza** el resultado .

### Árboles de Juego Adversarios

*   Se evalúa el **Valor de un Estado**: el mejor resultado (utilidad) alcanzable desde ese estado . Esto se aplica tanto a estados no terminales como a terminales .

### Búsqueda Minimax

*   Es un **árbol de búsqueda en el espacio de estados** donde los jugadores alternan turnos .
*   Se calcula el **valor minimax** de cada nodo: la mejor utilidad alcanzable contra un **adversario racional (óptimo)** .
*   Los valores terminales son parte del juego, los valores minimax se computan recursivamente .
*   **max-value(estado):** Inicializa v = -∞, itera sobre sucesores, v = max(v, value(sucesor)) .
*   **min-value(estado):** Inicializa v = +∞, itera sobre sucesores, v = min(v, value(sucesor)) .
*   **value(estado):** Si es terminal, retorna utilidad; si el siguiente agente es MAX, retorna max-value; si es MIN, retorna min-value .

### Propiedades y Eficiencia de Minimax

*   Es **óptimo contra un jugador perfecto** .
*   Su eficiencia es como una **búsqueda en profundidad (DFS) exhaustiva** .
*   **Tiempo:** O(bm) . **Espacio:** O(bm) .
*   Para juegos como el ajedrez (b ≈ 35, m ≈ 100), la **solución exacta es completamente inviable** . No se necesita explorar todo el árbol .

## Poda Alpha-Beta (α-β Pruning)

*   Es una técnica para podar partes del árbol de búsqueda Minimax .
*   **Preserva el valor minimax computado para la raíz** .
*   Los valores de nodos intermedios podrían ser incorrectos . Es importante notar que los hijos de la raíz pueden tener el valor incorrecto . La versión más ingenua no permite la selección de acción directa .
*   Las variables **α y β** rastrean los **mejores valores obtenibles** desde cualquier nodo max/min en el camino desde la raíz hasta el nodo actual .
*   **α:** El mejor valor que MAX puede obtener hasta ahora en cualquier punto de elección a lo largo del camino actual desde la raíz .
*   **β:** El mejor valor que MIN puede obtener hasta ahora en cualquier punto de elección a lo largo del camino actual desde la raíz .
*   **Poda de hijos de un nodo MIN:** Si el valor estimado del nodo MIN (su mínimo actual) se vuelve peor que α (el mejor valor que MAX ya tiene garantizado más arriba), MAX evitará este nodo, por lo que se pueden podar sus otros hijos .
*   **Poda de hijos de un nodo MAX:** Simétrico al caso MIN. Si el valor estimado del nodo MAX (su máximo actual) se vuelve mejor que β (el mejor valor que MIN ya tiene garantizado más arriba), MIN evitará este nodo, por lo que se pueden podar sus otros hijos .
*   La **ordenación de los hijos** (expandir primero los movimientos buenos) mejora la efectividad de la poda .
*   Con "ordenación perfecta", la complejidad de tiempo cae a **O(bm/2)** . Esto duplica la profundidad resoluble .
*   A pesar de la poda, una búsqueda completa en juegos como el ajedrez sigue siendo inviable .
*   Este es un ejemplo simple de **metarrazonamiento** (computar sobre qué computar) .

## Límites de Recursos y Evaluación

*   **Problema:** En juegos realistas, **no se puede buscar hasta las hojas** .
*   **Solución:** **Búsqueda de profundidad limitada** . Se busca solo hasta una profundidad limitada en el árbol .
*   Se reemplazan las utilidades terminales con una **función de evaluación** para posiciones no terminales .
*   La garantía de juego óptimo **desaparece** con la búsqueda de profundidad limitada .
*   Más *plies* (niveles en el árbol) hacen una **GRAN diferencia** .
*   Se puede usar **iterative deepening** para un algoritmo *anytime* .

### Funciones de Evaluación

*   Asignan puntuaciones a nodos no terminales en búsquedas de profundidad limitada .
*   La función ideal retornaría el **valor minimax real** de la posición .
*   En la práctica, suelen ser **sumas lineales ponderadas de características** (ej: número de reinas blancas vs. negras) . También pueden ser funciones no lineales más complejas (ej: redes neuronales entrenadas por auto-juego con RL) .
*   Las funciones de evaluación son **siempre imperfectas** .
*   Cuanto más profunda en el árbol esté "enterrada" la función de evaluación, menos importa la calidad de la función . Esto ilustra el **tradeoff entre la complejidad de las características y la complejidad de la computación** .

### Sinergias entre Función de Evaluación y Alpha-Beta

*   La cantidad de poda en Alpha-Beta depende del orden de expansión .
*   La función de evaluación puede **guiar para expandir primero los nodos más prometedores** . Esto hace más probable que ya exista una buena alternativa en el camino a la raíz, facilitando la poda .
*   Si una función de evaluación proporciona un **límite superior** al valor en un nodo MIN, y ese límite superior ya es más bajo que la mejor opción para MAX en el camino a la raíz, **entonces se puede podar** .