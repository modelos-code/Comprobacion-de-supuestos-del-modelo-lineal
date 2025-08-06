# Comprobacion-de-supuestos-del-modelo-lineal
Este documento brinda una guía útil y práctica para verificar si se satisfacen los supuestos de un modelo lineal clásico. Se pretende utilizar la menor sintaxis posible en la consola del Rcommander.
Autores: Santiago Delgado, Silvina San Martino.

# Introducción
Este documento fue elaborado para que pueda ser utilizado por estudiantes de posgrado en agronomía y ciencias afines. Nuestro propósito es brindar una guía útil y práctica para verificar si se satisfacen los supuestos de un modelo lineal clásico. Se utilizará un ejemplo muy común y ampliamente difundido en la bibliografía, en el cual la distribución de probabilidad de los residuales no es simétrica y la igualdad de varianzas de los tratamientos no se cumple. Se pretende utilizar la menor sintaxis posible en la consola del “Rcommander” y aplicar los conceptos estadísticos para dar interpretaciones en esta situación particular. Asimismo, se puede reproducir lo propuesto en este ejemplo a diseños similares que se lleven a cabo en investigaciones agronómicas.

# Generación y carga de datos
Comenzaremos cargando los datos del ejemplo 4.1.  Apéndice 4A (pág. 147, Kuehl, 2001) y se muestra a continuación las sentencias a utilizar.  Simplemente se pueden copiar desde este documento y pegar en la ventana de instrucciones en el “Rcommander”.  Para finalizar se deben ejecutar las instrucciones.
```r 
sitio <- rep(1:6, each=25)
conteo <- c(0, 0, 22, 3, 17, 0, 0, 7, 11, 11, 73, 33, 0, 65, 13, 44,
            20, 27, 48, 104, 233, 81, 22, 9, 2, 415, 463, 6, 14, 12, 
            0, 3, 1, 16, 55, 142, 10, 2, 145, 6, 4, 5, 124, 24, 204,
            0, 0, 56, 0, 8, 0, 0, 4, 13, 5, 1, 1, 4, 4, 36, 407, 0, 
            0, 18, 4, 14, 0, 24, 52, 314, 245, 107, 5, 6, 2, 0, 0, 
            0, 4, 2, 2, 5, 4, 2, 1, 0, 12, 1, 30, 0, 3, 28, 2, 21, 
            8, 82, 12, 10, 2, 0, 0, 1, 1,  2, 2, 1, 2, 29, 2, 2, 0, 
            13, 0, 19, 1, 3, 26, 30, 5, 4, 94, 1, 9, 3, 0, 0, 0, 0, 
            2, 3, 0, 0, 4, 0, 5, 4, 22, 0, 64, 4, 4, 43, 3, 16, 19, 
            95, 6, 22, 0, 0)
Kuehl4.1cangrejos <- data.frame(sitio, conteo)
```
# Activación del conjunto de datos en la consola del Rcmdr
VA FIGURA 1
Figura 1. Extracto de la pantalla superior del Rcmdr para activar el conjunto de datos generados
Como se muestra en la figura 1, se debe pulsar con el cursor en “<No hay conjunto de datos activo>” para que aparezca una ventana emergente y nos deje seleccionar los datos del ejemplo. Con este paso se logra activarlo y dejarlo disponible para poder trabajar con él utilizando los recursos que provee la librería “Rcmdr”.

# Medidas resumen
Para reproducir la tabla con las medidas resumen primero debemos convertir la variable numérica “sitio” en una variable categórica o factor. Mostraremos los siguientes pasos con detalle en las figuras 2 y 3
VA FIGURA 2
Figura 2. Pasos para modificar la variable “sitio”.

VA FIGURA 3
Figura 3. Ventana emergente en donde se culmina la conversión de la variable “sitio” a una variable categórica o factor denominada “sitioF”.
```r
Kuehl4.1cangrejos <- within(Kuehl4.1cangrejos, {sitioF <- as.factor(sitio)})
```
En la figura 3 se muestra cómo se genera una nueva variable llamada “sitioF”.  Esta nueva variable, a diferencia de la original “sitio”, es de tipo categórica o factor y tendrá seis categorías o niveles que son cada uno de los sitios de conteos de cangrejos. Con esta conversión lograremos obtener las medidas resumen para cada sitio. El inicio del proceso se describe en las figuras 4 y 5 en donde indicamos en “Resúmenes numéricos…” que vamos a utilizar como variable a describir a “conteo” pero clasificada por los sitios de medición. Como se ve en la figura 5 se debe no solo seleccionar “conteo” sino que también apretar el botón de “Resumir por grupos...”

VA FIGURA 4
Figura 4. Resúmenes numéricos para obtener medidas descriptivas del conjunto de datos.

VA FIGURA 5
Figura 5. Solapa “Datos” para seleccionar la variable “conteo” y “Resumir por grupos”.

