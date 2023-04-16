# Fpreguntas Afrecuentes Q

- [Fpreguntas Afrecuentes Q](#fpreguntas-afrecuentes-q)
  - [Programación](#programación)
    - [Leyendo la documentación del lenguaje encontré algo que no vimos en clases. ¿Lo puedo usar?](#leyendo-la-documentación-del-lenguaje-encontré-algo-que-no-vimos-en-clases-lo-puedo-usar)
    - [¿Cómo debo documentar mi código?](#cómo-debo-documentar-mi-código)
  - [Testing](#testing)
    - [¿Puedo hacer todos los tests en un solo archivo?](#puedo-hacer-todos-los-tests-en-un-solo-archivo)
    - [¿Cómo organizo mis tests?](#cómo-organizo-mis-tests)
  - [*IntelliJ*](#intellij)
    - [*IntelliJ* no me pone colores bonitos en mi código Q-Q](#intellij-no-me-pone-colores-bonitos-en-mi-código-q-q)
  - [*Git*](#git)
    - [Descargué mi código directo de *GitHub* no se como conectarlo por *Git* al repositorio D,:](#descargué-mi-código-directo-de-github-no-se-como-conectarlo-por-git-al-repositorio-d)
    - [¿Cómo creo una rama en Git?](#cómo-creo-una-rama-en-git)
  - [Linux](#linux)
    - [¿Cómo instalo las herramientas?](#cómo-instalo-las-herramientas)
  - [Mac](#mac)
    - [¿Cómo instalo las herramientas?](#cómo-instalo-las-herramientas-1)
  - [Windows](#windows)
    - [¿Cómo instalo las herramientas?](#cómo-instalo-las-herramientas-2)
    - [¿Cuál es la diferencia entre *cmd* y *Powershell*?](#cuál-es-la-diferencia-entre-cmd-y-powershell)
  - [Tarea 1](#tarea-1)

## Programación

La programación es una habilidad fundamental en la era digital en la que vivimos, y es especialmente 
relevante para carreras en tecnología, ciencias de la computación, ingeniería y otros campos 
relacionados. 
A continuación, te brindamos algunas preguntas frecuentes y respuestas útiles que pueden ayudarte en
tu curso de programación:


### ¿Cómo debo documentar mi código?

Pueden encontrar una guía de cómo documentar [aquí](Docstring.Scala.md).

### ¿Cómo funcionan los constructores en Scala?

Pueden encontrar una guía de cómo funcionan los constructores en Scala 
[aquí](Constructores.Scala.md).

### Leyendo la documentación del lenguaje encontré algo que no vimos en clases. ¿Lo puedo usar?

Sí, pero pregunta antes de hacerlo.
Puedes utilizar cualquier herramienta que provea el lenguaje, pero si no lo hemos visto en clases te 
arriesgas a utilizarlo mal; en este caso, es mejor no ocupar una herramienta a ocuparla mal.

## Testing

El testing es una parte crucial de la programación, ya que ayuda a asegurarse de que su código 
funciona correctamente y cumple con los requisitos del proyecto. 
En esta sección, se proporcionarán algunas pautas útiles para ayudarlo a organizar y escribir 
pruebas efectivas.

### ¿Puedo hacer todos los tests en un solo archivo?

No.

### ¿Cómo organizo mis tests? 

Algunas buenas prácticas para organizar sus pruebas son las siguientes:

1. Utilice una convención de nomenclatura clara y consistente para sus pruebas. 
  Esto hace que sea fácil entender qué se está probando y encontrar pruebas específicas cuando sea 
  necesario.
  Se recomienda nombrar las clases de prueba con el nombre de la clase que se está probando y 
  agregar el sufijo ``Test``. Por ejemplo, si está probando la clase ``String``, podría llamar a la
  clase de prueba ``StringTest``.
  Otros nombres comunes para las clases de prueba son agregar el sufijo ``Spec`` o ``Suite``,
  por ejemplo, ``StringSpec`` o ``StringSuite``.

2. Agrupe las pruebas relacionadas juntas. 
  Esto podría ser por clase, módulo o funcionalidad.
  Se recomienda crear una clase de prueba por cada clase que tenga su implementación.
  Si tiene una clase con muchas pruebas, puede dividirla en varias clases de prueba.
  Cada archivo debiera contener una sola clase de prueba.

3. Utilice métodos de configuración y desmontaje para evitar la duplicación de código 
  (``beforeEach`` y ``afterEach`` en *MUnit*), aunque es probable que no necesite los métodos de
  desmontaje. 
  Los métodos de configuración se utilizan para configurar el entorno de prueba y cualquier dato que
  sea necesario para la prueba, mientras que los métodos de desmontaje se utilizan para limpiar 
  después de que la prueba esté completa.
  Al utilizar métodos de configuración y desmontaje, puede evitar duplicar código en varias pruebas.

4. Utilice nombres descriptivos y concisos que expliquen lo que está probando.
  Por ejemplo, en lugar de escribir algo como ``test("Test constructor")``, utilice algo como 
  ``test("A String should be created from a Character array")``.
  Además, separe cada caso de prueba en bloques ``test`` individuales, en lugar de agrupar varios
  casos de prueba en un solo bloque ``test``.

5. Utilice la misma organización de código que la que utiliza en su implementación.
  Por ejemplo, si tenemos la siguiente estructura para nuestra implementación:
  ```
  src
  └── main
      └── scala
          └── example
              ├── String.scala
              └── StringOps.scala
  ```
  Entonces, nuestra estructura de pruebas debiera ser:
  ```
  src
  └── test
      └── scala
          └── example
              ├── StringTest.scala
              └── StringOpsTest.scala
  ```

6. Agregue comentarios y documentación sólo en casos en que la lógica del test es complicada, un
  nombre descriptivo y conciso para sus tests debiera ser suficiente en la mayoría de los casos.

## *IntelliJ*

### *IntelliJ* no me pone colores bonitos en mi código Q-Q

Anda a ``File | Invalidate Caches`` marca todas las checkboxes y espera a que se reinicie 
completamente *IntelliJ*.

## *Git*

### Descargué mi código directo de *GitHub* no se como conectarlo por *Git* al repositorio D,:

Lo más probable es que no hayas clonado correctamente el repositorio.
Para clonarlo debes ir a tu repositorio en *GitHub* y hacer click en el botón ``Code`` (El botón 
verde D:), ahí, dependiendo de cómo hayan configurado *Git* deben copiar el link HTTPS, SSH o GitHub 
CLI.

![](img/git_clone.png)

Luego, con el link copiado, hacen:

```bash
git clone <link HTTPS o SSH>
# Con GitHub CLI sólo tienen que pegar lo que copiaron en la terminal.
# Debiera verse así
gh repo clone <su repo>
```

Ahora comprueben que funciona haciendo:

```bash
# Desde la carpeta de su repo
git status
```

### ¿Cómo creo una rama en *Git*?

```bash
git branch nombreDeLaBranch
```

Ahora, para trasladarse a la branch creada, decimos:

```bash
git checkout nombreDeLaBranch
```

Para saber en qué rama están actualmente, pueden utilizar el comando git status, y la primera línea les indicará el nombre de la branch en la que se encuentran.


## Linux

### ¿Cómo instalo las herramientas?

Para esto vean las instrucciones de [aquí](Installation.Linux.md)

## Mac

### ¿Cómo instalo las herramientas?

Para esto vean las instrucciones de [aquí](Installation.Mac.md)

## Windows

### ¿Cómo instalo las herramientas?

Para esto vean las instrucciones de [aquí](Installation.Windows.md)

### ¿Cuál es la diferencia entre *cmd* y *Powershell*?

La diferencia es que nunca debieras usar *cmd*.
Hablo en serio.

Pero hablando en serio, *cmd* es una herramienta de 1987 (y la versión actual es de 1993), 
*Powershell* (PS) es una herramienta publicada el 2006 (con actualizaciones hasta la actualidad).
Un error común es creer que los comandos de *cmd* no pueden usarse desde PS, esto es mentira.
La mayoría de los comandos de *cmd* pueden ejecutarse directamente desde PS o tienen 
versiones equivalentes en PS, por ejemplo:

```cmd
REM En cmd podemos "limpiar" la consola con el siguiente comando
cls
```

```powershell
# El mismo comando puede ser usado desde PS
cls
# Ahora si hacemos:
cls -?
```
Obtenemos:

```
NAME
    Clear-Host
...
```

Esto nos indica que en PS ``cls`` es un alias de ``Clear-Host``, esto hace que podamos usar uno o el
otro indistintamente (aunque en PS se recomienda que no se use el alias en scripts para mejorar la 
legibilidad del código).

Por otro lado, tenemos comandos que no tienen un equivalente en PS como ``robocopy`` 
(*Robust Copy*), de nuevo, aquí no hay problemas porque podemos usar ``robocopy`` desde PS de la 
misma forma que lo haríamos en *cmd*

```powershell
robocopy /help
```

Hay otras diferencias que pueden ver en el cuadro de abajo, pero en general PS es mejor que *cmd* en
todos los sentidos.

![](img/cmd_vs_ps.png)

## Tarea 1

### ¿Y la carpeta de tests? ¿Dónde está la carpeta de tests?

Efectivamente, el repositorio que les entregamos no contaba con una carpeta de test. Para crearla, deben hacer click derecho en la carpeta src, y luego crear un nuevo directorio que diga lo siguiente:

```
test/scala
```

Podrán verificar que la carpeta está bien creada si el directorio scala les aparece de color verde, lo que significa que IntelliJ reconoce esa carpeta como la raíz de todos sus tests.
