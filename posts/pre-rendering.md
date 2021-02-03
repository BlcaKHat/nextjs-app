---
title: "Train your own custom model for Helmet Detection(object detection) using YOLO."
date: "2018-12-25"
---

I went through a lot of posts explaining object detection using different algorithms. but, somewhere I still feel the gap for beginners who want to train their own model to detect custom object. learning comes with doing. this post is for true beginners who have just started their journey in machine learning.I know a little and I believe there is always something to contribute.

I have came across different object detection algorithms. YOLO is one the fastest to train and detect different types of objects. machine learning is half science and half art. it’s not only about coding. we need to think about the scenario where we’re going to use it. specially when it comes to image, there are numerous factors that affect the accuracy of object detection model. it may be intensity of light, camera angle from where image/video is being captured, background of images and a lot. so the first step is to identify the problem and think about it’s use cases. once we’re done with it we can proceed training model.

**training your own custom object detection.**
![img1](https://github.com/BlcaKHat/yolov3-Helmet-Detection/blob/master/test_out/img3.jpg)

for training our custom object detection model, we will need a lot of images of objects which we’re going to train. nearly a few thousand. more number of images means more accuracy. I will suggest to collect data from real scenario rather downloading from google. collecting real images(in case of images) is really helpful. it’s like physics problems we performed in our schools. we use to calculate for ideal conditions. but in real life it doesn’t happen. we feel a lot of outside forces that affect the quality of machine. so, collect as much as possible images from real scenario.
Data-set preparation
For data preparation we need to use some tool to mark object in the image. YOLO have it’s own format for training data.
Yolo format is:

 **<object-class> <x> <y> <width> <height>**
- <x_center> = ((X_end + X_start) / 2) / image_width
- <y_center> = ((Y_end + Y_start) / 2) / image_height
- <width> = (X_end - X_start) / image_width
- <height> = (Y_end - Y_start) / image_height

if you are using some opensource data-set, you need to convert it into this format.otherwise use this tool for marking the objects in the image. <a href=" https://github.com/AlexeyAB/Yolo_mark" >Yolo_mark</a> this is a nice tool to mark images for YOLO. this tool will create a text file for each image and the file will contain the co-ordinate of objects in the image. there is a nice explanation for it in the given link.
Setting up platform for training data.
now we are done with data-set and next is to set the platform for training data. clone the repository from this link. <a href="https://github.com/AlexeyAB/darknet">
AlexAB/Darknet</a> . After downloading this file move to darknet folder and use the make command to make it. there are a lot of parameters in Makefile like CPU, GPU,CUDNN,opencv etc. if you’re on cloud server to train your model; you can make opencv 0.
after that, make a copy of yolov3.cfg file and make the modification in the following lines.
change the batch size to 64 and subdivision to 8. if you get any memory error in future try to adjust these values according to your hardware configuration.
change the number of classes in line 610, 696 and 783. number of classes is no of objects you’re going to train.

change the size of **filter** in line number 603, 689 and 776.

we define filters in YOLOv3 as, **filter= (classes + 5) x 3.**
create a file **obj.names** which contain the names of the objects (classes) we’re going to train. each class in a new line and put it in the **directory** build\darknet\x64\data\
create a file obj.data which will contain following lines:
- classes= 2
- train = data/train.txt
- valid = data/test.txt
- names = data/obj.names
- backup = backup/

now put all image-files of your objects in the directory build\darknet\x64\data\obj\
create a file train.txt with the help of linux a command . move to terminal and navigate till data folder . from outside of data folder type
ls data/obj/*.jpg >train.txt
it will create a text file with name of all .jpg images with path from data as data/obj/abc.jpg.
Download pre-trained weights for the convolutional layers (154 MB): https://pjreddie.com/media/files/darknet53.conv.74 and it put to the directory build\darknet\x64 .
To train on Linux use command: ./darknet detector train data/obj.data yolo-obj.cfg darknet53.conv.74
on windows machine use darknet.exe instead of ./darknet.
if all went well the training will start. you can see detailed information on training like when to stop how much iteration shall i train etc at https://github.com/AlexeyAB/darknet .
training on cloud:
If you don’t own a good GPU machine, you have to train your data on cloud .matrix calculation is almost 15 times faster on GPU then CPU. and that’s all we do during training. there are a lot of cloud services which provide some free credit. register there,setup CUDA and you can start training . once done , download the model from backup folder.
you can follow this link https://medium.com/devoops-and-universe/installation-of-cuda-toolkit-on-linux-54765a3e3c7d for setting up GPU.
using the model to detect the object.
I have trained a model for helmet detection. you an download the model from my github link: https://github.com/BlcaKHat/yolov3-Helmet-Detection. the script written is simple and fast. you can set it up easily within few minutes.
Opencv implementation :
The darknet implementation for detecting objects takes a lot of time to detect the object. I have implemented a simple opencv code for it.it is much faster than darknet. you can also use it for finding specific classes and finding the co-ordinates of detected objects. just visit the link and download the necessary files . https://github.com/BlcaKHat/yolov3-Helmet-Detection
this way, you can create your model for detecting different types of objects.
feel free to contact me. ping me at <a>vijaysingh88@hotmail.com</a>
that’s all for today. happy learning.

Thanks for being here 
**Vijay Singh**