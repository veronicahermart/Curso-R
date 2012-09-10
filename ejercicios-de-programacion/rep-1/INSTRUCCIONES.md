Ejercicios de programación I: Introducción
==========================================

### [IMSER 2012]

___

Archivos incluidos:
-------------------

El [archivo](http://eva.universidad.edu.uy/file.php/1454/ejercicios_de_programacion/rep-1.zip) con los ejercicios del práctico debe bajarse y descomprimirse en disco duro, creando la carpeta **`rep-1`**. Usted deberá abrir el RStudio y seleccionar dicha carpeta como su directorio de trabajo con `setwd` o en RStudio la combinación **Ctrl + Shift + K**. En esta carpeta se encuentran algunos archivos que usted deberá modificar:

* **` hipot.R `**
* **` areaMax.R `**
* **` dist.R `**
* **` varianza.R `**
* **` zenon.R `**
* **` geom.R `**
* **` shannon-1.R `**
* **` shannon-2.R `**

Adicionalmente los siguientes archivos son necesarios, pero **no deben ser modificados** para que el método de calificación automático funcione correctamente.

* ` evaluar.R `
* ` notas.csv `
* ` datos `
* ` INSTRUCCIONES.pdf `

Lo primero que debe hacer es cargar el archivo `evaluar.R` con la función `source`:


```r
source("evaluar.R")
```


Si usted ha ejecutado todos los pasos anteriores correctamente, la siguiente frase debería verse en la consola:

    Archivo de codigo fuente cargado correctamente

En caso de que ocurra un error o se vea otro mensaje en la consola, verifique que los archivos se descomprimieron correctamente y que usted está trabajando en la carpeta correspondiente con el comando `getwd()`.

Usted trabajará modificando los contenidos de dichos archivos con RStudio (u otro programa de su preferencia) según las consignas que se describen a continuación. Luego de terminar cada ejercicio usted podrá verificar rápidamente si su respuesta es correcta ejecutando el comando:


```r
evaluar()
```

```
## Error: could not find function "evaluar"
```


y además podrá en todo momento verificar su puntaje con la función `verNotas()`.

___


1. Geometría
------------

Como sabrá probablemente, el filósofo griego Pitágoras es el supuesto autor del teorema homónimo, el cual sirve para calcular la hipotenusa de un triángulo rectángulo. Este cálculo se realiza con la fórmula $h^2 = a^2 + b^2$, en donde $h$ es la hipotenusa y $a$ y $b$ los catetos.

### 1.a Funciones simples

El siguiente código sirve para crear la función `hipot`, la cual acepta como argumentos los anchos de los catetos y calcula el valor de $h$ correspondientes:


```r
hipot <- function(cat.ad, cat.op) {
    out <- sqrt(cat.ad^2 + cat.op^2)
    out
}
```




Tomando este caso como ejemplo, complete el código del script "hipot.R" para hacer las funciones `area` y `co` tales sean capaces de calcular el área de un triángulo y el cateto opuesto respectivamente. Las entradas para la función `area`, es decir, sus argumentos, serán los anchos de los catetos. Para el caso de `co` los argumentos serán los anchos del cateto adyacente y de la hipotenusa. Recuerde que para ejecutar este script debe usar la función `source` de la siguiente manera:

    source('hipot.R')

o usar los atajos de teclado de RStudio. Luego en la sesión de trabajo aparecerán los distintos objetos definidos en el script (verifíquelo con `ls()`), incluyendo a las funciones `area` y `co`.

Si sus funciones están correctas, usted debería obtener estos resultados:


```r
area(3, 4)
```

```
## [1] 6
```

```r
co(3, 5)
```

```
## [1] 4
```



### 1.b Área máxima

En la lección 1.2 vimos cómo con un loop podemos ver fácilemnte todos los valores posibles de salida de la función `prp`. Ahora queremos hacer algo similar con la función `area`. Es decir, si el cateto adyacente $x$ puede tomar cualquier valor tal que $0 < x < h$ (siendo $h$ la hipotenusa), entonces podemos buscar cuál es el área máxima posible y para qué valor de $x$ se da. Obviamente este resultado se puede encontrar analíticamente, pero para los propósitos del ejercicio es útil calcularlo numéricamente.

Siguiendo el ejemplo de la lección 1.2 entonces el siguiente código serviría para encontrar esa área (suponiendo que las funciones `area` y `co` están correctamente definidas):


```r
hip <- 10
cat.ad <- seq(0.001, hip - 0.001, by = 0.1)
for (i in 1:length(cat.ad)) {
    cat.op <- co(cat.ad[i], hip)
    cat("ancho del cateto ad.:", cat.ad[i], "\n")
    cat(">> area:", area(cat.ad[i], cat.op), "\n")
}
```


Si usted ejecuta este código, podrá ver que el área máxima está cercana a 25 y que el ancho del cateto adyacente correspondiente es aproximadamente 7.1 (si ejecuta el script "ejTriang.R" puede visualizar todos estos triángulos). Pero este es un método sumamente engorroso en comparación a lo aque se puede hacer en R. Para empezar, como se menciona en la lección 1.2, en su versión escrita la menos, el cálculo de todas estas áreas (y de los catetos opuestos asociados) se puede hacer sin loops. Esta capacidad de R es muy importante para aumentar la velocidad de ejecución de ciertas tareas y se denomina **vectorización**.

En el caso que las áreas, el siguiente código sirve para calcular todos estos valores de áreas:


```r
hip <- 10  # Hipotenusa
cat.ad <- seq(0.001, hip - 0.001, by = 0.1)  # Valores del cateto adyacente
cat.op <- co(cat.ad, hip)  # Cálculo de los catetos opuestos
a <- area(cat.ad, cat.op)  # Cálculo de las áreas
```


Nótese el uso de la función `seq` para crear una secuencia de valores entre $0.001$ y $h - 0.001$, utilizados como anchos de los distintos catetos adyacentes. De esta forma sólo quedaría buscar cuál de los valores de área es el mayor y determinar en qué posición se encuentra. Esto lo deberá hacer usted, completando el código en el archivo "areaMax.R"; para ello debería considerar dos funciones de R:

* Las funciones `which` o `which.max` se pueden utilizar para encontrar la posición del valor máximo dentro de un vector. Utilice la documentación para aprender más de estas funciones y así encontrar la posición del valor máximo dentro del vector `a`.  

* El uso de los corchetes `[` y `]` sirve para usar el i-ésimo elemento dentro de un vector (y también se usa con matrices). Acceda a la documentación del uso de estos operadores con el comando
    `?"["`

Cuando complete el código podrá confirmar que ha encontrado verdaderamente el valor máximo con los comandos:


```r
plot(cat.ad, a, type = "l", ylab = "Área", main = "Área máxima: línea roja")
abline(v = cat.ad[i], col = "red", lwd = 2)
```


### 1.c Distancias entre puntos

La definición de distancia euclidiana se basa en el teorema de pitágoras. Para dos puntos en un espacio bidimensional (un plano) la distancia entre ambos se puede calcular a través de las coordenadas cartesianas de los mismos, tal como lo muestra la siguiente figura:

___

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


**Figura 1:** Los puntos A, B y C de coordenadas $(x_A, y_A)$, $(x_B, y_B)$ y $(x_B, y_A)$ pueden formar un triángulo rectángulo como muestra la figura, cuyos catetos tienen los valores indicados entre paréntesis. Por lo tanto, la distancia entre A y B es la hipotenusa del triángulo y se puede hallar con la fórmula de Pitágoras.  
Nota: puede reproducir este gráfico con la función `plotTriang` contenida en el script "plotTriang.R"; además lo invitamos a que examine dicho código si le interesa entender cómo se crea tal figura.

___

En el presente ejercicio vamos a utilizar este concepto, así como las recién aprendidas funciones `which` y `which.max`, y también `which.min`. La idea es encontrar distancias entre puntos y determinar distancias mínimas y máximas. En el archivo "dist.R" el código inicial (8 líneas) debería reproducir un gráfico muy similar a la **Figura 2**.

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10.png) 


