# A subcortical segmentation tool based on anatomy attentional fusion network for developing macaques

<div align=center><img width="614" height="599" src="https://github.com/TaoZhong11/nBEST/blob/main/Subcortical_SDM/SDM_overview.png"/></div>


Considering that most macaque MRI samples usually contain T1w but not necessarily T2w, the model disclosed here supports inputting only T1w, and then generates corresponding SDM, subcortical region, and six types of subcortical fine structures(**thalamus, caudate, putamen, pallidum, hippocampus, and amygdala**).

<div align=center><img width="820" height="417" src="https://github.com/TaoZhong11/nBEST/blob/main/visual_result.png"/></div>


## How to run

### Try your data ###
Create a directory, e.g., named ```Macaque_testing_scans```, which will be mounted to the container. The T1w scans that need to be processed should be in ```Macaque_testing_scans/data/xxxxx.nii.gz```. 
Run by a new terminal:
```
docker run -it --gpus=all --ipc=host -v /absolute/path/to/Macaque_testing_scans:/workspace/demo/  wxyabc/nbest:1.3
cp nBEST_subcortical_SDM.py demo/nBEST_subcortical_SDM.py
cd demo
```
If your data are raw data with skull, run
```
python nBEST_subcortical_SDM.py
```
And if you want to process your data without brain extraction (which means your input should be brain images without skull),  run
```
python nBEST_subcortical_SDM.py -skip_be
```

### Output illustration ###
Upon completion of the processing, all resulting outputs will be generated and stored in the ```Macaque_testing_scans``` directory. The following explains what the results are: 
* ```brain_mask/```: The brain mask of raw image.
* ```brain_img/```: The skull stripped brain image.
*  ```brain_subcortical_6class/```:	The subcortical fine structures.
