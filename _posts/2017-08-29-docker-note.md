---
title: Docker Note
date: 2017-08-29 16:14:31
---

Docker as a new technique is becoming more and more popular in development. In the following passage, I will write down some very basic note about docker, just in case I might forget them in the future.

#### Terminology
1. Docker image: it is kind of similar to a "installation CD", you can pack up all your app requirement in an image and then you can install your app anywhere. Docker also provides an image registry called [Docker Hub](https://hub.docker.com). There are lots of images contribute by developers. You can download your needed image for convinence.
 
2. Docker container: container is different to virtual machine. Containers can share a single kernel and they are isolated based on process. As the picture showed, multiple container are running on the same operating system, but they are independently running.
![](/images/docker_note/container_architecture.png)

3. Dockerfile: Dockerfile is the necessary file to build an image. It's like a configuration file which tell the docker how to build this image. The following is an example Dockerfile:
	```
	FROM tensorflow/tensorflow:1.2.1-gpu-py3

	MAINTAINER jiahao.luo2014@gmail.com

	RUN mkdir /code
	RUN mkdir /data
	RUN mkdir /data/modified_training_data
	RUN mkdir /data/modified_testing_data

	ADD ./ /code

	WORKDIR /code

	CMD CUDA_VISIBLE_DEVICES=3 python claim_train.py python claim_eval.py
	```

#### Basic command
```
sudo service docker restart
sudo docker search ubuntu # search image named ubuntu
sudo docker login
sudo docker tag image username/repository:tag
sudo docker pull username/repository:tag
sudo docker run -it ubuntu:14.04 /bin/bash
sudo docker images
sudo docker rmi <image_name> 
sudo docker rm <container_id>
```

#### Working flow
The following picture shows the working flow to train and test a deep learning model with Docker as an example.
![](/images/docker_note/flow.png)

#### For more information:
[Docker tutorial](https://docs.docker.com/get-started/)
