# Using T1w and T2w MRI to obtain the segmentation of subcortical structures
We require a pair of aligned T1w and T2w MRIs to achieve this process.
Now this beta version only supports Macaque species, which have been validated in both cynomolgus and rhesus.


Overall the subcortical structures were delineated into 6 classes, including **thalamus, caudate, putamen, pallidum, hippocampus, and amygdala**.

<div align=center><img width="820" height="417" src="https://github.com/TaoZhong11/nBEST/blob/main/visual_result.png"/></div>


## How to run

### Try your data ###
Create a directory, e.g., named ```Macaque_testing_scans```, which will be mounted to the container. The T1w and T2w scans that need to be processed should be in ```Macaque_testing_scans/T1w_T2w_img/xxxxx_0000.nii.gz```(T1w) and ```xxxxx_0001.nii.gz```(T2w). 
Run by a new terminal:
```
docker run -it --gpus=all --ipc=host -v /absolute/path/to/Macaque_testing_scans:/workspace/demo/  wxyabc/nbest:1.3
cp nBEST_subcortical.py demo/nBEST_subcortical.py
cd demo
```
If your data are raw data with skull, run
```
python nBEST_subcortical.py
```
And if you want to process your data without brain extraction (which means your input should be brain images without skull),  run
```
python nBEST_subcortical.py -skip_be
```

### Output illustration ###
Upon completion of the processing, all resulting outputs will be generated and stored in the ```Macaque_testing_scans``` directory. The following explains what the results are: 
* ```brain_mask/```: The brain mask of raw image.
* ```T1w_T2w_img_brain/```: The skull stripped brain image.
*  ```brain_subcortical/```:	The subcortical structures.
