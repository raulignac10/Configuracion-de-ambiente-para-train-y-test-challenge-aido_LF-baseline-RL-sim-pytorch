# Configuración de ambiente para train y test challenge-aido_LF-baseline-RL-sim-pytorch
Instalación y configuración del ambiente de gym-duckietown de forma local para ejecutrar train y test del challenge-aido_LF-baseline-RL-sim-pytorch

## Requisitos: 
- SO Ubuntu
- GPU nvidia compatible con [CUDA](https://developer.nvidia.com/cuda-gpus)
- [Git](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-20-04-es)

## Instrucciones

1. Instalación de [Anaconda](https://www.digitalocean.com/community/tutorials/como-instalar-anaconda-en-ubuntu-18-04-quickstart-es) siguiendo cada uno de los pasos descritos en el articulo y **solo hasta el paso 6**
2. Clonar e Instalar repositorio de [gym-duckietown](https://github.com/duckietown/gym-duckietown) (Duckietown self-driving car simulator environments for OpenAI Gym) siguiendo el metodo de [instalación alternativo de Conda](https://github.com/duckietown/gym-duckietown#installation-using-conda-alternative-method)
```
git clone https://github.com/duckietown/gym-duckietown.git
cd gym-duckietown
conda env create -f environment.yaml
source activate gym-duckietown
export PYTHONPATH="${PYTHONPATH}:`pwd`"
pip3 install -e .
```
3. Instalación del modulo `pip install pyparsing==2.4.2a1`
4. Realizar prueba de funcionamiento de gym-duckietown
```
source activate gym-duckietown
cd gym-duckietown
cd exercises
python basic_control.py
```
<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/97913559/151086978-a662b0df-4c4a-4f15-b90b-b209d2a5afc4.png">
</p>

5. Clonar e Instalar repositorio del [challenge-aido_LF-baseline-RL-sim-pytorch](https://github.com/duckietown/challenge-aido_LF-baseline-RL-sim-pytorch) dentro de la carpeta de gym-duckietown
```
git clone https://github.com/duckietown/challenge-aido_LF-baseline-RL-sim-pytorch
cd challenge-aido_LF-baseline-RL-sim-pytorch
pip3 install -e .
```
6. Lo anterior lo dejaremos de lado un momento y ahora configuraremos los driver de la GPU nvidia, para ello utilizaremos el siguiente [tutorial](https://medium.com/@redowan/no-bullshit-guide-on-installing-tensorflow-gpu-ubuntu-18-04-18-10-238924cc4a6a) y **solo hasta el paso de** "Enable the driver". Al finalizar el tutorial deberiamos obtener un output similar a esto:

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/97913559/151089082-5c903cc7-b48b-4807-8742-684bbcbe14a3.png">
</p>

7. Cerrar todo y abrir una nueva terminal, activar el ambiente gym-duckietown `source activate gym-duckietown` para instalar [pytorch](https://pytorch.org/)
```
pip3 install torch==1.10.1+cu113 torchvision==0.11.2+cu113 torchaudio==0.10.1+cu113 -f https://download.pytorch.org/whl/cu113/torch_stable.html
```

8. Realizar prueba de funcionamiento de GPU con Cuda, abrir nuevo terminal, activar el ambiente gym-duckietown `source activate gym-duckietown`, ejecutar python `python` y ejecutar lo siguiente:
```
Python 3.6.13 |Anaconda, Inc.| (default, Jun  4 2021, 14:25:59) 
[GCC 7.5.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.cuda.is_available()
True

```
Es necesario que la respuesta sea True, con eso confirmamos que la GPU esta funcionando con CUDA

9. Finalmente estamos listos para poder ejecutar train y test del [challenge-aido_LF-baseline-RL-sim-pytorch](https://github.com/duckietown/challenge-aido_LF-baseline-RL-sim-pytorch). Para comenzar el entrenamiento del agente duckie, debemos ejecutar lo siguiente:
```
cd duckietown_rl
python -m scripts.train_cnn --seed 123
```
Deberiamos obtener un output similar a esto: 

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/97913559/151089082-5c903cc7-b48b-4807-8742-684bbcbe14a3.png">
</p>

10. Luego del entrenamiento con lo hiperparametros que configuramos, podremos ejecutar lo entrenado realizando lo siguiente:

* Copiar los archivos de gym-duckietown/challenge-aido_LF-baseline-RL-sim-pytorch/duckietown_rl/pytorch_models DDPG_XXX_actor.pth y DDPG_XXX_critic.pht (XXX es el hiperparametro seed) a la carpeta models localizada en root.
* Copiar el archivo gym-duckietown/challenge-aido_LF-baseline-RL-sim-pytorch/duckietown_rl/ddpg.py a la carpeta root renombrada como model.py
* Finalmente abrir nuevo terminal, activar el ambiente gym-duckietown `source activate gym-duckietown` y ejecutar lo siguiente:
```
cd duckietown_rl
python -m scripts.test_cnn --seed 123
```

Deberiamos obtener un output similar a esto (duckie interactuando en el ambiente de gym-duckietown): 


