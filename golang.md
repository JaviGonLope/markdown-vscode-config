# GO PROGRAMMING LANGUAGE

## QUÉ ES GO

Lenguaje de programación desarrollado por Google que se caracteriza por:

-	Ser compilado.
-	Es fuertemente tipado.
-	Tiene mecanismos muy poderosos para manejar concurrencia.

----------

## PAQUETES (PACKAGES)

Cada programa de Go está compuesto por paquetes. Un paquete es la forma en la que distribuimos o almacenamos el código de Go de una manera estructurada.

Un paquete se define en la primera la línea de los archivos de la siguiente forma:

```golang
package main // main = nombre del paquete
```

----------

## COMANDOS BÁSICOS

- `go build [filename].go`: compila el código del archivo indicado para generar un binario

- `go run [filename].go`: compila el código del archivo indicado para generar un binario temporal, ejecuta dicho binario y lo borra al finalizar la ejecución.

----------

## NOMBRES PÚBLICOS

En Go para exportar o hacer pública una variable o una función su nombre debe comenzar con letras mayúsculas.

Por lo tanto, cuando se importa un paquete solamente se pueden utilizar aquellas variables o funciones que sigan esta regla y comiencen con una letra mayúscula. Cualquier nombre "no exportado" no será accesible desde fuera del paquete en el que se define.

```golang
package main

import (
	"fmt"
	"math"
)

/* Podemos invocar la función Println del paquete main porque ha sido exportada.
En cambio, no podremos acceder a la variable pi del paquete math */
func main(){
	fmt.Println(math.pi)
}
```

## FUNCIONES

### Argumentos

En Go, las funciones puede tomar 0 o más argumentos.

```golang
func name(arg1 type, arg2 type) type {
	...
}
```

Si tenemos parámetros consecutivos con el mismo tipo, se puede omitir el tipo y especificarlo en el último parámetro.

```golang
func name(arg1, arg2, arg3 int, arg4 string) int {
	...
}
```

### Valores de retorno

Una función puede devolver cualquier cantidad de resultados. El tipo de estos valores de retorno se especifica tras la lista de argumentos que recibe la función.

```golang
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

Los valores de retorno de una función pueden estar nombrados, en cuyo caso son tratados como variables definidas al principio de la función.

Una sentencia return sin argumentos devolverá estos valores de retorno con nombre, en lo que se conoce como `naked return`.

```golang
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}
```

**IMPORTANTE**: Solo se recomienda utilizar `naked return` en funciones pequeñas, ya que pueden perjudicar la legibilidad del código en funciones largas.

----------

## VARIABLES

Para declarar una variable o un listado de variables se utiliza la sentencia `var` seguida del nombre de la variable y su tipo.

En la sentencia `var` se pueden inicializar las variables. En este caso, Go inferirá el tipo de la variable a partir de su valor, por lo que podemos omitir el tipo

```golang
var c, python, java bool
var op1, op2, result int
var name = "Geralt of Rivia"
```

### Operador `:=` (short assignment statement)

Dentro de una función se puede usar el operador `:=` en lugar de la sentencia `var` para hacer una declaración de variables con el tipo implícito.

```golang
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

Fuera de la función, cualquier sentencia tiene que comenzar con una palabra clave (`var`, `func`, etc.) por lo que su uso no está permitido.

----------

## Tipos básicos

```golang
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias para uint8

rune // alias para int32 => Unicode

float32 float64

complex64 complex128
```

Los tipos `int`, `uint` y `uintptr` normalmente son de 32 bits en sistemas de 32 bits y de 64 bits en sistemas de 64 bits.

### Valores cero

Cuando se declara una variable y no se inicializa de forma explícita, Go asigna por defecto los siguientes valores en función del tipo de variable:

- Tipos numéricos: 0
- Boolean: false
- Cadenas de texto: "" (cadena vacía)

### Conversión de tipos

Para convertir un valor(v) de un tipo a otro tipo(T) se utiliza la expresión T(v).

```golang
i := 42
f := float64(i)
u := uint(f)
```

### Constantes

En Go, las constantes se declaran igual que las variables pero usando la palabra `const` en lugar de `var`. No pueden ser declaradas usando el operador `:=`.

```golang
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

----------

## Bucles y condicionales

### Bucle For

A diferencia de otros lenguajes de programación, si queremos hacer bucles en Go solo disponemos del bucle `for`.

Un `for` básico consta de tres componentes separados por `;`:

- **init statement**: se ejecuta antes de la primera iteración. Aquí se suelen declarar variables usando el operador `:=`. Dichas variables solo son visibles en el ámbito del bucle `for`.
- **condition expression**: condición a evaluar antes de cada iteración para comprobar si hay que proseguir con las iteraciones o hemos llegado al final del bucle.
- **post statement**: ejecutado al final de cada iteración

```golang
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

