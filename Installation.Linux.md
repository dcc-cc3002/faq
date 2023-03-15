# Guía de Instalación

Voy a dar instrucciones para distribuciones basadas en Debian, si tienen otra no puedo ayudarles 
mucho uwu

## Configurar el entorno

Primero instalaremos algunas herramientas para facilitarnos la pega.

### Essentials

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential curl unzip zip
sudo apt-get autoremove
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
sudo apt-get install git
```
