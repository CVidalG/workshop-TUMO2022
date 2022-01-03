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

## Create data

## Training

```bash
cd samples/newspapers
python3 newspapers.py --dataset=/path/to/dataset --weights=coco
```

## Visualization

Create a new environment

```bash
conda create --name TUMO python=3.8
conda install -c conda-forge jupyterlab
pip install nbconvert==5.6.1
conda install matplotlib
```

and launch :

```
Mask-RCNN-TF2-3.0/samples/newspapers/inspect_newspapers_data.ipynb
```
