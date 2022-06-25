## Installing MinkowskiEngine on NYU Greene HPC

Full documentation of installing MinkowskiEngine in your project's anaconda environment.

### 1. Add path

```
nano ~/.bashrc
```
add the following to ~/.bashrc, replace <your_net_id> with your own net id. If you have your project somewhere else than `scratch`, replace the second line with your project path
```
export PATH=~/anaconda3/bin:$PATH

export PYTHONPAH=/scratch/<your_net_id>/<your_project_folder>

export CUDA_HOME=/usr/local/cuda-11.4

export CXX=g++
```
source it
```
source ~/.bashrc
```

### 2. 
