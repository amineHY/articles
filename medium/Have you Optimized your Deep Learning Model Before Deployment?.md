
![](https://miro.medium.com/max/700/1*RqwDtEJSexMeq5aymDqvKA.png)

Source:  [https://developer.nvidia.com/tensorrt#](https://developer.nvidia.com/tensorrt#)

# Have you Optimized your Deep Learning Model Before Deployment?

## 

Use NVIDIA TensorRT to optimize and speed up inference time on GPU. Illustration on an AI-based Computer Vision with YOLO.

This article is organized as follows:

-   Introduction
-   What is NVIDIA TensorRT?
-   Setup the Development Environment using docker
-   Computer Vision Application: Object detection with YOLOv3 model
-   References
-   Conclusion

# Introduction

This article presents how to use NVIDIA TensorRT to optimize a deep learning model that you want to deploy on the edge device (mobile, camera, robot, car ….).  , for instance, the intelligent double spectrum camera of Aerialtronics:  _Pensar_  [https://pensarsdk.com/](https://pensarsdk.com/)

![](https://miro.medium.com/max/700/1*kF5CrVqanC408hSvNYO1lg.png)

[https://pensarsdk.com/](https://pensarsdk.com/)

Which has onboard an NVIDIA Jetson TX2 GPU

![](https://miro.medium.com/max/700/1*QRaU5AHVxGZeKAqwpriktg.png)

[https://developer.nvidia.com/embedded/jetson-tx2-developer-kit](https://developer.nvidia.com/embedded/jetson-tx2)

## Why do I need an optimized deep learning model?

As an example, think of AI-based computer vision application, they need to process each frame captured by the camera. Thus, each frame makes a forward pass through the layers of the model to compute a certain output (detection, segmentation, classification…).

Whatever power your GPU has, we all want the number of frames per second (FPS) at the output to be equal to one at the input (example 24, 30 FPS…). This means that the GPU is processing each frame in real-time.

This notion is more than desired for Computer Vision applications that require a real-time decision, for instance, surveillance, fraud detection, or crowd counting during an event, to cite just a few.

## Pre-Deployment Optimization Workflow

Among tasks of a Data Scientist is to exploit the data and develop/train/test a neural network architecture. After the validation of the model, usually, the architecture and the model’s parameters are exported for deployment. Many ways are possible to do that, either on a cloud or an edge device. In this article, we focus on deployment on edge device (camera, mobile, robot, car…).

The workflow of deployment, generally speaking, follows the block diagram depicted in the figure below:

![](https://miro.medium.com/max/700/1*ftr5BLj4PitHbueEqS6JdQ.png)

From the exported, pre-trained, deep learning model  **->**  framework parser  **->**  TensorRT optimization  **->**  Inference on new data

# What Is NVIDIA TensorRT?

![](https://miro.medium.com/max/700/1*EWiIvB6RjwEZrlSLF1Xttw.png)

[https://docs.nvidia.com/deeplearning/sdk/tensorrt-archived/tensorrt_210/tensorrt-user-guide/](https://docs.nvidia.com/deeplearning/sdk/tensorrt-archived/tensorrt_210/tensorrt-user-guide/index.html)

> The core of NVIDIA TensorRT is a C++ library that facilitates high-performance inference on NVIDIA graphics processing units (GPUs). TensorRT takes a trained network, which consists of a network definition and a set of trained parameters, and produces a highly optimized runtime engine which performs inference for that network.
> 
> You can describe a TensorRT network using a C++ or Python API, or you can import an existing Caffe, ONNX, or TensorFlow model using one of the provided parsers.  
>   
> TensorRT provides API’s via C++ and Python that help to express deep learning models via the Network Definition API or load a pre-defined model via the parsers that allows TensorRT to optimize and run them on a NVIDIA GPU.  TensorRT applies graph optimizations, layer fusion, among other optimizations, while also finding the fastest implementation of that model leveraging a diverse collection of highly optimized kernels. TensorRT also supplies a runtime that you can use to execute this network on all of NVIDIA’s GPUs from the Kepler generation onwards.  
>   
> TensorRT also includes optional high speed mixed precision capabilities introduced in the Tegra X1, and extended with the Pascal, Volta, and Turing architectures.  
>   
> Learn more about how to use TensorRT in the [developer guide]([https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html)) and interaction with the TensorRT community in the [TensorRT forum](https://devtalk.nvidia.com/default/board/304/tensorrt/)

# Setup of a Development Environment using docker

## Docker image

For the development, we use a docker image that contains an installed version of NVIDIA TensorRT. This makes the development environment more reliable and scalable on the different operating system (Windows, Linux, macOS).

Here is an illustration of the docker positioning between the application and the GPU.

![](https://miro.medium.com/max/700/1*-wpy3yNzLK_T2valKYMdVw.png)

**Note:** If you never heard about ‘docker’ then I highly recommend you to invest in learning about it, you start here:  [https://docs.docker.com/engine/docker-overview/](https://docs.docker.com/engine/docker-overview/)

## Install Docker-CE

Head over to the official website of docker and follows the steps to install `docker-ce` (ce stands for community edition).

I am using Ubuntu 64 bit, thus, the installation link is:  [https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/](https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/)

## Install CUDA

In addition, you should have installed the latest version of  [CUDA](https://developer.nvidia.com/cuda-downloads) . It is a parallel computing platform and application programming interface model created by NVIDIA. It allows software developers and software engineers to use a CUDA-enabled GPU for general purpose processing (read more about it in  [https://developer.nvidia.com/cuda-zone](https://developer.nvidia.com/cuda-zone)).

To check if CUDA is correctly installed on you machine, just enter in the terminal

nvidia-smi

The output should look something like that (I have a NVIDIA GPU GeForce GTX 1660 Ti/PCIe/SSE2)

  
Thu Aug 1 10:43:37 2019   
+ — — — — — — — — — — — — — — — — — — —— — — — — — — — — — — -+  
| NVIDIA-SMI 430.40 **Driver Version:** 430.40 **CUDA Version:** 10.1 |  
| — — — — — — — — — — — — -+ — — — — — — — — + — — — —— — — — +  
| GPU Name Persistence-M| Bus-Id Disp.A | Volatile Uncorr. ECC |  
| Fan Temp Perf Pwr:Usage/Cap| Memory-Usage | GPU-Util Compute M. |  
|=====================+===================+===================|  
| 0 GeForce GTX 166… Off | 00000000:01:00.0 Off | N/A |  
| N/A 47C P8 4W / N/A | 994MiB / 5944MiB | 8% Default |  
+ — — — — —— — — — — — — -+ — — — — — — — — + — — — — — — — — +

## Run the docker image to use NVIDIA TensorRT

I have created a docker image that includes installation of TensorRT on top of Ubuntu along with the necessary prerequisites from NVIDIA, Python, OpenCV, …. You can pull the image directly from my personal account on  [docker hub](https://hub.docker.com/u/aminehy).

[](https://hub.docker.com/r/aminehy/tensorrt-opencv-python3)

## Docker Hub

### Welcome to my docker hub

hub.docker.com

-   First, open a terminal (ctrl+alt + t) and enter this command to pull the docker image

docker pull aminehy/tensorrt-opencv-python3

-   Enable launching GUI applications from inside the docker container bu entering the command

xhost +

-   Lastly, run the docker container with the command:

docker run -it — rm -v $(pwd):/workspace — runtime=nvidia -w /workspace -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY aminehy/tensorrt-opencv-python3:v1.1

# Computer Vision Application: Object Detection with YOLOv3

YOLO is a Real-Time Object Detection from the DarkNet project. You can read more about this project on the official website here:  [https://pjreddie.com/darknet/yolo/](https://pjreddie.com/darknet/yolo/)

![](https://miro.medium.com/max/700/1*aFeEgXtb0ar-Zrxujmu1oA.png)

[https://pjreddie.com/darknet/yolo/](https://pjreddie.com/darknet/yolo/)

In this experiment, we run YOLOv3 model on 500 images and compare the average inference time before and after optimization of the model with NVIDIA TensorRT. The images used in this experiment are from COCO dataset

[](http://cocodataset.org/#home)

## COCO - Common Objects in Context

### Edit description

cocodataset.org

## 1) Running unoptimized YOLOv3 from the DarkNet in Python

[](https://gitlab.com/aminehy/yolov3-darknet)

## Amine Hy / YOLOv3-DarkNet

### GitLab.com

gitlab.com

-   Clone the repository and run the docker image through the script docker_TensorRT_OpenCV_Python.sh I created

git clone [https://gitlab.com/aminehy/yolov3-darknet.git](https://gitlab.com/aminehy/yolov3-darknet.git)cd yolov3-darknetchmod +x docker_TensorRT_OpenCV_Python.sh./docker_TensorRT_OpenCV_Python.sh run

-   Download and unzip the test images folder in ./data/

wget [http://images.cocodataset.org/zips/test2017.zip](http://images.cocodataset.org/zips/test2017.zip)unzip test2017.zip ../test2017/

-   Download the weight file `yolov3.weights`

wget [https://pjreddie.com/media/files/yolov3.weights](https://pjreddie.com/media/files/yolov3.weights)

-   Then execute YOLOv3 python file

python darknet.py

-   **Results:**

The results should be saved in the folder `./data/results`

![](https://miro.medium.com/max/700/1*ySnKobAGy8_NFze7U0goYQ.png)

Output: The mean recognition time over 500 images is 0.044 seconds

## 2) Optimizing and Running YOLOv3 using NVIDIA TensorRT in Python

![](https://miro.medium.com/max/700/1*XM_KOVzJlT1F3sqP1Azfeg.png)

The first step is to import the model,  which includes loading it from a saved file on disk and converting it to a TensorRT network from its native framework or format. Our example loads the model in ONNX format from the  ONNX model.

> ONNX is a standard for representing deep  learning models enabling them to be transferred between frameworks. (Many frameworks such as Caffe2, Chainer, CNTK, PaddlePaddle, PyTorch, and MXNet support the ONNX format).

Next, an optimized TensorRT engine is built based on the input model, target GPU platform, and other configuration parameters specified. The last step is to provide input data to the TensorRT engine to perform inference.

The sample uses the following components in TensorRT to perform the above steps:  
-  ONNX parser: takes a trained model in ONNX format as input and populates a network object in TensorRT  
-  Builder: takes a network in TensorRT and generates an engine that is optimized for the target platform  
-  Engine: takes input data, performs inferences and emits inference output  
- Logger: object associated with the builder and engine to capture errors, warnings, and other information during the build and inference phases

[](https://gitlab.com/aminehy/YOLOv3-Darknet-ONNX-TensorRT)

## Amine Hy / YOLOv3-Darknet-ONNX-TensorRT

### GitLab.com

gitlab.com

-   Get the project from GitHub and change the working directory

git clone [https://gitlab.com/aminehy/YOLOv3-Darknet-ONNX-TensorRT.git](https://gitlab.com/aminehy/YOLOv3-Darknet-ONNX-TensorRT.git)cd YOLOv3-Darknet-ONNX-TensorRT/

-   Convert the model from Darknet to ONNX. This step will create an engine called: `yolov3.onnx`

python yolov3_to_onnx.py

-   Convert the model from ONNX to TensorRT. This step will create an engine called: yolov3.trt and use for the inference

python onnx_to_tensorrt.py

-   For this experiment, we set this parameter: builder.fp16_mode = True

builder.fp16_mode = True  
builder.strict_type_constraints = True

-   **Results:**

![](https://miro.medium.com/max/700/1*zcFERKUD21QuPYmK945eiw.png)

Output: The mean recognition time over 500 images is 0.018 seconds using the precision fp16. 

T**herefore, using NVIDIA TensorRT is 2.31 x faster than the unoptimized version!.**

## 3) Optimizing and Running YOLOv3 using NVIDIA TensorRT by importing a Caffe model in C++

![](https://miro.medium.com/max/700/1*N1LE_DRYsRdwHWJh6kkE_w.png)

-   Get the project and change the working directory

[](https://gitlab.com/aminehy/YOLOv3-Caffe-TensorRT)

## Amine Hy / YOLOv3-Caffe-TensorRT

### TensorRT for Yolov3

gitlab.com

If the folder `/Caffe` does not exist (for any reason), please download the model architecture and weights of YOLOv3 (.prototxt and .caffemodel) from Google Drive and insert them in a folder ‘Caffe’. Two options are possible, the 416 model and the 608 model. These parameters represent the height/width of the image at the input of the YOLOv3 network.

-   Download the file from the Google Drive folder:  [https://drive.google.com/drive/folders/18OxNcRrDrCUmoAMgngJlhEglQ1Hqk_NJ](http://cd%20tensorrt-yolov3/%20./docker_TensorRT_OpenCV_Python.sh%20run)
-   Compile and build the model

git submodule update — init — recursivemkdir buildcd build && cmake .. && make && make install && cd ..

-   Edit the YOLO configuration file and choose between the settings, YOLO 416 or YOLO 608, located in

~/TensorRT-Yolov3/tensorRTWrapper/code/include/YoloConfigs.h

-   You need also to take a look at the configuration file configs.h located in

~/TensorRT-Yolov3/include/configs.h

-   Create the TensorRT engine as mentioned and run YOLOv3 on a test image `dog.jpg `

# for yolov3–416 (don’t forget to edit YoloConfigs.h for YoloKernel)  
./install/runYolov3 — caffemodel=./caffe/yolov3_416.caffemodel — prototxt=./caffe/yolov3_416.prototxt — input=./dog.jpg — W=416 — H=416 — class=80 — mode=fp16

-   Once the engine is created, you can pass it as an argument

./install/runYolov3 — caffemodel=./caffe/yolov3_416.caffemodel  
 — prototxt=./caffe/yolov3_416.prototxt — input=./dog.jpg — W=416 — H=416 — class=80 — enginefile=./engine/yolov3_fp32.engine

-   **Results**:

Output: Time over all layers: 21.245 ms

S**ame observation as the previous experiment, using NVIDIA TensorRT is 2.07 x faster than the unoptimized version!.**

# Conclusion

This article presented the importance of optimizing a pre-trained deep learning model. We illustrated that on an example on object detection application for computer vision, where we obtained a speedup ratio of >2 of the inference time.

**What to do next?**

-   Let me know what you think in the comment section and/or direct message me on  [LinkedIn](https://www.linkedin.com/in/aminehy/).
-   Read my other article on medium:  [Deep Learning for image classification w/ implementation in PyTorch](https://towardsdatascience.com/convolutional-neural-network-for-image-classification-with-implementation-on-python-using-pytorch-7b88342c9ca9)

# References

-   Edge camera Pensar  [https://pensarsdk.com/](https://pensarsdk.com/)
-   CUDA:  [https://developer.nvidia.com/cuda-zone](https://developer.nvidia.com/cuda-zone)
-   docker-ce:  [https://docs.docker.com/](https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/)
-   NVIDIA TensorRT:  [https://ngc.nvidia.com/catalog/containers/nvidia:tensorrt](https://ngc.nvidia.com/catalog/containers/nvidia:tensorrt)
-   NVIDIA Container Best Practices:  [https://docs.nvidia.com/deeplearning/frameworks/bp-docker/index.html#docker-bp-topic](https://docs.nvidia.com/deeplearning/frameworks/bp-docker/index.html#docker-bp-topic)
-   TensorRT Release 19.05:  [https://docs.nvidia.com/deeplearning/sdk/tensorrt-container-release-notes/rel_19-05.html#rel_19-05](https://docs.nvidia.com/deeplearning/sdk/tensorrt-container-release-notes/rel_19-05.html#rel_19-05)