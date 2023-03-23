Taller 03
================
dacarras
`r format(Sys.time(), "%a %b %d, %Y")`

------------------------------------------------------------------------

# Instrucciones

- Los siguientes ejercicios implican generar los resultados de un
  instrumento compuesto por varias preguntas, hacer un análisis de un
  modelo de regresión simple, graficar los resultados y finalmente,
  comparar los resultados con un modelo de regresión con variables
  estandarizadas.

- Al igual que en la tarea 2, vamos a una copia de los datos del estudio
  de Poli victimizacion de Jovenes, realizada en Chile en Octubre de
  2017.

- Los datos que vamos a emplear son una versión recortada de los datos y
  con nombres adaptados, que se espera sean más amigables para generar
  los resultados programando en R.

- El archivo que contiene los datos que vamos a emplear se llama:

<!-- -->


    datos_poli_2017.csv

- El libro de codigos de la base de datos que vamos a emplear, se llama:

<!-- -->


    datos_poli_2017_codebook.xlsx

- **Advertencia**: Los datos originales provienen de una muestra
  probabilística. Este tipo de datos, permite realizar inferencias a la
  población, si la información del diseño es empleada para producir
  estimaciones. En este ejercicio con fines ilustrativos, vamos a
  ignorar este aspecto, y solo vamos a producir resultados descriptivos.

# Referencias

Alvarez, E., Guajardo, H., & Messen, R. (1986). Estudio exploratorio
sobre una escala de autoevaluación para la depresión en niños y
adolescentes. Revista Chilena de Pediatria, 57(1), 21–25.
<https://doi.org/10.4067/s0370-41061986000100003>

Birleson, P., Hudson, I., Buchanan, D. G., & Wolff, S. (1987). Clinical
Evaluation of a Self‐Rating Scale for Depressive Disorder in Childhood
(Depression Self‐Rating Scale). Journal of Child Psychology and
Psychiatry, 28(1), 43–60.
<https://doi.org/10.1111/j.1469-7610.1987.tb00651.x>

Consejo Nacional de la Infancia. (2018). Análisis Multivariable de
Estudio de Polivictimización en Niños, Niñas y Adolescentes realizado
por la Pontificia Universidad Católica de Chile.
<http://biblioteca.digital.gob.cl/handle/123456789/3535>

Denda, K., Kako, Y., Kitagawa, N., & Koyama, T. (2006). Assessment of
depressive symptoms in Japanese school children and adolescents using
the birleson depression self-rating scale. International Journal of
Psychiatry in Medicine, 36(2), 231–241.
<https://doi.org/10.2190/3YCX-H0MT-49DK-C61Q>

MINSAL. (2013). Guía Clínica para el tratamiento de adolescentes de 10 a
14 años con Depresión.
<https://www.guiadisc.com/wp-content/pdfs/guia-clinica-tratamiento-depresion-adolescentes.pdf>

Subsecretaria Prevención del Delito. (2018). Primera Encuesta Nacional
de Polivictimización en Niñas, Niños y Adolescentes: Presentación de
Resultados.

------------------------------------------------------------------------

# Tarea 03

## Ejercicio 1. Abrir los datos.

- Abra los datos `datos_poli_2017.csv`, empleando la función
  `read.csv()`. Emplee un objeto llamado `data_poli_full` para alojar a
  los datos abiertos.

``` {r}

# Instrucciones: Pegue o escriba los códigos utilizados en las siguientes 
#                líneas [no coloque el signo gato antes de su respuesta]
#                Una vez terminado su código, borre estos comentarios.


data_poli_full <- read.csv('datos_poli_2017.csv')

```

## Ejercicio 2. Vista previa de a los datos.

- **¿Cuántas variables, y cuántos casos posee la base de datos
  original?**
- Indique su respuesta bajo el código.

``` {r}

# Instrucciones: Escriba aqui un comando para obtener la 
#                cantidad de variables, y de casos observados
#                de la base de datos empleada.


dplyr::glimpse(data_poli_full)
```

- Respuesta
  - Casos: \[escribir aqui la cantidad de casos, y borrar los
    corchetes\]
  - Variables: \[escribir aqui la cantidad de variables, y borrar los
    corchetes\]

