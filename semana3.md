# Regresión Lineal

## Marco de trabajo de la regresión lineal

- **Datos de entrenamiento**: Conjunto de ejemplos utilizados para entrenar el modelo.
- **Algoritmo de aprendizaje**: Proceso para encontrar el mejor predictor.

## Decisiones de diseño

- **Clase de hipótesis**: ¿Qué predictores son posibles?
- **Función de pérdida**: ¿Qué tan bueno es un predictor?
- **Algoritmo de optimización**: ¿Cómo calculamos el mejor predictor?

## Clase de Hipótesis: ¿Qué predictores?

- **Notación vectorial**:
  - **w**: Vector de peso.
  - **Φ(x)**: Vector de características.
  - **fw(x) = w · Φ(x)**: Puntaje.
  - **F = {fw : w ∈ R2}**.

## Función de pérdida: ¿Qué tan bueno es un predictor?

- **Error cuadrático (squared loss)**: `Loss(x, y, w) = (fw(x) - y)^2`.
- **TrainLoss(w)**: `1/|Dtrain| * Σ(x,y)∈Dtrain Loss(x, y, w)`.

## Algoritmo de optimización: ¿Cómo calcular el mejor?

- **Objetivo**: `minw TrainLoss(w)`.
- **Descenso de gradiente**:
  - Inicializar `w = [0, . . . , 0]`.
  - Para `t = 1, . . . , T` (épocas):
    - `w ← w - ⌘ * rwTrainLoss(w)` (⌘ es el tamaño del paso).

### Descenso de gradiente estocástico (SGD)

- A diferencia del descenso de gradiente, SGD actualiza los pesos después de cada ejemplo de entrenamiento.
- Para `t = 1, . . . , T`:
  - Para `(x, y) ∈ Dtrain`:
    - `w ← w - ⌘ * rwLoss(x, y, w)`.

### Tamaño del paso (learning rate)

- **Estrategias**:
  - **Constante**: `⌘ = 0.1`.
  - **Decreciente**: `⌘ = 1 / (# actualizaciones realizadas hasta el momento)`.

## Características no lineales

- **Predictores cuadráticos**: `Φ(x) = [1, x, x^2]`.
- **Predictores constantes por partes**: `Φ(x) = [1[0 < x ≤ 1], 1[1 < x ≤ 2], 1[2 < x ≤ 3], 1[3 < x ≤ 4], 1[4 < x ≤ 5]]`.
- **Predictores con estructura de periodicidad**: `Φ(x) = [1, x, x^2, cos(3x)]`.

## Lineal en qué?

- `fw(x) = w · Φ(x)`
  - ¿Lineal en `w`? Sí.
  - ¿Lineal en `Φ(x)`? Sí.
  - ¿Lineal en `x`? ¡No!
- **Idea clave: no linealidad**
  - **Expresividad**: el puntaje `w · Φ(x)` puede ser una función no lineal de `x`.
  - **Eficiencia**: el puntaje `w · Φ(x)` siempre es una función lineal de `w`.

## Clasificadores cuadráticos

- `Φ(x) = [x1, x2, x1^2 + x2^2]`.

## Visualización en el espacio de características

- **Espacio de entrada**: `x = [x1, x2]`, el límite de decisión es un círculo.
- **Espacio de características**: `Φ(x) = [x1, x2, x1^2 + x2^2]`, el límite de decisión es una línea.


# Clasificación Lineal

## Marco de Clasificación Lineal
* **Datos de entrenamiento**: Incluyen ejemplos con sus respectivas etiquetas.
* **Algoritmo de aprendizaje**: Se utiliza para encontrar el mejor clasificador.

## Decisiones de Diseño
* **Clase de hipótesis**: Define qué clasificadores son posibles.
* **Función de pérdida**: Determina qué tan bueno es un clasificador.
* **Algoritmo de optimización**: Permite calcular el mejor clasificador.

## Clasificador Lineal de Ejemplo
* La función del clasificador se define como:
  
  ```math
  f(x) = \text{sign}(w \cdot \Phi(x))
  ```
  
  donde `w` es el vector de pesos y `Φ(x)` es la función que extrae características del input `x`.
* `sign(z)` devuelve:
  * `+1` si `z > 0`
  * `-1` si `z < 0`
  * `0` si `z = 0`
* **Límite de Decisión**: Se encuentra donde `w · Φ(x) = 0`.

