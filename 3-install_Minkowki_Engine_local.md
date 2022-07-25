# Install Minkowki Engine 0.5.4 on Local Ubuntu Machine

Taking the local implementation of this model as an example: https://github.com/jac99/MinkLoc3Dv2

## 0 create Conda environment

```
conda create -n minkowski python==3.8
conda activate minkowski
```

Install pytorch 1.10.1, install command [here](https://pytorch.org/get-started/previous-versions/)

Then install
```
pip install ninja
```
Check gcc
```
gcc --version
```

## 1 Install Minkowski Engine 0.5.4 using

```
pip install -U git+https://github.com/NVIDIA/MinkowskiEngine -v --no-deps --install-option="--blas_include_dirs=${CONDA_PREFIX}/include" 
--install-option="--blas=openblas"
```
Now Minkowske Engine is installed. Below are other stuff for the example repo.

## 2 Set up other stuff

```
pip install pytorch_metric_learning
pip install pandas
pip install wandb
pip install tensorboard
pip install numba
pip install setuptools==59.5.0
```

## 3. Export Python path

cd into the repo folder, get absolute path using `pwd` command, copy path. Then open

```
nano ~/.bashrc
```
or for others, 
```
nano ~/.zshrc
```
export path like this.
```
export PYTHONPATH=$PYTHONPATH:/home/.../MinkLoc3Dv2
```
And source
```
source ~/.bashrc
```
or 
```
source ~/.zshrc
```
## 4 Download Dataset and generate queries

extract benchmark dataset outside of the example repo folder.

run generate queries in `pointnetvlad`, move the pickle files into benchmark dataset folder

```
python generate_training_tuples_baseline.py --dataset_root /home/silvey/Documents/GitHub/benchmark_datasets
```

## 5 Run
### Train
```
python train.py --config ../config/config_baseline.txt --model_config ../models/minkloc3dv2.txt
```

If the below error occurs, 
```
  File "/root/anaconda3/envs/minklocv2/lib/python3.8/site-packages/torch/utils/tensorboard/__init__.py", line 4, in <module>
    LooseVersion = distutils.version.LooseVersion
AttributeError: module 'distutils' has no attribute 'version'
```
run
```
pip install setuptools==59.5.0
```