## Ejercicio 3. Generar muestra aleatoria

- Al igual que en la tarea 1, queremos que se produzcan resultados que
  sean únicos para cada uno de ustedes. De esta forma, les solicitamos
  que generen una muestra de datos que sea única a su rut. En esta
  sección solo tendra que reemplazar el valor de `set.seed()`, de modo
  que se genere una muestra de datos que fuera única. Recuerde que
  **todos los ejericicios** siguentes, **requieren** que **se emplen los
  datos generados**.

``` {r}

# Instrucciones: borre a "#" al lado de "set.seed()", e incluya su RUT
#                como argumento para fijar al seed.
#                Genere la muestra aleatoria solicitada.
#                Esta muestra contiene el 50% de los datos originales.

set.seed(123456789)     # solo reemplazar el set.seed, y conservar el resto del código.
library(dplyr)
data_poli <- dplyr::slice_sample(data_poli_full, prop = .5, by = comu)

```

## Ejercicio 4. Crear una variable con los puntajes de autoestima de Rosenberg

- La escala de autoestima de Rosenberg contiene 5 afirmaciones
  positivas, y 5 afirmaciones en negativo.

- Para que los puntajes totales varíen de 10 a 50 puntos, pero siempre
  representen que, mayor puntaje indique mayor autoestima, se invierte
  el valor digitado de los items invertidos. Una vez realizado el paso
  anterior, se suman las respuestas originales, y las recodificadas, y
  se produce un puntaje que varía de 10 a 50 puntos.

- A continuación, se indican los items positivos y negativos (au3, au5,
  au8, au9, au10).

<!-- -->

    variable  invertido texto_item
    au1       no        Siento que soy una persona valiosa, al menos igual que los demás
    au2       no        Siento que tengo cualidades positivas
    au3       sí        En general, tiendo a sentir que soy un fracaso
    au4       no        Soy capaz de hacer las cosas tan bien como la mayoría de las otras personas
    au5       sí        Siento que no tengo mucho de lo que sentirme orgulloso
    au6       no        Tengo una actitud positiva hacia mí mismo
    au7       no        Considerando todas las cosas, estoy satisfecho conmigo mismo
    au8       sí        Me gustaría tener más respeto conmigo mismo
    au9       sí        Me siento inútil a veces
    au10      sí        Algunas veces pienso que no soy bueno en absoluto

- En este ejericicio tiene que crear el puntaje de la escala de
  autoestima de Rosenberg. Se recomienda realizar esto en pasos. Los
  pasos son:

  - Recodificar los items negativos, en variables que se llamen `au*r`.
    Por ejemplo, `au3`, se recodifica como `au3r`.
  - Luego de recodificar los items respectivos, sume las respuestas de
    los items positivos, y los valores de las recodificaciones de los
    items negativos.
  - La suma, es por fila. Para estos fines emplee la función
    `rowSums(cbind(), na.rm = TRUE)`, u otra equivalente.
  - Guarde los resultados en una variable llamada `auto`.
  - Revise los resultados generados. Genere un plot que compare los
    puntajes de `auto`, y los de `sef`. Debiera encontrar una línea
    recta. Los puntajes deben ser iguales.

``` {r}

# Instrucciones: los códigos utilizados en la siguiente línea [no coloque el signo gato antes de su respuesta]

# invertir items, y crear puntaje total
data_model <- data_poli %>%
              mutate(au3r = 5 + 1 - au3) %>%
              mutate(au5r = 5 + 1 - au5) %>%
              mutate(au8r = 5 + 1 - au8) %>%
              mutate(au9r = 5 + 1 - au9) %>%
              mutate(au10r = 5 + 1 - au10) %>%
              mutate(auto = rowSums(cbind(
                au1, au2,   au3r,
                au4, au5r,  au6,
                au7, au8r,  au9r, au10r
                ), na.rm = TRUE)) %>%
              dplyr::glimpse()

# plot de puntajes originales y generados
plot(y = data_model$self, x = data_model$auto)
```

## Ejercicio 5. Diagrama de dispersión entre la variable autoestima (`auto`) y la variable depresion (`dep`).

- Inserte los códigos utilizados para generar un gráfico de dispersión
  de las dos variables indicadas.

