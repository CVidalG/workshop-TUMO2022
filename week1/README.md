# Week 1 of the workshop

## Dependencies installation

Requires [Conda](https://docs.conda.io/en/latest/miniconda.html)

```bash
conda create --name TUMO-newspaper python=3.6
conda activate TUMO-newspaper
```

**If not already done, clone the repository and go into week1:**

```bash
git clone https://github.com/CVidalG/workshop-TUMO2022.git
cd week1
```

**Install dependencies**

```bash
pip install -r requirements.txt 
cd Mask_RCNN/
pip install .
```

## Download pre-trained model

[Click here to download](https://www.dropbox.com/s/dcl53rl3xqndfdx/mask_rcnn_tablebank_cfg_0002.h5?dl=1)

```bash
mkdir pre-trained
mv mask_rcnn_tablebank_cfg_0002.h5 pre-trained
```

## Launch training

```bash
cd ..
python train.py
```