Tabla de contenidos
===================

- [Tabla de contenidos](#tabla-de-contenidos)
- [Parte 1: Patrones de diseño](#parte-1-patrones-de-diseño)
  - [Ejercicio 1: Identificación de patrones de diseño](#ejercicio-1-identificación-de-patrones-de-diseño)
    - [Programa 1: Interacción entre personajes](#programa-1-interacción-entre-personajes)
    - [Programa 2: Base de datos](#programa-2-base-de-datos)
    - [Programa 3: Control de acceso](#programa-3-control-de-acceso)
    - [Programa 4: Sistema de subastas](#programa-4-sistema-de-subastas)
    - [Programa 5: Niveles de juego](#programa-5-niveles-de-juego)
    - [Programa 6: Procesamiento de datos](#programa-6-procesamiento-de-datos)
    - [Programa 7: Gestión de tareas](#programa-7-gestión-de-tareas)
    - [Programa 8: Log Management](#programa-8-log-management)
    - [Programa 9: Creación de Pinceles](#programa-9-creación-de-pinceles)
    - [Programa 10: Servicio de Conexión](#programa-10-servicio-de-conexión)
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

### Programa 5: Niveles de juego

Considere el siguiente fragmento de código, que representa a un jugador progresando a través de los 
niveles de un juego.
El juego tiene tres niveles: "Principiante", "Intermedio" y "Avanzado".
Desde cada nivel, el jugador puede subir o bajar de nivel.
Sin embargo, hay ciertas reglas:

1. El jugador no puede bajar de nivel desde el nivel "Principiante".
2. El jugador no puede subir de nivel desde el nivel "Avanzado".

```scala
class GameLevel {
  def levelUp(): GameLevel = throw new UnsupportedOperationException
  def levelDown(): GameLevel = throw new UnsupportedOperationException
  def levelName: String = this.getClass.getSimpleName
}

class BeginnerLevel extends GameLevel {
  override def levelUp(): GameLevel = new IntermediateLevel
}

class IntermediateLevel extends GameLevel {
  override def levelUp(): GameLevel = new AdvancedLevel
  override def levelDown(): GameLevel = new BeginnerLevel
}

class AdvancedLevel extends GameLevel {
  override def levelDown(): GameLevel = new IntermediateLevel
}

class Player(var level: GameLevel = new BeginnerLevel) {
  def levelUp(): Unit = {
    level = level.levelUp()
  }
  def levelDown(): Unit = {
    level = level.levelDown()
  }
  def currentLevel: GameLevel = level
}
```

1. Identifique el patrón de diseño utilizado en este fragmento de código.
2. Dibuje un diagrama UML del sistema.
3. Explique por qué se utiliza este patrón de diseño y cómo beneficia al sistema.

### Programa 6: Procesamiento de datos
<!-- Template Method -->

Considere el siguiente fragmento de código, que representa un sistema que procesa dos tipos de 
datos: ``TextData`` y ``NumericData``. 
Ambos tipos de datos siguen un proceso similar pero con diferencias sutiles:

```scala
abstract class DataProcessor {
  def fetchData(): Unit
  def parseData(): Unit
  def processParsedData(): Unit
  def saveResults(): Unit

  final def processData(): Unit = {
    fetchData()
    parseData()
    processParsedData()
    saveResults()
    println("Data processed successfully")
  }
}

class TextDataProcessor extends DataProcessor {
  override def fetchData(): Unit = println("Fetching text data...")
  override def parseData(): Unit = println("Parsing text data...")
  override def processParsedData(): Unit = println("Processing parsed text data...")
  override def saveResults(): Unit = println("Saving text data results...")
}

class NumericDataProcessor extends DataProcessor {
  override def fetchData(): Unit = println("Fetching numeric data...")
  override def parseData(): Unit = println("Parsing numeric data...")
  override def processParsedData(): Unit = println("Processing parsed numeric data...")
  override def saveResults(): Unit = println("Saving numeric data results...")
}
```

1. Identifique el patrón de diseño utilizado en este fragmento de código.
2. Dibuje un diagrama UML del sistema.

### Programa 7: Gestión de tareas

Considere el siguiente fragmento de código, que representa un sistema para administrar una lista de tareas:

```scala
trait Task {
  def execute(): Unit
}

class SingleTask(name: String) extends Task {
  override def execute(): Unit = println(s"Executing task: $name")
}

class TaskGroup(name: String) extends Task {
  private var tasks: List[Task] = List.empty

  def addTask(task: Task): Unit = tasks = tasks :+ task
  def removeTask(task: Task): Unit = tasks = tasks.filterNot(_ == task)

  override def execute(): Unit = {
    println(s"Executing group of tasks: $name")
    tasks.foreach(_.execute())
  }
}
```

In this code, we can see that there is a `Task` trait that represents a task to be performed. This can be a `SingleTask` or a `TaskGroup`, which can contain multiple `Task` objects. 

En este código, podemos ver que hay un rasgo `Task` que representa una tarea a realizar.
Esto puede ser una `SingleTask` o un `TaskGroup`, que puede contener múltiples objetos `Task`.

1. Identifique el patrón de diseño utilizado en este fragmento de código.
2. Dibuje un diagrama UML del sistema.

### Programa 8: Log Management
<!-- Null Object -->

Considere el siguiente código Scala:

```scala
trait Logger {
  def log(message: String): Unit
}

class ConsoleLogger extends Logger {
  override def log(message: String): Unit = println(s"LOG: $message")
}

class SilentLogger extends Logger {
  override def log(message: String): Unit = {
    // Do nothing
  }
}

object Logger {
  def apply(enableLogging: Boolean): Logger = 
    if (enableLogging) new ConsoleLogger() else new SilentLogger()
}
```

Este sistema se utiliza para controlar el registro en una aplicación.
Hay dos tipos de registradores, uno es `ConsoleLogger` que registra el mensaje en la consola, y otro es `SilentLogger`, que no hace nada cuando se le pide que registre un mensaje.

1. Identifique el patrón de diseño utilizado en este fragmento de código.
2. Dibuje un diagrama UML del sistema.

### Programa 9: Creación de Pinceles

Considera el siguiente código en Scala:

```scala
trait Brush {
  def paint(): Unit
}

class OilBrush(val size: Int) extends Brush {
  override def paint(): Unit = println(s"Pintando con pincel de óleo de tamaño: $size...")
}

class WaterBrush(val size: Int) extends Brush {
  override def paint(): Unit = println(s"Pintando con pincel de agua de tamaño: $size...")
}

trait ArtKit {
  def createBrush(): Brush
}

class OilArtKit extends ArtKit {
  override def createBrush(): Brush = new OilBrush(size)
}

class WaterArtKit(size: Int) extends ArtKit {
  override def createBrush(): Brush = new WaterBrush(size)
}
```

En este sistema, diferentes tipos de kits de arte necesitan crear diferentes tipos de pinceles. Dependiendo del tipo de kit, se crea y se utiliza un tipo diferente de pincel para pintar. Los pinceles pueden ser de distintos tamaños dependiendo de los parámetros específicos de su kit.

**Preguntas:**

1. Identifica el patrón de diseño utilizado en este fragmento de código.
2. Dibuja un diagrama UML del sistema.

### Programa 10: Servicio de Conexión

Considere un sistema que requiere de un servicio de conexión a un servidor.
Dado que la creación de esta conexión es costosa en términos de recursos, y el hecho de que no se 
requieren múltiples conexiones al mismo servidor, una única instancia de este servicio es suficiente 
para toda la aplicación.

A continuación se muestra una implementación de este servicio:

```scala
class ConnectionService private (server: String) {
  
  def connect(): Unit = {
    println(s"Connecting to $server...")
  }
}

object ConnectionService {
  private val instance = new ConnectionService("localhost")

  def getInstance: ConnectionService = instance
}
```

1. ¿Qué patrón de diseño se está utilizando en este fragmento de código?
2. Dibuja un diagrama UML que refleje el diseño de esta implementación.
3. Escribe un programa de ejemplo que utiliza la clase `ConnectionService` para conectar al 
  servidor.

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
