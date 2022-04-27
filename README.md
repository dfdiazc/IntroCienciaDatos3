# Taller #3
Se presentan los resultados del Taller #3 del curso de Introducción a la Ciencia de Datos.

## Pregunta a Responder
**Es estadisticamente influyente la cantidad de profesoras en la cantidad de estudiantes mujeres de un pregrado en la Universidad de los Andes en el primer semestre del 2021? Si la respuesta a esto es sí, qué tan influyente es comparado a otros parámetros?**
<!--- https://towardsdatascience.com/train-a-regression-model-using-a-decision-tree-70012c22bcc1 --->

## Datos Utilizados
Los datos utilizados fueron tomados del Boletín Estadístico Uniandes 2021 disponible en el siguiente [enlace](https://planeacion.uniandes.edu.co/images/BoletinEstadistico/ComplementoEstadistico/SuplementoEstadistico2021.pdf). Un extracto del dataset utilizado se muestra a continuación:

| program | men students | women students | men professors | women professors | professors/doctorate | professors/masters | professors/bachelors | titular professors | associate professors | professor assistants | plant professors | instructor professors | emeritus professors | visitant professors |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Administración | 707 | 495 | 105 | 50 | 55 | 10 | 0 | 4 | 25.5 | 27 | 0 | 1 | 1 | 0.5 |
| Física | 231 | 95	| 35 | 8 | 23 | 2 | 0 | 4.50 | 16 | 1 | 0 |	0 |	0 |	0 |
| Matemáticas | 82 | 26 | 61 | 15	| 38 | 9 | 4 | 3.50 | 24 | 4 | 16 | 0 | 0	| 0 |
| Música | 133 | 75 | 50 | 15 | 8	| 11 | 2 | 0	| 13.50	| 6	| 0	| 0	| 0	| 1 | 
| Derecho	| 589	| 659	| 77 | 51	| 35 | 6 | 0	| 3.50 | 29.10 | 8	| 0	| 0	| 0	| 0 |

## Procedimiento

Inicialmente, utilizando los datos de cantidad de profesoras y estudiantes mujeres, se obtuvo la siguiente gráfica:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/decision_trees/data.png?raw=true">
</p>

Ahora, se realizó un árbol de decisión de profundidad 2, obteniendo el siguiente modelo inicialmente:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/decision_trees/initial_model.png?raw=true">
</p>

Vemos que existe un problema de *underfitting*. Entonces, graficamos el error medio cuadrado para diferentes profundidades del árbol, para así obtener la profundidad óptima que prevenga *underfitting* y *overfitting*:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/decision_trees/error.png?raw=true">
</p>

Podemos darnos cuenta de que la profundidad óptima es un valor alrededor de 5. Utilizando el método de K-Fold obtenemos los siguientes parámetros:

```python
'max_depth': 1
'min_samples_split': 5
```

Luego, el módelo óptimo efectivamente tendrá árboles de profundidad 5, obteniendo así el siguiente modelo:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/decision_trees/best_model.png?raw=true">
</p>

Ahora, se identificaron los parámetros que pudieran ser utilizados para realizar un modelo. Esto es, se quitaron del dataset el programa de pregrado, debido a que es irrelevante para el problema, y la cantidad de estudiantes mujeres, debido a que esto es precisamente lo que se busca predecir.

Así, utilizando sklearn, se divieron los datos en dos conjuntos: test y train. Igualmente, utilizando esta librería, se realizó inicialmente un bosque aleatorio con 10 árboles, obteniendo así árboles de profundidad 5 como el presentado en la siguiente imagen:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/random_forests/tree.png?raw=true">
</p>

Ahora, para estimar la profundidad de los árboles se calcularon los errores medios cuadrados para tanto el conjunto de testeo como para el de entrenamiento, obteniendo así la siguiente imagen:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/random_forests/error.png?raw=true">
</p>

A partir de esto, se determinó que la profundidad óptima de los árboles debería ser de 5. Una vez más, utilizando sklearn se realizó nuevamente el modelo ajustando este parámetro y se calcularon las siguientes importancias para cada uno de los parámetros del dataset, obteniendo el siguiente resultado:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/random_forests/importances.png?raw=true">
</p>

Con los valores específicos

```python
men students             0.125163
men professors           0.131296
women professors         0.205384
professors/doctorate     0.046386
professors/masters       0.086108
professors/bachelors     0.029837
titular professors       0.021236
associate professors     0.158023
professor assistants     0.176314
plant professors         0.003836
instructor professors    0.009130
emeritus professors      0.002977
visitant professors      0.004310
```

## Resultados
Se obtuvo un modelo capaz de predecir, teniendo algunos datos sobre estudiantes y profesores, la cantidad de estudiantes mujeres que habrá en un semestre determinado. Asimismo, respondiendo la pregunta planteada inicialmente, esta cantidad sí es estadísticamente relevante en la cantidad de estudiantes mujeres que habrá en ese semestre, 2021-1 en este caso. Asimismo, se destaca la importancia que tienen tanto la cantidad de profesores asistentes como la de profesores asociados, factores que, junto a la cantidad de profesoras, son los más relevantes para predecir correctamente la cantidad de estudiantes mujeres que habrá en un determinado semestre.
