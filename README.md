# YOLOv4_SHVN
Street View House Numbers detection

This repository is related to the [Selected Topics in Visual Recognition using Deep Learning Homework 2](https://docs.google.com/presentation/d/1uPEuvBi3gyX7tq4MvuPp3d5JePWJa9rmVqfMcDrm4Mg/edit?usp=sharing).


SVHN dataset contains 33,402 trianing images, 13,068 test images

Train a not only accurate but fast digit detector!

## Environment
use the Colab GPU

## Data Preprocessing
see data_preprocess.py , or open it on Colab
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1RuxMFvekwhziB--v54F-6brp2BdqPIVq?usp=sharing)

Due to the limitation of Colab, reading the large number of files in a single folder will cause Time out error.

I distributed 33402 photos to folder no.0~16. 

* Folder no.0~14 (30000 images) are used for training,

* folder no.15~16 (3402 images) are used for validation.


I extracted the annotation digitStruct.mat into normal data annotations.
and generate txt file that recorded bbox infomation for each image

```
work1/svhn_HW2
        ├── split_train
        │     ├── 0   ├── 1.png
        │     │       ├── 1.txt
        │     │       │  .
        │     │       │  .
        │     │       │  .
        │     │       ├── 2000.png
        │     │       └── 2000.txt
        │     │  .     
        │     │  .     
        │     │  .
        │     └── 14  ├── 28001.png
        │             ├── 28001.txt
        │             │  .
        │             │  .
        │             │  .
        │             ├── 30000.png
        │             └── 30000.txt
        └── split_train
              ├── 15  ├── 30001.png
              │       ├── 30001.txt
              │       │  .
              │       │  .
              │       │  .
              │       ├── 32000.png
              │       └── 32000.txt
              └── 16  ├── 32001.png
                      ├── 32001.txt
                      │  .
                      │  .
                      │  .
                      ├── 33402.png
                      └── 33402.txt

```
## Getting started
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1mkPeyi8yzdhkvIJb29P_kWZWNkVgKbN8?usp=sharing)

please use colab to open the file "inference.py"
just execute this file on colab,

it will automatically load the model and reproduce submission file



## Training 
see svhn_yolov4.py , or open it on Colab
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1r4tfoWkZZJrGEVIUI72L6FJzK7y_m9K2?usp=sharing)

if you want to try this file, please change the file path in code

setting of .cfg file:
* batch=64 and subdivisions=16
* width=416 and height=416
* flip=0, because no digit numbers are reversed.
* max_batches = 20000 (number of classes x 2000)
* mosaic=0
* calculated the anchors by using command. 
  ./darknet detector calc_anchors data/obj.data -num_of_clusters 9 -width 416 -height 416 -show


Reference:
https://github.com/NCTUwayne/SVHN-YoloV4
https://github.com/StephenEkaputra/SVHN-YOLOV3-CUSTOM


## Summary:
I use Colab to train YOLOv4, it takes about 1~2 days to train once. 

Although my accuracy is not good enough, but the speed is not bad.

I think setting image size larger is better, and I don’t recommend to use data augmentation in this task.

To predict a large number of pictures at once, Darknet has two methods:

(1)	 ./darknet detector test … . 
Put the paths of images you want to predict into the same txt file, and use this txt file as input.
The result will be stored in the txt file you specified. But these value of the result score , width , position x y …are integer number, Not a floating point number, so it is not very precise.
(2)	./darknet detector valid … .
It predict validation data list which is defined in your data cfg file. So what you can do is simply modify your data cfg file to point to your own batch of images. 
It outputs are much cleaner compared to ./darknet detector test. And have exact value of the result.
	
  Even though the outputs of method (2) are more precise. 
  
  But I don't have time to process the files generated by method (2). 
  
  Although the results of method (1) are not accurate enough, they are relatively easy to process into answer.json files.
  


## Future work:

If there is time, I will try to use method (2) to generate anser.json to see if it will improve the performance

Also, If there is time, I will try to use different threshold and try to use different pretrained weight to train the model.
