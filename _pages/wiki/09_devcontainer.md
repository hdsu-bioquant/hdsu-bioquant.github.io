# Working in devcontainer

Working on visual studio code became best practice, due to the ease to integrate several languages at once (e.g. python and R) and several script modalities (e.g. python scripts and Jupyter notebooks). In addition, in visual studio code, debugging is made very easy, just as working with GitHub. 

In the following, we are going to show how to mount visual studio code in a docker, on the cluster. By this means, you are going to be able to develop in an environment that is going to ensure compatibilities between your packages at all time, and to give you the computing power of the clusters. 

In short, working comfortably on a powerful machine. 

I assume for the following that you already have visual studio code installed, as well as access to the DGX workstation. I also suggest that you create an automatic connection from your machine to the cluster by creating keys (keygen) and by modifying the .bashrc of your local machine. Keyword for this procedure is called SSH key exchange authentication. You can google how to do it, it's classic. 

Note that if you have access to GPU1 and GPU2 as well (lucky you), you can follow the exact same procedures as the one described below. In particular you can keep the content of the .devcontainer exactly the same. It is not cluster dependent. At the time of writing, the rights granted to the workspaces on the cluster did not allow to mount the container from vsCode. When you read this, it should be solved. Otherwise, ask for help. The problem is known. 

# Connect to DGX via vsCode

Open vscode. 

In vscode, connect to the cluster by hitting the button on the left bottom corner:

<img src="../../images/devcontainer_section/01.png" style="display: block; margin: 20px auto; width: 200px;"/>

And hit **Connect to host:**

<img src="../../images/devcontainer_section/02.png" style="display: block; margin: 20px auto; width: 800px;"/>

If you have created an automatic connection in .bashrc it will appear automatically.

<img src="../../images/devcontainer_section/03.png" style="display: block; margin: 20px auto; width: 800px;"/>

Otherwise you will need to enter user@host e.g. for me:

```python
[bq_jradig@cln-dgx.bioquant.uni-heidelberg.de](mailto:bq_jradig@cln-dgx.bioquant.uni-heidelberg.de)
```

And **then enter your password**.

A new window opens.

<img src="../../images/devcontainer_section/04.png" style="display: block; margin: 20px auto; width: 800px;"/>

You see you are connected to DGX. 

<img src="../../images/devcontainer_section/05.png" style="display: block; margin: 20px auto; width: 200px;"/>

# Create .devcontainer

Now, we are going to create two files, that are going to automatically create a docker - a docker is a bit like a conda environment, but that is saved on a kind of virtual machine. That means, regardless of whether you are using Mac, linux or windows (why would use the two latter even?), if everyone works on that docker, everyone can run the code. It's like a machine stored on a cloud. It is not though. You can google. Good luck to understand - and launch it on the cluster. 

The two files are going to be stored in a folder, that is going to be called .devcontainer (convention). It is going to contain two files: devcontainer.json (convention) and Dockerfile (needed, caps lock important). 

Go to your workspace:

<img src="../../images/devcontainer_section/06.png" style="display: block; margin: 20px auto; width: 800px;"/>

Create the folder .devcontainer and create two files called devcontainer.json and Dockerfile inside them.

<img src="../../images/devcontainer_section/07.png" style="display: block; margin: 20px auto; width: 800px;"/>

<img src="../../images/devcontainer_section/08.png" style="display: block; margin: 20px auto; width: 200px;"/>

## devcontainer.json

In devcontainer.json, copy paste:

