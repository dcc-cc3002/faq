Tabla de contenidos
===================

- [Tabla de contenidos](#tabla-de-contenidos)
- [Parte 1: Patrones de diseño](#parte-1-patrones-de-diseño)
  - [Ejercicio 1: Identificación de patrones de diseño](#ejercicio-1-identificación-de-patrones-de-diseño)
    - [Programa 1: Interacción entre personajes](#programa-1-interacción-entre-personajes)
    - [Programa 2: Base de datos](#programa-2-base-de-datos)
    - [Programa 3: Control de acceso](#programa-3-control-de-acceso)
    - [Programa 4: Sistema de subastas](#programa-4-sistema-de-subastas)
    - [Programa 5: Sistema de navegación](#programa-5-sistema-de-navegación)
    - [Ejercicio 2: Implementación de patrones de diseño](#ejercicio-2-implementación-de-patrones-de-diseño)
    - [a. Sistema monitor de clima](#a-sistema-monitor-de-clima)

Parte 1: Patrones de diseño
===========================

Para los siguientes ejercicios, considere los siguientes patrones de diseño:

- Adapter
- Composite
- Double Dispatch
- Factory Method
- Flyweight
- Null Object
- Observer
- Proxy
- Singleton
- State
- Template
- Visitor

_Hint: Cada patrón de diseño se utiliza solo una vez._

Ejercicio 1: Identificación de patrones de diseño
-------------------------------------------------

### Programa 1: Interacción entre personajes
<!-- Double Dispatch -->

En un juego 2D, tenemos diferentes tipos de personajes: un Guerrero y un Mago. 
Cada personaje puede interactuar con otros, causando diferentes resultados basados en el tipo de los
personajes involucrados.

```scala
trait Character {
  def interactWith(character: Character): Unit
  def interactWithWarrior(warrior: Warrior): Unit
  def interactWithMage(mage: Mage): Unit
}

class Warrior extends Character {
  override def interactWith(character: Character): Unit = {
    character.interactWithWarrior(this)
  }

  override def interactWithWarrior(warrior: Warrior): Unit = {
    println("Warrior interacts with another Warrior")
  }

  override def interactWithMage(mage: Mage): Unit = {
    println("Warrior interacts with a Mage")
  }
}

class Mage extends Character {
  override def interactWith(character: Character): Unit = {
    character.interactWithMage(this)
  }

  override def interactWithWarrior(warrior: Warrior): Unit = {
    println("Mage interacts with a Warrior")
  }

  override def interactWithMage(mage: Mage): Unit = {
    println("Mage interacts with another Mage")
  }
}

val warrior = new Warrior
val mage = new Mage

warrior.interactWith(mage)  // prints "Warrior interacts with a Mage"
mage.interactWith(warrior)  // prints "Mage interacts with a Warrior"
```

1. Identifique el patrón de diseño que se utiliza para implementar la interacción entre personajes.
2. Dibuje el diagrama de clases (UML) del programa.

### Programa 2: Base de datos
<!-- Adapter -->

Considere una aplicación que interactúa con varios tipos de bases de datos. 
Inicialmente, solo admitía bases de datos SQL, pero los requisitos ahora se han ampliado para 
incluir bases de datos NoSQL también.

A continuación se muestra una implementación de esta aplicación:

```scala
class UserDatabase(private val database: Database) {
  def addUser(user: User): Unit = {
    database.insert(user)
  }

  def getUser(id: String): User = {
    database.select(id)
  }

  def removeUser(id: String): Unit = {
    database.delete(id)
  }
}

trait Database {
  def insert(user: User): Unit
  def select(id: String): User
  def delete(id: String): Unit
}

class SQLDatabase extends Database {
  override def insert(user: User): Unit = {
    println(s"Inserting user ${user.id} into MySQL database")
  }
  override def select(id: String): User = {
    println(s"Selecting user $id from MySQL database")
    User(id)
  }
  override def delete(id: String): Unit = {
    println(s"Deleting user $id from MySQL database")
  }
}

class MongoDB extends Database {
  override def insert(user: User): Unit = {
    println(s"Inserting user ${user.id} into MongoDB")
  }
  override def select(id: String): User = {
    println(s"Selecting user $id from MongoDB")
    User(id)
  }
  override def delete(id: String): Unit = {
    println(s"Deleting user $id from MongoDB")
  }
}

case class User(id: String)

val userDatabase = new UserDatabase(new SQLDatabase)
userDatabase.addUser(User("1"))  // prints "Inserting user 1 into MySQL database"

val userDatabase = new UserDatabase(new MongoDB)
userDatabase.addUser(User("1"))  // prints "Inserting user 1 into MongoDB"
```

1. Dada la implementación anterior, ¿puede identificar el patrón de diseño que se utiliza para hacer 
  que `MongoDB` sea compatible con el resto de la aplicación?
2. Dibuje el diagrama de clases (UML) del programa.

### Programa 3: Control de acceso
<!-- Proxy -->

Considere el siguiente escenario en el que tenemos un objeto que representa un sistema seguro al que
solo pueden acceder los usuarios autorizados.
Sin embargo, los usuarios pueden intentar acceder al sistema directamente, lo que podría provocar
riesgos de seguridad.
Para evitar esto, hemos agregado una capa adicional entre los usuarios y el sistema seguro.
Esta capa verifica las credenciales del usuario antes de permitirles acceder al sistema seguro.

Aquí hay una implementación de ejemplo en Scala:


```scala
trait SecureSystem {
  def access(user: String, password: String): String
}

class ActualSystem extends SecureSystem {
  def access(user: String, password: String): String = {
    s"Access granted for user: $user"
  }
}

class AccessControlLayer extends SecureSystem {
  private val system: SecureSystem = new ActualSystem
  // Don't do this in real life!
  private val users = Map("John" -> "1234", "Jane" -> "4321")

  def access(user: String, password: String): String = {
    if (users.get(user).contains(password)) {
      system.access(user, password)
    } else {
      "Access denied"
    }
  }
}

val acl = new AccessControlLayer

// Try to access the system with valid and invalid credentials
val response1 = acl.access("John", "1234")
println(response1) // Should print: "Access granted for user: John"

val response2 = acl.access("John", "wrong_password")
println(response2) // Should print: "Access denied"

val response3 = acl.access("Jane", "4321")
println(response3) // Should print: "Access granted for user: Jane"

val response4 = acl.access("UnknownUser", "1234")
println(response4) // Should print: "Access denied"
```

En este escenario:
1. ¿Qué patrón de diseño se puede aplicar para resolver el problema?
2. ¿Puede dibujar un diagrama UML que represente la relación entre las diferentes clases e 
  interfaces?

### Programa 4: Sistema de subastas
<!-- Observer -->

Considere el siguiente escenario en el que tenemos un sistema de subastas. 
Hay varios postores para un artículo, y cada vez que se realiza una nueva oferta, todos los postores
deben ser notificados del nuevo monto de la oferta.

Aquí hay una implementación de ejemplo en Scala:

```scala
trait Bidder {
  def update(newBidAmount: Double): Unit
}

class ConcreteBidder(name: String) extends Bidder {
  def update(newBidAmount: Double): Unit = {
    println(s"$name has been notified. New bid amount is $newBidAmount")
  }
}

class Auction {
  private var bidders = List[Bidder]()
  private var _bidAmount = 0.0

  def addBidder(bidder: Bidder): Unit = {
    bidders = bidder :: bidders
  }

  def removeBidder(bidder: Bidder): Unit = {
    bidders = bidders.filterNot(_ == bidder)
  }

  def setBidAmount(bidder: Bidder, amount: Double): Unit = {
    println(s"New bid placed by ${bidder.getClass.getSimpleName} of $amount")
    _bidAmount = amount
    bidders.foreach(_.update(_bidAmount))
  }
}
```

En este escenario:
1. ¿Qué patrón de diseño se ha utilizado para resolver el problema?
2. ¿Puede dibujar un diagrama UML que represente la relación entre las diferentes clases e 
  interfaces?

### Programa 5: Sistema de navegación
<!-- State -->

Considere un sistema de automóvil simplificado donde un automóvil puede estar en uno de los tres 
modos: estacionado, conduciendo o en reversa.
El comportamiento del pedal del acelerador cambia según el modo actual del automóvil.

Aquí hay una implementación de ejemplo en Scala:

```scala
trait CarMode {
  def accelerate(): Unit
}

class ParkedMode extends CarMode {
  override def accelerate(): Unit = {
    println("Car is parked. Accelerating won't do anything.")
  }
}

class DriveMode extends CarMode {
  override def accelerate(): Unit = {
    println("Car is in drive mode. Accelerating...")
  }
}

class ReverseMode extends CarMode {
  override def accelerate(): Unit = {
    println("Car is in reverse mode. Moving backwards...")
  }
}

class Car {
  private var mode: CarMode = new ParkedMode()

  def setMode(newMode: CarMode): Unit = {
    mode = newMode
  }

  def accelerate(): Unit = {
    mode.accelerate()
  }
}
```

En este escenario:
1. ¿Qué patrón de diseño se ha utilizado para resolver el problema?
2. ¿Puede dibujar un diagrama UML que represente la relación entre las diferentes clases e 
  interfaces?
1. Escriba un método para cambiar el modo del automóvil y luego acelerar.

### Ejercicio 2: Implementación de patrones de diseño

### a. Sistema monitor de clima
<!-- Adapter -->

Asumamos que tenemos un sistema monitor de clima que obtiene datos de diferentes sensores de 
clima. 
Acabamos de comprar un nuevo set de sensores de clima de un fabricante diferente. 
La interfaz de los nuevos sensores es diferente a la que tenemos actualmente. 
Para mantener la funcionalidad del sistema existente, necesitamos adaptar estos nuevos sensores a 
nuestra interfaz existente.

1. Identifique el patrón de diseño que se puede aplicar para resolver este problema.
2. Dibuje el diagrama de clases (UML) de la solución.
3. Implemente la solución en Scala.