VA FIGURA 6
Figura 6. Selección de “sitioF” para que clasifique los resúmenes numéricos de conteos de cangrejos por cada sitio en donde fueron medidos.

VA FIGURA 7
Figura 7. Visualización de que se está clasificando la información por sitios.
La figura 6 muestra que vamos a tomar como variable clasificatoria a “sitiosF” y una vez que apretamos “Aceptar”, en la figura 7 vemos que ya está listo el proceso clasificatorio. Ahora debemos ir a la solapa “Estadísticos” para elegir las medidas deseadas.

VA FIGURA 8
Figura 8. Elección de los estadísticos para terminar de elaborar la tabla con medidas de resumen.
En la figura 8 elegimos “Media”, “Desviación típica”, los cuantiles “0” (mínimo), “.5” (mediana) y “1” (máximo). Con estas elecciones obtenemos la siguiente salida con la tabla resumen.
```r
library(abind, pos=16)
library(e1071, pos=17)
numSummary(Kuehl4.1cangrejos[,"conteo", drop=FALSE], groups=Kuehl4.1cangrejos$sitioF, 
  statistics=c("mean", "sd", "quantiles"), quantiles=c(0,.5,1))
##    mean        sd 0% 50% 100% conteo:n
## 1 33.80  50.38518  0  17  233       25
## 2 68.60 124.95833  0  10  463       25
## 3 50.64 107.43792  0   5  407       25
## 4  9.24  17.38601  0   2   82       25
## 5 10.00  19.84103  0   2   94       25
## 6 12.64  23.01065  0   4   95       25
```
Como puede notarse en la tabla, dentro de cada sitio, la media (columna “mean”) del conteo de cangrejos es mucho más grande que la mediana (columna “50%”).  Como consecuencia se obtiene una distribución de las observaciones sesgada a derecha 

# Ajuste de un modelo lineal

Ajustaremos un modelo lineal, donde la variable respuesta es el conteo y la variable explicativa es un factor que contiene la identificación de los sitios. Las instrucciones de cómo hacerlo se pueden ver en las figuras 9 y 10.

VA FIGURA 9

Figura 9. Primer paso para ajustar un modelo lineal.

VA FIGURA 10
Figura 10. Ajuste del modelo lineal de la variable “conteo” en función de “sitiosF”.

Los pasos para ajustar un modelo lineal del conteo en función de los sitios son relativamente sencillos. En la figura 9 se ve como se hace en un primer paso. En la figura 10, dejaremos el nombre del modelo por defecto, o sea “LinearModel.1” y, como se indica en la pantalla, haremos “doble clic” en las variables “conteo” y, luego de la virgulilla “~”, en el espacio en blanco, con el cursor haremos “doble click” en “sitiosF”. Así, obtendremos la siguiente salida:
```r
LinearModel.1 <- lm(conteo ~ sitioF, data=Kuehl4.1cangrejos)
summary(LinearModel.1)
## 
## Call:
## lm(formula = conteo ~ sitioF, data = Kuehl4.1cangrejos)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -68.60 -33.80  -9.24   0.37 394.40 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept)    33.80      14.36   2.354   0.0199 *
## sitioF2        34.80      20.30   1.714   0.0887 .
## sitioF3        16.84      20.30   0.829   0.4083  
## sitioF4       -24.56      20.30  -1.210   0.2284  
## sitioF5       -23.80      20.30  -1.172   0.2431  
## sitioF6       -21.16      20.30  -1.042   0.2991  
## ---
## Signif. Codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ‘ 1
## 
## Residual standard error: 71.79 on 144 degrees of freedom
## Multiple R-squared:  0.09341,    Adjusted R-squared:  0.06194 
## F-statistic: 2.968 on 5 and 144 DF,  p-value: 0.014
```
Del ajuste del modelo, se obtiene cierta información resumida. En una primera instancia su fórmula, luego unas medidas de resumen de los residuales. Le sigue una estimación de los coeficientes del modelo, sus errores estándar y el resultado de pruebas de hipótesis para evaluar si el parámetro correspondiente es igual cero o no, realizada con un estadístico t y su correspondiente valor p. Por último, se agregan medidas de variabilidad, de bondad de ajuste y el resultado de otra prueba de hipótesis. En esta instancia no prestaremos atención a toda la información descripta porque perseguimos el objetivo de saber si la distribución de probabilidad de los errores puede asumirse Normal y si se cumple también el supuesto de igualdad de varianzas de los conteos de cangrejos para todos los sitios. 

# Verificación de los supuestos del modelo 

Se verificará si puede asumirse que los errores tienen una distribución normal con la misma varianza. Para ello, describiremos algunos métodos gráficos, basados en residuos, y la prueba de Levene como un método analítico para evaluar la igualdad de varianzas de los conteos de cangrejos para todos los sitios. Los pasos para obtener las gráficas básicas de diagnóstico se muestran a continuación:

