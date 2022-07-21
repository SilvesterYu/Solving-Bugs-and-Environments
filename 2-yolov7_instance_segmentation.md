# yolo v7 instance segmentation on custum dataset -- how to run

### 0. Create Conda Environment

```
conda create -n yolov7 python=3.8
conda activate yolov7
```
Install pytorch 1.11
```
conda install pytorch==1.11.0 torchvision==0.12.0 torchaudio==0.11.0 cudatoolkit=11.3 -c pytorch
```
Install other libraries
```
pip install onnx
pip install onnx-simplifier==0.3.7
pip install alfred-py
pip install timm
pip install nbnb
pip install cython
pip install mmpycocotools
pip install omegaconf
pip install "git+https://github.com/philferriere/cocoapi.git#egg=pycocotools&subdirectory=PythonAPI"
```

### 1. Clone yolo v7

```
git clone https://github.com/jinfagang/yolov7.git
cd yolov7
```

### 2. Clone detectron 2 and build

yolo v7 is built on this. Clone it **inside yolov7 folder**

```
git clone https://github.com/facebookresearch/detectron2.git
```
and build
```
python -m pip install -e detectron2
```

### 4. Prepare Dataset

In yolov7 folder, create datasets/ folder, enter the foler

```
mkdir datasets
cd datasets
```
and put your coco format dataset folder here

If you don't have a dataset, follow the below lines. If you do, ignore the below lines.

example dataset: (for windows, `import wget` and then `wget.download("xxxxx.zip")`
```
wget "https://github.com/matterport/Mask_RCNN/releases/download/v2.1/balloon_dataset.zip"
```
and unzip it. 
```
mkdir balloon_dataset
```
and put the "balloon" folder inside balloon_dataset. the foll path is `datasets/balloon_dataset/ballonn`

Convert it to coco dataset format. Follow this tutorial: https://www.youtube.com/watch?v=qej73NGDQfo&t=201s

### 5. Configs and Registering Dataset

create a folder in configs/ with a custom name. I named it `train_test_coco`

go to configs/coco-instance and copy yolomask.yaml into train_test_coco/

change the following in the new yolomask.yaml to register the dataset

1. DATASETS: TRAIN: and TEST: into your own train and test or train and val dataset folder. in my case my folders are `my_train`, `my_val`

2. CLASS: how many classes of objects are there in the dataset.

3. IMS_PER_BATCH: 8

4. OUTPUT_DIR: "output/my_inseg_output"

### 6. Run Training

make a copy of `train_inseg.py` and rename it as `my_custom_train_inseg.py`

in outputs/, create a folder named `my_inseg_output`

open file `my_custom_train_inseg.py`

put the following lines in:
```
from detectron2.data.datasets.coco import load_coco_json, register_coco_instances

def register_custom_datasets():
    # -- my data -- #
    DATASET_ROOT = "./datasets/TrainTestingCOCO"
    ANN_ROOT = DATASET_ROOT
    TRAIN_PATH = os.path.join(DATASET_ROOT, "train")
    TRAIN_JSON = os.path.join(ANN_ROOT, "train/annotations.json")
    VAL_PATH = os.path.join(DATASET_ROOT, "val")
    VAL_JSON = os.path.join(ANN_ROOT, "val/annotations.json")
    register_coco_instances("my_train", {}, TRAIN_JSON, TRAIN_PATH)
    register_coco_instances("my_val", {}, VAL_JSON, VAL_PATH)

register_custom_datasets()

```
run

```
python my_custom_train_inseg.py --config-file configs/train_test_coco/yolomask.yaml --num-gpus 0
```























