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
| Matemáticas | 82 | 26 | 61 | 15	| 38 | 9 | 4 | 3.50 | 24 | 4 | 16 | 0 | 0	| 0 |
| Física | 231 | 95	| 35 | 8 | 23 | 2 | 0 | 4.50 | 16 | 1 | 0 |	0 |	0 |	0 |
| Música | 133 | 75 | 50 | 15 | 8	| 11 | 2 | 0	| 13.50	| 6	| 0	| 0	| 0	| 1 | 
| Derecho	| 589	| 659	| 77 | 51	| 35 | 6 | 0	| 3.50 | 29.10 | 8	| 0	| 0	| 0	| 0 |

## Procedimiento

Inicialmente, utilizando los datos de cantidad de profesoras y estudiantes mujeres, se obtuvo la siguiente gráfica:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/data.png?raw=true">
</p>

Ahora, se identificaron los parámetros que pudieran ser utilizados para realizar un modelo. Esto es, se quitaron del dataset el programa de pregrado, debido a que es irrelevante para el problema, y la cantidad de estudiantes mujeres, debido a que esto es precisamente lo que se busca predecir.

Así, utilizando sklearn, se dividieron los datos en dos conjuntos: test y train. Igualmente, utilizando esta librería, se realizó inicialmente un bosque aleatorio con 10 árboles, obteniendo así árboles de decisión como el presentado en la siguiente imagen:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/tree.png?raw=true">
</p>

Ahora, para estimar la profundidad de los árboles se calcularon los errores medios cuadrados para tanto el conjunto de testeo como para el de entrenamiento, obteniendo así la siguiente imagen:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/error.png?raw=true">
</p>

A partir de esto, se determinó que la profundidad óptima de los árboles debería ser de 5. Una vez más, utilizando sklearn se realizó nuevamente el modelo ajustando este parámetro y se calcularon las siguientes importancias para cada uno de los parámetros del dataset, obteniendo el siguiente resultado:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/importances.png?raw=true">
</p>

Con los valores específicos

```python
men students             0.110345
men professors           0.139276
women professors         0.239065
professors/doctorate     0.087986
professors/masters       0.050079
professors/bachelors     0.029413
titular professors       0.045629
associate professors     0.129312
professor assistants     0.087268
plant professors         0.035533
instructor professors    0.021254
emeritus professors      0.019948
visitant professors      0.004891
```

Así, utilizando el modelo obtenido se logró realizar una predicción para la cantidad de estudiantes mujeres dada una cantidad de profesoras, junto al resto de parámetros ya presentados, obteniendo así el siguiente resultado:

<p align="center">
  <img src="https://github.com/dfdiazc/IntroCienciaDatos3/blob/main/results/predictions.png?raw=true">
</p>

Con los siguientes parámetros de error:

```python
Mean Absolute Error: 94.45
Mean Squared Error: 18637.06
R-squared scores: 0.59
```
```python
Mean Absolute Error 94.45
Mean Squared Error 18637.06
R-squared scores 0.59
```

## Resultados
Se obtuvo un modelo capaz de predecir, teniendo algunos datos sobre estudiantes y profesores, la cantidad de estudiantes mujeres que habrá en un semestre determinado. Asimismo, respondiendo la pregunta planteada inicialmente, esta cantidad sí es estadísticamente relevante en la cantidad de estudiantes mujeres que habrá en ese semestre, 2021-1 en este caso. Asimismo, se destaca la importancia que tienen tanto la cantidad de profesores hombres como la de profesores asociados, factores que, junto a la cantidad de profesoras, son los más relevantes para predecir correctamente la cantidad de estudiantes mujeres que habrá en un determinado semestre.
