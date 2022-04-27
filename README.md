# Taller #3
Se presentan los resultados del Taller #3 del curso de Introducción a la Ciencia de Datos.

## Pregunta a Responder
**Es estadisticamente influyente la cantidad de profesoras en la cantidad de estudiantes mujeres de un pregrado en la Universidad de los Andes en el primer semestre del 2021?**
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
Inicialmente, se identificaron los parámetros que pudieran ser utilizados para realizar un modelo. Esto es, se quitaron del dataset el programa de pregrado, debido a que es irrelevante para el problema, y la cantidad de estudiantes mujeres, debido a que esto es precisamente lo que se busca predecir.

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
men students             0.160399
men professors           0.112066
women professors         0.238121
professors/doctorate     0.051018
professors/masters       0.086136
professors/bachelors     0.031398
titular professors       0.071216
associate professors     0.087614
professor assistants     0.136652
plant professors         0.020155
instructor professors    0.003926
emeritus professors      0.000000
visitant professors      0.001298
```

## Resultados
Se obtuvo un modelo capaz de predecir, teniendo algunos datos sobre estudiantes y profesores, la cantidad de profesoras que habrá en un semestre determinado. Asimismo, respondiendo la pregunta planteada inicialmente, esta cantidad sí es estadísticamente relevante en la cantidad de estudiantes mujeres que habrá en ese semestre, 2021-1 en este caso. Asimismo, se destaca la importancia que tienen tanto la cantidad de estudiantes hombres como la de profesores asistentes, factores que, junto a la cantidad de profesoras, son los más relevantes para predecir correctamente la cantidad de estudiantes mujeres que habrá en un determinado semestre.
