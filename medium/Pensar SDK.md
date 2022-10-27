
![](https://miro.medium.com/max/933/1*6AWXdmz23ZuYcyTVFAzNtw.png)

# Pensar SDK: A Rapid Artificial Intelligence Application Development Framework on the Edge Device

A Software Development Kit (SDK) for rapid development of Artificial Intelligence (AI) based Computer Vision (CV) application

> **Note:**  A demo is presented at the end of the article

In the past days, I have been using and testing  [Pensar SDK](https://pensarsdk.com/), a Software Development Kit (SDK) for rapid development of AI-based Computer Vision (CV) application. It allows the development of AI-based Computer Vision application that can easily be deployed on Pensar, an embedded camera powered by NVIDIA Jetson TX2 that integrates a SONY FullHD camera with 20x optical zoom and a FLIR thermal camera up to 640x480.

![](https://miro.medium.com/max/933/1*f0Q89WXGRVCtJeYJ-XvpfQ.png)

Source: The official website [https://pensarsdk.com/](https://pensarsdk.com/)

> Pensar is developed by  [Aerialtronics](https://www.aerialtronics.com/en/products/pensar), a manufacturer of unmanned aerial vehicles, commonly known as drones, for aerial photography and videography.

In this article, I provide an overview and walk you through the development of an object detection application using Pensar SDK.

This article is organized as follow:

-   [Rapid Artificial Intelligence Application Development framework](https://medium.com/swlh/pensar-sdk-1-647f778bc11#rapid-artificial-intelligence-application-development-framework)
-   [Why Pensar SDK?](https://medium.com/swlh/pensar-sdk-1-647f778bc11#why-pensar-sdk)
-   [Features of Pensar SDK](https://medium.com/swlh/pensar-sdk-1-647f778bc11#features-of-pensar-sdk)
-   [The concept of Pensar SDK](https://medium.com/swlh/pensar-sdk-1-647f778bc11#the-concept-of-pensar-sdk)
-   [Application Pipeline](https://medium.com/swlh/pensar-sdk-1-647f778bc11#application-pipeline)
-   [Demo: Development of an Application for Object Detection](https://medium.com/swlh/pensar-sdk-1-647f778bc11#use-case-development-of-an-application-for-object-detection)

## Why Pensar SDK?

The domain of Artificial Intelligence is increasingly expanding and has already started disrupting many industries. New deep learning models and use-cases are proposed on a daily basis. The amount of data is flourishing thanks to the availability of the data-generator devices, smartphones, IoT, drones, robots…. On the other side, companies like NVIDIA are pushing the boundaries of computing devices by developing powerful computing (a.k.a, Graphical Processing Unit: GPU) which are faster, smaller, energy-efficient, and cheaper.

One of the most important fields where AI is making a big step is Computer Vision. It is considered one of the most disrupting fields in today’s world. It started little by little shifting many technologies across industries, from self-driving cars, smart city, video surveillance to robotics, transportation, defense and security, and more. To have an idea, here is a link on the latest workshop from the Computer Vision Foundation listing advanced research in the field:  [CVPR 2019 Workshops](http://openaccess.thecvf.com/CVPR2019_workshops/menu.py)

If you are a data scientist working on an AI-based computer vision solution and have experience on deploying these models on an edge device (e, g, smart camera), you probably agree with me that this is time (and money) consuming and is no fun at all. Building your development environment from scratch could be a nightmare, especially when the edge device has multiple components. If you don’t strategically plan your solution, you end up wasting too much time setting up a workable development environment, instead of focusing on unlocking information from your data and developing a fast and accurate solution for your business.

Practically speaking, not every business is able to do that for a lack of skill and knowledge. As a result, Pensar SDK is created to offer a resilient development and deployment solution for business. Pensar SDK is the first R(ai)AD, Rapid artificial intelligence Application Development framework. It is shipped with Pensar, an intelligent dual-camera based on the NVIDIA Jetson TX2. Pensar SDK’s mission is to save time and help businesses to easily and rapidly develop an AI-based computer vision application.

## Features of Pensar SDK

Pensar SDK is more than a list of compiled libraries. It includes many features, among which:

-   A complete ready-to-use application framework
-   Enables deep learning inference at the edge
-   Plug and play pre-trained deep learning models (Latest versions of YOLO (v2, v3, and v2-tiny), ResNet, MobileNetSSD, GoogleNet, Dlib FHogFaceDetector, Dlib Frontal Face Detector, Dlib ANetFaceEncoder, adapted for fast inference on GPU).
-   Pensar SDK is designed for real-time processing and to work on a live stream from sensors but it also supports offline processing from different sources.
-   Compatible with Linux-based desktop machine (AMD64) and edge device (ARM64) powered with NVIDIA Jetson TX1/TX2
-   It supports the most common Deep Learning and Computer Vision frameworks: OpenCV, Caffe model parser, TensorRT…
-   Integrate most used programming languages: Python / C++ / Kivy
-   The possibility to easily collect your data on the fly
-   and more features

## The concept of Pensar SDK

In the figure below I illustrate the workflow of development and deployment of an AI-based computer vision application using Pensar SDK

![](https://miro.medium.com/max/864/1*QRn-1k9t08ePod2K5DOFjA.png)

Pensar SDK is installed on:

-   The host machine: from which the developer develop the application
-   On the Edge device: Pensar SDK needs to be installed on the edge device to ensure a compatible and an easy deployment

**Note**: Please note that Pensar SDK has no tool to help you train your neural network. To do that you can use a tool such as Amazon AWS, Google cloud platform, Azure …

## Application Pipeline

The figure below illustrates the application development pipeline:

![](https://miro.medium.com/max/884/1*d-JFXLZhv3jfPi7sXJU-zg.png)

## Preprocessor pipeline

Here the trained deep learning model is defined. The user can either use the installed one or import his trained model. The preprocessor takes care of all the hardware acceleration and optimization of AI/CV libraries

## Plugin pipeline

This is the core of the application that the user must configure and personalized. It is a plugin-based structure, where the preprocessed video passes through each one of them.

## Demo: Development of an Application for Object Detection

At the input, the application takes a video captured by the edge device (the camera Pensar of Aerialtronics) and outputs the same video with an overlay from the inference.

The figure below illustrates the application development pipeline:

![](https://miro.medium.com/max/884/1*Imwq2Yl844JfSUNRsMlUEQ.png)

Let’s see how this is done in practice on my laptop computer. I have Ubuntu 18.04 64-bits installed and NVIDIA GPU GeForce GTX 1660 Ti (I know it is not the best :)).

## Create the Application Folder

By running the script  `new_app_skeleton.sh`, a new application folder is created with the name you enter. The folder is not empty, it contains a template application file that you have to edit and adapt to your application.

The application folder is created with a path

~/pensarsdk/applications/my_app

## Launch VS Code from Inside the Development Environment

To launch Visual Studio Code, simply enter in the terminal

code .

## Configure the Application

We now have to configure the application. To do that simply edit the file  `my_app.xml`, particularly, the  `preprocessor_pipeline`  and the  `plugins_pipeline`.

## Create your Personalized Plugins

You can either use the predefined plugins or define yours. On the left of  `VS Code`  you find a list of all installed plugins under  `Framework Plugins`, among which  `pre_processor_inference.py`. You can also define your personalized plugins, in our case,  `filter.py`  and  `visualize.py`.

-   `filter.py`:

-   `visualize.py`:

## Edit the GUI (if needed)

Say we want to personalize the GUI of the application, for instance, I want to create a button to turn on/off filtering and add a spinner to select the label of the object of interest. To do that you need to create and edit a kivy file for each desired plugin, in our case  `filter.py`  and  `visualize.kv`.

-   `visualize.kv`:

-   `filter.kv`

## Run the Application

To run the application, two choices are possible, either:

-   use the debug mode by pressing F5 then again F5 to launch the application.
-   Or, you can run the application directly from the application folder  `run_pensar.sh -a my_app.xml`

A GUI should pop-up in full screen running the created application

![](https://miro.medium.com/max/833/1*TcGng3smXvoWlDVybSE-ww.gif)

**Note**: This GUI is created and adapted for Aerialtronics camera, Pensar, notice the setup for the twin cameras. Thanks to Pensar SDK, we can develop and test the application on the host machine before deployment on the camera.

## What to do next?

-   Let me know what you think in the comment section and/or direct message me on  [LinkedIn](https://www.linkedin.com/in/aminehy/).
-   Visit the official website  [https://pensarsdk.com/](https://pensarsdk.com/)  to learn more about this solution.
-   You are a professional and you are interested in Pensar SDK solution, please feel free to contact Aerialtronics staff here.

**Here is the list of my past articles on** [**medium**](https://medium.com/@Amine_hy)**:**

-   [Have you Optimized your Deep Learning Model Before Deployment?](https://towardsdatascience.com/have-you-optimized-your-deep-learning-model-before-deployment-cdc3aa7f413d)
-   [Deep Learning for image classification w/ implementation in PyTorch](https://towardsdatascience.com/convolutional-neural-network-for-image-classification-with-implementation-on-python-using-pytorch-7b88342c9ca9)