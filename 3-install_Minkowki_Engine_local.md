# Install Minkowki Engine 0.5.4 on Local Ubuntu Machine

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

