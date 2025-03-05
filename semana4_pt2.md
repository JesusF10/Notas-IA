# 🔎 Búsquedas Informadas

Las **búsquedas informadas** utilizan conocimiento adicional (una **heurística**) para guiar la exploración del espacio de estados hacia el objetivo. A diferencia de las búsquedas no informadas (BFS, DFS, UCS), que exploran ciegamente, las informadas tratan de ser **inteligentes** para ahorrar tiempo y memoria.

---

## ⭐ A*:

### ✅ Función de evaluación
La función de prioridad central de A* es:
$$
f(n) = g(n) + h(n)
$$
- \(g(n)\): costo real desde el inicio hasta el nodo \(n\).
- \(h(n)\): estimación heurística desde \(n\) hasta el objetivo.
- A* siempre expande el nodo con el menor \(f(n)\) en cada paso.

### ✅ Condiciones para optimalidad
- Si \(h(n)\) es **admisible** (no sobreestima el costo real restante), A* es **óptimo** (encuentra la solución de menor costo).
- Si \(h(n)\) es **consistente (o monótona)**, A* también es **óptimo** y además puede garantizar que un nodo se expande como mucho una vez. Esto mejora eficiencia.
    - Una heurística es **consistente** si:
$$
h(n) \leq c(n, n') + h(n')
$$
    Donde \(c(n, n')\) es el costo de ir de \(n\) a \(n'\). Es decir, la heurística no puede "saltar" y sobreestimar el costo de un paso.

---

## 📊 Búsqueda Óptima vs Eficiencia

| Propiedad | Con heurística admisible | Con heurística consistente |
|---|---|---|
| Optimalidad | ✔️ | ✔️ |
| Expansión de nodos | Puede expandir un nodo varias veces | Cada nodo expandido a lo sumo 1 vez |

---

## 🧩 Ejemplos de heurísticas

Para el **8 puzzle**:
- **h1 = número de piezas mal colocadas** (simple, admisible, pero no muy precisa).
- **h2 = suma de distancias Manhattan** (más precisa, aún admisible).

En general, **heurísticas más informativas (más altas sin sobreestimar)** reducen el número de nodos expandidos.

---

## ⚠️ Heurísticas no admisibles
- Si la heurística **sobreestima**, A* puede ser más rápido, pero deja de ser óptimo.
- Ejemplo: una heurística que ignora obstáculos puede dar rutas imposibles.

---

## ⚔️ Comparación con otras búsquedas informadas

| Algoritmo | Función de evaluación | Optimalidad | Característica clave |
|---|---|---|---|
| **Greedy Best-First** | \(f(n) = h(n)\) | ❌ | Solo sigue la heurística |
| **A*** | \(f(n) = g(n) + h(n)\) | ✔️ (con \(h\) admisible) | Combina costo real y estimado |

- **Greedy Best-First Search**: Rápida en espacios grandes, pero puede atascarse en caminos subóptimos.
- **A***: Más lenta si la heurística es mala, pero garantiza el mejor camino.

---

## 📏 Dominancia de heurísticas

Si tienes dos heurísticas \(h_1\) y \(h_2\):
- \(h_1\) **domina** a \(h_2\) si:
$$
\forall n \quad h_1(n) \geq h_2(n)
$$
- Usar una heurística más fuerte (más cercana al costo real) suele reducir la cantidad de nodos expandidos.

---

## 🧵 Otras técnicas relacionadas

### ✅ IDA* (Iterative Deepening A*)
- A* puede consumir mucha memoria (guarda todos los nodos en memoria).
- **IDA*** es una versión que usa *backtracking* (como DFS) pero con un umbral de costo \(f\). Se ajusta en cada iteración.
- Menor memoria, pero puede ser más lento.

### ✅ Weighted A*
- Modifica:
$$
f(n) = g(n) + w \cdot h(n)
$$
- Si \(w > 1\), da más peso a la heurística. Es **más rápido**, pero puede perder optimalidad.
- Permite **ajustar la rapidez vs calidad**.

---

## 🏁 Comparativa General

| Algoritmo | Optimalidad | Eficiencia |
|---|---|---|
| BFS (costo uniforme si hay pesos) | ✔️ (con pesos iguales) | ❌ (explora todo) |
| UCS | ✔️ | ❌ |
| Greedy Best-First | ❌ | ✔️ |
| A* | ✔️ (con h admisible) | ✔️ (con buena heurística) |
| IDA* | ✔️ (con h admisible) | ✔️ (baja memoria) |

---

## 🔥 Idea central

La clave en **búsqueda informada** es encontrar una buena **heurística**:
- **Admisible**: no sobreestima.
- **Informativa**: lo más cercana posible al costo real.
- **Consistente** (ideal): garantiza no expandir nodos múltiples veces.

