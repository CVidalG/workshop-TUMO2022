# Week 1 of the workshop

## Installation

### Clone repository and go to the week 1


```bash
git clone https://github.com/CVidalG/workshop-TUMO2022.git
cd week1
```

### Create a environment and install Mask-RCNN

```bash
conda create --name TUMO python=3.6
conda activate TUMO
cd Mask-RCNN-TF2-3.0
pip install -r requirements.txt
python3 setup.py install
```

### Install JupyterLab

```bash
conda install -c conda-forge jupyterlab
```

## Create data

## Training

```bash
cd samples/newspapers
python3 newspapers.py --dataset=/path/to/dataset --weights=coco
```