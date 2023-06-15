# Ejercicios

## Ejercicio 1: Identificación de patrones de diseño

Considerando los patrones de diseño:

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

Para cada uno de los programas que se presentan a continuación, identifique qué patrón de diseño se 
está aplicando

### Programa 1 <!-- Double Dispatch -->

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

## Ejercicio 2: Implementación de patrones de diseño

### a. Sistema monitor de clima <!-- Adapter -->

Asumamos que tenemos un sistema monitor de clima que obtiene datos de diferentes sensores de 
clima. 
Acabamos de comprar un nuevo set de sensores de clima de un fabricante diferente. 
La interfaz de los nuevos sensores es diferente a la que tenemos actualmente. 
Para mantener la funcionalidad del sistema existente, necesitamos adaptar estos nuevos sensores a 
nuestra interfaz existente.

#### i

Identifique el patrón de diseño que se puede aplicar para resolver este problema.