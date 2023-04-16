# Método toString

El método ``toString`` es un método en _Scala_ que devuelve un string que representa a un objeto. 
Es usado generalmente para propósitos de debugging y logging, y puede ser sobreescrito para proveer
una implementación personalizada.

## Índice

- [Implementación por defecto](#implementación-por-defecto)
- [Sobreescritura de toString](#sobreescritura-de-tostring)
- [Interpolación de strings](#interpolación-de-strings)

## Implementación por defecto

La implementación por defecto de ``toString`` devuelve el nombre de la clase seguido por la dirección
de memoria del objeto.
Consideremos la siguiente clase:

```scala	
class Person(val name: String, val age: Int)
```

Si creamos una instancia de esta clase y la imprimimos en la consola:

```scala
val person = new Person("Shinji Ikari", 14)
println(person)
```

El resultado será una referencia a la dirección de memoria del objeto, como:

```
Person@1234abcd
```

Esto es porque la implementación por defecto de ``toString`` en _Scala_ es imprimir el nombre de la 
clase seguido por un signo de arroba (``@``) y el código hash del objeto en formato hexadecimal.

## Sobreescritura de toString

Si queremos una representación más significativa del objeto cuando se imprima, podemos sobreescribir
el método ``toString`` en nuestra clase.
Por ejemplo, en una clase ``Person``, el método ``toString`` podría devolver un string que incluya 
el nombre, la edad y otra información relevante de la persona.

```scala
class Person(val name: String, val age: Int) {
  override def toString: String = s"Person(name=$name, age=$age)"
}

val person = new Person("Shinji Ikari", 14)
println(person.toString) // Person(name=Shinji Ikari, age=14)
```

## Interpolación de strings

La interpolación de strings es una forma de crear strings que incluyen valores de variables y
expresiones.
En _Scala_, la interpolación de strings se realiza con el prefijo ``s`` seguido de un string entre
comillas dobles.
El string puede contener expresiones entre llaves (``{}``), que serán reemplazadas por sus valores
cuando el string sea creado.

Cuando se usa la interpolación de strings, las variables y expresiones que se incluyen en el string
se evalúan con sus métodos ``toString``.
Por ejemplo, utilizando la misma clase ``Person`` que en el ejemplo anterior:

```scala
val person = new Person("Shinji Ikari", 14)
println(s"The string interpolation uses the toString method: $person")

/*
Output:
The string interpolation uses the toString method: Person(name=Shinji Ikari, age=14)
*/
```