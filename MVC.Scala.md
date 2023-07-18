## Ejemplo MVC

Este código es un ejemplo de la implementación del patrón Modelo-Vista-Controlador (MVC) en Scala para manejar una lista de tareas.

## Modelo

```scala
// Task (Model)
case class Task(description: String) {
  // El modelo limita el acceso a los datos a través de métodos públicos.
  private var status: Boolean = false
  
  def markAsComplete(): Unit = {
    status = true
  }

  override def toString: String = {
    s"Task(description = $description, status = ${if (status) "Completed" else "Pending"})"
  }
}
```

Aquí definimos el modelo `Task`, que representa una tarea individual. Cada tarea tiene una descripción y un estado (`status`) que es verdadero si la tarea está completa y falso en caso contrario. La función `markAsComplete` se usa para marcar una tarea como completada.

## Controlador

```scala
// TaskController (Controller)
class TaskController {
  // El controlador puede almacenar objetos del modelo
  private val tasks = mutable.ArrayBuffer.empty[Task]

  // El controlador expone métodos públicos para acceder a los datos del modelo
  // sin exponer el modelo en sí.
  // Para esto es importante que los valores que reciban y retornen los métodos
  // públicos sean tipos "primitivos"
  def addTask(description: String): Unit = {
    tasks += Task(description)
  }

  def completeTask(index: Int): Unit = {
    tasks(index).markAsComplete()
  }

  def listTasks(): Unit = {
    tasks.zipWithIndex.foreach { case (task, index) =>
      println(s"$index. $task")
    }
  }
}
```

El controlador, `TaskController`, contiene la lógica de negocio de la aplicación. Gestiona la creación de nuevas tareas, el marcado de tareas como completadas y la impresión de todas las tareas. Los datos se almacenan en un `ArrayBuffer` de `Task`.

## Vista

```scala
// UserInterface (View)
class UserInterface(controller: TaskController) {
  // La vista es la que se encarga de interactuar con el usuario
  private def getUserInput = scala.io.StdIn.readLine()

  private def showMenu(): Unit = {
    println("1. Add task")
    println("2. Complete task")
    println("3. List tasks")
    println("4. Exit")
  }

  private def showAddTask(): String = {
    println("Enter task description:")
    getUserInput
  }

  private def showCompleteTask(): Int = {
    println("Enter task index:")
    getUserInput.toInt
  }

  private def showExit(): Unit = {
    println("Bye!")
  }

  private def showInvalidOption(): Unit = {
    println("Invalid option")
  }

  def run(): Unit = {
    var option = 0
    while (option != 4) {
      showMenu()
      option = getUserInput.toInt
      option match {
        case 1 =>
          val description = showAddTask()
          controller.addTask(description)
        case 2 =>
          val index = showCompleteTask()
          controller.completeTask(index)
        case 3 =>
          controller.listTasks()
        case 4 =>
          showExit()
        case _ =>
          showInvalidOption()
      }
    }
  }
}
```

`UserInterface` es la vista de nuestra aplicación. Interactúa con el usuario, muestra el menú, recibe las entradas del usuario y llama a las funciones apropiadas del controlador basándose en esas entradas. Observe cómo la vista no interactúa directamente con el modelo (`Task`), sino que utiliza el controlador para manejar todas las interacciones.

## Aplicación

```scala
object TaskApp {
  def main(args: Array[String]): Unit = {
    val controller = new TaskController
    val ui = new UserInterface(controller)
    ui.run()
  }
}
```

Finalmente, `TaskApp` es nuestra aplicación principal. Crea una instancia del controlador y de la interfaz de usuario, y luego inicia la interfaz de usuario.