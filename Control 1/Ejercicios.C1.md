# Ejercicios

## Ejercicio 1: Orden de ejecución

Para cada uno de los siguientes programas, indique si el programa compila y si lo hace, indique qué
imprime en pantalla.

### a

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

### b

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

### c

```scala
class A {
  def m1(): String = {
    "A.m1"
  }
  def m2(): String = {
    s"A.m2 > ${this.m1()}"
  }
  def m5(): String = {
    s"A.m5 > ${this.m2()}"
  }
}

class B extends A() {
  override def m1(): String = {
    "B.m1"
  }
  def m3(): String = {
    s"B.m3 > ${super.m1()}"
  }
  def m4(): String = {
    s"B.m4 > ${super.m2()}"
  }
  override def m5(): String = {
    s"B.m5 > ${super.m5()}"
  }
}

class C extends B() {
  override def m2(): String = {
    s"C.m2 > ${this.m1()}"
  }
}

println(s"1. ${new C().m1()}")
println(s"2. ${new B().m1()}")
println(s"3. ${new A().m1()}")
println(s"4. ${new C().m2()}")
println(s"5. ${new B().m2()}")
println(s"6. ${new A().m2()}")
println(s"7. ${new B().m3()}")
println(s"8. ${new C().m4()}")
println(s"9. ${new C().m5()}")
```

## Ejercicio 2: UML

Para las siguientes implementaciones, haga un diagrama UML que modele la jerarquía de clases 
correspondiente.

### a

```scala
trait Animal {
  def speak(): Unit
}

trait Mammal extends Animal {
  def giveBirth(): Unit
}

abstract class Carnivore extends Animal {
  def eatMeat(): Unit
}

abstract class Herbivore extends Animal {
  def eatPlants(): Unit
}

class Dog extends Mammal {
  def speak(): Unit = println("Woof!")
  def giveBirth(): Unit = println("Giving birth to puppies...")
}

class Lion extends Carnivore {
  def speak(): Unit = println("Roar!")
  def eatMeat(): Unit = println("Eating meat...")
}

class Elephant extends Herbivore with Mammal {
  def speak(): Unit = println("Trumpet!")
  def eatPlants(): Unit = println("Eating plants...")
  def giveBirth(): Unit = println("Giving birth to a calf...")
}
```

### b

```scala
trait Logger {
  def log(message: String): Unit
}

class ConsoleLogger extends Logger {
  def log(message: String): Unit = println(s"Logging to console: $message")
}

class FileLogger(filename: String) extends Logger {
  def log(message: String): Unit = {
    // code to write to file
    println(s"Logging to file $filename: $message")
  }
}

class AccountService(logger: Logger) {
  def transfer(from: String, to: String, amount: Double): Unit = {
    logger.log(s"Transferring $amount from $from to $to")
    // code to transfer money
  }
}
```

### c

```scala
trait Employee {
  def name: String
  def salary: Double
}

trait Manager extends Employee {
  def subordinates: List[Employee]
}

class SalesPerson(val name: String, val sales: Double) extends Employee {
  def salary: Double = 0.1 * sales
}

class Engineer(val name: String, val baseSalary: Double) extends Employee {
  def salary: Double = baseSalary
}

class SalesManager(val name: String, val sales: Double, val subordinates: List[Employee]) 
    extends Manager {
  def salary: Double = 0.1 * sales + subordinates.map(_.salary).sum
}

class EngineeringManager(val name: String, val baseSalary: Double, val subordinates: List[Employee]) extends Manager {
  def salary: Double = baseSalary + subordinates.map(_.salary).sum
}
```

### d

```scala
class Book(val title: String, val author: String, val year: Int)

abstract class LibraryItem {
  def checkout(): Unit
  def checkin(): Unit
}

class BookItem(book: Book) extends LibraryItem {
  var checkedOut: Boolean = false

  def checkout(): Unit = {
    if (!checkedOut) {
      checkedOut = true
      println(s"Checked out book: ${book.title}")
    }
  }

  def checkin(): Unit = {
    if (checkedOut) {
      checkedOut = false
      println(s"Checked in book: ${book.title}")
    }
  }
}

class Patron(val name: String) {
  def checkout(item: LibraryItem): Unit = {
    item.checkout()
  }

  def checkin(item: LibraryItem): Unit = {
    item.checkin()
  }
}

class Library {
  var items: List[LibraryItem] = List.empty

  def add(item: LibraryItem): Unit = {
    items = item :: items
  }

  def remove(item: LibraryItem): Unit = {
    items = items.filterNot(_ == item)
  }

  def listItems(): Unit = {
    items.foreach(item => println(item.toString))
  }
}
```

## Ejercicio 3: Modelado de clases

### a
 
#### i

Suponga que queremos modelar una aplicación para simular el funcionamiento de varios vehículos.
Para ello considere lo siguiente:

1. Todos los vehículos tienen un método `start` y un método `stop` que se encargan de iniciar y
   detener el vehículo respectivamente.
2. Los automóviles son vehículos que se caracterizan por tener 4 ruedas.
3. Los automóviles pueden ser sedanes o SUVs.

Haga un diagrama UML que modele la jerarquía de clases que se debe implementar para modelar
esta situación, señalando cuáles son clases concretas, cuáles son clases abstractas y cuáles son
interfaces/traits.
Su diagrama debe incluir las relaciones de herencia y composición que se deben implementar.
Además debe indicar los modificadores de acceso de cada variable, método y constructor.
Utilice sintaxis UML estándar.

#### ii

Suponga ahora que queremos agregar un nuevo tipo de vehículo, los barcos, que no tienen ruedas.
¿Cómo debería modificarse el diagrama UML anterior para modelar esta situación?

#### iii

Implemente las clases que se indican en el diagrama UML anterior, junto con sus respectivos
constructores, métodos y atributos.
Para la implementación concreta de los métodos `start` y `stop` puede simplemente imprimir
por pantalla un mensaje indicando que el vehículo está iniciando o deteniendo.

### b

#### i

Suponga que queremos modelar una aplicación para simular la compra de productos en una tienda
online.
Para ello considere lo siguiente:

1. Todos los productos tienen un nombre y un precio.
2. Los productos pueden ser comprados por medio de PayPal o tarjeta de crédito.
3. Los productos pueden ser enviados por medio de envío estándar o envío exprés.

Haga un diagrama UML que modele la jerarquía de clases que se debe implementar para modelar
esta situación, señalando cuáles son clases concretas, cuáles son clases abstractas y cuáles son
interfaces/traits.

_Hint: Modele los tipos de pago y envío como interfaces/traits_.

#### ii

Implemente las clases que se indican en el diagrama UML anterior, junto con sus respectivos
constructores, métodos y atributos.

### c

#### i

Suponga que queremos modelar una aplicación para simular el funcionamiento de una red social.
Para ello considere lo siguiente:

1. Todos los usuarios tienen un nombre, un apellido y una edad.
2. Los usuarios pueden ser de tipo `Estudiante` o `Profesor`.
3. Los usuarios pueden tener amigos.
4. Los usuarios pueden publicar mensajes en su muro.

Haga un diagrama UML que modele la jerarquía de clases que se debe implementar para modelar
esta situación, señalando cuáles son clases concretas, cuáles son clases abstractas y cuáles son
interfaces/traits.

#### ii

Implemente las clases que se indican en el diagrama UML anterior, junto con sus respectivos
constructores, métodos y atributos.

## Ejercicio 4: Liskov

Para las siguientes implementaciones, indique si se cumple o no el principio de Liskov.
Si no se cumple, explique por qué y cómo lo podría corregir (no es necesario implementar, sólo
dar una solución conceptual).

### a

```scala	
class Rectangle {
  protected var _width: Int = 0
  protected var _height: Int = 0

  def width: Int = _width
  
  def width_=(w: Int): Unit = {
    _width = w
  }

  def height: Int = _height

  def height_=(h: Int): Unit = {
    _height = h
  }

  def area: Int = {
    width * height
  }
}

class Square extends Rectangle {
  override def width_=(w: Int): Unit = {
    _width = w
    _height = w
  }

  override def height_=(h: Int): Unit = {
    _width = h
    _height = h
  }
}

// Example usage
val rect: Rectangle = new Square()
rect.setWidth(5)
rect.setHeight(4)
println(rect.area)
```

### b

```scala
trait Animal {
  def speak(): String
}

class Dog extends Animal {
  override def speak(): String = "Woof!"
}

class Cat extends Animal {
  override def speak(): String = "Meow!"
}

class AnimalShelter(animals: List[Animal]) {
  def makeAllSpeak(): Unit = {
    animals.foreach(animal => println(animal.speak()))
  }
}

// Example usage
val dog: Dog = new Dog()
val cat: Cat = new Cat()
val shelter: AnimalShelter = new AnimalShelter(List(dog, cat))
shelter.makeAllSpeak()
```

### c

```scala	
abstract class Shape {
  def area: Double
}

class Circle(val radius: Double) extends Shape {
  override def area: Double = math.Pi * radius * radius
}

class Square(val side: Double) extends Shape {
  override def area: Double = side * side
}

// Example usage
val shapes: List[Shape] = List(new Circle(2), new Square(3))
shapes.foreach(shape => println(shape.area))
```	

### d

```scala
trait Animal {
  def speak(): Unit
}

abstract class Mammal extends Animal {
  def walk(): Unit
}

trait Feline extends Mammal {
  def purr(): Unit
}

class Cat extends Feline {
  override def speak(): Unit = println("Meow")
  override def walk(): Unit = println("Walking like a cat")
  override def purr(): Unit = println("Purring like a cat")
}

class Lion extends Feline {
  override def speak(): Unit = println("Roar")
  override def walk(): Unit = println("Walking like a lion")
  override def purr(): Unit = println("Lions don't purr")
}
```

