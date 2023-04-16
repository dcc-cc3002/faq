# Constructores en Scala

Los constructores en _Scala_ (y en general en la programación orientada a objetos) no son métodos, 
sino más bien **bloques de código** que se utilizan para inicializar instancias de una clase. 
Se llaman _automáticamente_ cuando se crea una instancia de la clase y son responsables de 
establecer el estado inicial del objeto.

Si bien los constructores pueden parecerse a los métodos en algunos aspectos, como su sintaxis y el 
hecho de que toman argumentos, **difieren de los métodos en varias formas clave**.
Una de las principales diferencias es que los constructores _no devuelven valores_, mientras que los 
métodos sí (un método que "no retorna nada" en realidad retorna `Unit`).
Además, los constructores _no se heredan_ por las subclases, mientras que los métodos sí.

Por lo tanto, es más preciso referirse a los constructores como un bloque de código que se ejecuta
cuando se crea una instancia de un objeto, en lugar de como un "método especial".

## Índice

* [Constructores primarios](#constructores-primarios)
* [Constructores auxiliares](#constructores-auxiliares)
* [Herencia](#herencia)
* [Valores por defecto](#valores-por-defecto)
* [Errores comunes](#errores-comunes)
* [Ejercicios resueltos](#ejercicios-resueltos)

## Constructores primarios

En _Scala_, el constructor primario es el constructor principal que se define en la definición de la
clase.
El constructor primario es responsable de inicializar las propiedades de la clase y se llama
automáticamente cuando se crea una instancia de la clase. 
Si hay algún parámetro en el constructor primario, debe proporcionarse al momento de la
instanciación.

Un ejemplo de constructor primario se puede ver en una clase que representa una persona.
La clase podría tener propiedades como nombre, edad y dirección, que se pueden inicializar en el
constructor primario. 

```scala
class Person(val name: String, val age: Int, val address: String) {
  // other methods and properties
}
```

En este ejemplo, el constructor primario toma tres parámetros: nombre, edad y dirección.
Estos parámetros se utilizan para inicializar las propiedades correspondientes de la clase `Person`.
Cuando se crea una instancia de la clase, el constructor primario se llama automáticamente, con los
valores para nombre, edad y dirección proporcionados en el momento de la instanciación. Por ejemplo:

```scala
val john = new Person("Ichigo Kurosaki", 17, "Karakura Town")
```

## Constructores auxiliares

Los constructores auxiliares son constructores adicionales que una clase puede tener aparte del
constructor primario.
A diferencia del constructor primario, que se define en la firma de la clase y se llama
automáticamente cuando se crea una instancia de la clase, los constructores auxiliares se definen
usando la palabra clave `this` y deben llamar al constructor primario o a otro constructor auxiliar
como su primera declaración.

Un ejemplo de clase que tiene un constructor primario y dos constructores auxiliares se muestra a
continuación:

```scala
class Person(var name: String, var age: Int) {
  println("Primary constructor")
  def this(name: String) {
    this(name, 0)
    println("Auxiliary constructor 1")
  }
  def this(age: Int) {
    this("Unknown", age)
    println("Auxiliary constructor 2")
  }
}

val person1 = new Person("John Doe")
val person2 = new Person(30)

/*
Output:
Primary constructor
Auxiliary constructor 1
Primary constructor
Auxiliary constructor 2
*/
```

En este ejemplo, la clase `Person` tiene un constructor primario que toma dos parámetros, nombre y
edad.
También tiene dos constructores auxiliares que cada uno toma un parámetro, nombre y edad, 
respectivamente.

El primer constructor auxiliar toma solo un parámetro de nombre y establece la edad en 0.
El segundo constructor auxiliar toma solo un parámetro de edad y establece el nombre en "Unknown".
Ambos constructores auxiliares llaman al constructor primario usando la palabra clave `this`.

## Herencia

Cuando se trata de herencia, los constructores en _Scala_ funcionan de manera similar a otros
lenguajes orientados a objetos.

Consideremos un ejemplo donde tenemos una clase `Person` con un constructor primario y una clase
`Employee` que extiende a `Person` y también tiene su propio constructor primario.

```scala
class Person(val name: String, val age: Int) {
  println("Person primary constructor")
}

class Employee(name: String, age: Int, val salary: Double) extends Person(name, age) {
  println("Employee primary constructor")
}

val employee = new Employee("John Doe", 30, 50000.0)

/*
Output:
Person primary constructor
Employee primary constructor
*/
```

## Valores por defecto

En _Scala_, los valores por defecto se pueden proporcionar en una definición de constructor de 
clase, lo que le permite especificar valores predeterminados para los parámetros de clase.
Esto puede simplificar el código reduciendo la necesidad de constructores auxiliares.

Usar valores predeterminados en un constructor proporciona una forma concisa de especificar
valores predeterminados para los parámetros de clase, lo que puede ser muy útil cuando se trata de
clases con muchos parámetros. Por ejemplo:

```scala
class Employee(val id: Int, val name: String, val title: String = "Employee", val salary: Double = 0.0)
```

Este constructor define cuatro parámetros: ```id```, ```name```, ```title``` y ```salary```.
```title``` y ```salary``` tienen valores predeterminados de ```"Employee"``` y ```0.0```,
respectivamente.
Esto significa que si crea una nueva instancia de ```Employee``` sin pasar valores para ```title``` 
y ```salary```, se usarán los valores predeterminados:

```scala
val emp1 = new Employee(1, "John Doe")
val emp2 = new Employee(2, "Jane Smith", "Manager", 50000.0)
```

En el primer ejemplo, ```emp1``` se crea con los valores predeterminados para ```title``` y 
```salary```, ya que no se especificaron.
En el segundo ejemplo, ```emp2``` se crea con los valores especificados para todos los cuatro
parámetros.

Usando constructores auxiliares, en cambio, requiere que defina múltiples constructores que se
llamen entre sí en un orden específico.
Esto puede hacer que el código sea más verboso y difícil de entender, especialmente para clases
con muchos parámetros.

## Errores comunes

### No llamar al constructor primario o a otro constructor auxiliar desde un constructor auxiliar

Si no se llama al constructor primario en un constructor auxiliar, se producirá un error de
compilación.

```scala
class Person(name: String, age: Int) {
  def this(name: String) {
    println("Constructor 1")
  }
}

/*
Output:
'this' expected but identifier found.
    println("Constructor 1")
*/
```

### No llamar al constructor primario o a otro constructor auxiliar como la primera declaración

Si no se llama al constructor primario o a otro constructor auxiliar como la primera declaración en
un constructor auxiliar, se producirá un error de compilación.

```scala
class Person(name: String, age: Int) {
  def this(name: String) {
    println("Constructor 1")
    this(name, 0)
  }
}

/*
Output:
'this' expected but identifier found.
    println("Constructor 1")
*/
```

### No llamar al constructor primario o a otro constructor auxiliar desde la subclase

Si no se llama al constructor primario o a otro constructor auxiliar desde la subclase, se producirá
un error de compilación.

```scala
class Person(name: String, age: Int) {
  println("Person primary constructor")
}

class Employee(name: String, age: Int, salary: Double) extends Person {
  println("Employee primary constructor")
}

/*
Output:
not enough arguments for constructor Person: (name: String, age: Int): Person.
Unspecified value parameters name, age.
class Employee(name: String, age: Int, salary: Double) extends Person {
*/
```

### Declarar var en lugar de val para el parámetro del constructor

Si un parámetro del constructor de una clase se declara usando `var`, entonces debe sobrescribirse
en la subclase con otra declaración `var`.
De la misma manera, si un parámetro del constructor de una clase se declara usando `val`, entonces
debe sobrescribirse en la subclase con otra declaración `val`.
Mezclar los dos dará como resultado un error de compilación. Por ejemplo:

```scala
class Parent(val name: String)

class Child(var name: String) extends Parent(name)

/*
Output:
`override` modifier required to override concrete member:
val name: String (defined in class Parent)
class Child(var name: String) extends Parent(name)
*/
```

### Tipos de parámetros incorrectos

Si un constructor de subclase tiene un parámetro con un tipo diferente al parámetro correspondiente
en el constructor de la superclase, el código no se compilará. Por ejemplo:

```scala
class Parent(val name: String)

class Child(name: Int) extends Parent(name)

/*
Output:
type mismatch;
 found   : Int
 required: String
class Child(name: Int) extends Parent(name)
*/
```

## Ejercicios resueltos

### Ejercicio 1

¿Qué imprime el siguiente código?

```scala
class A(x: Int) {
  println("A: " + x)
}

class B(y: Int) extends A(y + 1) {
  println("B: " + y)
}

class C(z: Int) extends B(z * 2) {
  println("C: " + z)
}

new C(3)
```

#### Respuesta

```
A: 7
B: 6
C: 3
```

### Ejercicio 2

¿Qué imprime el siguiente código?

```scala
class A(x: Int) {
  println("A: " + x)
}

class B(y: Int) extends A(y) {
  println("B: " + y)
}

class C(z: Int) extends B(z) {
  println("C: " + z)
  def this() = this(10)
}

new C()
```

#### Respuesta

```
A: 10
B: 10
C: 10
```

### Ejercicio 3

¿Qué imprime el siguiente código?

```scala
class A(x: Int) {
  println("A: " + x)
}

class B(y: Int) extends A(y) {
  println("B: " + y)
}

class C(z: Int) extends B(z * 2) {
  println("C: " + z)
  def this() = {
    this(10)
    println("C: constructor without arguments")
  }
}

new C()
```

#### Respuesta

```
A: 20
B: 20
C: 10
C: constructor without arguments
```

### Ejercicio 4

¿Qué imprime el siguiente código?

```scala
class Animal {
  val name = "Animal"

  def sound(): Unit = {
    println("The " + name + " makes a sound")
  }
}

class Cat extends Animal {
  override val name = "Cat"

  override def sound(): Unit = {
    println("The " + name + " says meow")
  }
}

val animal: Animal = new Cat()
animal.sound()
```

#### Respuesta

```
The Cat says meow
```

### Ejercicio 5

¿Qué imprime el siguiente código?

```scala
class Vehicle {
  def start(): Unit = {
    println("Vehicle started")
  }

  def stop(): Unit = {
    println("Vehicle stopped")
  }
}

class Car extends Vehicle {
  override def start(): Unit = {
    super.start()
    println("Car started")
  }

  override def stop(): Unit = {
    println("Car stopped")
    super.stop()
  }
}

val car: Vehicle = new Car()
car.start()
car.stop()
```

#### Respuesta

```
Vehicle started
Car started
Car stopped
Vehicle stopped
```

### Ejercicio 6

¿Qué imprime el siguiente código?

```scala
class A {
  val x: Int = 1

  def f(): Unit = {
    println(x)
  }
}

class B extends A {
  override val x: Int = 2
}

val a: A = new B()
a.f()
```

#### Respuesta

```
2
```

### Ejercicio 7

¿Qué imprime el siguiente código?

```scala
class Shape {
  val name: String = "Shape"

  def area(): Double = {
    0
  }
}

class Circle(radius: Double) extends Shape {
  override val name: String = "Circle"

  override def area(): Double = {
    math.Pi * radius * radius
  }
}

class Rectangle(width: Double, height: Double) extends Shape {
  override val name: String = "Rectangle"

  override def area(): Double = {
    width * height
  }
}

val shapes = Array(new Circle(1.0), new Rectangle(2.0, 3.0), new Circle(2.5))
shapes.foreach(shape => println(s"${shape.name} area: ${shape.area()}"))
```

#### Respuesta

```
Circle area: 3.141592653589793
Rectangle area: 6.0
Circle area: 19.634954084936208
```

### Ejercicio 8

¿Qué imprime el siguiente código?

```scala
class A {
  private val x: Int = 1

  def f(): Unit = {
    println(x)
  }
}

class B extends A {
  val x: Int = 2
}

val a: A = new B()
a.f()
```

#### Respuesta

```
1
```

## Referencias

1. Scala Community. “Tools.” Scala Documentation. Accessed April 16, 2023. https://docs.scala-lang.org/scala3/book/domain-modeling-tools.html.