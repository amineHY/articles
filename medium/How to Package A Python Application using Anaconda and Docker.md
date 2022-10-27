
# How to Package A Python Application using Anaconda and Docker

## Quick tutorial for packaging Python project using Anaconda with Docker

![](https://miro.medium.com/max/700/1*ttkvEjwbf0GSYG2zdWflvA.png)

[https://unsplash.com/photos/cOkpTiJMGzA](https://unsplash.com/photos/cOkpTiJMGzA)

# 

Introduction

I rarely use anaconda for my python projects, but recently it happened to me to rely on a python package which is available only via Anaconda. Therefore, I had to set my project using  _conda_  environment. However, I encountered a problem packaging my python application using Docker, because of the  _conda_  virtual environment and I didn’t find much documentation to solve my problem. I finally found a solution and I share it with you here in this article.

_The Source codes for this article are available on GitHub below:_

[](https://github.com/amineHY/tuto-python-anaconda-docker)

## amineHY/tuto-python-anaconda-docker

### A project to quickly setup a python project using anaconda packages and Docker GitHub is home to over 50 million…

github.com

## Project Structure

The project structure for this article is the simple. Four files are necessary :

## Create a YAML file for Conda Environment

Create a file with  _.yaml_  extension:

_environment.yaml_

environment.yaml

-   Name your conda environment, for instance  `app`
-   Add a list of  _channels_  where the desired packages are hosted
-   List packages you want to install such as flask, gunicorn, numpy… under  _dependencies_
-   You can also install pip packages from the YAML file. One way to it is by adding a pip command to install (pip) packages from a  `requirements.txt`, (-r in the command stands for requirement)

For instance:

_requirements.txt_

requirements.txt

## Create a Python Script

Here we have created a test file  `main.py`  for testing the application.

_main.py_

main.py

# Dokerization

Create a file named  `Dockerfile`  (without extension) from which we build a docker image for our Python application.

_Dockerfile_

-   For simplicity we start from the base image of  `miniconda3` (line1)
-   Copy the files from the folder to docker (line 6)
-   Create a conda environment and install the packages defined in YAML file (line 9)
-   Edit the shell command so we can launch python scripts from within the newly created conda environment (line 11)
-   Define an entry-point for the docker image such that it execute the  `main.py` python script of the application (line 13)

## Build a docker image

We build a docker image, named  `app`, with the following commands

docker build --tag myapp .

… then wait for docker to build an image for your app (it might tight a take for downloading and update it the packages list).

**Note:**

1.  The  `Dockerfile`  must be located in the root directory of the application
2.  Make sure  `[Docker](https://www.docker.com/products/docker-desktop)`  is installed on your machine ([www.docker.com](https://www.docker.com/)).

## Run the docker image

To run the App from the docker image we created

docker run --rm -ti myapp 

This should outputs:

Hello World !  
Numpy version is  1.19.4

Done.

# Conclusion

In this tutorial I walked you through a quick tutorial on how to package your python application using Docker and leveraging python package from pip and anaconda together. The key solution is in the SHELL command of Dockerfile. Without it Docker can not run the code inside the conda environment.

I hope it will benefit you.

Peace.

# Resources

[](https://github.com/amineHY/tuto-python-anaconda-docker)

## amineHY/tuto-python-anaconda-docker

### A project to quickly setup a python project using anaconda packages and Docker GitHub is home to over 50 million…

github.com

[](https://www.python.org/)

## Welcome to Python.org

### The official home of the Python Programming Language

www.python.org

[](https://www.anaconda.com/)

## Anaconda | The World’s Most Popular Data Science Platform

### The modern world of data science is incredibly dynamic. Every day, new challenges surface — and so do incredible…

www.anaconda.com

[](https://www.docker.com/)

## Empowering App Development for Developers | Docker

### Docker offers free plans for individual developers and teams just starting out. We also have new monthly plans for…

www.docker.com

PS: If you encounter a problem please create an issue on GitHub or comment below.