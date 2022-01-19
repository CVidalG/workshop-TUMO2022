# Week 1 of the workshop

# Table of contents

1. [Installation](#installation)

2. [Building a custom dataset](#dataset)

3. [Understand the impact of hyper-parameters](#parameters)

4. [Data Visualization](#visualization)

5. [Training](#training)

6. [Prediction](#prediction)

## Installation<a name="installation"></a>

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

# If error try : 
xcode-select --install
# Else continue

python3 setup.py install
```

## Building a custom dataset<a name="dataset"></a>

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

* Step 4 : Split your images into three folders : train (with approximately 70% of your dataset), val (20%) and test (10%)

* Step 5 : Annotate your data

For annotating your dataset, we will use our [annotator tool v1.0.6](../annotator).
Please annotate each subset independently.

<img src="../assets/annotation.gif" style="width: 80%;" alt="annotation process" class="inline"/>

## Understand the impact of hyper-parameters<a name="parameters"></a>

The goal is to discover parameters you can use to control and optimize the learning.
* Learning rate
* Batch size
* Dataset size
* Number of layers
* Number of hidden layers
* Number of epochs

Challenge **level 1** : [Access to the challenge](http://playground.tensorflow.org/#activation=tanh&batchSize=10&dataset=gauss&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=&seed=0.77737&showTestData=false&discretize=false&percTrainData=50&x=false&y=false&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false)

Challenge **level 2** : [Access to the challenge](http://playground.tensorflow.org/#activation=tanh&batchSize=10&dataset=circle&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=&seed=0.65267&showTestData=false&discretize=false&percTrainData=50&x=false&y=false&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false)

Challenge **level 3** : [Access to the challenge](http://playground.tensorflow.org/#activation=tanh&batchSize=10&dataset=xor&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=&seed=0.38987&showTestData=false&discretize=false&percTrainData=50&x=false&y=false&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false)


## Data Visualization<a name="visualization"></a>

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
Mask-RCNN-TF2-3.0/samples/newspapers/inspect_newspapers_data-student.ipynb
```


## Training<a name="training"></a>

```bash
cd samples/newspapers
python3 newspapers.py train --dataset=/path/to/dataset --weights=coco
```


## Prediction<a name="prediction"></a>

```
activate your TUMO-pred environment
```

Start a JupyterNotebook:

```bash
jupyter notebook
```

and launch :

```
Mask-RCNN-TF2-3.0/samples/newspapers/inspect_newspapers_model.ipynb
```