```python
{
    // Name of the development environment. Please change
    "name": "my_env",

    // Build configuration for the container
    "build": {
        // Specifies the Dockerfile to use for building the container
        "dockerfile": "Dockerfile"
    },

    // Additional arguments to pass when running the container
    "runArgs": [
        // Name of the container. Please change
        "--name", "jradig",

        // Enable GPU support for the container
        "--gpus", "all"
    ],

    // Configuration for mounting the workspace
    "workspaceMount": "source=${localWorkspaceFolder},target=/workspace,type=bind",
    
    // Specifies the folder inside the container to mount the workspace
    "workspaceFolder": "/workspace",
    
    // Customizations for VS Code inside the container
    "customizations": {
        "vscode": {
            // List of VS Code extensions to install in the container
            "extensions": [
                "ms-python.python",            // Python extension for VS Code
                "ms-python.vscode-pylance",    // Pylance extension for Python language support
                "ms-toolsai.jupyter",          // Jupyter support in VS Code
                "ms-python.black-formatter"    // Black formatter for Python code
            ]
        }
    }
}
```

Please modify the docker name 

<img src="../../images/devcontainer_section/09.png" style="display: block; margin: 20px auto; width: 400px;"/>

and the image name 

<img src="../../images/devcontainer_section/10.png" style="display: block; margin: 20px auto; width: 400px;"/>

Such that we know who did what. 

The following line is the most important:

<img src="../../images/devcontainer_section/11.png" style="display: block; margin: 20px auto; width: 400px;"/>

It mounts all the files that you have on your cluster's workspace to the docker, in a folder that is called workspace. Hence all the files you have on the cluster will be accessible from the container. **Attention**, if you create folders from within the container (it will become clear at some point), you won't be able to scp to or from them, since you have created them from the container, and not from the cluster. If you want to scp stuff, create a folder on the cluster, and then open the project in the container. You will understand once you encounter the issue. Keep this warning in mind. 

## Dockerfile

The dockerfile specifies the image (kind of the virtual machine, though not quiet) that is going to be used. I was doing deep learning, so I pulled the image of pytorch. The image ensures there is no compatibility issues between your hardware and software (essentially, you can run stuff on the GPUs without problem). Additionally, you download the terminal widget you like (like nano, vim, unzip and so on) and the packages that you want to have installed (for example scanpy, pandas and else). The content of my docker file looks like (please copy paste and adjust): 

```python
FROM pytorch/pytorch:1.13.1-cuda11.6-cudnn8-runtime

# Install sudo and other system packages
RUN apt-get update && \
    apt-get install -y \
    sudo \
    git \
    htop \
    less \
    tmux

# Install required python packages
RUN pip install \
    ipython \
    ipykernel \
    black \
    goatools \
    scanpy \
    neptune \
    seaborn \
    colorcet \
    ray
```

Note that even once built, you can install more packages by running pip on the container terminal. It's like working in a normal machine. 

# Building the devcontainer

We have everything set to create the devcontainer. 

Hit cmd + shift + P (I hope you are using Mac). And hit “Dev Containers: Rebuild and Reopen in Container”.

 

<img src="../../images/devcontainer_section/12.png" style="display: block; margin: 20px auto; width: 800px;"/>

It will load everything, it takes a couple of minutes at the beginning maybe. Hit **Allow**.

<img src="../../images/devcontainer_section/13.png" style="display: block; margin: 20px auto; width: 800px;"/>

You are in the devcontainer! Hurray!

<img src="../../images/devcontainer_section/14.png" style="display: block; margin: 20px auto; width: 800px;"/>

<img src="../../images/devcontainer_section/15.png" style="display: block; margin: 20px auto; width: 200px;"/>

The terminal looks like this and you can use it like normal:

<img src="../../images/devcontainer_section/16.png" style="display: block; margin: 20px auto; width: 400px;"/>

See, all the files you had on your cluster's workspace are now inside the container under the folder workspace. 

I can for instance run a python file from the terminal, like usual:

<img src="../../images/devcontainer_section/17.png" style="display: block; margin: 20px auto; width: 400px;"/>

I can create a Jupyter notebook, just select the default kernel. 

<img src="../../images/devcontainer_section/18.png" style="display: block; margin: 20px auto; width: 400px;"/>


<img src="../../images/devcontainer_section/19.png" style="display: block; margin: 20px auto; width: 800px;"/>

