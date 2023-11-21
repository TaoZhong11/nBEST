<div>
<img src="nBEST_logo.jpg" align="left" style="margin: 10 10 10 10;" height="150px">
	<h1>nBEST</h1>
<blockquote> An accurate, adaptable, and automated AI pipeline called non-human primates Brain Extraction and Segmentation Toolbox (nBEST) for processing NHPs from multi-species, multi-site, and multi-developmental stages.
</blockquote>
</div>
<br />
<img src="multi_species_logo.jpg">

<hr />

# New features and updates :star:

11/20/2023: **Subcortical structures** 🐒 \
nBEST now can provide the segmentation of subcortical structures for macaques. See the details and instructions [here](Subcortical.md).



# Introduction
This ```README``` illustrates how to install and run the Docker version of the nBEST pipeline. We aim to continually update it via lifelong learning using newly added data. This approach guarantees that nBEST will remain pertinent and effective as new data becomes available, providing a reliable resource. The nBEST works out-of-the-box without any retraining, and is also robust to:
* more than ten species, including macaque, chimpanzee, marmoset, etc.
* raw scans with or without any preprocessing: bias field correction, re-orientation, normalization, etc.
* iso-intensity scans.

![](https://github.com/TaoZhong11/nBEST/blob/main/nBEST_pipeline.jpg)

![](https://github.com/TaoZhong11/nBEST/blob/main/Result.jpg)

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
docker pull wxyabc/nbest:1.3
```

After downloading, you can use ```docker images``` to see the container images you have downloaded.

# Running the pipeline container


## Run the pipeline
### Demo ###
Some examples from multi-species NHPs have been provided in ```demo/data```. Run the demo and see results in ```demo/``` by a new terminal:
```
docker run -it --gpus=all --ipc=host wxyabc/nbest:1.3
cd demo
python nBEST.py
```
### Try your own T1w raw data ###
Create a directory, e.g., named ```Macaque_testing_scans```, which will be mounted to the container. The T1w scans that need to be processed should be in ```Macaque_testing_scans/data/xxxxx.nii.gz```. Run by a new terminal:
```
docker run -it --gpus=all --ipc=host -v /absolute/path/to/Macaque_testing_scans:/workspace/demo/  wxyabc/nbest:1.3
cp nBEST.py demo/nBEST.py
cd demo
```
If you want to process your data with all three steps (i.e., brain extraction, cerebrum&brainstem removal, and tissue segmentation), run
```
python nBEST.py
```
And if you want to process your data without brain extraction (which means your input should be brain images without skull),  run
```
python nBEST.py -skip_be
```

### Output illustration ###
Upon completion of the processing, all resulting outputs will be generated and stored in the ```Macaque_testing_scans``` directory. The following explains what the results are: 
* ```brain_img/```:	The skull stripped brain image.
* ```brain_mask/```: The brain mask of raw image.
* ```brain_cerebrum/```: The cerebrum (remove cerebellum and brainstem from brain image) image.
* ```brain_cerebrum_mask/```: The cerebrum mask of raw image.
* ```brain_tissue/```: The cerebrum tissue segmentation.

# Feedback
We are committed to continuous improvement and value your feedback. To help us enhance our processes, we kindly request you evaluate the processing results by downloading and completing ```nBEST_evaluate.xlsx```. The results are categorized into three levels: Good, Fair, and Poor. Detailed descriptions for each level are provided within the table. Your insights are crucial in helping us and we await your valuable feedback.

Additionally, we welcome the sharing of any brain MRI data from NHPs to continuously update our toolbox. We will strictly adhere to the relevant data usage protocols and provide data contributors with both processed results and manual corrections, leveraging the expertise of our seasoned professionals. We look forward to connecting!

# Contacts
For questions/bugs/feedback, please contact:

Tao Zhong, Ph.D., taozh2315@gmail.com\
School of Biomedical Engineering\
Southern Medical University, China

