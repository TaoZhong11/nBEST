
# Introduction
A convenient, precise, and fully automated AI pipeline called non-human primates Brain Extraction and Segmentation Toolbox (nBEST) for preprocessing NHPs from multi-species, multi-site, and multi-developmental stages. We aim to continually update it via lifelong learning using newly added data. This approach guarantees that nBEST will remain pertinent and effective as new data becomes available, providing a reliable resource.
![](https://github.com/TaoZhong11/nBEST/blob/main/nBEST_overview.jpg)

# System requirement
Since this is a *Linux* based container, please install the container on a Linux system. The supported systems include but not limited to `Ubuntu`, `Debian`, `CentOS` and `Red Hat`. 

The pipeline is developed based on deep convolutional neural network techniques. A GPU (≥4GB) is required to support the processing. 

# Installation
### Install Docker
Please refer to the official installation guideline [Install Docker on Linux](https://docs.docker.com/desktop/install/linux-install/). You can follow the below commands to install the Docker on the popular Ubuntu Linux system. If you have previously installed Docker on your computer, please skip this step.
```
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release
sudo apt mkdir -p /etc/share/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/share/keyrings/docker.gpg
sudo echo "deb [arch=$(dpkg --print-architechture) signed-by=/etc/share/keyrings/docker.png] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
Since the docker needs the `sudo` to run the general commands, to run without `sudo`, you need to add your user name `${USER}` to the docker group, which can be done
```
sudo group add docker
sudo usermod -aG docker ${USER}
```
After running these commands, you may need to restart your computer to make the configuration take effect. One easy way to check whether you have successfully installed the Docker or not is to run the Docker using the command ```docker info```, which dose not need ```sudo``` requirement if you have added your username to the `docker` group. Please refer to [more details](https://docs.docker.com/get-started/) for potential issues on Docker.


### Install Nvidia-docker
The ```Nvidia-docker``` is required since our pipeline needs GPU support. Please refer to the official installation guideline [Install Nvidia-docker on Linux](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker). If you have previously installed Nvidia-docker, please skip this step. The following commands can be used to install the Nvidia-docker on the popular Ubuntu Linux system.
```
sudo apt update
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-docker.gpg
distribution=$(. /etc/os-release; echo ${ID}${VERSION_ID})
curl -sL https://nvidia.github.io/nvidia-docker/libnvidia-container/${distribution}/libnvidia-container.list | sed 's#deb https://# deb [signed-by=/usr/share/keyrings/nvidia-docker.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt update
sudo apt install -y nvidia-docker2
```
After the installation, please restart the Docker daemon to complete the installation after setting the default funtime:
```
sudo systemctl restart docker
```
Finally, you can check whether you have successfully installed the ```Nvidia-docker``` using the following command:
```
docker run --rm --gpus=all nvidia/cuda:9.0-base nvidia-smi
```
If succeeded, the output should be the GPU card information on your PC. 

### Download the pipeline

Run 
```
docker pull wxyabc/nbest:1.0
```

After downloading, you can use ```docker images``` to see the container images you have downloaded.

# Running the pipeline container
## Get the free license
The container is totally free. Please first contact taozh2315@gmail.com to get a free license. Then create a folder (License), and put the ```License.txt``` into this folder.

## Run the pipeline
### Demo ###
Some examples from multi-species NHPs have been provided in ```demo/data```. Run the demo and see results in ```demo/```:
```
docker run -it --gpus=all --ipc=host wxyabc/nbest:1.0
```
### Try your own T1w raw data ###
Create a directory ```data_folder```, which will be mounted to the container. Run:
```
docker run -it --gpus=all --ipc=host -v /datafolder:/workspace/demo/data -v /License:/workspace/demo/License wxyabc/nbest:1.0 cd /demo python preprocess_all.py
```

### Output illustration ###
After the processing is finished, in the "mounted" folder ```data_folder```, all the processing results will be generated. The following explains what the results are: 
* ```brain_img/```:	The skull stripped brain image.
* ```brain_mask/```: The brain mask of raw image.
* ```brain_cerebrum/```: The cerebrum (remove cerebellum and brainstem from brain image) image.
* ```brain_cerebrum_mask/```: The cerebrum mask of raw image.
* ```brain_tissue/```: The cerebrum tissue segmentation.


