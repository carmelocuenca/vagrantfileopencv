# Máquina Virtual con Ubuntu 18.04 y la librería OpenCV para C, C++ y Python compiladas desde los repositorios
El fichero `Vagranfile` contine un script de instalación para la librería OpenCV desde los repositorios originales.
Es una instalación desde cero que incluye los algortimos extras y patentados que no son distribuidos.

Esto es necesario cuando queremos usar los algoritmos con patente que no pueden ser distribuidos como paquetes.
Uno de ellos por ejemplo el filtro de eliminación de ruido `bm3dDenoising`.

También crea un fichero `.whl` para instalar con `pip` en cualquiera otra máquina Ubuntu 18.04 e instala el entorno de desarrolo Miniconda.

La máquina virtual arranca con un mínimo de 2GB de RAM y el script de instalación compila con hilos. En el script el número en `make -jn` indica el número de hilos para compilar.

## ¿Qué pasa con el soporte para otras extensiones como  por ejemplo CUDA?
Desde el directorio de construcción de opencv (`~/opencv/build`) la salida de la ejecución de utilidad `cmake ..` muestra información de cómo OpenCV va a ser construida.
La utilidad de línea `ccmake ..` (desde el directorio de construcción `~/opencv/build`) permite identificar y editar los valores de los flags de construcción utilizados por `cmake`.
El script en el fichero `Vagrantfile` debe modificarse para instalar con la utilidad `apt-get` o equivalente las librerías necesarias (por ejemplo blas, lapack, atrlas...) y añadir también los nuevos flags (-D XXX) para `cmake` de ser necesario.

## Instalación de `jupyter` en la máquina virtual
```
$ vagrant ssh
$ conda activate opencv
$ conda install -y jupyter
```

## Uso `jupyter` desde la máquina virtual en la máquina anfitrión

```
$ vagrant ssh -- L 8888:localhost:8888
$ conda activate opencv
$ cd /vagrant
$ jupyter notebooks
```

## Conda environments not showing up in Jupyter
https://stackoverflow.com/questions/39604271/conda-environments-not-showing-up-in-jupyter-notebook

```
$ vagrant ssh
$ conda activate opencv
$ conda install -y ipykernel
$ python -m ipykernel install --user --name $CONDA_DEFAULT_ENV --display-name "Python ($CONDA_DEFAULT_ENV)"
```
## Manejo básico de Miniconda
```
$ conda env list
$ conda activate opencv
```

## Manejo básico de Vagrant
Tenga paciencia, el comando `vagrant up` tarda (65 minutos en un MacBook Pro 2013).

```
$ vagrant destroy -f && vagrant up # be patient
$ vagrant hatl
$ vagrant up
$ vagrant reload
$ vagrant ssh -- ls
$ vagrant ssh
```

## Camera fingerprint+
http://dde.binghamton.edu/download/camera_fingerprint

## Package python
https://packaging.python.org/tutorials/packaging-projects/
