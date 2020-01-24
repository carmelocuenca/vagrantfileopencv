# Máquina Virtual con Ubuntu 18.04 y la librería OpenCV instalada desde fichero
El fichero `Vagranfile` contine un script de instalación para la librería OpenCV desde los repositorios originales.
Es una instalación desde cero.

Esto es necesario cuando queremos usar los algoritmos con patente que no pueden ser distribuidos como paquetes.
Uno de ellos por ejemplo el filtro de iluminación de ruido `bm3dDenoising`.

Utiliza Miniconda con python 3.7 para crear el paquete `cv2`. Lo instala en el entorno `opencv` de miniconda y además crea un fichero `.whl` para instalar con `pip` en cualquier otro máquina Ubunt 18.04.

La máquina virtual arranca con 4GB de RAM y el script compila con un único hilo. En el script el número en `make -j1` indica el número de hilos para compilar.

## ¿Qué pasa con el soporte para otras extensiones como  por ejemplo CUDA?
Desde el directorio de construcción de opencv (`~/opencv/build`) la salida de la ejecución de utilidad `cmake ..` muestra información de cómo OpenCV va a ser construida.
La utilidad de línea `ccmake ..` (desde el directorio de construcción) permite identificar y editar los valores de los flags de construcción utilizados por `cmake`.
El script en el fichero `Vagrantfile` debe modificarse para instalar las librerías necesarias (por ejemplo blas, lapack, atrlas...) y añadir también los nuevos flags para `cmake` de ser necesario.

## Instalación de `jupyter` en la máquina virtual
```
$ vagrant ssh
$ conda activate opencv
$ conda install -y jupyter
```

## Usando `jupyter` desde la máquina virtual en la máquina anfitrión

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