De los 3 componentes, el **init statement** y el **post statement** son opcionales (consiguiendo el mismo comportamiento que tendríamos en un `while` en otros lenguajes):

```golang
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

Si omitimos la condición del bucle, tendremos un bucle infinito:

```golang
package main

func main() {
	for {
	}
}
```

### Sentencias If

En el caso de los `if`, Go no requiere el uso de paréntesis`()` para la condición pero si obliga al uso de llaves`{}`.

Al igual que ocurre en los bucles `for`, un `if` puede comenzar con una asignación usando `:=` que se ejecuta antes de la evaluación de la condición. Dichas variables solo son visibles para el código que haya dentro del `if` (incluido en el `else`).

```golang
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}

	return lim // aquí no se puede usar v
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

### Switch

La sentencia `switch` es igual que la de lenguajes como C, C++, Java, JavaScript o PHP con la única diferencia de que Go solo ejecuta el caso que coincide con condición (o `default`). Es decir, no es neceario usar la palabra `break` al final de cada opción.

Otra diferencia importante es que los opciones del bucle no tienen porqué ser constantes o valores numéricos enteros.

```golang
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	switch time.Saturday {
	case today + 0:
		fmt.Println("Today.")
	case today + 1:
		fmt.Println("Tomorrow.")
	case today + 2:
		fmt.Println("In two days.")
	default:
		fmt.Println("Too far away.")
	}
}
```

----------

## Defer

La sentencia `defer` retrasa la ejecución de una función hasta que la función que la contiene ha concluido su ejecución, aunque sus argumentos son evaluados inmediatamente, no esperan a la invocación para ser calculados.

```golang
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

Las funciones invocadas usando `defer` son colados en una pila, por lo que son llamadas siguiendo un orden LIFO.

```golang
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

## Memoria y Punteros

Cuando se declara una variable y se le asigna un valor, Go asigna una cantidad de memoria RAM para almacenarla, dependiendo dicha cantidad de memoria del tipo de datos empleado.

Esa memoria tiene una *dirección de memoria* (representada mediante código hexadecimal) que permite a Go encontrar el valor de la variable cuando lo necesita.

Para acceder a la dirección de memoria de una variable Go dispone del operador `&`, que se utiliza delante del nombre de la variable

```golang
i := 20
fmt.Println(&i) // prints 0xc000100010
```

Un **puntero** es una variable que apunta a la dirección de memoria de otra variable.
La sintaxis para crear un puntero es:

```golang
var p *Type //Type = tipo de datos de la variable a la que apunta
```

El valor de un puntero que no ha sido inicializado es `nil`, ya que no está apuntando a ningún valor de la RAM en ese momento.

Para acceder al valor de la variable a la que apunta el puntero tenemos que usar el operador `*`, conocido como **operador de indirección**.

```golang
a := 7
pa := &a // Go infiere el tipo *int para "pa"

fmt.Printf("El dato que hay en %v es %v\n", pa, *pa)
```

### Función new()

La función `new()` reserva memoria y devuelve un puntero a esa dirección de memoria. Esta función recibe como argumento el tipo de dato que se quiere almacenar, procede a la asignación de memoria, escribe el valor por defecto del tipo en esa dirección de memoria y devuelve el puntero.

```golang
package main

import "fmt"

func main(){
	pa := new(int)

	fmt.Printf("El dato que hay en %v es %v\n", pa, *pa)
}
```

## Aspectos básicos

La función main() es el punto de inicio de todo el código, es lo que va a ejecutar el compilador.

Para declarar una variable disponemos de dos mecanismos, la declaración explícita y el operador “:=”.

----------

## Error Handling

El manejo de errores dentro de Go es algo más sencillo que en otros lenguajes de programación dado que las funciones pueden tener más de un valor de retorno.

La convención dentro de Go es que una función debe regresar dos valores: el valor esperado y un valor de error.

Si la función se ejecuta con éxito devolverá el valor esperado y un error con valor de nil. Si por el contrario ocurrió algún error durante la ejecución de la función, el valor esperado se devuelve como nil y el error se devuelve con su información correspondiente.

```go
value, error := foo.Func(args)
if err != nil{
	fmt.Println("Ha ocurrido un error")
}
```

----------

## Imports

A la hora de importar librerías, podemos escribir la palabra import y agrupar todas las que queramos entre paréntesis:

```go
import (
   “fmt”
   “math”
)
```

También se pueden escribir múltiples sentencias import, pero es aconsejable utilizar la primera opción.

```go
import “fmt”
import “math”
```

----------

## Structs

Los `structs` son tipos de datos definidos por el usuario.

Más temas a desarrollar:

•	Receivers

•	Manejo de errores

•	Slices vs Array

•	Punteros

•	Iteradores (range)

•	Maps

•	Interfaces

•	Gorutinas y channels

•	Operador ...