VA FIGURA 11
Figura 11. Pasos para obtener las gráficas de diagnóstico de modelo.

```r
oldpar <- par(oma=c(0,0,3,0), mfrow=c(2,2))
plot(LinearModel.1)
par(oldpar)
```
VA FIGURA 12
Figura 12. Gráficas básicas de diagnóstico del modelo.

Con la figura 11 se muestra en dos simples pasos como obtener las gráficas básicas de diagnóstico del modelo. La figura 12 nos muestra cuatro gráficos. A la izquierda arriba se grafica los residuales puros en función de los valores ajustados por el modelo que, en este caso, son las medias de conteos de cangrejos para cada sitio.  Se ve una tendencia a aumentar la variabilidad de los residuales (vista en un plano vertical) a medida que aumentan los valores predichos por el modelo. Se observa una desigualdad de varianzas entre los residuales de los diferentes sitios, encontrándose que a medida que aumenta el promedio de los conteos para sitio, aumenta también la variabilidad. También puede apreciarse que, por debajo de cero, los residuales tienden a concentrarse, mientras que, por encima de cero, están más dispersos y aparecen algunos residuales alejados con magnitudes grandes.  Como consecuencia de lo anterior, la distribución de los residuales no es simétrica. A la derecha arriba, se grafican los residuales (estudentizados, en este caso) en función de los correspondientes cuantiles teóricos. Como primer paso, se ordenan los residuales de forma creciente y a cada uno de ellos se le calcula su frecuencia relativa acumulada. Con esta última, se obtiene el cuantil de la distribución teórica correspondiente. Como se ve en esta gráfica, hay muchos residuales de valores altos que están muy por encima de la recta que los une a los cuantiles con iguales probabilidades acumuladas. Sus magnitudes son mucho más altas que las esperadas bajo normalidad y este patrón indica que hay sesgo hacia la derecha en la distribución. Como conclusión, no se cumple el supuesto de distribución Normal de los errores. A la izquierda abajo, se grafica la raíz cuadrada del valor absoluto de los residuales estudentizados en función de los valores ajustados por el modelo. La línea colorada une las medianas de estas raíces cuadradas. Si no hubiera heterogeneidad de varianzas encontraríamos una recta horizontal. En este caso, con esta gráfica se confirma que a medida que aumenta la media del sitio aumenta también la dispersión de los residuales. A la derecha abajo se grafican los residuales estudentizados en función de los sitios. Esta gráfica tiene como intención detectar las observaciones atípicas 

# Prueba de igualdad de varianzas

Utilizaremos la prueba de Levene que en el ejemplo afirma en su hipótesis nula que las varianzas de los conteos de cangrejos son iguales para todos los sitios. Seguiremos los siguientes pasos en Rcmdr

VA FIGURA 13
Figura 13. La prueba de Levene comienza desde la solapa “Estadísticos”.