## Clase de Hipótesis
* La clase de hipótesis `F` se define como el conjunto de todos los clasificadores `fw` posibles, donde `w` pertenece a `ℝ²` (en un espacio bidimensional).
* Ejemplos de funciones de características `Φ(x)` pueden ser:
  
  ```math
  \Phi(x) = [x_1, x_2]
  ```

## Función de Pérdida
* La pérdida 0-1 se define como:
  
  ```math
  \text{Loss}_{0-1}(x, y, w) = 1[f_w(x) \neq y]
  ```
  
  donde `1[condición]` es `1` si la condición es verdadera y `0` si es falsa.
* Esta función de pérdida penaliza las clasificaciones incorrectas.

## Puntaje y Margen
* **Puntaje**: Se define como `w · Φ(x)` e indica la confianza en la predicción de la etiqueta `+1`.
* **Margen**: Se define como `(w · Φ(x))y` y mide qué tan correcta es la clasificación.

## Pérdida 0-1 Reescrita
* La pérdida 0-1 se puede reescribir como:
  
  ```math
  \text{Loss}_{0-1}(x, y, w) = 1[(w \cdot \Phi(x))y \leq 0]
  ```
  
  donde se penaliza cuando el margen es menor o igual a `0`.

## Algoritmo de Optimización
* El objetivo es minimizar la pérdida de entrenamiento `TrainLoss(w)`.
* El descenso de gradiente busca encontrar el mínimo de la función de pérdida.
* El gradiente de la pérdida 0-1 es cero casi en todas partes, lo que dificulta su uso en el descenso de gradiente.
* **Hinge Loss**:
  
  ```math
  \text{Loss}_{\text{hinge}}(x, y, w) = \max(1 - (w \cdot \Phi(x))y, 0)
  ```
  
* **Logistic Loss**:
  
  ```math
  \text{Loss}_{\text{logistic}}(x, y, w) = \log(1 + e^{- (w \cdot \Phi(x))y})
  ```

## Descenso de Gradiente Estocástico (SGD)
* El descenso de gradiente requiere calcular el gradiente sobre todos los ejemplos de entrenamiento en cada iteración, lo cual puede ser costoso.
* **SGD** actualiza los pesos después de cada ejemplo de entrenamiento, lo que puede ser más rápido.

## Características No Lineales
* Se pueden usar características no lineales para crear clasificadores no lineales.
* Ejemplos de características no lineales incluyen características cuadráticas:
  
  ```math
  \Phi(x) = [x_1, x_2, x_1^2 + x_2^2]
  ```

## Clasificadores Cuadráticos
* El límite de decisión puede ser un círculo en el espacio de entrada cuando se utilizan características cuadráticas.
* En el espacio de características, el límite de decisión sigue siendo una línea, aunque la función sea no lineal en el espacio de entrada.

---


# Descenso de Gradiente

## Objetivo

El objetivo principal del descenso de gradiente es **minimizar la función de pérdida de entrenamiento**, denotada como `TrainLoss(w)`. Esto se logra ajustando iterativamente los pesos `w` del modelo.

## Algoritmo

El algoritmo básico del descenso de gradiente implica los siguientes pasos:

### Inicialización
Se inicializan los pesos `w` con valores pequeños, a menudo ceros.

### Iteración
Se repite el siguiente proceso durante un número determinado de épocas `T`:

- **Actualización de pesos:** Se actualizan los pesos `w` utilizando la fórmula:
  
  ```
  w ← w - η * ∇TrainLoss(w)
  ```
  
  donde:
  - `η` (se pronuncia "eta") es el tamaño del paso (*learning rate*), que controla la magnitud de la actualización.
  - `∇TrainLoss(w)` es el gradiente de la función de pérdida de entrenamiento con respecto a los pesos `w`. El gradiente indica la dirección de mayor aumento en la pérdida, por lo que nos movemos en la dirección opuesta para disminuir la pérdida.

## Cálculo del Gradiente

El gradiente `∇TrainLoss(w)` se calcula como la derivada parcial de la función de pérdida con respecto a cada peso. Para el error cuadrático (*squared loss*), el gradiente se calcula como:

```
∇TrainLoss(w) = (1 / |Dtrain|) * Σ(x,y)∈Dtrain 2(w · Φ(x) - y)Φ(x)
```

## Descenso de Gradiente Estocástico (SGD)

