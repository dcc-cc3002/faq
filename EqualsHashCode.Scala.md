# Índice

- [Método equals](#método-equals)
  - [Igualdad referencial](#igualdad-referencial)
    - [Igualdad referencial en strings](#igualdad-referencial-en-strings)
  - [Igualdad estructural](#igualdad-estructural)
    - [Implementación por defecto](#implementación-por-defecto)
    - [Sobrescritura de equals](#sobrescritura-de-equals)
- [Método hashCode](#método-hashcode)
  - [Implementación por defecto](#implementación-por-defecto-1)
  - [Sobrescritura de hashCode](#sobrescritura-de-hashcode)
- [¿Por qué debería sobrescribir hashCode cuando sobrescribo equals?](#por-qué-debería-sobrescribir-hashcode-cuando-sobrescribo-equals)
- [Referencias](#referencias)
- [Anexo: Utilizando el método hashCode](#anexo-utilizando-el-método-hashcode)
  - [Tabla de hashing personalizada](#tabla-de-hashing-personalizada)
  - [Tabla de hashing usando la librearía estándar de Scala](#tabla-de-hashing-usando-la-librearía-estándar-de-scala)

# Método equals

## Igualdad referencial

En _Scala_, la igualdad referencial se determina usando el método ``eq``.
Este método compara si dos referencias apuntan al mismo objeto en memoria.
También se le conoce como "igualdad de identidad".

Por ejemplo, si tenemos dos instancias de la clase ``Person``, aunque ambas tengan las mismas 
propiedades, son objetos diferentes en memoria, y por lo tanto no son iguales referencialmente.

### Igualdad referencial en strings

Los strings en _Scala_ son almacenados en el pool de strings.
Esto se hace para evitar crear múltiples instancias de un mismo string y así mejorar el rendimiento.
Entonces, si tenemos dos variables que contienen el mismo string, ambas variables apuntan al mismo 
objeto a menos que se cree un nuevo string explícitamente con el operador ``new``.

```scala
val s1 = "Hello"
val s2 = "Hello"
val s3 = new String("Hello")

s1 eq s2 // true
s1 eq s3 // false
```

## Igualdad estructural

La igualdad estructural en _Scala_ se basa en la comparación de los valores de los atributos de dos
objetos en lugar de sus referencias.
Esta comparación se realiza usando el método ``equals`` que puede ser sobrescrito para definir
comparaciones personalizadas.

### Implementación por defecto

La implementación por defecto de ``equals`` en _Scala_ es equivalente a la igualdad referencial.

```scala
class Person(val name: String, val age: Int)

val p1 = new Person("Shinji Ikari", 14)
val p2 = new Person("Shinji Ikari", 14)

p1 eq p2 // false
p1 equals p2 // false
```

### Sobrescritura de equals

Para definir una comparación personalizada, podemos sobrescribir el método ``equals`` en nuestra 
clase.
El método ``equals`` recibe un parámetro de tipo ``Any`` que representa el objeto con el que se
va a comparar.

La comparación estructural siempre se hace haciendo las siguientes comprobaciones:

1. Comprobar si los objetos son iguales referencialmente.
2. Comprobar si el objeto recibido es de la misma clase que el objeto actual.
3. Comprobar si los objetos tienen los mismos atributos y sus valores son iguales.

En _Scala_, es una buena práctica extender el trait ``Equals`` para sobrescribir el método 
``equals``.
Este trait impone un contrato que requiere la implementación del método ``canEqual``, que se
encarga de determinar si dos objetos son comparables.
Esto en general se hace comprobando si el objeto recibido es de la misma clase que el objeto
actual.

```scala
class Person(val name: String, val age: Int) extends Equals {
  // 2. Comprobar si el objeto recibido es de la misma clase que el objeto actual.
  override def canEqual(that: Any): Boolean = that.isInstanceOf[Person]

  override def equals(that: Any): Boolean = {
    if (canEqual(that)) {
      // Ahora que sabemos que el objeto recibido es de la misma clase que el objeto actual,
      // podemos hacer un cast seguro.
      val other = that.asInstanceOf[Person]
      // 1. Comprobar si los objetos son iguales referencialmente.
      (this eq other) ||
        // 3. Comprobar si los objetos tienen los mismos atributos y sus valores son iguales.
        (this.name == other.name && this.age == other.age)
    } else {
      false
    }
  }
}
```

Es probable que si buscan en internet encuentren ejemplos de implementación de ``equals`` que 
utilizan la instrucción ``case``.
Esta instrucción se utiliza para hacer _pattern matching_, este es un concepto avanzado, así que
se recomienda no utilizarla si sus conocimientos de _Scala_ son básicos.

# Método hashCode

El método ``hashCode`` en _Scala_ se usa para generar un valor hash para un objeto.
Este valor hash se usa en tablas hash y otras estructuras de datos que necesitan almacenar y
recuperar objetos de forma eficiente.

Se puede obtener el valor hash de un objeto usando el método ``hashCode`` o el operador ``##``,
donde la segunda forma es la recomendada.

El método ``hashCode`` debe ser implementado de forma consistente con el método ``equals``.
De esta forma, dos objetos que son iguales según el método ``equals`` deben tener el mismo valor
hash.
También se recomienda que si dos objetos no son iguales según el método ``equals``, deben tener
valores hash diferentes, aunque no es obligatorio.

## Implementación por defecto

La implementación por defecto de ``hashCode`` en _Scala_ es equivalente a la referencia del objeto
en memoria.

```scala
class Person(val name: String, val age: Int)

val p1 = new Person("Shinji Ikari", 14)
val p2 = new Person("Shinji Ikari", 14)

p1.## // 1163157884
p2.## // 1311053135
```

## Sobrescritura de hashCode

Para definir una implementación personalizada, podemos sobrescribir el método ``hashCode`` en
nuestra clase.

```scala
class Person(name: String, age: Int) {
  // ...
  override def hashCode(): Int = {
    val prime = 31
    var result = 1
    result = prime * result + name.##
    result = prime * result + age
    result
  }
}
```

En este ejemplo, el método ``hashCode`` usa los campos ``name`` y ``age`` del objeto ``Person``
para calcular un valor hash.
El valor hash se calcula multiplicando el valor hash del campo ``name`` por un número primo
(31 en este caso), sumando el valor hash del campo ``age``, y devolviendo el resultado.

Otra forma de implementar el método ``hashCode`` es utilizando la función 
``Objects::hash(Array[Object])`` de la librería estándar de _Java_.
De esta forma, el método ``hashCode`` se puede implementar de forma más concisa.

```scala
import java.util.Objects

class Person(name: String, age: Int) {
  // ...
  override def hashCode(): Int = Objects.hash(name, age)
}
```

### Problemas con la implementación anterior

La implementación anterior tiene un problema.
Si calculamos el valor hash de dos objetos de clases diferentes, pero que tienen los mismos
campos, el valor hash será el mismo.

```scala
class Person(name: String, age: Int) {
  // ...
  override def hashCode(): Int = Objects.hash(name, age)
}

class Employee(name: String, age: Int) {
  // ...
  override def hashCode(): Int = Objects.hash(name, age)
}

val p = new Person("Shinji Ikari", 14)
val e = new Employee("Shinji Ikari", 14)

p.## // 1163157884
e.## // 1163157884
```

Esto se debe a que no estamos siendo del todo consistentes con el método ``equals``.
En el método ``equals``, comprobamos que el objeto recibido es de la misma clase que el objeto
actual, pero nuestra implementación de ``hashCode`` no tiene en cuenta la clase del objeto.
Para solucionar este problema, podemos añadir la clase del objeto al valor hash.

```scala
class Person(val name: String, val age: Int) {
  override def hashCode: Int = {
    val prime = 31
    var result = 1
    result = prime * result + classOf[Person].##
    result = prime * result + name.##
    result = prime * result + age.##
    result
  }
}

class Employee(val name: String, val age: Int) {
  override def hashCode: Int = {
    val prime = 31
    var result = 1
    result = prime * result + classOf[Employee].##
    result = prime * result + name.##
    result = prime * result + age.##
    result
  }
}

val p = new Person("Shinji Ikari", 14)
val e = new Employee("Shinji Ikari", 14)

p.## // 1163157884
e.## // 1311053135
```

Alternativamente, podemos usar la función ``Objects::hash(Array[Object])`` de la librería estándar
de _Java_ para calcular el valor hash.

```scala
import java.util.Objects

class Person(val name: String, val age: Int) {
  override def hashCode: Int = Objects.hash(classOf[Person], name, age)
}

class Employee(val name: String, val age: Int) {
  override def hashCode: Int = Objects.hash(classOf[Employee], name, age)
}

val p = new Person("Shinji Ikari", 14)
val e = new Employee("Shinji Ikari", 14)

p.## // 1163157884
e.## // 1311053135
```

# ¿Por qué debería sobrescribir hashCode cuando sobrescribo equals?

Cuando sobrescribimos el método ``equals`` en una clase, es recomendable sobrescribir también el
método ``hashCode``.
Esto es porque el método ``hashCode`` se usa en estructuras de datos basadas en tablas hash,
como ``HashSet`` o ``HashMap``, para determinar dónde almacenar los objetos y cómo recuperarlos
de forma eficiente.
Si dos objetos son iguales según el método ``equals`` pero tienen diferentes valores hash, pueden
ser almacenados en diferentes buckets de una estructura de datos basada en tablas hash, lo que
puede causar problemas al intentar recuperar o eliminar los objetos.

El contrato general entre los métodos ``hashCode`` y ``equals`` es que si dos objetos son iguales
según el método ``equals``, deben tener el mismo valor hash.
Por lo tanto, es importante asegurarse de que el método ``hashCode`` se implementa de forma
consistente con el método ``equals`` para mantener la integridad de las estructuras de datos
basadas en tablas hash.

# Referencias

* baeldung. “Equality in Scala | Baeldung on Scala,” June 3, 2020. https://www.baeldung.com/scala/equality-in-scala.

* chaotic3quilibrium. “Answer to ‘What Is the Standard Idiom for Implementing Equals and HashCode in Scala?’” Stack Overflow, June 8, 2019. https://stackoverflow.com/a/56509518.

# Anexo: Utilizando el método hashCode

## Tabla de hashing personalizada

A continuación pueden ver un ejemplo de cómo utilizar el método ``hashCode`` para crear una tabla
de hash personalizada.

```scala
import scala.collection.mutable

class Person(name: String, age: Int) extends Equals {
  override def canEqual(that: Any): Boolean = that.isInstanceOf[Person]
  
  override def equals(that: Any): Boolean = {
    if (canEqual(that)) {
      val other = that.asInstanceOf[Person]
      (this eq other) ||
        (this.name == other.name && this.age == other.age)
    } else {
      false
    }
  }

  override def hashCode: Int = Objects.hash(classOf[Person], name, age)
}

class PersonHashSet(size: Int) {
  private val table = new Array[mutable.ListBuffer[Person]](size)

  def put(person: Person): Unit = {
    val index = hash(person)
    if (table(index) == null) {
      table(index) = mutable.ListBuffer(person)
    } else {
      table(index) += person
    }
  }

  def contains(person: Person): Boolean = {
    val index = hash(person)
    if (table(index) == null) {
      false
    } else {
      table(index).exists(p => p.equals(person))
    }
  }

  private def hash(person: Person): Int = {
    Math.abs(person.## % size)
  }
}

val table = new PersonHashSet(10)

table.put(new Person("Shinji Ikari", 14))
table.put(new Person("Asuka Langley Soryu", 14))
table.put(new Person("Rei Ayanami", 14))
table.put(new Person("Misato Katsuragi", 32))
table.put(new Person("Ritsuko Akagi", 32))
table.put(new Person("Gendo Ikari", 42))

table.contains(new Person("Shinji Ikari", 14)) // true
table.contains(new Person("Asuka Langley Soryu", 14)) // true
table.contains(new Person("Rei Ayanami", 14)) // true
table.contains(new Person("Misato Katsuragi", 32)) // true
table.contains(new Person("Ritsuko Akagi", 32)) // true
table.contains(new Person("Gendo Ikari", 42)) // true
table.contains(new Person("Kaworu Nagisa", 14)) // false
```

## Tabla de hashing usando la librearía estándar de Scala

A continuación pueden ver una implementación "equivalente" a la anterior usando la librería estándar
de _Scala_.

```scala
import scala.collection.mutable

class Person(name: String, age: Int) extends Equals {
  override def canEqual(that: Any): Boolean = that.isInstanceOf[Person]
  
  override def equals(that: Any): Boolean = {
    if (canEqual(that)) {
      val other = that.asInstanceOf[Person]
      (this eq other) ||
        (this.name == other.name && this.age == other.age)
    } else {
      false
    }
  }

  override def hashCode: Int = Objects.hash(classOf[Person], name, age)
}

val table = new mutable.HashSet[Person]()
table.add(new Person("Shinji Ikari", 14))
table.add(new Person("Asuka Langley Soryu", 14))
table.add(new Person("Rei Ayanami", 14))
table.add(new Person("Misato Katsuragi", 32))
table.add(new Person("Ritsuko Akagi", 32))
table.add(new Person("Gendo Ikari", 42))

table.contains(new Person("Shinji Ikari", 14)) // true
table.contains(new Person("Asuka Langley Soryu", 14)) // true
table.contains(new Person("Rei Ayanami", 14)) // true
table.contains(new Person("Misato Katsuragi", 32)) // true
table.contains(new Person("Ritsuko Akagi", 32)) // true
table.contains(new Person("Gendo Ikari", 42)) // true
table.contains(new Person("Kaworu Nagisa", 14)) // false
```

Donde la implementación del método ``contains`` es más complejo que la implementación de la
tabla de hashing personalizada, pero en esencia es lo mismo.

```scala	
override def contains(elem: A): Boolean = findNode(elem) ne null

@`inline` private[this] def findNode(elem: A): Node[A] = {
  val hash = computeHash(elem)
  table(index(hash)) match {
    case null => null
    case nd => nd.findNode(elem, hash)
  }
}
```	