VA FIGURA 14
Figura 14. Se toma como centro la “mediana”.
```r
tapply(conteo ~ sitioF, var, na.action=na.omit, data=Kuehl4.1cangrejos) # variances by group
##          1          2          3          4          5          6 
##  2538.6667 15614.5833 11542.9067   302.2733   393.6667   529.4900
leveneTest(conteo ~ sitioF, data=Kuehl4.1cangrejos, center="median")
## Levene's Test for Homogeneity of Variance (center = "median")
##        Df F value  Pr(>F)  
## group   5  2.9285 0.01506 *
##       144                  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
En las figuras 13 y 14 se muestran los pasos para obtener la prueba de Levene. Se toma como centro la mediana y vemos que en el resultado del test el valor p (0,01506) es menor que un nivel de significancia muy aceptado como 0,05. Se rechaza la hipótesis nula, por lo que se concluye que no hay igualdad de varianzas en los conteos de cangrejos para los diferentes sitios.

Hemos logrado hasta el momento poder verificar si dos de los supuestos del modelo modelos lineal se cumplen o no. Hemos utilizado gráficas básicas de diagnóstico y una prueba analítica que es el test de Levene. Claramente en este ejemplo no podemos seguir utilizando el modelo lineal ajustado del conteo de cangrejos en función de los sitios. 

A continuación, se plantean dos alternativas de análisis de los datos para probar la hipótesis de que la cantidad media de cangrejos no difiere según los sitios. La primera alternativa tiene que ver con un método muy conocido, de concepción simple y ampliamente difundido que consiste en la transformación de la variable conteo. La segunda alternativa implica el uso de modelos más modernos, un poco más complejos para su entendimiento y que en la actualidad no se han adoptado masivamente o a lo mejor su uso se ha difundido más en ciertas áreas que en otras. Estos modelos son los modelos lineales generalizados y sus variantes. 

# Transformación de la variable conteo

Utilizaremos el método de Box y Cox (1964) para realizar la transformación de la variable conteo. Dicho método es muy difundido y consta de aplicar la siguiente fórmula:

$$ Y_t=\frac{Y^\lambda-1}{\lambda}$$

donde:

$Y$: es la variable conteo

$Y_t$: es la variable conteo transformada

$\lambda$: es el parámetro de la transformación

Para realizar esta transformación se deberá cargar la librería “MASS”. Una de las formas de cargar una librería se muestra a continuación

VA FIGURA 15
Figura 15. Carga de una librería.

VA FIGURA 16
Figura 16. Carga de la librería “MASS”.
Una vez cargado el paquete se deberán escribir las siguientes sentencias en la ventana de instrucciones en el “Rcommander”. Se obtendrá una gráfica con la estimación de λ.
```r
library(MASS)
boxcox((Kuehl4.1cangrejos$conteo+1/6)~1, lambda = seq(-0.75, 0.75, length = 10))
abline(v=0.01)
```
VA FIGURA 17
Figura 17. Estimación de Lambda.
En este ejemplo, vemos que la estimación de λ se encuentra alrededor del cero (figura 17). Pero puede suceder, en otros ejemplos, que la estimación de λ tome otros valores. Dichos valores pueden ajustarse con el argumento “seq” de la sentencia “boxcox” anteriormente mostrada.

La línea vertical negra de la figura 17 marca el valor 0,01. Esta estimación de λ está muy cerca del valor “0”. Más aún, como el intervalo con un 95% de confianza para λ (delimitado por las líneas punteadas negras verticales) contiene al “0”, podemos asumir que dicho parámetro toma ese valor. Por lo tanto, procederemos a transformar la variable “conteo” con la función logarítmica. No se podrá tomar la transformación como tal sobre dicha variable ya que hay algunos conteos de cangrejos que toman valor “0” y no es posible obtener el logaritmo del mismo.  Entonces, a cada valor del conteo se le sumará una constante de magnitud muy pequeña (1/6) para poder utilizar logaritmo natural.  Con estas acciones, que se muestran en las figuras 17 y 18, estaremos listos para ajustar un nuevo modelo y volver a hacer una comprobación de supuestos. 

VA FIGURA 18
Figura 18. La transformación de la variable conteo comienza con la solapa “Datos”.

VA FIGURA 19
Figura 19. La nueva variable se llama “conteot”.
```r
Kuehl4.1cangrejos$conteot <- with(Kuehl4.1cangrejos, log(conteo+1/6))
```

# Ajuste de un modelo lineal con la variable conteo transformada

El primer paso para ajustar el nuevo modelo lineal se mostró en la figura 9. El nuevo modelo tendrá como variable respuesta “conteot”. Como se muestra en la figura 20, dejaremos por defecto el nombre del modelo como “LinearModel.2”

VA FIGURA 20
Figura 20. Ajuste del modelo con la variable transformada.
```r
LinearModel.2 <- lm(conteot ~ sitioF, data=Kuehl4.1cangrejos)
summary(LinearModel.2)
## 
## Call:
## lm(formula = conteot ~ sitioF, data = Kuehl4.1cangrejos)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.0893 -1.3272  0.1455  1.5909  4.2602 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   2.1612     0.4341   4.978 1.81e-06 ***
## sitioF2       0.1363     0.6140   0.222   0.8246    
## sitioF3      -0.4122     0.6140  -0.671   0.5031    
## sitioF4      -1.2534     0.6140  -2.041   0.0430 *  
## sitioF5      -1.1540     0.6140  -1.880   0.0622 .  
## sitioF6      -1.3397     0.6140  -2.182   0.0307 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.171 on 144 degrees of freedom
## Multiple R-squared:  0.07462,    Adjusted R-squared:  0.04249 
## F-statistic: 2.322 on 5 and 144 DF,  p-value: 0.04606
```
# Verificación de los supuestos del modelo lineal

Una vez ajustado el modelo con la variable transformada, el programa nos muestra mucha información resumida con el “LinearModel.2”. Pero, al igual que el modelo anterior, por ahora nos concentraremos en el siguiente paso que es hacer nuevamente una comprobación de supuestos.  Para eso se siguen los mismos pasos que se detallan en la figura 11 y, tal como se muestra en la figura 21, la transformación de la variable “conteo” pareciera haber mejorado la distribución de los residuales.
```r
oldpar <- par(oma=c(0,0,3,0), mfrow=c(2,2))
plot(LinearModel.2)
par(oldpar)
```
VA FIGURA 21
Figura 21. Gráfica de diagnóstico del modelo con la variable transformada.

A la izquierda arriba, la gráfica nos muestra que ya no crece la variabilidad de los residuales a medida que aumentan las medias de la transformación de los conteos. Además, la distribución de los mismos pareciera estar de forma simétrica. A la derecha arriba, se observa una mejor correspondencia entre la distribución de los residuales estudentizados con respecto a los cuantiles de la distribución teórica. A la izquierda abajo podemos concluir mediante esa gráfica que la transformación de la variable conteo ha logrado homogeneizar las varianzas de los residuales de los diferentes sitios.

Como segundo paso haremos una nueva prueba de Levene para probar la igualdad de varianzas de conteos transformados, según los sitios de medición.  Los pasos fueron descriptos en las figuras 13 y 14, pero hay que hacer con la salvedad de que ahora hay que elegir la variable “conteot”.
```r
tapply(conteot ~ sitioF, var, na.action=na.omit, data=Kuehl4.1cangrejos) # variances by group
##        1        2        3        4        5        6 
## 5.151297 5.931168 5.795753 3.482232 3.022960 4.889385
leveneTest(conteot ~ sitioF, data=Kuehl4.1cangrejos, center="median")
## Levene's Test for Homogeneity of Variance (center = "median")
##        Df F value Pr(>F)
## group   5  0.7511 0.5866
##       144
```

Como se muestra en el resultado del test de Levene, el valor p (0,5866) es muy superior al nivel de significancia 0,05. No se rechaza la hipótesis nula que afirma que las varianzas de los conteos transformados para cada sitio son iguales. Se verifica que la transformación de la variable conteo por su logaritmo natural logró homogeneizar las varianzas. 

# Análisis de la varianza con la variable transformada

Ahora podemos probar la hipótesis de que las medias de los conteos transformados no difieren según el sitio. Para ello, los pasos que continúan con el análisis consisten en realizar el análisis de la varianza y, en caso de que el efecto del sitio sea significativo con respecto al conteo transformado, se procederá a las comparaciones múltiples de las medias entre sitios, por ejemplo, según una prueba de Tukey.

Los pasos en “Rcommander” para continuar con el análisis es relativamente sencillo. Se muestra a continuación como con muy pocas operaciones con el cursor se logra completar el análisis

VA FIGURA 22
Figura 22. Pasos para realizar el análisis de la varianza.

Con los pasos mostrados en la figura 22 podremos realizar un análisis de la varianza y también nos dará más opciones de análisis. Una de ellas es las comparaciones múltiples de medias en caso de que el efecto de los sitios sobre los conteos transformados sea significativo.

VA FIGURA 23
Figura 23. Análisis de varianza para un factor.
En la figura 23 se ajusta un nuevo modelo llamado “AnovaModel.1”, se elige la variable “conteot” y se la analiza en función de “sitioF”. Por último, se tilda en “Comparaciones de dos a dos de las medias” para que realice las comparaciones múltiples de promedios estilo Tukey.
```r
library(mvtnorm, pos=16)
library(survival, pos=16)
library(MASS, pos=16)
library(TH.data, pos=16)
## 
## Attaching package: 'TH.data'
## The following object is masked _by_ 'package:MASS':
## 
##     geyser
library(multcomp, pos=16)
library(abind, pos=21)
AnovaModel.1 <- aov(conteot ~ sitioF, data=Kuehl4.1cangrejos)
summary(AnovaModel.1)
##              Df Sum Sq Mean Sq F value Pr(>F)  
## sitioF        5   54.7  10.943   2.322 0.0461 *
## Residuals   144  678.5   4.712                 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
with(Kuehl4.1cangrejos, numSummary(conteot, groups=sitioF, statistics=c("mean", "sd")))
##        mean       sd data:n
## 1 2.1611973 2.269647     25
## 2 2.2975292 2.435399     25
## 3 1.7490387 2.407437     25
## 4 0.9078185 1.866074     25
## 5 1.0072048 1.738666     25
## 6 0.8215220 2.211195     25
local({
  .Pairs <- glht(AnovaModel.1, linfct = mcp(sitioF = "Tukey"))
  print(summary(.Pairs)) # pairwise tests
  print(confint(.Pairs, level=0.95)) # confidence intervals
  print(cld(.Pairs, level=0.05)) # compact letter display
  old.oma <- par(oma=c(0, 5, 0, 0))
  plot(confint(.Pairs))
  par(old.oma)
})
## 
##   Simultaneous Tests for General Linear Hypotheses
## 
## Multiple Comparisons of Means: Tukey Contrasts
## 
## 
## Fit: aov(formula = conteot ~ sitioF, data = Kuehl4.1cangrejos)
## 
## Linear Hypotheses:
##            Estimate Std. Error t value Pr(>|t|)
## 2 - 1 == 0  0.13633    0.61398   0.222    1.000
## 3 - 1 == 0 -0.41216    0.61398  -0.671    0.985
## 4 - 1 == 0 -1.25338    0.61398  -2.041    0.324
## 5 - 1 == 0 -1.15399    0.61398  -1.880    0.419
## 6 - 1 == 0 -1.33968    0.61398  -2.182    0.253
## 3 - 2 == 0 -0.54849    0.61398  -0.893    0.948
## 4 - 2 == 0 -1.38971    0.61398  -2.263    0.216
## 5 - 2 == 0 -1.29032    0.61398  -2.102    0.292
## 6 - 2 == 0 -1.47601    0.61398  -2.404    0.162
## 4 - 3 == 0 -0.84122    0.61398  -1.370    0.745
## 5 - 3 == 0 -0.74183    0.61398  -1.208    0.832
## 6 - 3 == 0 -0.92752    0.61398  -1.511    0.658
## 5 - 4 == 0  0.09939    0.61398   0.162    1.000
## 6 - 4 == 0 -0.08630    0.61398  -0.141    1.000
## 6 - 5 == 0 -0.18568    0.61398  -0.302    1.000
## (Adjusted p values reported -- single-step method)
## 
## 
##   Simultaneous Confidence Intervals
## 
## Multiple Comparisons of Means: Tukey Contrasts
## 
## 
## Fit: aov(formula = conteot ~ sitioF, data = Kuehl4.1cangrejos)
## 
## Quantile = 2.8889
## 95% family-wise confidence level
##  
## 
## Linear Hypotheses:
##            Estimate lwr      upr     
## 2 - 1 == 0  0.13633 -1.63737  1.91003
## 3 - 1 == 0 -0.41216 -2.18586  1.36154
## 4 - 1 == 0 -1.25338 -3.02708  0.52032
## 5 - 1 == 0 -1.15399 -2.92770  0.61971
## 6 - 1 == 0 -1.33968 -3.11338  0.43403
## 3 - 2 == 0 -0.54849 -2.32219  1.22521
## 4 - 2 == 0 -1.38971 -3.16341  0.38399
## 5 - 2 == 0 -1.29032 -3.06403  0.48338
## 6 - 2 == 0 -1.47601 -3.24971  0.29770
## 4 - 3 == 0 -0.84122 -2.61492  0.93248
## 5 - 3 == 0 -0.74183 -2.51554  1.03187
## 6 - 3 == 0 -0.92752 -2.70122  0.84619
## 5 - 4 == 0  0.09939 -1.67432  1.87309
## 6 - 4 == 0 -0.08630 -1.86000  1.68741
## 6 - 5 == 0 -0.18568 -1.95939  1.58802
## 
##   1   2   3   4   5   6 
## "a" "a" "a" "a" "a" "a"
```
La salida de las instrucciones mostradas en las figuras 22 y 23 es muy extensa. Solo nos enfocaremos en el análisis de la varianza y vemos que el valor p (0,0461) para el efecto de los sitios sobre los conteos transformados es menor que 0,05, aunque muy cercano a dicho valor. Para ese nivel de significación, se rechaza la hipótesis que las medias de los conteos transformados es la misma para todos los sitios. Para culminar el análisis, al final de los resultados mostrados, se muestran las comparaciones múltiples de medias transformadas. Vemos que para cada sitio el resultado del análisis colocó la misma letra (“a”). A pesar de que se rechazó la hipótesis nula con el análisis de la varianza, no se logró detectar diferencias significativas entre los sitios para los conteos transformados. Esto se debe a que el valor p estuvo muy cerca del nivel de significancia, y el análisis de comparaciones de medias tipo Tukey es muy exigente para detectar diferencias significativas.

# Ajuste de modelos lineales generalizados

En esta sección mostraremos una alternativa a la transformación de la variable conteo. Dicha variable, por su naturaleza, solo puede tomar valores enteros positivos y como consecuencia de esto, podría considerarse a priori que su distribución de probabilidades se asemejaría a una Poisson. Para profundizar los conocimientos sobre los modelos lineales generalizados nos basamos en lo propuesto por Dunn y Smyth (2018). 

Siguiendo con la idea principal de este documento, con la menor sintaxis posible y mayoritariamente usando las herramientas que ofrece el “Rcommander”, los pasos para ajustar un modelo de este tipo son muy parecidos al de los modelos lineales.  Se muestra en la figura 24 que el proceso comienza en la solapa “Estadísticos” opción “Ajuste de modelos”. Luego de haber seleccionado “Modelo lineal generalizado…” aparecerá una ventana como se ve en la figura 25. En ella dejaremos por defecto el nombre del modelo “GLM.1”, ajustaremos la variable “conteo” en función de “sitioF” y, para finalizar, elegiremos la familia “Poisson” con la salvedad de hacer un “doble click” en ella, así el programa elige por defecto la función de enlace “log”.

VA FIGURA 24
Figura 24. Pasos para ajustar un modelo lineal generalizado.

VA FIGURA 25
Figura 25. Ajuste de un modelo lineal generalizado.

```r
GLM.1 <- glm(conteo ~ sitioF, family=poisson(log), data=Kuehl4.1cangrejos)
summary(GLM.1)
## 
## Call:
## glm(formula = conteo ~ sitioF, family = poisson(log), data = Kuehl4.1cangrejos)
## 
## Deviance Residuals: 
##      Min        1Q    Median        3Q       Max  
## -11.7132   -6.7252   -3.3662    0.1082   31.3642  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept)  3.52046    0.03440 102.336   <2e-16 ***
## sitioF2      0.70783    0.04203  16.841   <2e-16 ***
## sitioF3      0.40428    0.04442   9.101   <2e-16 ***
## sitioF4     -1.29692    0.07425 -17.468   <2e-16 ***
## sitioF5     -1.21788    0.07200 -16.916   <2e-16 ***
## sitioF6     -0.98359    0.06594 -14.917   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for poisson family taken to be 1)
## 
##     Null deviance: 12718  on 149  degrees of freedom
## Residual deviance: 10243  on 144  degrees of freedom
## AIC: 10753
## 
## Number of Fisher Scoring iterations: 6
exp(coef(GLM.1))  # Exponentiated coefficients
## (Intercept)     sitioF2     sitioF3     sitioF4     sitioF5     sitioF6 
##  33.8000000   2.0295858   1.4982249   0.2733728   0.2958580   0.3739645
```

Luego de ajustar el modelo lineal generalizado observamos mucha información, pero solo nos focalizaremos en el final del resumen. Allí notamos que la deviance residual (“Residual deviance”: 10243) es mucho mayor que los grados de libertad (“degrees of freedom”, 144). Esto último significa que el modelo no es bueno y para corroborar esto realizaremos una gráfica de residuales.

En primer lugar, cargaremos la librería “hnp” de la misma forma que se indicó en la figura 15. Luego con la sentencia “hnp” obtendremos una gráfica de los residuales en función de los cuantiles teóricos.

```r
library(hnp, pos=22)
hnp(GLM.1)
## Poisson model
```
VA FIGURA 26
Figura 26. Gráfica de residuales del modelo lineal generalizado con distribución Poisson.
Como se observa en la figura 26 no se corresponden los residuales con los cuantiles teóricos. Vemos un total alejamiento y muy marcado entre ambas variables. Los residuales no se ubican dentro de las bandas de confianza esperables para los cuantiles teóricos. Se descarta este modelo y se procede a probar otros con la familia de distribución “binomial negativa” y “cuasipoisson”. Ambas distribuciones también son utilizadas para variables aleatorias de este tipo. La distribución de probabilidades Poisson asume que la media es igual a la varianza, probablemente es por ello que el modelo ajustado no haya sido bueno. La distribución binomial negativa asume que las varianzas son mayores a las medias. Con el ajuste de modelos lineales generalizados con estas dos últimas distribuciones mencionadas, se pueden modelar esos aumentos de las varianzas a medida que crecen los valores medios. A continuación del ajuste de los modelos también se realizarán las gráficas de diagnóstico. Las sentencias son relativamente sencillas y se muestran a continuación.
```r
GLM.2 <- glm.nb (conteo~sitioF, data=Kuehl4.1cangrejos)
summary(GLM.2)
## 
## Call:
## glm.nb(formula = conteo ~ sitioF, data = Kuehl4.1cangrejos, init.theta = 0.3402398618, 
##     link = log)
## 
## Deviance Residuals: 
##      Min        1Q    Median        3Q       Max  
## -1.90112  -1.21757  -0.68552   0.00815   2.02643  
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   3.5205     0.3446  10.216   <2e-16 ***
## sitioF2       0.7078     0.4867   1.454   0.1459    
## sitioF3       0.4043     0.4869   0.830   0.4064    
## sitioF4      -1.2969     0.4906  -2.644   0.0082 ** 
## sitioF5      -1.2179     0.4902  -2.484   0.0130 *  
## sitioF6      -0.9836     0.4894  -2.010   0.0444 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for Negative Binomial(0.3402) family taken to be 1)
## 
##     Null deviance: 204.73  on 149  degrees of freedom
## Residual deviance: 174.00  on 144  degrees of freedom
## AIC: 1145
## 
## Number of Fisher Scoring iterations: 1
## 
## 
##               Theta:  0.3402 
##           Std. Err.:  0.0384 
## 
##  2 x log-likelihood:  -1130.9960
```

En el resumen del nuevo modelo vemos una notable mejoría ya que el valor de “Residual deviance” (174) es más cercano a los grados de libertad (“degrees of freedom”, 144). A este modelo promisorio se le realiza un chequeo de residuales en seguida.

```r
hnp(GLM.2)
## Negative binomial model (using MASS package)
```
VA FIGURA 27

Figura 27. Gráfica de residuales del modelo lineal generalizado con distribución binomial negativa.
Tal como se preveía en el resumen del modelo GLM.2, en la figura 27 se muestra una gran concordancia entre los residuales y los cuantiles teóricos. Si bien hay residuales de menor magnitud que están por fuera de las bandas de confianza, la mejoría es muy notable.

A continuación, probaremos un tercer modelo lineal generalizado con distribución quasipoisson y con función de ligadura logaritmo natural. Los pasos son similares a los que se muestran en las figuras 24 y 25.

VA FIGURA 28
Figura 28. Ajuste de un modelo lineal generalizado con familia “quasipoisson”.
```r
GLM.3 <- glm(conteo ~ sitioF, family=quasipoisson(log), data=Kuehl4.1cangrejos)
summary(GLM.3)
## 
## Call:
## glm(formula = conteo ~ sitioF, family = quasipoisson(log), data = Kuehl4.1cangrejos)
## 
## Deviance Residuals: 
##      Min        1Q    Median        3Q       Max  
## -11.7132   -6.7252   -3.3662    0.1082   31.3642  
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   3.5205     0.3566   9.873   <2e-16 ***
## sitioF2       0.7078     0.4357   1.625   0.1064    
## sitioF3       0.4043     0.4604   0.878   0.3814    
## sitioF4      -1.2969     0.7696  -1.685   0.0941 .  
## sitioF5      -1.2179     0.7463  -1.632   0.1049    
## sitioF6      -0.9836     0.6835  -1.439   0.1523    
## ---
## Signif. Codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ‘ 1
## 
## (Dispersion parameter for quasipoisson family taken to be 107.4421)
## 
##     Null deviance: 12718  on 149  degrees of freedom
## Residual deviance: 10243  on 144  degrees of freedom
## AIC: NA
## 
## Number of Fisher Scoring iterations: 6
exp(coef(GLM.3))  # Exponentiated coefficients
## (Intercept)     sitioF2     sitioF3     sitioF4     sitioF5     sitioF6 
##  33.8000000   2.0295858   1.4982249   0.2733728   0.2958580   0.3739645
```

A este modelo en particular no le compararemos la “Residual deviance” con los “degrees of freedom”. Por otra parte, realizaremos una gráfica de diagnóstico de sus residuales.
```r
hnp(GLM.3)
## Quasi-Poisson model
```
VA FIGURA 29
Figura 29. Gráfica de residuales del modelo lineal generalizado con la familia quasipoisson.
Los mejores modelos lineales generalizados ajustados son el segundo y el tercero. En este último, casi la totalidad de sus residuales se encuentra dentro de las bandas de confianza (figura 29). Decidimos quedarnos con el segundo modelo ya que contamos también con criterios de información como la “Residual deviance” y el AIC. Con este modelo realizaremos las comparaciones múltiples de medias. Cargaremos los paquetes “emmeans” y “multcomp” como se indica en la figura 15 y luego se utilizarán las siguientes sentencias:

```r
library(emmeans)
library(multcomp)
cld(emmeans(GLM.2, ~ sitioF, type = "response"), details = FALSE, sort = TRUE, alpha = 0.05, Letters = c(LETTERS))
##  sitioF response    SE  df asymp.LCL asymp.UCL .group
##  4          9.24  3.23 Inf      4.66      18.3  A    
##  5         10.00  3.49 Inf      5.05      19.8  A    
##  6         12.64  4.39 Inf      6.40      25.0  AB   
##  1         33.80 11.65 Inf     17.20      66.4  ABC  
##  3         50.64 17.42 Inf     25.80      99.4   BC  
##  2         68.60 23.58 Inf     34.97     134.6    C  
## 
## Confidence level used: 0.95 
## Intervals are back-transformed from the log scale 
## P value adjustment: tukey method for comparing a family of 6 estimates 
## Tests are performed on the log scale 
## significance level used: alpha = 0.05 
## NOTE: Compact letter displays can be misleading
##       because they show NON-findings rather than findings.
##       Consider using 'pairs()', 'pwpp()', or 'pwpm()' instead.
```
Como podemos observar en la salida de las comparaciones de medias, el modelo lineal generalizado con una distribución binomial negativa, logró detectar diferencias significativas entre los sitios en cuanto al conteo de cangrejos. Se señalan tres grupos, uno de ellos en donde se encontraron los menores promedios de conteos que son los sitios 1, 4, 5 y 6. Un grupo intermedio compuesto por los sitios 1, 3 y 6. Por último, el grupo con mayor promedio de conteos de cangrejos está compuesto por los sitios 1, 2 y 3. Este solapamiento entre grupos, puede deberse al aumento de la variabilidad de conteos a medida que aumenta la media, pero asimismo logró detectarse diferencias.

# Bibliografía

- Box, G.E.P. & Cox, D.R. 1964. An analysis of transformations. Journal of the Royal Statistical Society. Series B. 26: 211–252.
- Dunn, P.K. & Smyth, G.K. 2018. Generalized linear models with examples in R. New York: Springer. 562 p.
- Kuehl, R.O. 2001. Diseño de experimentos: Principios estadísticos de diseño y análisis de investigación. 
- R Core Team 2021. R: A language and environment for statistical computing. R Foundation for Statistical Computing, Vienna, Austria. URL https://www.R-project.org/.
