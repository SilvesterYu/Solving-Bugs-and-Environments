## Installing MinkowskiEngine on NYU Greene HPC

Full documentation for installing MinkowskiEngine in an anaconda environment for running `MinkLoc3D-SI`.

Best practice, first go to 

```
cd scratch/<your_net_id>
```

### 1. Build anaconda environment

Here I use `python=3.8` as an example (it is suitable for `MinkLoc3D-SI` implementation with MinkowkiEngine) to build a new anaconda environment in `scratch/<your_net_id>` directory

```
conda create -p ./minklocconda python=3.8
```

### 2. Add path

```
nano ~/.bashrc
```
add the following to ~/.bashrc, replace `<your_net_id>` with your own net id. If you have your project somewhere else than `scratch`, replace the second line with your project path
```
export PATH=~/anaconda3/bin:$PATH

export PYTHONPAH=/scratch/<your_net_id>/MinkLoc3D-SI

export CUDA_HOME=/usr/local/cuda-11.4

export CXX=g++
```
source it
```
source ~/.bashrc
```