<img src="../../images/devcontainer_section/20.png" style="display: block; margin: 20px auto; width: 800px;"/>

<img src="../../images/devcontainer_section/21.png" style="display: block; margin: 20px auto; width: 200px;"/>

And run it as usual.

<img src="../../images/devcontainer_section/22.png" style="display: block; margin: 20px auto; width: 200px;"/>

You can import .py files in your Jupyter notebooks and all other stuff that you can usually do. 

But the most interesting thing, is that you can debug! Never used debugging? Google. In vscode you need to install an extension, easy. Then you can do e.g. the following.

<img src="../../images/devcontainer_section/23.png" style="display: block; margin: 20px auto; width: 800px;"/>

It returns an error, let's debug. Set a flag (red dot, just click on the left of the line numbers to the line you wish the code to stop) and hit the run button with the bus on the left bottom corner:

<img src="../../images/devcontainer_section/24.png" style="display: block; margin: 20px auto; width: 800px;"/>

Hit run and debug. Actually I suggest that you create the launch.json file they propose to automatically write for you. This will ensure that, each time you hit debug, it debugs the file that is currently open. 

<img src="../../images/devcontainer_section/25.png" style="display: block; margin: 20px auto; width: 800px;"/>

Hence it does this step automatically:

<img src="../../images/devcontainer_section/26.png" style="display: block; margin: 20px auto; width: 800px;"/>

You enter debug mode and you can debug as usual (look YouTube videos to understand how to debug. Don't wonder whether it's worth the time. It is.)

<img src="../../images/devcontainer_section/27.png" style="display: block; margin: 20px auto; width: 800px;"/>

# Coming back to the devcontainer after having logged out

At the end of this tutorial, at some point, you are going to go home, and the connection to the cluster will be lost. You will have to reopen your work environment the next day. We are the next day. You reboot your computer. How to go back to your devcontainer and continue working? Follow me. 

Open vsCode. 

 

<img src="../../images/devcontainer_section/28.png" style="display: block; margin: 20px auto; width: 800px;"/>

Connect to DGX

<img src="../../images/devcontainer_section/29.png" style="display: block; margin: 20px auto; width: 800px;"/>

<img src="../../images/devcontainer_section/30.png" style="display: block; margin: 20px auto; width: 800px;"/>

You were too lazy to create the key-authentification yesterday? You can still do it now.

The new window opens, you are connected to DGX. 

<img src="../../images/devcontainer_section/31.png" style="display: block; margin: 20px auto; width: 200px;"/>

Click the TV icon:

<img src="../../images/devcontainer_section/32.png" style="display: block; margin: 20px auto; width: 200px;"/>

<img src="../../images/devcontainer_section/33.png" style="display: block; margin: 20px auto; width: 200px;"/>

Click on the drop-down menu “Remotes (Tunnels/SSH)”

<img src="../../images/devcontainer_section/34.png" style="display: block; margin: 20px auto; width: 600px;"/>

Select Dev Containers

<img src="../../images/devcontainer_section/35.png" style="display: block; margin: 20px auto; width: 600px;"/>

You can see all the devcontainers that were created, in particular yours (mine is bq_jradig jradig_dock)

<img src="../../images/devcontainer_section/36.png" style="display: block; margin: 20px auto; width: 600px;"/>

I want to go to my workspace and I click the arrow.

<img src="../../images/devcontainer_section/37.png" style="display: block; margin: 20px auto; width: 600px;"/>

I am back in my devcontainer

<img src="../../images/devcontainer_section/38.png" style="display: block; margin: 20px auto; width: 800px;"/>

<img src="../../images/devcontainer_section/39.png" style="display: block; margin: 20px auto; width: 400px;"/>

Hurray! 

You can open each folder in a separate vscode window, as you would do usually. You shouldn't be encountering any more problems. If you do, first go ask chatgpt. If it fails, ask google. If it fails, you are in trouble.