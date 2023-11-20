Tabla de contenidos
===================

- [Tabla de contenidos](#tabla-de-contenidos)
- [Parte 1: Patrones de diseño](#parte-1-patrones-de-diseño)
  - [Ejercicio 1: Identificación de patrones de diseño](#ejercicio-1-identificación-de-patrones-de-diseño)
    - [Programa 1: Base de datos](#programa-1-base-de-datos)
    - [Programa 2: Control de acceso](#programa-2-control-de-acceso)
    - [Programa 3: Compartir Recursos en una Galería de Arte](#programa-3-compartir-recursos-en-una-galería-de-arte)
    - [Programa 4: Sistema de subastas](#programa-4-sistema-de-subastas)
    - [Programa 5: Interacción entre personajes](#programa-5-interacción-entre-personajes)
    - [Programa 6: Creación de Pinceles](#programa-6-creación-de-pinceles)
    - [Programa 7: Niveles de juego](#programa-7-niveles-de-juego)
    - [Programa 8: Procesamiento de datos](#programa-8-procesamiento-de-datos)
    - [Programa 9: Operaciones Extensibles en Archivos](#programa-9-operaciones-extensibles-en-archivos)
    - [Programa 10: Servicio de Conexión](#programa-10-servicio-de-conexión)
    - [Programa 11: Gestión de tareas](#programa-11-gestión-de-tareas)
    - [Programa 12: Log Management](#programa-12-log-management)
  - [Ejercicio 2: Implementación de patrones de diseño](#ejercicio-2-implementación-de-patrones-de-diseño)
    - [1. Tratamiento de formas geométricas](#1-tratamiento-de-formas-geométricas)
    - [2. Manejo de usuarios desconocidos](#2-manejo-de-usuarios-desconocidos)
    - [3. Creación Personalizada de Documentos](#3-creación-personalizada-de-documentos)
    - [4. Sistema de pagos](#4-sistema-de-pagos)
    - [5. Máquina de Dulces](#5-máquina-de-dulces)
    - [6. Monitor del Stock de Productos](#6-monitor-del-stock-de-productos)
    - [7. Simulador de Bosques](#7-simulador-de-bosques)
    - [8. Gestión de Configuración Global](#8-gestión-de-configuración-global)
    - [9. Calculadora de derivadas](#9-calculadora-de-derivadas)
    - [10. Sistema monitor de clima](#10-sistema-monitor-de-clima)
    - [11. Creación de Informes](#11-creación-de-informes)
    - [12. Interacciones de Objetos en Juegos de Estrategia](#12-interacciones-de-objetos-en-juegos-de-estrategia)
- [Parte 2: Excepciones](#parte-2-excepciones)
  - [Ejercicio 1: Clases de Excepciones](#ejercicio-1-clases-de-excepciones)
  - [Ejercicio 2: Checked vs Unchecked Exceptions](#ejercicio-2-checked-vs-unchecked-exceptions)
  - [Ejercicio 3: Buenas prácticas](#ejercicio-3-buenas-prácticas)
  - [Ejercicio 4: Manejo de Excepciones](#ejercicio-4-manejo-de-excepciones)
    - [4.1.](#41)
    - [4.2.](#42)
    - [4.3.](#43)
  - [Ejercicio 5: Orden de Ejecución](#ejercicio-5-orden-de-ejecución)
    - [5.1.](#51)
    - [5.2.](#52)
    - [5.3.](#53)
    - [5.4.](#54)
    - [5.5.](#55)
- [Parte 3: Generics](#parte-3-generics)
  - [Ejercicio 1: Clases Genéricas](#ejercicio-1-clases-genéricas)
    - [1.1.](#11)
    - [1.2.](#12)
  - [Ejercicio 2: Varianza](#ejercicio-2-varianza)
  - [Ejercicio 3: Type Constraints](#ejercicio-3-type-constraints)
    - [1. Caja de elementos](#1-caja-de-elementos)
    - [2. Ordenación de elementos](#2-ordenación-de-elementos)
    - [3. Método genérico para listas](#3-método-genérico-para-listas)
    - [4. Veterinario](#4-veterinario)
  - [Ejercicio 4: Curiously Recurring Template Pattern](#ejercicio-4-curiously-recurring-template-pattern)

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

### Programa 1: Base de datos
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

### Programa 2: Control de acceso
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

### Programa 3: Compartir Recursos en una Galería de Arte
<!-- Flyweight -->

Eres parte del equipo de desarrollo de una aplicación para una galería de arte. 
La aplicación necesita representar miles de pinturas, muchas de las cuales comparten ciertos 
atributos como el autor, el estilo o el año en que fueron creadas.

Para optimizar el uso de la memoria, decides compartir estos atributos comunes en lugar de 
repetirlos en cada pintura.
Para hacer esto, implementas una fábrica que crea un nuevo objeto solo si no existe ninguno con los
atributos deseados.

Se te proporciona el siguiente fragmento de código:

```scala
case class ArtworkAttributes(author: String, style: String, year: Int)

class ArtworkFactory {
  private val attributesPool = scala.collection.mutable.Map[String, ArtworkAttributes]()

  def getArtworkAttributes(author: String, style: String, year: Int): ArtworkAttributes = {
    val key = s"$author:$style:$year"
    attributesPool.getOrElseUpdate(key, ArtworkAttributes(author, style, year))
  }
}

class Artwork(val name: String, private val attributes: ArtworkAttributes) {
  def print(): Unit = {
    println(s"$name is a ${attributes.style} painting by ${attributes.author} from ${attributes.year}.")
  }
}
```

1. ¿Qué patrón de diseño se está utilizando en este fragmento de código? Explica tu razonamiento.
2. Dibuja un diagrama UML que refleje el diseño de esta implementación.
3. Escribe un programa de ejemplo que utilice la clase `ArtworkFactory` para crear varias pinturas, 
  algunas de las cuales comparten atributos.

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

### Programa 5: Interacción entre personajes
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

### Programa 6: Creación de Pinceles
<!-- Factory Method -->

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

### Programa 7: Niveles de juego
<!-- State -->

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

class Player(private var level: GameLevel = new BeginnerLevel) {
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

### Programa 8: Procesamiento de datos
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

### Programa 9: Operaciones Extensibles en Archivos
<!-- Visitor -->

Eres un desarrollador trabajando en una aplicación de manejo de archivos.
En la aplicación, hay diferentes tipos de archivos, y se espera que en el futuro se puedan agregar 
más tipos.
Quieres poder realizar operaciones en estos archivos, pero no quieres cambiar las clases de los 
archivos cada vez que agregas una nueva operación.

Decides implementar una solución que permita agregar nuevas operaciones sin cambiar las clases de
los archivos.

Se te proporciona el siguiente fragmento de código:

```scala
trait FileElement {
  def accept(operation: FileOperation): Unit
}

class TextFile(val name: String) extends FileElement {
  override def accept(operation: FileOperation): Unit = operation.perform(this)
}

class ImageFile(val name: String) extends FileElement {
  override def accept(operation: FileOperation): Unit = operation.perform(this)
}

trait FileOperation {
  def perform(file: TextFile): Unit
  def perform(file: ImageFile): Unit
}

class OpenOperation extends FileOperation {
  override def perform(file: TextFile): Unit = println(s"Opening text file ${file.name}")
  override def perform(file: ImageFile): Unit = println(s"Opening image file ${file.name}")
}

class DeleteOperation extends FileOperation {
  override def perform(file: TextFile): Unit = println(s"Deleting text file ${file.name}")
  override def perform(file: ImageFile): Unit = println(s"Deleting image file ${file.name}")
}
```

1. ¿Qué patrón de diseño se está utilizando en este fragmento de código?
2. Dibuja un diagrama UML que refleje el diseño de esta implementación.

### Programa 10: Servicio de Conexión
<!-- Singleton -->

Considere un sistema que requiere de un servicio de conexión a un servidor.
Dado que la creación de esta conexión es costosa en términos de recursos, y el hecho de que no se 
requieren múltiples conexiones al mismo servidor, una única instancia de este servicio es suficiente 
para toda la aplicación.

A continuación se muestra una implementación de este servicio:

```scala
class ConnectionService private () {
  private var server: Option[String] = None
  
  def connect(): Unit = {
    println(s"Connecting to $server...")
  }
}

object ConnectionService {
  private var instance: Option[ConnectionService] = None
  private var _server: String = "localhost"

  def server_=(newServer: String): Unit = {
    _server = newServer
    getInstance.server = Some(newServer)
  }

  def getInstance: ConnectionService = {
    if (instance.isEmpty) {
      println("Creating connection service...")
      instance = Some(new ConnectionService())
      instance.server = Some(_server)
    }
    instance.get
  }
}
```

1. ¿Qué patrón de diseño se está utilizando en este fragmento de código?
2. Dibuja un diagrama UML que refleje el diseño de esta implementación.
3. Escribe un programa de ejemplo que utiliza la clase `ConnectionService` para conectar al 
  servidor.

### Programa 11: Gestión de tareas
<!-- Composite -->

Considere el siguiente fragmento de código, que representa un sistema para administrar una lista de tareas:

```scala
trait Task {
  def execute(): Unit
}

class SingleTask(val name: String) extends Task {
  override def execute(): Unit = println(s"Executing task: $name")
}

class TaskGroup(val name: String) extends Task {
  private var tasks: List[Task] = List.empty

  def addTask(task: Task): Unit = tasks = tasks :+ task
  def removeTask(task: Task): Unit = tasks = tasks.filterNot(_ == task)

  override def execute(): Unit = {
    println(s"Executing group of tasks: $name")
    tasks.foreach(_.execute())
  }
}
```

En este código, podemos ver que hay un rasgo `Task` que representa una tarea a realizar.
Esto puede ser una `SingleTask` o un `TaskGroup`, que puede contener múltiples objetos `Task`.

1. Identifique el patrón de diseño utilizado en este fragmento de código.
2. Dibuje un diagrama UML del sistema.

### Programa 12: Log Management
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


Ejercicio 2: Implementación de patrones de diseño
-------------------------------------------------

### 1. Tratamiento de formas geométricas
<!-- 1 Visitor -->

Suponga que está trabajando en un programa de diseño gráfico que maneja varias formas geométricas 
como círculos, rectángulos y triángulos.
Cada forma tiene sus propias características y formas de calcular su área y perímetro.

Ahora, se espera que su programa pueda exportar estas formas a diferentes formatos como XML y JSON.
Es probable que en el futuro se requieran más formatos de exportación.
No queremos cambiar cada clase de forma cada vez que necesitemos un nuevo formato de exportación.

1. Con base en el escenario proporcionado, ¿qué patrón de diseño debe usarse para resolver este 
  problema? Justifique su elección.
2. Cree un diagrama UML para su solución propuesta.
3. Implemente su solución en código. Debe incluir las clases para las formas geométricas (círculos, 
  rectángulos y triángulos), así como las clases necesarias para exportar estas formas a XML y JSON.
  Asegúrese de demostrar cómo se puede agregar un nuevo formato de exportación sin modificar las 
  clases de las formas.

*Hint: Recuerde que su solución debe permitir agregar nuevas formas de exportación sin cambiar las clases de las formas geométricas. Su diseño debe ser lo suficientemente flexible como para manejar fácilmente la incorporación de nuevas formas de exportación en el futuro.*

### 2. Manejo de usuarios desconocidos
<!-- 2 Null Object -->                             

Suponga que está construyendo una aplicación que tiene un sistema de inicio de sesión.
Al ingresar su nombre de usuario, el sistema buscará en la base de datos la información de dicho 
usuario.
Si el usuario existe, el sistema proporcionará toda la información del perfil de usuario.
Sin embargo, si el usuario no existe, el sistema simplemente mostrará un mensaje de error.

Ahora se le pide que cambies este comportamiento.
En lugar de simplemente mostrar un mensaje de error para un usuario desconocido, se desea que el
sistema muestre un perfil de usuario con información predeterminada (como "Invitado" para el nombre
de usuario, "N/A" para el correo electrónico, etc.).

1. Con base en el escenario proporcionado, ¿qué patrón de diseño debes usar para resolver este 
  problema?
2. Cree un diagrama UML para su solución propuesta.
3. Implemente su solución en código.

### 3. Creación Personalizada de Documentos
<!-- Factory Method -->

Te encuentras trabajando en una aplicación de procesamiento de texto.
Esta aplicación puede manejar diferentes tipos de documentos como documentos de texto (.txt), 
documentos Word (.docx) y documentos PDF (.pdf).
Cuando un usuario desea crear un nuevo documento, se le presenta una lista de los diferentes tipos
de documentos que puede crear.
Además, el usuario tiene la opción de personalizar el nombre del documento.

Tu tarea es implementar la funcionalidad para crear un nuevo documento basado en el tipo y el nombre
especificados por el usuario.

1. Dado el escenario anterior, ¿qué patrón de diseño crees que debes utilizar para resolver este problema? Explica tu elección.

2. Cree un diagrama UML para tu solución propuesta. 
  El diagrama debe ilustrar claramente las clases que interactúan, su relación y cómo se implementa el 
  patrón de diseño elegido.
3. Implementa tu solución en código.
  Debes incluir las clases para los diferentes tipos de documentos y la lógica para crear un nuevo 
  documento basado en la elección y la entrada del usuario.
  Tu solución debería permitir la fácil adición de nuevos tipos de documentos en el futuro.
4. Debes crear una clase de fábrica para cada tipo de documento.
  Esta clase de fábrica debería tener una propiedad name que puede ser ajustada re-usando la misma 
  fábrica y un método create() que crea una nueva instancia del tipo de documento correspondiente 
  con el nombre especificado.

*Hint: Recuerda que tu solución debe permitir la creación de diferentes tipos de documentos sin tener que modificar el código cada vez que se añade un nuevo tipo de documento. En lugar de ello, deberías poder extender la funcionalidad existente de una manera ordenada y estructurada.*

### 4. Sistema de pagos
<!-- 4 Proxy -->

Considere que se desea implementar un sistema de pago que permita realizar pagos a través de
tarjetas de crédito, tarjetas de débito y efectivo. Además, se desea que el sistema pueda
realizar pagos a través de múltiples monedas.
Para esto considere que desde el punto de vista del sistema de pago, todos los tipos de pago son
equivalentes, es decir, no importa si el pago se realiza con tarjeta de crédito, tarjeta de débito
o efectivo, el sistema de pago no necesita tener conocimiento de la forma en que se realiza el pago.

1. Con base en el escenario anterior, ¿qué patrón de diseño crees que debes utilizar para resolver este problema?
2. Cree un diagrama UML para su solución propuesta.
3. Implemente su solución en código.

*Hint: Implemente su solución de tal forma que todos los tipos de pago implementen la __misma interfaz__*

### 5. Máquina de Dulces
<!-- 5 State -->

Estás desarrollando un software para una máquina expendedora de dulces.
La máquina tiene varias operaciones y estados, entre los cuales se incluyen "Sin Dinero", "Dinero 
Ingresado", "Dispensando Dulce" y "Sin Dulces".
Cada estado tiene operaciones asociadas y transiciones a otros estados.

La máquina puede:

- Recibir dinero: Si la máquina está en el estado "Sin Dinero", cambia al estado "Dinero 
  Ingresado".
- Regresar dinero: Si la máquina está en el estado "Dinero Ingresado", puede regresar el dinero y 
  volver al estado "Sin Dinero".
- Dispensar dulce: Si la máquina está en el estado "Dinero Ingresado", puede dispensar un dulce y 
  volver al estado "Sin Dinero", a menos que ya no tenga dulces, en cuyo caso se moverá al estado 
  "Sin Dulces".

1. Con base en el escenario anterior, ¿qué patrón de diseño crees que debes utilizar para resolver 
  este problema?
2. Cree un diagrama UML para su solución propuesta.
3. Implemente su solución en código.

### 6. Monitor del Stock de Productos
<!-- Observer -->

Estás desarrollando un sistema de inventario para un gran almacén.
Cada vez que un producto llega al límite de stock establecido, es necesario alertar a varios 
departamentos (compras, ventas, logística, etc.) para que realicen las acciones correspondientes.

El sistema debe permitir lo siguiente:

- Registrar productos y su stock inicial.
- Actualizar el stock de un producto.
- Registrar departamentos interesados en un producto específico.
- Cuando el stock de un producto llegue al límite establecido, todos los departamentos registrados 
  deben ser notificados.

1. Identifique el patrón de diseño que debiera ocuparse para resolver el problema descrito.
2. Dibuje un diagrama UML de su solución propuesta.
3. Implemente la solución propuesta. 

*Nota: Considere que cada departamento puede tener diferentes maneras de ser notificado (por ejemplo, algunos pueden preferir un correo electrónico, mientras que otros pueden preferir una notificación en una aplicación interna). Por otro lado, es posible que en el futuro se requiera agregar más formas de notificación.*

### 7. Simulador de Bosques
<!-- 7 Flyweight -->

Estás desarrollando un simulador de bosques para un estudio de cambio climático.
En tu simulación, hay millones de árboles de diferentes tipos (como pino, roble y abeto), cada uno 
con propiedades como color, textura y altura.

Es importante destacar que aunque hay millones de árboles, el número de combinaciones únicas de las
propiedades de los árboles (tipo, color, textura, altura) es significativamente menor.

Crear una representación independiente de cada árbol en la simulación sería muy costoso en términos 
de memoria y recursos de computación.
Tu tarea es diseñar e implementar una solución eficiente para este problema.

1. Identifique el patrón de diseño que debiera ocuparse para resolver el problema descrito.
2. Dibuje un diagrama UML de su solución propuesta.
3. Implemente la solución propuesta.
4. Suponga que ahora se requiere que cada árbol tenga una posición en el mapa. 
  ¿Cómo afectaría esto su solución actual?
  Plantee una solución que permita agregar esta nueva funcionalidad aumentando lo menos posible el
  consumo de recursos.
  No es necesario implementar esta solución.
  <!-- 2 Flyweights, el propuesto en 1 y otro para la posición. El segundo utiliza al primero. --> 

### 8. Gestión de Configuración Global
<!-- 8 Singleton -->

Estás desarrollando una aplicación de gran tamaño que requiere manejar una configuración global 
(representada como un diccionario de tipo ``Map<String, String>``) que puede ser modificada en
tiempo de ejecución.
Esta configuración debe ser accesible desde cualquier parte de la aplicación y debe garantizarse que 
sólo existe una única instancia de la configuración en todo el programa.

Debido a la naturaleza crítica de esta configuración, es fundamental que ninguna parte del código 
pueda crear una segunda instancia de la configuración, o cambiar la instancia existente sin un
control adecuado. 

Tu tarea es diseñar e implementar una solución que garantice que la configuración sea única y que
sólo pueda ser modificada a través de un mecanismo seguro.

1. Identifique el patrón de diseño que debiera ocuparse para resolver el problema descrito.
2. Dibuje un diagrama UML de su solución propuesta.
3. Implemente la solución propuesta.

### 9. Calculadora de derivadas
<!-- 9 Composite -->

Estás desarrollando una calculadora de derivadas para ser utilizada en un curso de cálculo.
La calculadora debe ser capaz de calcular la derivada de una función compuesta de otras funciones
simples, para lo cual debe aplicar la regla de la cadena.

La regla de la cadena establece que la derivada de una función compuesta es igual a la derivada de
la función externa evaluada en la función interna, multiplicada por la derivada de la función
interna.

Para esto considere las siguientes 4 reglas:

- La derivada de una función constante es 0.
- La derivada de una variable es 1.
- La derivada de una suma es la suma de las derivadas.
- La derivada de una multiplicación es la suma de los productos de las derivadas de cada factor.

Por ejemplo, la derivada de la función ``f(x) = 2x + 3`` es ``f'(x) = 2``.

1. Identifique el patrón de diseño que debiera ocuparse para resolver el problema descrito.
2. Dibuje un diagrama UML de su solución propuesta.
3. Implemente la solución propuesta.
4. Suponga ahora que quiere agregar la capacidad de realizar otras operaciones sobre las funciones 
  (como integración, por ejemplo). 
  ¿Cómo afectaría esto su solución actual?
  Plantee una solución que de flexibilidad a la calculadora para agregar nuevas operaciones de 
  manera extensible.
  <!-- Composite + Visitor -->

### 10. Sistema monitor de clima
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

### 11. Creación de Informes
<!-- 11 Template -->

Estás trabajando en un sistema de información empresarial que necesita generar varios tipos de 
informes.
Cada informe tiene una estructura similar, pero los datos y la presentación pueden variar 
dependiendo del tipo de informe (por ejemplo, un informe financiero frente a un informe de ventas).

Todos los informes siguen el mismo proceso general: recolección de datos, análisis de los datos, y 
finalmente la presentación de los datos.
Sin embargo, cada uno de estos pasos puede variar dependiendo del tipo específico del informe.

Tu tarea es diseñar e implementar una solución que proporcione una estructura común para generar 
estos informes, pero que permita a cada tipo de informe definir su propia implementación para la 
recolección de datos, el análisis y la presentación.

1. Identifique el patrón de diseño que debiera ocuparse para resolver el problema descrito.
2. Dibuje un diagrama UML de su solución propuesta.
3. Implemente la solución propuesta.

### 12. Interacciones de Objetos en Juegos de Estrategia
<!-- 12 Double Dispatch -->

Estás desarrollando un juego de estrategia en tiempo real donde varios tipos de unidades militares 
pueden interactuar entre sí. 
Las unidades tienen diferentes habilidades y comportamientos dependiendo del tipo de unidad con la 
que interactúan.

Por ejemplo, la unidad "Infantería" puede atacar a otra unidad "Infantería" con un cuchillo, 
mientras que si la misma unidad "Infantería" interactúa con la unidad "Tanque", usaría una granada 
antitanque.
En el caso de la unidad "Avión", si ataca a "Infantería" realiza un ataque aéreo, pero si ataca a 
otra unidad "Avión" se produce una batalla aérea.

Tu tarea es diseñar e implementar una solución que permita manejar estas interacciones de manera 
eficiente y sin tener que verificar el tipo de cada unidad cada vez que una interacción se lleva a 
cabo.

1. Identifique el patrón de diseño que debiera ocuparse para resolver el problema descrito.
2. Dibuje un diagrama UML de su solución propuesta.
3. Implemente la solución propuesta.

*Nota: Asegúrate de que tu diseño permita agregar fácilmente nuevos tipos de unidades e interacciones en el futuro.*

Parte 2: Excepciones
====================

Ejercicio 1: Clases de Excepciones
----------------------------------

Explique qué representan las clases:

- `Throwable`
- `Exception`
- `Error`

Ejercicio 2: Checked vs Unchecked Exceptions
--------------------------------------------

¿Qué significa que una excepción sea *checked* o *unchecked*?
¿Cómo se implementan en Scala?

Ejercicio 3: Buenas prácticas
-----------------------------

¿Por qué es una buena práctica crear excepciones personalizadas?
¿Por qué es mala práctica atrapar excepciones de tipo `Exception`?

Ejercicio 4: Manejo de Excepciones
----------------------------------

### 4.1.

Implementa una función `isOutOfRange(Int, Int, Int)` que reciba un número entero y un rango (dos 
enteros) que definen el límite inferior y superior, y que arroje una excepción `OutOfRangeException`
si el número está fuera del rango especificado.

La excepción `OutOfRangeException` debe ser implementada por ti, y debe heredar de `Exception`.

### 4.2.

Escribe una función `checkRange(Int, Int)` que reciba dos números enteros que definen el límite
inferior y superior de un rango, y que arroje una excepción `InvalidRangeException` si el límite
inferior es mayor o igual al límite superior.

La excepción `InvalidRangeException` debe ser implementada por ti, y debe heredar de `Exception`.

Agregue este chequeo a la función `isOutOfRange` del ejercicio anterior.

### 4.3.

Crea un programa que primero solicite al usuario que ingrese un rango (dos enteros) que definen el
límite inferior y superior, y luego solicite al usuario que ingrese un número entero.

Ilustra el uso de esta función en un programa que solicita al usuario que ingrese un número y
luego imprime un mensaje que indica si el número está dentro o fuera del rango.
El programa debe continuar solicitando al usuario que ingrese valores hasta que el usuario ingrese
un valor que no sea un número entero.

En caso de que ocurra un error, el programa debe imprimir un mensaje describiendo el error y
continuar solicitando al usuario que ingrese valores.
El programa no debe en ningún caso terminar abruptamente debido a una excepción no controlada.

Utiliza el siguiente código como punto de partida:

```scala
import scala.io.StdIn.readLine

object Main {
  def main(args: Array[String]): Unit = {
    while (true){
      val lo = readLine("Enter a number for the lower bound: ")
      // If all characters are digits then the input is an integer
      if (lo.forall(_.isDigit)) {
        val loNumber = lo.toInt
        // Implement the rest of the program here
      } else {
        return
      }
    }
  }
}
```

Ejercicio 5: Orden de Ejecución
------------------------------

Para los siguientes programas indique el orden de ejecución de las líneas:

### 5.1.

```scala
try {
  println("A")
  throw new Exception("B")
} catch {
  case e: Exception => println("C")
} finally {
  println("D")
}
```

### 5.2.

```scala
try {
  println("A")
  throw new Exception("B")
} catch {
  case e: IllegalArgumentException => println("C")
} finally {
  println("D")
}
```

### 5.3.

```scala
try {
  println("A")
  throw new Exception("B")
} catch {
  case e: IllegalArgumentException => println("C")
  case e: Exception => println("D")
} finally {
  println("E")
}
```

### 5.4.

```scala
try {
  println("A")
  throw new IllegalArgumentException("B")
} catch {
  case e: Exception => println("D")
  case e: IllegalArgumentException => println("C")
} finally {
  println("E")
}
```

### 5.5.

```scala
def foo(): Int = {
  try {
    println("A")
    return 1
  } catch {
    case e: Exception => println("B")
      return 2
  } finally {
    println("C")
    return 3
  }
}

println(foo())
```

Parte 3: Generics
=================

Ejercicio 1: Clases Genéricas
-----------------------------

### 1.1.

Considere la interfaz genérica `Stack[A]` que representa una pila de elementos de tipo `A`:

```scala
trait Stack[A] {
  def push(x: A): Unit
  def pop(): A
  def peek: A
  def isEmpty: Boolean
}
```

1. Implemente una clase `ListStack[A]` que implemente la interfaz `Stack[A]` utilizando una lista
  enlazada.
  ¿Qué patrón de diseño utilizaría para implementar una lista enlazada?
  *Hint: Piense cómo representar el final de la lista.*
2. ¿Qué tipo de varianza debiera tener la interfaz `Stack[A]`? Justifique su respuesta.

### 1.2.

Considere la interfaz genérica `Either[A, B]` que representa un valor de uno de dos tipos posibles
(el valor puede ser de tipo `A` **o** de tipo `B`):

```scala
trait Either[A, B] {
  def isLeft: Boolean
  def isRight: Boolean
  def left: A
  def right: B
}
```

1. Implemente las clases `Left[A, B]` y `Right[A, B]` que implementan la interfaz `Either[A, B]` y
  representan valores de los tipos `A` y `B` respectivamente.
  Si se llama al método `left` en un valor `Right`, o al método `right` en un valor `Left`, se debe
  arrojar una excepción `NoSuchElementException`.
2. ¿Qué tipo de varianza debiera tener la interfaz `Either[A, B]`? Justifique su respuesta.

Ejercicio 2: Varianza
---------------------

Señale si las siguientes afirmaciones son verdaderas o falsas, y justifique su respuesta:

1. Si `S` es subtipo de `T`, entonces `List[S]` es subtipo de `List[T]`. <!-- Falso -->
2. Si `A` es co-variante en `T`, entonces `A[T]` es subtipo de `A[Any]`. <!-- Verdadero -->
3. Si `A` es subtipo de `B`, y `B` es co-variante en `T`, entonces `A` es co-variante en `T`. <!-- Verdadero -->
4. Si `A` es subtipo de `B` y `B` es invariante en `T`, entonces `B[T]` es subtipo de `A[T]`. <!-- Falso -->
5. Si `A` es subtipo de `B` y `B` es contra-variante en `T`, entonces `A[T]` es subtipo de `B[T]`. <!-- Verdadero -->

Ejercicio 3: Type Constraints
-----------------------------

En los ejercicios siguientes, `:<:` representa una relación de subtipado entre tipos.
Diremos que `A :<: B` si `A` es subtipo de `B` o `B` es subtipo de `A`.

### 1. Caja de elementos

Implementa una clase `Box[T]` con un tipo genérico `T` que contenga un elemento del tipo `T`.
Agrega un método `replace[A :<: T](nuevoElemento: A): Box[A]` que acepte un nuevo elemento del 
tipo `A` y retorne una nueva caja con el nuevo elemento.

¿Qué relación de subtipado deben tener `A` y `T` para que el método `replace` funcione?

### 2. Ordenación de elementos

Crea una función `max[T :<: Ordered[T]](elementos: List[T]): T` que recibe una lista de elementos 
del tipo `T` y retorna el máximo elemento.

¿Qué relación de subtipado deben tener `T` y `Ordered[T]` para que la función `max` funcione?

### 3. Método genérico para listas

Implementa una función `último[T](lista: List[T]): T` que retorne el último elemento de la lista.
Luego, extiende la funcionalidad para permitir que el método trabaje con cualquier subtipo de 
`List[T]`, es decir, si `U` es un subtipo de `T`, se puede pasar `List[U]`.

### 4. Veterinario

Supón que tienes una jerarquía de clases donde `Animal` es la superclase y tienes varias subclases como `Perro`, `Gato`, etc. 
Define una clase `Veterinario[T :<: Gato :<: Animal]` y asegúrate de que sólo se puedan crear 
instancias de `Veterinario[T]` donde `T` es al menos un `Gato` y a lo sumo un `Animal`.

Ejercicio 4: Curiously Recurring Template Pattern
------------------------------------------------

Suponga que se desea implementar una jerarquía de clases que represente distintos tipos de
estructuras genéticas.
Para esto, considere la siguiente interfaz:
  
```scala
trait Gene[DNA, G] {
  val dna: DNA
  def mutate(): G
  def withDna(dna: DNA): G
}
```

La interfaz `Gene` representa una estructura genética que almacena su ADN en el campo `dna`.
El método `mutate` retorna una nueva estructura genética con una mutación aleatoria del mismo tipo
que la estructura original.
El método `withDna` retorna una nueva estructura genética del mismo tipo que la estructura original,
pero con el ADN especificado.

Considere las siguientes implementaciones de la interfaz `Gene`:

```scala
class BoolGene(val dna: Boolean) extends Gene[...] {
  def mutate(): BoolGene = new BoolGene(new Random().nextBoolean())
  def withDna(dna: Boolean): BoolGene = new BoolGene(dna)
}

class IntGene(val dna: Int) extends Gene[...] {
  def mutate(): IntGene = new IntGene(new Random().nextInt())
  def withDna(dna: Int): IntGene = new IntGene(dna)
}
```

1. ¿Qué tipo de varianza debiera tener la interfaz `Gene[DNA, G]`? Justifique su respuesta.
2. ¿Qué restricciones de tipo deben tener los parámetros `DNA` y `G` para que la interfaz 
  `Gene[DNA, G]` funcione correctamente?
  *Hint: Piense en cómo utilizar Curiously Recurring Template Pattern (CRTP) para resolver el problema.*
3. Complete la implementación de las clases `BoolGene` e `IntGene` para que funcionen correctamente.
