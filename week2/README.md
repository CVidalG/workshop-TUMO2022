# Week 2 of the workshop

# Table of contents

1. [Just for fun](#bonus)

2. [Task 1 - Introduction to a multi-class problem](#task1)

    1. [Building a custom dataset](#dataset)

    2. [Training (TUMO-train)](#training)

    3. [Prediction (TUMO-pred)](#prediction)

3. [Task 2 - Live Demo and Video detection](#task2)
4. [Task 3 - Just for fun - Webcam Live Demo](#task3)



## 1. Just for fun - Apply your last newspaper model to a street view image <a name="bonus"></a>

```bash
conda activate TUMO-pred
jupyter notebook
```

And use the notebook ```demo.ipynb```, similar to the one we have use during week1 to run your last model on new images (it's in Mask-RCNN-TF2-3.0/samples/).

> What do you think about the results?


## 2. Task 1 - Introduction to a multi-class problem<a name="task1"></a>

<p align="center">
<img src="assets/lesson-general-support-week2.jpeg" width="80%"/>
</p>


### Building a custom dataset<a name="dataset"></a>

The first step consists in building a customized dataset with at least two different objects to recognize (up to 4).

You can use data of your choice (images from internet, images from the [Fundamental Scientific Library of the National Academy of Sciences of the Republic of Armenia](https://www.flib.sci.am/index.php/en/knowledge/), or you can also use the **dataset we provide below**.

===========
Google Drive with some data : [access to Data](https://drive.google.com/drive/folders/1HtCaCthql443kRR-bAD2RjarF7wHqSst?usp=sharing)
===========


1. Download your images in a folder ```my_dataset_week2```.
2. Resize images to a height of 1000px.
3. Split into 3 folders (train, val, test).
4. Annotate images with the objects of your choice, with the **polygon** function.

**For annotating your dataset, we will use our [annotator tool v2.0.11](../annotator).**


5. Export annotations as json and save the json file into each folder (train, val, test) with the name ```via_region_data```.


### Training (TUMO-train)<a name="training"></a>

```bash
conda activate TUMO-train
```

Open the file:
```
Mask-RCNN-TF2-3.0/samples/custom/custom.py
```

Update the classes according to your dataset:

```line 85```: add as much classes as necessary, with the name you have used during your annotation.
```python
self.add_class("object", 1, "name1") #for instance : name = car
self.add_class("object", 2, "name2") #for instance : name = people
#self.add_class("object", 3, "name3") #uncomment if you have more than 2 classes
#self.add_class("object", 4, "name4") #uncomment if you have more than 3 classes
```

and around ```line 124```, add correct informations:
```python
name_dict = {"name1": 1, "name2": 2} #if you have more than 2 classes, don't forget to add name3 and name4
```

And finally, you can launch training:

```bash
cd Mask-RCNN-TF2-3.0/samples/custom
python3 custom.py train --dataset=/absolute/path/to/your/dataset --weights=coco
```

In case of errors:

```bash
conda install -c conda-forge opencv 
```


### Prediction (TUMO-pred)<a name="prediction"></a>

```bash
conda activate TUMO-pred
pip install pycocotools
jupyter notebook
```

And open the notebook ```custom_model.ipynb``` (Mask-RCNN-TF2-3.0/samples/custom).


## 3. Task 2 - Live Demo and Video detection<a name="task2"></a>


**Choose some newspapers and street views**

We are working with:
**TUMO-pred** and **demo.ipynb**

To download all images from Notebook, please follow the solution of Andranik:

> By tednaaa, follow me in github [tednaaa](https://github.com/tednaaa)

```js
const images = document.querySelectorAll('.output_subarea');
const names = [];
const links = [];

images.forEach(({children}) => {
  const image = children[0]
  
  if (image.textContent) return names.push(image.textContent.replace("\n", ""));

  links.push(image.src)
})

const correctNames = names.splice(4, names.length)

links.forEach((link, index) => {
  const name = correctNames[index]
  const linkElement = document.createElement('a')
  linkElement.download = name
  linkElement.href = link
  document.body.appendChild(linkElement)
  linkElement.click()
})
```


To create GIF file, add in your notebook demo :

```python
import glob
from PIL import Image
def make_gif(frame_folder):
    frames = [Image.open(image) for image in sorted(glob.glob(f"{frame_folder}/*.png"))]
    frame_one = frames[0]
    frame_one.save("my_awesome.gif", format="GIF", append_images=frames,
               save_all=True, duration=600, loop=0)
```

and then run :

```python
make_gif("/video/output/path/with/your/predicted/images")
```

If doesn't work, install Pillow in TUMO-pred:

```bash
python3 -m pip install Pillow
```

## 3. Task 3 - Just for fun - Webcam Live Demo<a name="task3"></a>

We are working with:
**TUMO-pred** and **demo.ipynb**

In demo.ipynb, run cells to load your paths and your model.

**ATTENTION:**

> If you want to use your **custom model**, be sure to **import custom, to have custom functions** in the code (import custom, path-to-custom, custom.CustomConfig, etc) and to import only the required classes in the **same order** of your training parameters.

> If you want to use your **newspaper model**, be sure to **import newspapers and to have newspapers functions** in the code (import newspapers, path-to-newspapers, newspapers.NewspaperConfig, etc) and to import only the required classes in the **same order** of your training parameters.


Then add and run the following cells:

```python
import cv2
```

```python
def random_colors(N):
    np.random.seed(1)
    colors = [tuple(255 * np.random.rand(3)) for _ in range(N)]
    return colors
```

```python
colors = random_colors(len(class_names))
class_dict = {
    name: color for name, color in zip(class_names, colors)
}
```

```python
def apply_mask(image, mask, color, alpha=0.5):
    """apply mask to image"""
    for n, c in enumerate(color):
        image[:, :, n] = np.where(
            mask == 1,
            image[:, :, n] * (1 - alpha) + alpha * c,
            image[:, :, n]
        )
    return image
```

```python
def display_instances(image, boxes, masks, ids, names, scores):
    """
        take the image and results and apply the mask, box, and Label
    """
    n_instances = boxes.shape[0]

    if not n_instances:
        print('NO INSTANCES TO DISPLAY')
    else:
        assert boxes.shape[0] == masks.shape[-1] == ids.shape[0]

    for i in range(n_instances):
        if not np.any(boxes[i]):
            continue

        y1, x1, y2, x2 = boxes[i]
        label = names[ids[i]]
        color = class_dict[label]
        score = scores[i] if scores is not None else None
        caption = '{} {:.2f}'.format(label, score) if score else label
        mask = masks[:, :, i]

        image = apply_mask(image, mask, color)
        image = cv2.rectangle(image, (x1, y1), (x2, y2), color, 2)
        image = cv2.putText(
            image, caption, (x1, y1), cv2.FONT_HERSHEY_COMPLEX, 0.7, color, 2
        )

    return image
```

```python
capture = cv2.VideoCapture(0)
capture.set(cv2.CAP_PROP_FRAME_WIDTH, 300)
capture.set(cv2.CAP_PROP_FRAME_HEIGHT, 300)
```

**And then run the following code to launch your webcam and perform live detection with newspaper samples available in the room.**

```python
while True:
        ret, frame = capture.read()
        results = model.detect([frame], verbose=0)
        r = results[0]
        frame = display_instances(
            frame, r['rois'], r['masks'], r['class_ids'], class_names, r['scores']
        )
        cv2.imshow('frame', frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

capture.release()
cv2.destroyAllWindows()
```



In case of errors, install opencv in TUMO-pred:

```bash
conda install -c conda-forge opencv 
```

