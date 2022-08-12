# Guía de Instalación

## Configurar el entorno

Primero necesitamos configurar algunas cosas para hacernos más fácil la vida

### Winget

Windows debiera traerlo instalado por defecto, para ver esto abran *PowerShell* y hagan

```powershell
winget -?
```

Si eso no funciona, pueden descargarlo aquí: https://apps.microsoft.com/store/detail/instalador-de-aplicaci%C3%B3n/9NBLGGH4NNS1?hl=es-cl&gl=CL

### Windows Terminal

También debiera venir instalado, pero si no lo pueden instalar desde Powershell haciendo:

```powershell
winget install Microsoft.WindowsTerminal
```

Desde ahora, cuando diga Powershell me voy a estar refiriendo a una sesión de Powershell corriendo
en Windows Terminal

### Oh-My-Posh

Esta herramienta es para personalizar la consola.

Para instalarla hagan:

```powershell
winget install JanDeDobbeleer.OhMyPosh -s winget
```

Ahora, vamos a editar el perfil de Powershell.
En Powershell con permisos de administrador:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Force
# Creamos el archivo sólo si no existe
if (-not (Test-Path $PROFILE)) {
  New-Item -ItemType File -Path $PROFILE -Force
}
notepad.exe $PROFILE
```

Ahora, al comienzo del archivo agregamos:
```powershell
oh-my-posh.exe init pwsh --config "$env:POSH_THEMES_PATH\powerlevel10k_rainbow.omp.json" | `
  Invoke-Expression
```

Guarden el archivo y reinicien Windows Terminal.

### Chocolatey

Esta es otra herramienta como *winget* que vamos a usar para instalar cosas.
Para instalarlo (en una sesión de Powershell como administrador):

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
Invoke-WebRequest "https://chocolatey.org/install.ps1" -UseBasicParsing | Invoke-Expression
refreshenv
```

Pueden probar que se haya instalado con:
```powershell
choco -?
```

## Compiladores

### Java

```powershell
winget install Azul.Zulu.18.JDK
refreshenv
```

Luego lo prueban haciendo:

```powershell
java -version
```

Es importante que esto retorne la versión 18, si retorna otra versión desinstalen cualquier 
instalación de Java que tengan e instalen de nuevo con el comando.

### Kotlin

Para instalar el compilador de Kotlin hacemos (desde Powershell como admin):

```powershell
choco install kotlinc
refreshenv
```

Y lo probamos:

```powershell
kotlinc -version
```

### Scala

Scala también lo podemos instalar desde la terminal haciendo:
```powershell
choco install sbt
```


## Herramientas

### Jetbrains Toolbox

Desde Powershell:

```powershell
winget install JetBrains.Toolbox
```

### IntelliJ

Usen Toolbox para instalar IntelliJ IDEA Ultimate y registren su correo de la u aquí: https://www.jetbrains.com/community/education/#students

### Git

Desde Powershell:

```powershell
winget install Git.Git
```