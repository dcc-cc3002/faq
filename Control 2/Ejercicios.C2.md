# Ejercicios

## Tabla de contenidos

- [Ejercicios](#ejercicios)
  - [Tabla de contenidos](#tabla-de-contenidos)
  - [Parte 1: Patrones de diseño](#parte-1-patrones-de-diseño)
    - [Ejercicio 1: Identificación de patrones de diseño](#ejercicio-1-identificación-de-patrones-de-diseño)
      - [Programa 1: Interacción entre personajes](#programa-1-interacción-entre-personajes)
      - [Programa 2: Base de datos](#programa-2-base-de-datos)
    - [Ejercicio 2: Implementación de patrones de diseño](#ejercicio-2-implementación-de-patrones-de-diseño)
      - [a. Sistema monitor de clima](#a-sistema-monitor-de-clima)

## Parte 1: Patrones de diseño

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

### Ejercicio 1: Identificación de patrones de diseño

Para cada uno de los programas que se presentan a continuación, identifique qué patrón de diseño se 
está aplicando

#### Programa 1: Interacción entre personajes
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

#### Programa 2: Base de datos
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
  override def insert(user: User): Unit = ???
  override def select(id: String): User = ???
  override def delete(id: String): Unit = ???
}

class MongoDB extends Database {
  override def insert(user: User): Unit = ???
  override def select(id: String): User = ???
  override def delete(id: String): Unit = ???
}

case class User(id: String)

val userDatabase = new UserDatabase(new SQLDatabase)
userDatabase.addUser(User("1"))  // prints "Inserting user 1 into MySQL database"

val userDatabase = new UserDatabase(new MongoDB)
userDatabase.addUser(User("1"))  // prints "Inserting user 1 into MongoDB"
```

Dada la implementación anterior, ¿puede identificar el patrón de diseño que se utiliza para hacer 
que `MongoDB` sea compatible con el resto de la aplicación?

### Ejercicio 2: Implementación de patrones de diseño

#### a. Sistema monitor de clima
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

