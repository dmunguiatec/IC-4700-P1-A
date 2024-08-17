IC-4700 Lenguajes de programación  
Prof. Diego Munguia Molina  
IC-AL  
---
# Proyecto 1

## Objetivos de aprendizaje

1. Programar soluciones a problemas computacionales utilizando el lenguaje de programación C (III).

## Descripción

Para demostrar que C es un lenguaje de programación Turing-completo vamos a escribir un simulador de máquinas de Turing. El simulador leerá la descripción de una máquina de Turing desde un archivo de texto con extensión `.turing` y ejecutará el algoritmo descrito por dicha máquina.

### Formato del archivo

El archivo de descripción de máquinas de Turing tendrá el siguiente formato

```
# <COMENTARIOS DE LÍNEA>
entrada: <ESTADO INICIAL DE LA CINTA>
inicio: <ESTADO INICIAL DEL AUTÓMATA>
reglas:
# múltiples líneas, cada línea representa una regla en el siguiente formato:
# estado, lectura, escritura, movimiento, siguiente
<ETIQUETA ESTADO>,<SÍMBOLO LECTURA>,<SÍMBOLO ESCRITURA>,<MOVIMIENTO>,<ETIQUETA SIGUIENTE ESTADO>
```

El siguiente es un ejemplo de archivo que describe una máquina de Turing que incrementa un número binario dado en 1

```
# Incrementa el número binario en 1
entrada: 10
inicio: derecha
reglas:
# estado, lectura, escritura, movimiento, siguiente
derecha,1|0,,>,derecha
derecha,_,,<,acarreo
acarreo,1,0,<,acarreo
acarreo,0|_,1,<,fin
fin,,,,  
```

Los **COMENTARIOS DE LÍNEA** no deben ser procesados de forma alguna, y pueden colocarse en cualquier lugar del archivo. La única restricción es que deben estar en su propia línea, no pueden colocar al final de una línea de datos del archivo. Por ejemplo, el siguiente es un comentario inválido:

```
entrada: 10 # Incrementa el número binario en 1
```

El **ESTADO INICIAL DE LA CINTA** se presenta como una hilera de caracteres que llamaremos *símbolos*. Al iniciar la simulación, cada símbolo se carga en una celda de la cinta siguiendo la secuencia indicada por la hilera.

El **ESTADO INICIAL DEL AUTÓMATA** es un identificador que corresponde al estado inicial del autómata a partir del cual inicia la iteración sobre las reglas.

Cada **ETIQUETA ESTADO** y **ETIQUETA SIGUIENTE ESTADO** es un identificador que representa el nombre del estado correspondiente. 

El **SÍMBOLO LECTURA** representa la condición basada en el símbolo que se encuentra en la celda actual de la cinta. El símbolo especial `_` será llamado vacío y representa el hecho de que la celda de la cinta no contiene ningún símbolo, es decir, que está vacía. Este componente de la regla es opcional.

Se puede representar como un único símbolo, o como una conjunción de símbolos concatenados por el operador `|`. Por ejemplo la expresión `x` indica "cuando se lee el símbolo `x` en la celda actual de la cinta...", mientras que la expresión `0|1|_` se lee "cuando la celda contiene un `0` o un `1` o está vacía...".

El **SÍMBOLO ESCRITURA** representa el símbolo que se debe escribir en la posición actual de la celda, puede ser cualquier símbolo incluyendo `_`. Este componente de la regla es opcional.

El operador **MOVIMIENTO** indica en qué dirección se debe mover la cabeza lectora de la cinta, el caracter `<` indica un movimiento a la izquierda, mientras que `>` indica un movimiento a la derecha.

Debemos asumir que el archivo `.turing` siempre estará bien formado, es decir no es necesario implementar ninguna validación sintáctica.

### Funcionamiento del simulador

El simulador será una herramienta de línea de comando que recibirá como argumento el nombre del archivo que contiene la descripción de la máquina de Turing a simular

```bash
$ bin/turing inc.turing
```

Al ejecutar el comando inmediatamente inicia la simulación de la máquina de Turing. El usuario podrá observar el progreso del autómata en la consola. En cada iteración se imprimirá en pantalla la etiqueta del estado actual en que se encuentra la máquina, y el estado de la cinta y de la cabeza lectora.

```bash
$ bin/turing inc.turing
derecha
_ 1 0 _
  ^

derecha
_ 1 0 _
    ^

derecha
_ 1 0 _
      ^

acarreo
_ 1 0 _
    ^

fin
_ 1 1 _
  ^

$
```

No podemos saber a priori si la máquina va a finalizar su ejecución en algún momento. De forma que la ejecución podría finalizar cuando se alcance un estado para el cual no se puede aplicar una transición a un siguiente estado, pero podría ser el caso que por el propio diseño de la máquina esto nunca suceda. De esta manera la ejecución se puede detener en cualquier momento interrumpiendo el proceso a través de `CTRL+C`.

### Recomendaciones técnicas

* Para simular la cinta infinita utilizaremos una lista doblemente enlazada, lo cual facilitará los movimientos a la derecha y a la izquierda.

* La cinta se inicializará con los valores descritos en el archivo `.turing` y adicionalmente se creará una celda vacía al inicio y al final.

* Puesto que el manejo de hileras en C es limitado, se recomienda hacer un mapeo entre los identificadores de estado y números enteros.

## Metodología

* El proyecto se trabajará en equipos.
* Cada equipo designará dos personas con el rol de relatoras, estas personas tendrán la responsabilidad de tomar notas durante las reuniones de revisión que serán utilizadas posteriormente para hacer mejoras al código.
* El proyecto se desarrollará en el transcurso de dos semanas y tendrá dos entregas, una entrega parcial después de la primera semana de trabajo, y una entrega final después de la segunda semana de trabajo.
* El proyecto se desarrollará en C siguiendo el paradigma imperativo procedimental.

## Rúbricas de evaluación

Disponible en el siguiente enlace:

https://docs.google.com/spreadsheets/d/1WWrRt78QxuyEEPirRy2Gi2BHrDPxsZruDwzx4GRwVcM



