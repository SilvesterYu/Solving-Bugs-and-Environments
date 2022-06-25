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
add the following to ~/.bashrc, replace `<your_net_id>` with your own net id. If you have your project somewhere else than `scratch`, replace the second line with your project path. The line `export CXX=g++` resolves a previous g++ problem.
```
export PATH=~/anaconda3/bin:$PATH

export PYTHONPAH=/scratch/<your_net_id>/MinkLoc3D-SI

export CUDA_HOME=/usr/local/cuda-11.4

export CXX=g++
```
close the file and source it
```
source ~/.bashrc
```

### 3. Add requirements.txt

I automatically generated requirements for MinkLoc3D-SI using `pipreqs /path/to/project` and manually filtered it.

Create a `requirements.txt` file with the following:

```
bitarray==2.4.1
numba==0.55.1
numpy==1.21.5
pandas==1.4.2
psutil==5.8.0
pytorch_metric_learning==1.3.2
scikit_learn==1.1.1
scipy==1.7.3
tqdm==4.64.0
```

### 4. Add init.py to `MinkLoc3D-SI` folder

```
cd MinkLod3D-SI
```
```
touch init.py
```
```
cd ..
```

### 5. Create a job script to run the code

```
touch runmink.SBATCH
```
in the file, add the following. Replace `<your_net_id>` with your own net id.
```
#!/bin/bash

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=1:00:00
#SBATCH --mem=40GB
#SBATCH --gres=gpu:1
#SBATCH --job-name=torch

module purge
modue load anaconda3/2020.07
eval "$(conda shell.bash hook)"
source activate /scratch/<your_net_id>/minklocconda

export CUDA_HOME=/usr/local/cuda-11.4
export PYTHONPAH=$PYTHONPATH:/scratch/<your_net_id>/MinkLoc3D-SI

pip install -U git+https://github.com/NVIDIA/MinkowskiEngine -v --no-deps --install-option="--blas_include_dirs=${CONDA_PREFIX}/include" 
--install-option="--blas=openblas"

pip install tensorboard
pip install -r requirements.txt

source ~/.bashrc
cd MinkLoc3D-SI
cd training
python torchtest.py
python train.py --config ../config/config_usyd.txt --model_config ../models/minkloc_config.txt
```

### 6. Submit job script

```
sbatch runmink.SBATCH
```
It is a good practice to submit a job script in order to install libraries that require GPU because in interactive mode (temporarily requesting a GPU using, say `srun --gres=gpu:v100 --pty /bin/bash`), only 2G memory will be provided, and installation will fail due to insufficient memory.


