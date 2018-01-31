# Introduccion

## Reinforcement learning

![rl loop](https://github.com/Danielhp95/mcts-workshop/blob/master/images/RL-diagram.png "Diagrama Reinforcement Learning")

Reinforcement Learning es una rama de inteligencia artificial que base su aprendizaje en el concepto de "prueba y error". Todo algoritmo de Reinforcement Learning presenta a un **agente** que actua en un **entorno** y recibe una **recompensa** (positiva o negativa) por cada una de sus **acciones**. El objetivo de un **agente** es encontrar una **estrategia** que maximize su **recompensa** a largo plazo.

### Loop de Reinforcement Learning.

Por cada escalon de tiempo **t**:
* El Agente:
  1. Recibe recompensa **r<sub>t</sub>**
  2. Recibe observacion **s<sub>t</sub>**
  3. Emite accion **a<sub>t</sub>**
* El Entorno:
  1. Recibe accion **a<sub>t</sub>** y la ejecute. La ejecucion de la accion **a<sub>t</sub>** modifica el entorno.
  2. Emite recompensa **r<sub>t+1</sub>**
  3. Emite observacion **s<sub>t+1</sub>**

### El entorno

## Estados

Set de posibles estados de un entorno **S**. Un agente en RL (reinforcement learning) procesa una representacion interna del entorno. **s<sub>t</sub>** es la representacion del entorno para cada instante **t**. Normalmente, escoger una buena representacion del estado **s<sub>t</sub>** no es facil, y una buena representacion puede simplificar mucho la tarea de aprendizaje.

Para este taller, la representacion sera una matriz de 2 dimensiones, que representa el tablero del 4 en ralla.  *Board<sub>ij</sub>* denotara el estado de la casilla en la fila *i* y columna *j*. *Board<sub>ij</sub>* = 0: casilla vacia. *Board<sub>ij</sub>* = 1: ficha del jugador 1. *Board<sub>ij</sub>* = 2: ficha del jugador 2.

Incluso para un juego tan "sencillo" como el 4 en ralla, hay 4,531,985,219,092 posibles estados.

## Acciones

Set de posibles acciones de un agente **A**. En el instante **t** el agente "observa" el estado **s<sub>t</sub>**. Para este taller, un estado **s<sub>t</sub>** tendra un maximo de 7 acciones posibles, **A** =  [0, 1, 2, 3, 4, 5, 6]. Cada ![possible actions](https://latex.codecogs.com/gif.latex?a%20%5Cin%20%5B0%2C1%2C2%2C3%2C4%2C5%2C6%5D) representa la accion de colocar un ficha en una de las 7 columnas. En caso de que una columna este llena, no se podra colocar una ficha en ella.

El objetivo de los problemas de RL se basan en encontrar una estrategia optima para el problema en cuestion. Donde *optimo* se considera que se consige la mayor recompensa posible.


### Recompensa

Reinforcement learning se basa en encontrar una policy que devuelva la mayor recompensa acumulada. En el caso del 4 en raya, el objetivo del agente es ganar la partida. Siguiendo un orden logico: un movimiento que gane la partida otorgara al agente una recompensa de +1, un movimiento que no termine la partida otorgara una recompensa de 0, un movimiento que pierda la partida penalizara al agente con una recompensa de -1. Realmente esto importa?

### El agente

## Estrategia

Tras "observar" el estado **s<sub>t</sub>**, el agente escoge que accion **a<sub>t</sub>** va a ejecutar usando una **estrategia** ![policy](https://latex.codecogs.com/gif.latex?%5Cpi). Una estrategia es un mapeado de estados a acciones. Dado un estado **s<sub>t</sub>**, ![pi state_t](https://latex.codecogs.com/gif.latex?%5Cpi%28s_t%29) representa la accion **a<sub>t</sub>** que el agente ejecutaria siguiendo la estrategia ![policy](https://latex.codecogs.com/gif.latex?%5Cpi). La accion **a<sub>t</sub>** se lleva a cabo en el entorno, modificandolo. Un vez el entorno se haya modificado, este presentara un estado **s<sub>t+1</sub>** junto con una recompensa **r<sub>t+1</sub>** al agente. Una estrategia puede ser deterministica y mapear una unica accion para cada estado ![deterministic policy](https://latex.codecogs.com/gif.latex?a_t%20%3D%20%5Cpi%28%20s_t%29). Una estrategia tambien puede ser *estocastica* y mapear cada estado a una distribucion de acciones ![stochastic policy](https://latex.codecogs.com/gif.latex?a_t%20%5Csim%20%5Cpi%28s_t%29). Para este taller solo nos interesan estrategias deterministicas.


La imagen de abajo ilustra una representacion mas matematica del agente y el entorno:


![rl loop 2](https://github.com/Danielhp95/mcts-workshop/blob/master/images/RL-diagram2.png "Diagrama Reinforcement Learning")

# Taller: Dia 1.

## Game Trees

Un game tree es un arbol donde los nodos son estados **s<sub>0</sub>**,**s<sub>1</sub>**,**s<sub>2</sub>**...**s<sub>T</sub>** y los enlaces corresponden a acciones permitidas en ese estado **a<sub>1</sub>**,**a<sub>2</sub>**,**a<sub>3</sub>**...**a<sub>n</sub>**. El nodo raiz no tiene por que corresponder al estado inicial del juego (en el caso del 4 en raya, un tablero vacio). MEJORAR

## Monte Carlo Tree Search (MCTS)

MCTS es un metodo de Monte Carlo. Los metodos de Monte Carlo se basan en la siguiente idea: hay un fenomeno que queremos estudiar y tenemos acceso a un modelo de este fenomeno. Utilizando el modelo podemos generar muchas simulaciones de este fenomeno. Con estas simulaciones podemos calcular estadisticas  peprtinentes del fenomeno que queremos estudiar. En el campo de inteligencia artificial para videojuegos, el modelo son las regls del juego. El fenomeno a investigar es la estrategia optima para el juego en cuestion. 

Siendo mas concretos, utilizamos MCTS para responder a la siguiente pregunta. Dado un estado **s<sub>t</sub>** (signo de pregunta) que accion **a<sub>t</sub>** nos dara una mayor probabilidad de ganar la partida?

## Monte Carlo Tree Search - Upper Confidence Bound applied to Trees (MCTS-UCT)

![mcts diagram](https://github.com/Danielhp95/mcts-workshop/blob/master/images/UCT-diagram.png "Diagrama MCTS-UCT")


### Estructura del algoritmo MCTS-UCT
El algoritmo de MCTS-UCT se divide en 4 fases, seleccion, expansion, simulacion y retropropagacion (backpropagation).

Repetir durante **ITERMAX** iteraciones:     
    * **Seleccion**: empezar desde la raíz R y seleccionar nodos hijos sucesivos hasta alcanzar un nodo hoja L. Esto permite que el árbol de juego se expanda hacia movimientos más prometedores, que es la esencia del algoritmo MCTS-UDT.     
    * **Expansion**: a menos que L termine el juego con una victoria/pérdida para cualquiera de los jugadores, ya sea al crear uno o más nodos hijos o elegir entre ellos un nodo C.    
    * **Simulacion**: jugar una partida aleatoria empezando desde el nodo C hasta llegar a un nodo terminal / hoja.    
    * **Retropropagacion**: utilizar el resultado de la simulacion para actualizar la información en los nodos en el camino de C a R.    
**Seleccion de accion** escoger que accion tomar basado en las estadisticas calculadas durante las previas iteraciones.    
END

#### Seleccion

Formula de UCB1: ![ucb1](https://latex.codecogs.com/gif.latex?%5Cfrac%7Bw_i%7D%7Bn_i%7D%20&plus;%20c%20%5Csqrt%7B%5Cfrac%7B%5Cln%20N_i%7D%7Bn_i%7D%7D)

+ **w<sub>i</sub>**: numero de victorias acumuladas en el nodo hijo **i**.
+ **n<sub>i</sub>**: numero de simulaciones acumuladas en el nodo hijo **i**.
+ **N<sub>i</sub>**: numero de simulaciones acumuladas en el nodo actualmente escogido.
+ **c**: parametro de exploracion, es una constante. Nos permite escoger entre los dos terminos de la equacion de UCB1. Un **c** grande da mas importancia a la exploracion. Un **c** pequenho (**c < 1**) da mas importancia a la explotacion. Ver (ingles) ![explotation-vs-exploration](https://medium.com/@dennybritz/exploration-vs-exploitation-f46af4cf62fe "Explotation vs Exploration")

El nodo hijo **i** que reciba el valor UCB1 mas alto sera seleccionado. Esta fase de seleccion se repetira hasta que se seleccione un nodo que no este completamente expandido (que tenga nodos hijo que nunca hayan sido seleccionados) o al llegar un nodo hoja / terminal.

#### Expansion

El paso mas sencillo. Una vez se ha seleccionado un nuevo nodo para anhadirlo en el game tree, este se iniciara con contadores para diferentes estadisticas que serviran para guiar la fase de **seleccion** en futuras iteraciones. Viendo la equacion de UCB1 las estadisticas que nos interesa guardar son:

+ **w<sub>i</sub>**: numero de victorias acumuladas en el nodo hijo **i**.
+ **n<sub>i</sub>**: numero de simulaciones acumuladas en el nodo hijo **i**.

#### Simulacion

En terminos generales, una simulacion es una sucesion de acciones por partes de todos los agentes que cambian el entorno hasta llegar a un estado terminal. En un game tree, una simulacion empieza en un nodo raiz y se toman acciones posibles que llevan a otros nodos. La simulacion termina cuando se llega a un nodo hoja / terminal. Terminada la iniciacion del nodo escogido por la equacion UCB1 (u otra estrategia de seleccion de nodo) comenzamos una simulacion del juego (en este taller 4 en raya) desde este nodo. Cada una de las acciones escogidas durante toda la simulacion son aleatorias (se juegan movimientos aleatorios). Se pueden utilizar acciones no aleatorias para obtener mejores resultados, pero esto no es necesario para el 4 en raya. Otros terminos utilizados para hablar de simulaciones en la literaturas son *rollout* o *playout*. (mejorar wording)

**Nota**: Todos los nodos por los que se pasa en cada simulacion *NO* forman parte del game tree que se esta formando durante MCTS-UCT. (anhadir mas clarificacion?)

#### Retropropagacion

Todas las estadisticas de los nodos escogidos durante la fase de **seleccion** son actualizadas con el resultado de la simulacion. En otras palabras, el resultado de la simulacion se propaga empezando por el ultimo nodo escogido en la fase de **seleccion** y terminando en el nodo raiz del game tree. Para **actualizar** las estadisticas basta con actualizar el numero de simulaciones y victorias (en caso de que la simulacion haya sido victoriosa) en cada uno de los nodos. Este proceso tambien se conoce como **backpropagation**.

#### Seleccion de accion.

El uso de las estadisticas calculadas durante las previas fases es la de seleccionar una accion **a<sub>t</sub>** para tomar en el movimiento numero **t**. Donde **s<sub>t</sub>** es el estado correspondiente al nodo raiz del game tree generado por MCTS-UCT. Hay varias posbilidades para escoger que accion tomar una vez tenemos un game tree suficientemente grande. Nosotros utilizaremos una idea sencillla. Inspeccionamos a todos los nodos hijo correspondientes al nodo raiz y tomamos el que tiene un valor mayor de posibilidad de victoria. Tomamos la accion asignada al nodo hijo **c** cuyas estadisticas maximizen la equacion: 


+ **w<sub>c</sub>**: numero de victorias acumuladas en el nodo hijo **c**.
+ **n<sub>c</sub>**: numero de simulaciones acumuladas en el nodo hijo **c**.


### Propiedades de metodos Monte Carlo

**Seccion de opcional lectura, pero atender este taller tambien es opcional, con lo que ya que estamos...**


## Terminologia
**Juego de informacion completa:** Toda la informacion del juego (en caso del 4 en raya, la posicion de todas las piezas) y las reglas del juego son conocidas por todos los agentes (jugadores).    
**Juego deterministico:** Cada movimiento tiene un resultado unico. Es decir, cada movimiento solo tiene una posible resultado. Si esto no fuera asi, el juego seria estocastico.    
**Nodo terminal:** Un nodo que no tiene ningun movimiento posible (porque, por ejemplo, uno de los jugadores ha ganado).

## El Reto

Programar una implementacion del algoritmo MCTS-UCT descrito arriba para el juego del 4 en raya.

### Instalacion

Necesitaras Python 2.7 para este ejercicio. El script solo tiene una dependencia: `colorama`, un modulo para imprimir texto con colores en la terminal, su uso en este ejercicio es puramente estetico. La implementacion del algoritmo no requiere ninguna herramienta que no venga dentro de la distribucion estandard de Python 2.7. Para instalar `colorama`:

```python
pip install colorama
```

## El script

El script `MCTS.py` contiene todo el codigo necesario para los dos talleres. Tambien es el unico archivo que debera ser modificado durante los talleres. Su contenido esta escrito en ingles para facilitar busquedas relacionadas en internet. Los comentarios estan en espanhol.

```python
class GameState
```
Interfaz minima y necesaria para crear un estado de juego que sea compatible con el algoritmo MCTS-UCT. Sirve para entender la implementacion de la clase `class Connect4State` y como referencia para el ejercicio del dia 2.    

```python
class Connect4State
```


```python
class Node
```


```python
def PlayGame
```




## TODO
f
### Introduction
* Polish everything
* Add mathematical equations
* Give more workshop specific comments

### MCTS
    * Explotation vs Exploration link

### El reto
* Dar una pauta de guia para donde empezar y decir que se me pueden hacer preguntas. El internet es tu amigo.

## Ejercicio

Explicar como correr una partida contra otro jugador. Como jugar contra la maquina.