Los objetos `coorx` y `coory` son las coordenadas en los ejes x e y respectivamente de todos los puntos blancos. Estos fueron obtenidos con un generador de números aleatorios. El punto negro en el centro de la figura es nuestro foco de interés.

Para encontrar cuáles son el más cercano y el más lejano del punto negro primero debemos calcular las distancias desde éste a todos los demás. Esto lo podemos hacer usando la función `hipot` creada anteriormente. Para esto debemos calcular, con los valores de las coordenadas, los "catetos", usando de referencia el método descripto en la **Figura 1**. Para esto usted necesita saber que las coordenadas del punto central son (0.5, 0.5).

Para comprobar que su resultado es correcto, puede utilizar el siguiente código:


```r
plot(coorx, coory)
points(0.5, 0.5, pch = 19)
points(coorx[i], coory[i], pch = 19, col = "red")
points(coorx[j], coory[j], pch = 19, col = "darkgreen")
```


Complete el código del archivo "dist.R" para completar esta parte del ejercicio.

2. Cálculo de sumatorias
------------------------

Crear el código necesario para calcular sumatorias en R u otros lenguajes no es intiutivo cuando no se tiene experiencia en programación. En R la función `sum` sirve para este propósito. Si tenemos un vector `s` con una cantidad arbitraria de elementos, entonces la suma de todos ellos se realiza con el comando `sum(s)`. Por ejemplo:


```r
s <- c(-10, 5, 6)
sum(s)
```

```
## [1] 1
```


Nota: la función `c` aquí es usada para crear `s`, un vector que es simplemente la secuencia de los números -10, 5, 6. Dicha función es una de las más usadas en R.

Sin duda que este es el caso más trivial. Cuando queremos trabajar con algún caso más elaborado, es conveniente entonces separar el problema en dos partes, (1) Crear el vector `s`, el cual contiene todos los términos de la sumatoria y (2) ejecutar la función `sum`. Por supuesto que en la primer etapa es en donde se hace casi todo el trabajo. Muchas veces nos encontramos con sumatorias que están definidas con funciones matemáticas. Un ejemplo es la fórmula para calcular la varianza no sesgada de una muestra de `n` datos:

$$\sigma ^ 2 = \frac{1}{n - 1} \cdot \sum_{i=1}^{i=n} (x_i - \overline{x}) ^ 2$$

En donde $\overline{x}$ denota el valor promedio de la muestra de datos, calculado como $\frac{1}{n} \cdot \sum_i x_i$. Llamaremos "término de la sumatoria" a la función está incluida en la misma, es decir $(x_i - \overline{x}) ^ 2$.

### 2.a Cálculo de la varianza de una muestra

En el archivo "varianza.R" usted deberá escribir los pasos necesarios para calcular la varianza del vector `x` que se muestra en el archivo (siga las instrucciones incluidas en los comentarios).  Para confirmar que su resultado es correcto, puede ejecutar el comando `var(x)`, el cual debería devolver el mismo valor que el objeto `out`.

### 2.b Paradoja de Zenón

