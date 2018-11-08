# DensePoseDocker
How to set up Densepose with Docker file 
You can refer to main version of the [`detectron/GETTING_STARTED.md`](https://github.com/facebookresearch/Detectron/blob/master/GETTING_STARTED.md).

**Requirements:**
- NVIDIA GPU, Linux, Python2

## Setting Up Docker
This is a modifed and extended version of the blog post by [`Trung Tran`](https://chunml.github.io/ChunML.github.io/project/Installing-NVIDIA-Docker-On-Ubuntu-16.04/)

Remove old versions of Docker

```
$ sudo apt-get remove docker docker-engine docker.io
```
If you don't have it, above line will tell you.

```
$ sudo apt-get update
```

Installing any missing packages
```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

Next, add the official GPG key of Docker:
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
```
It must match with **9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88**
```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
```
$ sudo apt-get update && apt-get install docker-ce
```
check if Docker is installed correctly
```
sudo docker run hello-world

```
It should write :Hello from Docker! 

### Setting Up NVIDIA-DOCKER
First, you need to remove NVIDIA docker 1.0
```
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
sudo apt-get purge -y nvidia-docker
```
If this gives permission error like **Got permission denied while trying to connect to the Docker daemon socket**
```
sudo usermod -a -G docker $USER
```
Then, close the terminal and open a new one. If 
```
sudo docker run hello-world

```
Still fails, then **reboot**
After reboot, it should work and continue with
```
sudo apt-get purge -y nvidia-docker
```
If it may say Unable to locate package nvidia-docker
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/ubuntu16.04/amd64/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
```
```
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
```
Now, you have to install right version of NVIDIA
```
docker run --runtime=nvidia --rm nvidia/cuda:9.0-cudnn7-devel nvidia-smi
```
It should prompt the output of nvidia-smi

### Setting Up Densepose

I did not download the Dockerfile at their official page [`facebook/densepose`](https://github.com/facebookresearch/DensePose/blob/master/INSTALL.md)
Instead I used [`garyfeng/densepose`](https://github.com/garyfeng/DensePose).
```
docker pull garyfeng/densepose
```
You should have around 10GB free space
To check it
```
df -h
```
It should prompt :
```
Status: Downloaded newer image for garyfeng/densepose:latest
```
Next, run  with. This is to run for each session. 
```
nvidia-docker run --rm -it garyfeng/densepose bash
```
This command will start garyfeng/densepose virtualmachine and run bash command
Try if it passes the tests
```
python detectron/tests/test_spatial_narrow_as_op.py
python detectron/tests/test_batch_permutation_op.py
```
For further commands refer to [`facebook/densepose`](https://github.com/facebookresearch/DensePose/blob/master/INSTALL.md)
If you want to train a model or test with pretrained models, you must link COCO and Densepose Dataset while starting docker.
The commands are stated in their Install.md

### Docker Commands

These are some useful commands for playing docker image 
This to save changes with a new tag
```
docker commit $(docker ps --last 1 -q) densepose:yourtag
```