### e

```scala
trait Vehicle {
  def startEngine(): Unit
  def stopEngine(): Unit
}

class Car extends Vehicle {
  override def startEngine(): Unit = println("Starting car engine...")
  override def stopEngine(): Unit = println("Stopping car engine...")
}

class Truck extends Vehicle {
  override def startEngine(): Unit = println("Starting truck engine...")
  override def stopEngine(): Unit = println("Stopping truck engine...")
}

class Driver(val vehicle: Vehicle) {
  def drive(): Unit = {
    vehicle.startEngine()
    println("Driving...")
    vehicle.stopEngine()
  }
}

// Example usage
val carDriver = new Driver(new Car())
carDriver.drive()

val truckDriver = new Driver(new Truck())
truckDriver.drive()
```

## Ejercicio 5: Testing

### a

Implemente las funcionalidades necesarias para que pasen cada uno de los siguientes tests.

#### i

```scala
class CalculatorSuite extends FunSuite {
  test("Calculator should add two numbers") {
    val calculator = new Calculator()
    assertEquals(calculator.add(2, 3), 5)
  }

  test("Calculator should subtract two numbers") {
    val calculator = new Calculator()
    assertEquals(calculator.subtract(5, 2), 3)
  }
}
```	

#### ii

```scala
import munit._

class StackSuite extends FunSuite {
  test("Stack should push and pop elements") {
    val stack = new Stack()
    stack.push(1)
    stack.push(2)
    assertEquals(stack.pop(), 2)
    assertEquals(stack.pop(), 1)
  }

  test("Stack should be empty after popping all elements") {
    val stack = new Stack()
    stack.push(1)
    stack.push(2)
    stack.pop()
    stack.pop()
    assert(stack.isEmpty())
  }
}
```	

#### iii

```scala
import munit._

class SortTest extends FunSuite {
  test("Sorting an empty list should return an empty list") {
    val emptyList = List()
    val result = Sorter.sortList(emptyList)
    assertEquals(result, emptyList)
  }

  test("Sorting a list with one element should return the same list") {
    val list = List(42)
    val result = Sorter.sortList(list)
    assertEquals(result, list)
  }

  test("Sorting a list with multiple elements should return a sorted list") {
    val unsortedList = List(10, 5, 7, 3, 9)
    val expectedList = List(3, 5, 7, 9, 10)
    val result = Sorter.sortList(unsortedList)
    assertEquals(result, expectedList)
  }
}
```

### b

Para las siguientes implementaciones, escriba los tests que considere necesarios para probar
que la implementación es correcta.

#### i

```scala
object PasswordValidator {
  def validate(password: String): Boolean = {
    if (password.length < 8) {
      false
    } else if (!password.exists(_.isDigit)) {
      false
    } else if (!password.exists(_.isLetter)) {
      false
    } else if (password.toUpperCase == password || password.toLowerCase == password) {
      false
    } else {
      true
    }
  }
}
```

#### ii

```scala
trait TemperatureUnit {
  def toCelsius(value: Double): Double
  def fromCelsius(value: Double): Double
}

object Celsius extends TemperatureUnit {
  override def toCelsius(value: Double): Double = value
  override def fromCelsius(value: Double): Double = value
}

object Fahrenheit extends TemperatureUnit {
  override def toCelsius(value: Double): Double = (value - 32) * 5 / 9
  override def fromCelsius(value: Double): Double = (value * 9 / 5) + 32
}

object Kelvin extends TemperatureUnit {
  override def toCelsius(value: Double): Double = value - 273.15
  override def fromCelsius(value: Double): Double = value + 273.15
}
```

#### iii

```scala
class Item(val name: String, val price: Double)

class ShoppingCart {
  private var items = List[Item]()

  def addItem(item: Item): Unit = {
    items = item :: items
  }

  def removeItem(item: Item): Unit = {
    items = items.filter(_ != item)
  }

  def getTotalCost: Double = {
    items.map(_.price).sum
  }

  def applyDiscount(discount: Double): Unit = {
    val discountAmount = getTotalCost * discount
    items = items.map(item => new Item(item.name, item.price - discountAmount))
  }

  def applyTax(taxRate: Double): Unit = {
    val taxAmount = getTotalCost * taxRate
    items = items.map(item => new Item(item.name, item.price + taxAmount))
  }

  def generateOrderSummary: String = {
    val itemList = items.map(item => s"${item.name}: ${item.price}")
    val totalCost = getTotalCost
    val orderSummary = itemList :+ s"Total cost: $totalCost"
    orderSummary.mkString("\n")
  }
}
```