Según la clásica [paradoja de la dicotomía de Zenón](https://es.wikipedia.org/wiki/Paradojas_de_Zen%C3%B3n#La_dicotom.C3.ADa) es imposible caminar de un punto A a un punto B, debido a que primero debemos movernos la mitad del camino, posteriormente avanzar la mitad de la mitad del camino y así ad infinitum, sin llegar jamás a B. Esto se traduce en avanzar primero 1/2 de camino, luego 1/4, luego 1/8, luego 1/16 y así sucesivamente. Entonces, luego de n pasos de este tipo se alcanza una fracción de camino equivalente a:

$$Z_n = \sum_{i=1}^{i=n} \frac{1}{2 ^ i} \;=\;
  \frac{1}{2 ^ 1} + \frac{1}{2 ^ 2} + \frac{1}{2 ^ 3} + ... + \frac{1}{2 ^ n} \;=\;
  \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + ... + \frac{1}{2 ^ n}$$

Si es cierta la proposición de Zenón, entonces no importa que tan grande sea $n$, esta sumatoria siempre será menor a 1, a pesar de que el límite de la misma cuando $n \to \infty$ es 1. Podemos verificar numéricamente esta afirmación usando R: si elegimos un valor $\varepsilon$ arbitrariamente pequeño, entonces podemos encontrar un $n$ tal que $1 - Z_n < \varepsilon$.

En este ejercicio, usted deberá completar el código del archivo "zenon.R" según las instrucciones que se indican en el mismo. Si el código hace correctamente la sumatoria entonces para $n = 3$ el valor `out` debería ser 0.875.

Además usted debe encontrar el entero `n` mínimo necesario para lograr un valor de `out` muy cercano a 1. Específicamente, la diferencia entre `out` y 1 debe ser menor a 0.000001 (i.e.: 1e-06). Puede comprobar que su resultado cumple con esa premisa con el comando `1 - out < 1e-06`. El valor de `n` encontrado debe ser el generado en el archivo "zenon.R".

Para visualizar la convergencia de la serie puede usar el comando:


```r
plot(cumsum(s), type = "o", xlab = "n", ylab = expression(Z[n]))
```



### 2.c Extra: series geométricas

(*Este ejercicio es opcional, aunque puede sumar puntos en su calificación final del repartido*)

La serie $Z_n$ es un caso particular de serie geométrica. La fórmula general de este tipo de series es:

$$S_n = \sum_{i=0}^{i=n} \frac{1}{z ^ i}$$

(Nota: ahora el primer término corresponde a i=0, por lo que a toda $S_n$ se le agrega un +1 en comparación con la misma sumatoria empezando por i=1.)

Un resultado importante es que la serie converge a un valor determinado (es decir, tiene un límite finito para $n \to \infty$) para todos los z tales que $1 > \|z\|$ (valor absoluto de $z$). El objetivo de este ejercicio es confirmar este resultado. Para esto debe completarse el código del archivo "geom.R", siguiendo las instrucciones que allí se indican.

Si el código es correcto, el resultado `out` debería ser idéntico (o casi igual, debido a pequeños errores numéricos de redondeo) al valor de la siguiente fórmula:

$$S_n = \frac{1 - z ^ {- n}}{1 - z ^ {-1}}$$

___

3. Índice de Shannon-Wiener
---------------------------

El índice de [Shannon-Wiener](https://es.wikipedia.org/wiki/%C3%8Dndice_de_Shannon) o simplemente índice de Shannon ha sido utilizado en ecología y otras ciencias como indicador de diversidad de una comunidad de especies. Este índice fue creado originalmente para ser usado como medida de entropía en cadenas de caracteres (i.e., texto) en el contexto de la teoría de la información (Legendre & Legendre, 1998). En general para una colección de objetos de distintas categorías (i.e., individuos de distintas especies), el índice de Shannon se calcula con la siguiente fórmula:

$$H = - \sum_{i=1}^{i=S} p_i \cdot log_2 (p_i)$$

(La base del logaritmo no es demasiado importante, pero en este ejercicio nos vamos a regir a esta definición en particular; es decir, vamos a usar logarítmo de base 2)

Aquí S es el número total de categorías o especies y $p_i$ es la frecuencia relativa de la categoría i. Es decir:

$$p_i = \frac{n_i}{N}$$

En donde $n_i$ es la cantidad de objetos de la categoría i contenidos en nuestra colección y N es la cantidad total de objetos. Por ejemplo, para la palabra "banana", los $p_i$ de las letras "a", "b" y "n" son $3/6$, $1/6$ y $2/6$ respectivamente y el H resultante es 1.46.

### 3.a Cálculo de H para una colección de números

En el archivo "shannon-1.R" usted encontrará código de R incompleto. El objetivo de este script es calcular el H de un vector de números llamado `coleccion`. Pero a diferencia del ejercicio 2, vamos a utilizar otra estrategia para de hacer la sumatoria necesaria, a través del operador `%*%` de R.

Una forma de hacer sumatorias del tipo $\sum v_i \cdot u_i$ es usando el [producto escalar](https://es.wikipedia.org/wiki/Producto_escalar) o producto interior de vectores, para lo cual en R se usa el operador `%*%`. Específicamente, si $v$ y $u$ son vectores columna, entonces $\langle v, u \rangle = \sum v_i \cdot u_i$. Otra representación común es: 

$$v^T \cdot u = (v_1 v_2 \cdots v_d)
  \begin{pmatrix}
    u_1 \\
    u_2 \\
    \vdots \\
    u_d
  \end{pmatrix}
$$

en dónde $v^T$ es el vector columna $v$ transpuesto y $d$ es la dimensión o número de elementos de ambos vectores. En R el producto escalar se escribe

    v %*% u
    
simplemente, ignorando problemas de transposición. De hecho los vectores en R no son fila o columna, si no que simplemente son una secuencia de elementos. En general este operador sirve para toda multiplicación de matrices en R.

En este ejercicio deberá completar el archivo "shannon-1.R" de forma que sea capaz de calcular el valor de H para el vector `coleccion` o *cualquier otro vector* de R, utilizando el producto escalar de vectores para calcular el resultado de la sumatoria. Para el caso particular de `coleccion`, el H esperado es de 2.699514.

#### Observaciones a tener en cuenta:

El cálculo del H puede dividirse en (1) calcular los valores de $p_i$ y (2) realizar la sumatoria. Para calcular los $p_i$ es necesario a su vez tener los $n_i$ y el valor $N$. Para estas tareas las funciones `table` y `length` pueden ser de gran ayuda. La primera sirve para hacer un conteo de la cantidad de objetos por categoría, mientras que la función `length` devuelve la cantidad de elementos de un vector cualquiera. No dude en consultar la documentación de R de estas dos funciones en caso de que no comprenda del todo bien su accionar.

### 3.b Extra: función `shannon`

(*Este ejercicio es opcional, aunque puede sumar puntos en su calificación final del repartido*)

El archivo "shannon-2.R" contiene el código incompleto necesario para crear una función que calcule el índice de Shannon para cualquier vector de R. Si le resulta útil, utilice la función `prp` creada en la lección 1.2 ("una sesión de ejemplo") como referencia.

Tenga en cuenta que en los paréntesis vacíos de luego de `function` es en donde se ponen los argumentos o entradas de la función. Esta función sólo debería usar un argumento; el nombre elegido el mismo debe ser mismo que se use luego en los comandos internos de la misma (los comandos comprendidos entre las llaves `{}`).

Si su función está correcta, el siguiente código debería correr correctamente y el resultado debería ser el predicho por el objeto `frase`:




```r
frase <- "Esta frase tiene un H = 3.7"
x <- strsplit(frase, "")[[1]]
shannon(x)
```

```
## [1] 3.708
```


(Nota: el segundo comando toma la `frase` y la parte en todos los caracteres que la componen)