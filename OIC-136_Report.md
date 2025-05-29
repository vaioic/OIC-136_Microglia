# OIC-136 Zebrafish Microglia
### Summary of Request
From Request:
>I would like to request the following analyses of microglia from z-stack images of 3 days post-fertilization zebrafish:
>
> 1. Cell numbers
>
> 2. Morphology (quantification of roundness or cellular projections)
>
> 3. Fluorescence intensity

#### Brief summary of analysis pipeline
Python-based analysis pipeline that normalized images using py-clesperanto package, CellPose to segment microglia and SciKit-Image to measure fluorescence intensities and morphology metrics (volume, surface area, and sphericity). 

Custom CellPose model to improve segmentation and skeleton length measurements will be added once data collection approach is fine tuned.

### Data
3D images of zebrafish brains were collected on the OIC's Andor Spinning Disk Confocal Dragonfly 620.

Voxel size: 4.5 um, 0.3 um, 0.3 um

Three conditions: control morpholino (MO) (n=8), *gnas*-MO 1ng (n=8), and *gnas*-MO 2ng (n=9)

Challenges of dataset: large z-spacing gives boxy objects in z that are less representative of the microglia; high background in some images gives a low signal to noise ratio that is challenging to correct for.

### Analysis Pipeline

To correct changes in illumination in Z and to increase the signal to background ratio, images were normalized by the mean intensity of the image and then a white top hat filter with radius 10 was applied.

Example image before normalization and background subtraction:

![](snapshots\gnas2ng_05_BeforeNorm_XZ.png)

Example image after normalization and background subtraction:
![](snapshots\gnas2ng_05_AfterNorm_XZ.png)


### Output