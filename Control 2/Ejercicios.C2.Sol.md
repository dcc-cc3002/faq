# Tabla de contenidos

- [Tabla de contenidos](#tabla-de-contenidos)
- [Parte 2: Excepciones](#parte-2-excepciones)
  - [Ejercicio 1: Clases de Excepciones](#ejercicio-1-clases-de-excepciones)
  - [Ejercicio 2: Checked vs Unchecked Exceptions](#ejercicio-2-checked-vs-unchecked-exceptions)

Parte 2: Excepciones
====================

Ejercicio 1: Clases de Excepciones
----------------------------------

- `Throwable` es la clase base de todos los objetos que pueden ser lanzados y capturados por un 
  bloque `try`.
- `Exception` es la clase base de todas las excepciones que no son errores.
  Por lo general, las excepciones representan condiciones de las que un programa razonable podría 
  querer recuperarse. 
- `Error` es la clase base para las condiciones de error que una aplicación normalmente no intenta 
  capturar.
  La clase ``Error`` define excepciones que no se espera que sean capturadas bajo circunstancias 
  normales por tu programa. 
  Estas condiciones normalmente ocurren en casos donde la JVM se encuentra en un estado que no puede 
  recuperarse, como ``OutOfMemoryError``.
  Mientras que Exception y sus subclases representan condiciones excepcionales que tu programa puede
  querer capturar, ``Error`` y sus subclases representan condiciones anormales que pueden hacer que 
  tu programa se comporte de manera impredecible o incluso se bloquee. 

Ejercicio 2: Checked vs Unchecked Exceptions
--------------------------------------------

Las checked exceptions son aquellas que el compilador obliga a capturar o relanzar. Las unchecked exceptions son aquellas que el compilador no obliga a capturar o relanzar.

En Scala no existen las checked exceptions.
