# Week 1 of the workshop

## Installation

### Clone repository and go to the week 1


```bash
git clone https://github.com/CVidalG/workshop-TUMO2022.git
cd week1
```

### Create an environment and install Mask-RCNN

```bash
conda create --name TUMO-train python=3.6
conda activate TUMO-train
cd Mask-RCNN-TF2-3.0
pip install -r requirements.txt
python3 setup.py install
```

## Building a custom dataset

The first step consists in building a dataset of persons in Newspapers.

We will use data from the [Fundamental Scientific Library of the National Academy of Sciences of the Republic of Armenia](https://www.flib.sci.am/index.php/en/knowledge/).
In particular, we will use the [Periodicals collection](https://arar.sci.am/dlibra/results?q=&action=SimpleSearchAction&type=-6&p=0&qf1=collections:10) of the ARAR Pan-Armenian Digital Library they have released.

* Step 1 : download some pdf from ARAR

```
You have to choose ~ 50 PDF from different sources / dates / names (up to 300 pages)
```

* Step 2 : convert pdf into images

```
You can use Automator to create a converter pdf2jpg, or you can also install the python library imagemagick.
```

* Step 3 : Set a max-height of 1000px for all the images

```
You can use Automator to create a converter img2img, or you can also use imagemagick.
```

* Step 4 : Annotate your data

For annotating our dataset, we will use our [annotator tool v1.0.6](../annotator).

<img src="../assets/annotation.gif" style="width: 80%;" alt="annotation process" class="inline"/>


## Training

```bash
cd samples/newspapers
python3 newspapers.py train --dataset=/path/to/dataset --weights=coco
```

## Data Visualization

Create a new environment

```bash
conda create --name TUMO-viz python=3.8
conda activate TUMO-viz
conda install -c conda-forge jupyterlab
pip install nbconvert==5.6.1
conda install matplotlib numpy
```

Start a JupyterNotebook:

```bash
jupyter notebook
```

and launch :

```
Mask-RCNN-TF2-3.0/samples/newspapers/inspect_newspapers_data.ipynb
```

## Prediction

We are in the ```week1``` folder.

```bash
conda create --name TUMO-pred python=3.7
conda activate TUMO-pred
pip install -r requirements.txt
python3 setup.py install
python -m pip install Keras==2.3.1 tensorflow==2.1.0
pip install scikit-image==0.14.2
conda install -c conda-forge jupyterlab
```

Start a JupyterNotebook:

```bash
jupyter notebook
```

and launch :

```
Mask-RCNN-TF2-3.0/samples/newspapers/inspect_newspapers_model.ipynb
```
