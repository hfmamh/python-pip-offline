# Instalacion de bibliotecas de Python sin internet con `pip download`

Esta es una versión actualizada de una guía que hice hace tiempo. Es particularmente útil cuando tienes un servidor en una intranet detrás de un firewall que bloquea el acceso a PyPI.

Además, utilizando los mismos principios, puedes construir tu build para despplegar en servicios cloud serverless como AWS Lambda y Azure Functions.

## Descargar bibliotecas

Para este caso utilizaremos la libreria pandas de ejemplo. De igual forma, puedes usar un archivo `requirements.txt` para multiples librerias.

Creamos una carpeta donde se guardaran las dependencias

```shell 
mkdir venv_libs
cd venv_libs
```
Dentro, ejecutamos:

```powershell
python -m pip download --platform manylinux1_x86_64 --python-version 3.6.8 --only-binary=:all: --no-binary=:none: pandas
```
## Parametros

#### `--python-version`

Version de python de tu maquina sin conexion.

#### `--platform`

Este parametro depende del sistema operativo de tu máquina sin conexión. En este caso utilizamos manylinux1_x86_64 en un servidor con RedHat. Para obtener más información sobre qué parámetro debes usar, consulta el repositorio el [repositorio de manylinux](https://github.com/pypa/manylinux).

#### `--only-binary=:all:` y `--no-binary=:none:`

Estos parametros definen si queremos descargar paquetes precompilados o source code, en este caso se fuerzan a descargar paquetes precompilados ya que `pip download` no permite descargar paquetes en codigo fuente si usarmos `--platform` o `--python-version`.

## Instalacion

Mover la carpeta `venv_libs` a el servidor donde queremos instalarlas. Antes de la instalacion, procedemos a crear un entorno virtual 

```shell
python3 -m venv venv
source venv/bin/activate

```

Luego instalamos apuntando a la carpeta donde se encuentran las librerias con el parametro `--find-links` y la libreria a instalar, en este caso `pandas`

```powershell
python -m pip install --no-index --find-links=venv_libs/ pandas
```

## Errores al instalar librerias no compiladas

Algunas librerias no tienen disponible codigo precompilado en binarios (por ejemplo `pyautogui`) por lo que fallara en la descarga por usar `--only-binary=:all:` y `--no-binary=:none:`.

Ante este caso se recomienda usar un contenedor de Docker, con la misma plataforma que usa tu servidor sin conexion y utilizar `pip download` sin `´platformn` o `--python-version`

```powershell
python -m pip download pyautogui
```