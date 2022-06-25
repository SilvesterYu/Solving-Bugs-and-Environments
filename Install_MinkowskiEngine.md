## Installing MinkowskiEngine on NYU Greene HPC

Full documentation for installing MinkowskiEngine in an anaconda environment for running `MinkLoc3D-SI`.

Best practice, first go to 

```
cd scratch/<your_net_id>
```

### 1. Build anaconda environment

Here I use `python=3.8` as an example (it is suitable for `MinkLoc3D-SI` implementation with MinkowkiEngine) to build a new anaconda environment in `scratch/<your_net_id>` directory
```
module purge
module load anaconda3/2020.07
```
```
conda create -p ./minklocconda python=3.8
```
Try activating the environment
```
conda activate ./minklocconda
```
and make sure you meet all the following requirements for MinkowkiEngine:

---

Ubuntu >= 14.04 # not for HPC

CUDA >= 10.1.243 and the same CUDA version used for pytorch (e.g. if you use conda cudatoolkit=11.1, use CUDA=11.1 for 

MinkowskiEngine compilation)

pytorch >= 1.7 (pytorch 1.8.1 + CUDA 11.X is unstable. To specify CUDA version, please use conda for installation. conda install -y -c conda-forge -c pytorch pytorch=1.8.1 cudatoolkit=10.2)

python >= 3.6

ninja (for installation)

GCC >= 7.4.0

---

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

Create a `requirements.txt` file
```
cd MinkLod3D-SI
```
```
touch requirements.txt
```
copy these into the file:
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
(The above come from `pipreqs /path/to/project` and manual filtering)


### 4. Add init.py to `MinkLoc3D-SI` folder

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
module load anaconda3/2020.07
eval "$(conda shell.bash hook)"
source activate /scratch/<your_net_id>/minklocconda

export CUDA_HOME=/usr/local/cuda-11.4
export PYTHONPAH=$PYTHONPATH:/scratch/<your_net_id>/MinkLoc3D-SI

pip install -U git+https://github.com/NVIDIA/MinkowskiEngine -v --no-deps --install-option="--blas_include_dirs=${CONDA_PREFIX}/include" 
--install-option="--blas=openblas"

pip install tensorboard
pip install -r requirements.txt

source ~/.bashrc

# run the code
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