El descenso de gradiente estocástico es una **variante del descenso de gradiente** que actualiza los pesos **después de procesar cada ejemplo de entrenamiento individualmente**, en lugar de esperar a calcular el gradiente sobre todo el conjunto de datos.

### Algoritmo:

1. Inicializar `w = [0, . . . , 0]`.
2. Para `t = 1, . . . , T`:
   - Para `(x, y) ∈ Dtrain`:
     ```
     w ← w - η * ∇Loss(x, y, w)
     ```

### Ventajas
SGD puede ser significativamente más rápido que el descenso de gradiente estándar, especialmente para conjuntos de datos grandes, ya que cada actualización es computacionalmente más barata.

### Tamaño del Paso (*Learning Rate*)

La elección del tamaño del paso `η` es crucial:

- Un tamaño de paso **demasiado grande** puede hacer que el algoritmo **oscile y no converja**.
- Un tamaño de paso **demasiado pequeño** puede hacer que el aprendizaje sea **muy lento**.

Se pueden utilizar estrategias como:

- Un tamaño de paso constante (ej., `η = 0.1`).
- Un tamaño de paso decreciente (ej., `η = 1 / (# actualizaciones realizadas hasta el momento)`).

## Hinge Loss y Descenso de Gradiente

Cuando se utiliza la función de pérdida *Hinge*, el gradiente tiene la siguiente forma:

```
∇Losshinge(x, y, w) = -Φ(x)y si {1 - (w · Φ(x))y} > 0, y 0 en caso contrario.
```

# Características No Lineales

## Necesidad de No Linealidad

Los modelos lineales a veces no son suficientes para capturar la complejidad de los datos. Las características no lineales permiten a los modelos representar relaciones más complejas entre las variables de entrada y la salida.

## Transformación de Características

La clave para introducir no linealidad en un modelo lineal es transformar las características de entrada `x` usando una función `Φ(x)`. Esta función mapea las características originales a un nuevo espacio de características donde las relaciones pueden ser modeladas linealmente.

## Tipos de Características No Lineales

Existen varios tipos de transformaciones no lineales que se pueden aplicar a las características:

- **Cuadráticas**: Se incluye el término al cuadrado de las características originales.  
  Ejemplo:  
  `Φ(x) = [1, x, x^2]`  
  Esto permite modelar relaciones parabólicas.

- **Constantes por partes (Piecewise constant)**: Se divide el espacio de entrada en intervalos y se asigna un valor constante a cada intervalo.  
  Ejemplo:  
  `Φ(x) = [1 si 0 < x ≤ 1, 1 si 1 < x ≤ 2, ...]`  
  Esto permite modelar funciones escalonadas.

- **Periódicas**: Se incluyen funciones periódicas como el coseno o el seno de las características originales.  
  Ejemplo:  
  `Φ(x) = [1, x, x^2, cos(3x)]`  
  Esto permite modelar patrones que se repiten.

- **Combinaciones de características**: Se pueden combinar características de diferentes maneras, como `x1 * x2` o `x1^2 + x2^2`.

- **Cualquier otra función** que sea relevante para el problema.

## Linealidad en los Parámetros

Aunque las características transformadas `Φ(x)` pueden ser no lineales con respecto a las entradas originales `x`, el modelo sigue siendo lineal con respecto a los parámetros `w`.  

La función de predicción tiene la forma:  
`fw(x) = w1 * Φ1(x) + w2 * Φ2(x) + ... + wn * Φn(x)`  

donde `w = [w1, w2, ..., wn]` es un vector de pesos. La función de pérdida sigue siendo lineal con respecto a los parámetros, permitiendo el uso de técnicas de optimización eficientes.

## Ejemplo en Clasificación

En clasificación, se pueden usar características no lineales para crear límites de decisión no lineales.  

Por ejemplo, con características cuadráticas, el límite de decisión puede ser un círculo en lugar de una línea recta:  
Si `Φ(x) = [1, x1, x2, x1^2 + x2^2]`, el modelo puede aprender un límite circular en lugar de uno lineal.

## Espacio de Características

La transformación `Φ(x)` mapea los datos del espacio de entrada original al espacio de características. En este nuevo espacio, el modelo lineal puede separar los datos que no eran linealmente separables en el espacio original.

## Ventajas

- **Expresividad**: Permiten modelar relaciones complejas entre las variables de entrada y la salida.
- **Eficiencia**: Mantienen la eficiencia computacional al ser lineales en los parámetros.