- Especifique a depresion como variable de respuesta, y a autoestima
  como variable predictora.

- En el mismo gráfico incluya una linea que muestre el promedio de la
  variable de respuesta.

``` {r}

# Instrucciones: los códigos utilizados en la siguiente línea [no coloque el signo gato antes de su respuesta]

# Recomendaciones: para agregar una linea al gráfico puede  
#                  utilizar el comando "abline()".

# plot de puntajes originales y generados
plot(y = data_model$dep, x = data_model$auto)
abline(h = mean(data_model$dep, na.rm = TRUE), col = "red", lty = 1)
```

## Ejercicio 6. Regresión lineal simple prediciendo la variable depresión (dep) en base al predictor autoestima (auto)

- Inserte los códigos utilizados para ajustar una regresión lineal
  utilizando la variable depresión como variable de respuesta y la
  variable autoestima como variable predictora.

- Despliegue el output del modelo de regresión ajustado empleando al
  comando `summary()`

``` {r}

# Instrucciones: los códigos utilizados en la siguiente línea [no coloque el signo gato antes de su respuesta]

lm(dep ~ 1 + auto, data = data_model) %>%
summary()

```

## Ejercicio 7. Reporte los resultados de la regresión.

- Indique los valores de los siguientes resultados en el modelo de
  regresión:

- **Respuesta**

  - El coeficiente del intercepto es:
    - b0 = 30,49 (SE = 0.22, t = 139.06, p \< .001)
  - El coeficiente de la pendiente es
    - b1 = -0.54 (SE = 0.01, t = -87.63, p \< .001)
  - La desviación estándar de los residuos (i.e., **residual standard
    error**)
    - RSE = 4.54 (df = 9041)
  - El coeficiente de determinación (r cuadrado) es
    - R^2 = .46

## Ejercicio 9. Graficar la recta de regresión

- Genere un nuevo diagrama de dispersión entre la variable depresion y
  la autoestima.

- En el mismo gráfico use los coeficientes de intercepto y pendiente
  para mostrar la recta de regresión que estimo en el ejercicio
  anterior.

``` {r}

# Instrucciones: los códigos utilizados en la siguiente línea [no coloque el signo gato antes de su respuesta]


# Recomendaciones: para agregar una linea al gráfico puede  
#                  utilizar el comando "abline()".


plot(y = data_model$dep, x = data_model$auto)
abline(lm(dep ~ 1 + auto, data = data_model), col = "red", lty = 1)
```

## Ejercicio 10. Interprete la desviación estándar de los residuos y el coeficiente de determinación

- **Respuesta**

  - La desviación estándar de los residuos me indica qué:
    - La desviación estandar de los residuos del modelo ajustado,
      indican que la dispersion típica de los residuos es de 4.54
      puntos.
  - El coeficiente de determinación (r cuadrado) me indica qué:
    - El coeficiente de determinación indica que la inclusión del
      predictor redujo el total de los residuos del modelo base en un
      45%.

## Ejercicio 11. Repetir la regresión usando variables estandarizadas.

- Inserte los códigos para convertir a puntaje Z la variable depresion y
  guardela en la variable `dep_z`.

- Convierta a puntaje Z la variable autoestima (`age`) y guardela en la
  variable `auto_z`

- Inserte los códigos utilizados para realizar una regresión lineal
  utilizando la variable `dep_z` como variable de respuesta y la
  variable `auto_z` como variable predictora.

``` {r}

# Pegue los códigos utilizados en la siguiente línea [no coloque el signo gato antes de su respuesta]

data_model <- data_model %>%
              mutate(dep_z = scale(dep, scale = TRUE)) %>%
              mutate(auto_z = scale(auto, scale = TRUE)) %>%
              dplyr::glimpse()


lm(dep_z ~ 1 + auto_z, data = data_model) %>%
summary()

```

## Ejercicio 12. Comparación de salidas

- ¿Que elementos se mantienen equivalente entre ambos modelos ajustados?

- Intercept (b0) = (Sí/No)

- Pendiente (b1) = (Sí/No)

- Residuos (e_i) = (Sí/No)

- Coeficiente de deterninación (R^2) = (Sí/No)
