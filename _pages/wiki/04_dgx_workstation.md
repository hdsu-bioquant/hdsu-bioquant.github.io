---
title: "HDSU - Wiki"
layout: wiki
excerpt: "HDSU -- Wiki"
sitemap: false
permalink: /wiki/dgx_workstations/
---

# DGX Workstation

## Guide on how to use our NVIDIA GPU workstation

## First steps

For access to the workstation, please contact Daria: daria.doncevic@bioquant.uni-heidelberg.de

Once your access has been approved, a user account will be set up for you:

e.g. if your Name is Max Mustermann, your USER ID will be bq_mmustermann

You will also receive an initial password that you will have to change after your first login.

To log in onto the station, type:
```
ssh bq_mmustermann@cln-dgx.bioquant.uni-heidelberg.de
```
The workstation is integrated in the BioQuant network, but does not belong to the BioQuant cluster. As long as you have access to the BioQuant network, you should in principle also have access to the workstation.

Your home folder will be located at:
```
/home/bq_mmustermann
```
Storage capacity in this folder is limited, so every user has their own working folder in another directory, which you can access under:

```
/raid/mmustermann
```

In this folder, you should store your data, scripts, analysis results and so on. The workstation is basically intended for people that want to use the GPU, so please do not store data here that you do not want to process using GPUs, because that would only clutter up the storage space.


## Working on the GPU station


On this workstation, we work exclusively in docker environments. This has two main reasons:

1) It is not that easy to properly configure an environment that is GPU enabled, and popular applications like rapids or pytorch already provide ready-to-use environments that have been extensively tested.  
2) The storage space on the work station is limited, and the docker environments can be shared by all users.


#### a) Retrieving/modifying docker images

When pulling GPU enabled docker images, one has to make sure that they have the right CUDA version installed.  
You can run ```nvidia-smi```on the cln-dgx and check its output:

Here you can see that our workstation has four GPUs available and that the CUDA version is 10.1

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.165.02   Driver Version: 418.165.02   CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla V100-DGXS...  On   | 00000000:07:00.0 Off |                    0 |
| N/A   33C    P0    35W / 300W |     27MiB / 32475MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  Tesla V100-DGXS...  On   | 00000000:08:00.0 Off |                    0 |
| N/A   32C    P0    36W / 300W |      0MiB / 32478MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   2  Tesla V100-DGXS...  On   | 00000000:0E:00.0 Off |                    0 |
| N/A   33C    P0    37W / 300W |      0MiB / 32478MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   3  Tesla V100-DGXS...  On   | 00000000:0F:00.0 Off |                    0 |
| N/A   33C    P0    36W / 300W |      0MiB / 32478MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      2111      G   /usr/lib/xorg/Xorg                             9MiB |
|    0      2140      G   /usr/bin/gnome-shell                          15MiB |
+-----------------------------------------------------------------------------+
```

For example, if you want to obtain the docker image for pytorch, you can run:
```
docker pull pytorch/pytorch:1.6.0-cuda10.1-cudnn7-runtime
```
Again, make sure it has the right CUDA version!

The image for pytorch has already been pulled, so feel free to use it.

To see all docker images which are currently available, just type:

```
docker images

REPOSITORY          TAG                               IMAGE ID            CREATED             SIZE
pytorch             lightning                         5bc730d29aab        7 days ago          4.21GB
rapids              with_dask-optuna                  12ba7b2ada2a        2 months ago        8.23GB
pytorch             with_torch-sparse                 2a1b1a5cd7f7        2 months ago        3.74GB
rapidsai/rapidsai   cuda10.1-base-ubuntu16.04-py3.8   b02068c722e8        3 months ago        8.21GB
pytorch/pytorch     1.6.0-cuda10.1-cudnn7-runtime     6a2d656bcf94        6 months ago        3.47GB
```

The two images at the bottom of the output are images that were pulled from the official docker repositories.
The three images at the top are based on these images and have some packages installed on top of them. In fact this is the way I would recommend you when you need your own libraries. First pull an official GPU-enabled docker image (or use the ones we already have on the machine), start the container, then install your packages on top of it and export the container as a new image.

To do this, you need to run the following steps:

```
docker run -it pytorch/pytorch:1.6.0-cuda10.1-cudnn7-runtime /bin/bash # start the container
pip install pytorch-lightning # install the packages you need
...
exit # exit the container
docker ps -a # list all running docker containers

CONTAINER ID        IMAGE                                           COMMAND             CREATED             STATUS                     PORTS               NAMES
59eae84dde9b        pytorch/pytorch:1.6.0-cuda10.1-cudnn7-runtime   "/bin/bash"         9 seconds ago       Exited (0) 6 seconds ago                       youthful_wright
```

From this list, you can see the Container ID.
Then run:

```
docker commit [Container ID] [Name]:[Tag]
docker commit 59eae84dde9b pytorch:lightning # example
```
to create the new image, which will now also be available when you run ```docker images```

If you do not need the container anymore, don't forget to run

```
docker rm [Container ID]
```

#### b) Working in docker containers

You can start a docker container with the following command:

```
docker run --gpus all -p 8888:8888 --rm -ti -v /raid/ddoncevic/projects/:$HOME pytorch:lightning
```

The flag ```--gpus```will specify how many GPUs the container will have access to, e.g. if you set ```--gpus 1```, the container will only be able to recognize and use 1 GPU, although we have 4 GPUs on the machine.

You have to be careful here, because by default, e.g. if you set ```--gpus 1```, docker will always use the first GPU. If some GPUs are currently in use, make sure to check with the ```nvidia-smi``` command which GPUs still have memory available, and then explicitly specify the GPU like this:

```
docker run --gpus device=1 all -p 8888:8888 --rm -ti -v /raid/ddoncevic/projects/:$HOME pytorch:lightning
```

The GPUs are 0-indexed, so ```device=0``` will use the first GPU, ```device=1``` the second GPU and so on.

The flag ```-p```is used for port-forwarding. If you want to output anything on your local machine, e.g. you want to work with the tensorboard or a jupyter notebook, you will have to use port forwarding when starting the container. Be careful that you also have to have already forwarded the port you want to use when connecting to the workstation via ssh.

The flag ```--rm```is optional. If this flag is set, then the container will automatically be deleted after you exit from the container.

The flag ```-v```is used to mount a folder to the container environment. This is neccessary, as otherwise you will not have access to your data and scripts from within the container.

At the end, you need to specify the docker image that the container should be started from.


If you want to work in a Jupyter notebook, make sure Jupyter is installed in your environment and then type the following in your running container instance:

```
jupyter notebook --port=8888 --ip=0.0.0.0 --allow-root --no-browser
```

This will open jupyter notebook in your browser. To be able to run this, you have to make sure that you have already forwarded the respective port when starting up the container.


### 3) Questions/Remarks

If you have questions about this document, please ask Daria (daria.doncevic@bioquant.uni-heidelberg.de).  

If you have additional remarks, let me know! If you think something else should be included here, please do so!



