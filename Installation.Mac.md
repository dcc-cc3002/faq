# Guía de Instalación (Mac)

No tengo Mac, así que no puedo probar que funcione lo que les digo acá

## Configurar el entorno

Primero tenemos que verificar que tengamos todo lo necesario para conseguir las herramientas:

### Essentials

Debiera venir instalado con MacOS, pueden probar si está instalado haciendo:

```bash
brew --version
```

Si eso no funciona, entonces deben instalarlo haciendo:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Luego, reinicien la terminal y ejecuten:

```bash
brew install curl zip unzip
```

### SDKMAN!

```bash
curl -s "https://get.sdkman.io" | bash
```

Luego reinicien la terminal.
Pueden probar la instalación haciendo:

```bash
sdk
```

## Compiladores
### Java

```bash
sdk install java
```

Después, pueden reiniciar la terminal y ejecutar:

```bash
java -version
```

### Scala

Blabla:

```bash
sdk install sbt
```

Reiniciamos la terminal y hacemos:

```bash
sbt -version
```

## Herramientas

### Jetbrains Toolbox


### IntelliJ

### Git

```bash
brew install git
```
