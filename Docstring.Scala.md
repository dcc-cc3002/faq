# Guía de documentación

Cuando escribimos código, es sumamente importante documentarlo para que otros desarrolladores puedan 
entender qué hace nuestro código y cómo usarlo. 
Aquí hay algunas buenas prácticas para documentar código en Scala:

## Utilizar la sintaxis de documentación de ScalaDoc

*ScalaDoc* es una herramienta de documentación para Scala que genera documentación en formato HTML a
partir del código fuente.

La sintaxis básica es la siguiente:

- ``/**`` para iniciar un comentario de documentación (un comentario que comienza con ``/*`` no es
  considerado como un comentario de documentación)
- ``*/`` para terminar un comentario de documentación
- ``*`` para iniciar una nueva línea en el comentario de documentación
- ``@`` para indicar una etiqueta de documentación

## Documentar clases

Para documentar una clase, se debe escribir un comentario de documentación justo antes de la 
definición de la clase.

```scala
/** A class representing a rectangle.
 *
 * The rectangle is defined by its width and height, both of which must be greater than zero.
 *
 * @param width The width of the rectangle.
 * @param height The height of the rectangle.
 *
 * @constructor Creates a new rectangle with the specified width and height.
 *
 * @example
 * {{{
 * val rectangle = new Rectangle(5.0, 10.0)
 * val area = rectangle.area()
 * println(s"The area of the rectangle is $area")
 * }}}
 *
 * @throws InvalidShapeException if either width or height is less than or equal to zero.
 *
 * @see Square
 *
 * @author Todoroki Shoto
 * @since 1.0.0
 * @version 1.2.2
 */
class Rectangle(val width: Double, val height: Double) extends Shape {
  // ...
}
```

- La primera línea del comentario de documentación debe ser una breve descripción de la clase.
- Las siguientes líneas deben ser una descripción más detallada de la clase (en caso de que sea
  necesario).
- ``@param`` se usa para documentar los parámetros que recibe el constructor.
- ``@constructor`` se usa para documentar lo que hace el constructor de la clase.
- ``@example`` se usa para documentar un ejemplo de cómo usar la clase. El ejemplo debe estar
  encapsulado entre ``{{{`` y ``}}}``.
- ``@throws`` se usa para documentar las excepciones que puede lanzar el constructor.
- ``@see`` se usa para documentar una referencia a la documentación relacionada.
  También se puede agregar un enlace a una url externa utilizando ``@see [[url]]``.
- ``@author`` se usa para documentar el autor de la clase.
- ``@since`` se usa para documentar la versión de la clase desde la cual fue introducida.
- ``@version`` se usa para documentar la versión actual de la clase.

## Documentar métodos y funciones

Para documentar un método, se debe escribir un comentario de documentación justo antes de la
definición del método.

```scala
/** Calculates the factorial of a given integer.
 *
 * The factorial of a non-negative integer n is defined as the product of all positive integers less
 * than or equal to n.
 *
 * This function calculates the factorial of n using a recursive algorithm. 
 * If n is zero or one, the function returns 1.
 * Otherwise, it multiplies n by the factorial of (n - 1) and returns the result.
 *
 * @param n The integer whose factorial is to be calculated.
 * @return The factorial of n.
 * @throws NegativeIntegerException if n is negative.
 *
 * @example
 * {{{
 * val factorialOfFive = factorial(5)
 * println(s"The factorial of 5 is $factorialOfFive")
 * }}}
 *
 * @see [[https://en.wikipedia.org/wiki/Factorial]]
 */
def factorial(n: Int): Int = {
  // ...
}
```

- La primera línea del comentario de documentación debe ser una breve descripción del método.
- Las siguientes líneas deben ser una descripción más detallada del método (en caso de que sea
  necesario).
- ``@param`` se usa para documentar los parámetros que recibe el método.
- ``@return`` se usa para documentar lo que retorna el método.
- ``@throws`` se usa para documentar las excepciones que puede lanzar el método.
- ``@example`` se usa para documentar un ejemplo de cómo usar el método. El ejemplo debe estar
  encapsulado entre ``{{{`` y ``}}}``.
- ``@see`` se usa para documentar una referencia a la documentación relacionada.

## Documentar variables y constantes

Para documentar una variable o constante, se debe escribir un comentario de documentación justo
antes de la definición de la variable o constante.

```scala
/** ... */
class Book(val title: String, val author: String, val numPages: Int) {

  /** The format of the book.
   *
   * This variable specifies the format of the book, such as hardcover, paperback, or e-book.
   */
  var format: String = "hardcover"
}
```	

## Documentar _generics_

```scala
/**
 * ...
 * @tparam A The type of element held in the queue.
 */
trait Queue[A] {
  // ...
}
```

- ``@tparam`` se usa para documentar los parámetros de tipo de una clase, trait o método.

## Buenas prácticas

- Escribir la documentación en inglés. Esto permite que la documentación sea más fácil de entender
  para la mayoría de los desarrolladores.
- Como mínimo, *Scaladoc* está presente para cada clase pública, y cada miembro público o protegido 
  de tal clase.
- Los nombres de las clases, métodos, variables y otros elementos de código deben ser 
  significativos. Esto hace que sea más fácil para los lectores entender el propósito del código
  sin tener que leer la documentación.
- Las etiquetas de una función pueden ser omitidas si la función es simple y queda claro de su 
  documentación lo que hace.
```scala	
/** Returns the sum of two integers. */
def sum(a: Int, b: Int): Int = a + b
```
- Usa comentarios para explicar aspectos complejos o no obvios de tu código. 
  No simplemente repitas lo que el código hace, sino que explica por qué lo está haciendo y cómo se 
  ajusta al contexto más amplio de tu aplicación.

```scala
/**
  * Returns the maximum element in a list of integers.
  */
def max(xs: List[Int]): Int = {
  // If the list is empty, return 0
  if (xs.isEmpty) 0
  else {
    // Find the maximum element in the rest of the list
    val maxRest = max(xs.tail)
    // Return the maximum of the first element and the maximum of the rest
    if (xs.head > maxRest) xs.head else maxRest
  }
}
```
- Utiliza la sintaxis de *Scaladoc* para documentar tu código.
  Esto permitirá que tu documentación se genere automáticamente e se integre en herramientas de 
  desarrollo como IDEs y sistemas de construcción.
- Utiliza un estilo consistente para tu documentación. 
  Esto incluye formato, mayúsculas, puntuación y terminología. 
  La consistencia hace que tu documentación sea más fácil de leer y entender.
- Mantén tu documentación actualizada. 
  A medida que hagas cambios en tu código, asegúrate de actualizar la documentación para reflejar 
  esos cambios. 
  Esto ayudará a prevenir la confusión y asegurará que otros desarrolladores puedan entender y 
  utilizar tu código de manera efectiva.
- La documentación de un método puede omitirse cuando éste sobrescribe un método. 
  En este caso, la documentación se hereda.