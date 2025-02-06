# Aprendizaje Máquina  

## Motivación  

Es muy difícil escribir programas que resuelvan problemas complejos, como:  

- Reconocer objetos en 3D desde diferentes perspectivas y condiciones de luz.  
- Calcular la probabilidad de que una transacción en línea sea fraudulenta.  

Las dificultades principales son:  

1. **Falta de conocimiento explícito:** No sabemos exactamente cómo procesa nuestro cerebro este tipo de información.  
2. **Complejidad del código:** Aunque supiéramos cómo hacerlo, el programa resultante sería extremadamente complejo.  
3. **Reglas insuficientes:** No hay reglas simples y confiables para ciertos problemas. Se requiere una gran cantidad de reglas, y aún así, estas deben actualizarse constantemente debido a la naturaleza dinámica del problema.  

## La Solución  

En lugar de escribir programas específicos para cada tarea, podemos:  

1. **Recolectar datos:** Obtener muchos ejemplos que representen la respuesta correcta en cada caso.  
2. **Utilizar un algoritmo de aprendizaje automático:** Este tomará los datos y generará un programa basado en ellos.  
3. **Obtener un programa flexible:** A diferencia de un programa escrito a mano, este programa puede:  
   - Generalizar a nuevos casos con cierto margen de confianza.  
   - Adaptarse cuando los datos cambian.  

### Ventajas  

- El acceso a **grandes volúmenes de datos** y **potencia computacional masiva** ha reducido los costos en comparación con contratar expertos en tareas específicas.  

## Definición  

Existen varias definiciones que nos ayudan a comprender el aprendizaje automático:  

### Arthur Samuel (1959)  
> **"Machine Learning is the field of study that gives computers the ability to learn without being explicitly programmed."**  

Samuel introduce el concepto de que una computadora puede aprender a partir de datos sin que un programador le indique cada paso explícitamente.  

### Tom Mitchell (1998)  
> **"A computer program is said to learn from experience (E) with respect to some task (T) and some performance measure (P), if its performance on T, as measured by P, improves with experience E."**  

Mitchell proporciona una definición más formal, donde el aprendizaje ocurre cuando el desempeño de un programa mejora a medida que obtiene más experiencia en una tarea específica.

## Generalización  

- **Error en muestra (\(E_{in}\))**:  
  \[
  E_{in} = \frac{1}{N} \sum_{i=1}^N e(y^{(i)}, \hat{y}^{(i)})
  \]  
  Es el error calculado dentro del conjunto de entrenamiento.  

- **Error fuera de muestra (\(E_{out}\))**:  
  \[
  E_{out} = \mathbb{E}_{x \in X}[e(y, \hat{y})]
  \]  
  Representa el error esperado en datos nuevos o no vistos durante el entrenamiento.  

- **Condiciones para el aprendizaje**:  
  - El aprendizaje existe si y solo si \(E_{out} \approx 0\).  
  - Esto requiere que \(E_{in} \approx 0\) y \(E_{in} \approx E_{out}\).  

---

## Método del Vecino Más Próximo (K-Nearest Neighbors, KNN)  

1. **No paramétrico**:  
   Este método no supone ninguna forma funcional específica para los datos.  

2. **Requiere almacenar los datos del conjunto de entrenamiento**:  
   Todos los datos deben mantenerse en memoria para realizar predicciones.  

3. **Mide la similitud**:  
   Se calcula una medida de similitud (como la distancia euclidiana) entre el dato desconocido y cada uno de los datos del conjunto de entrenamiento.  

4. **Predicción**:  
   La clase del dato desconocido se asigna según la clase del dato más cercano.  

5. **Simplicidad**:  
   Es un método conceptualmente simple, ideal para comprender cómo funciona la clasificación basada en la proximidad.  

## Metodos de aprendizaje maquina:
- Modelos descriptivos
- Modelos lineales generalizados
- Arboles de decision
- Redes neuronales
- Metodos de ensemble