# OIC-136_Microglia
Analysis of microglia activation states in zebrafish

From Request:
>I would like to request the following analyses of microglia from z-stack images of 3 days post-fertilization zebrafish:
>
> 1. Cell numbers
>
> 2. Morphology (quantification of roundness or cellular projections)
>
> 3. Fluorescence instensity

## Analysis Approach
Using the cle (gpu accelerated image analysis package) and scikit-image python packages to segment and quantify micoglia.

Voxel size of dataset:

`voxel_size_x = 0.301
voxel_size_y = 0.301
voxel_size_z = 4.55`

Large Z-step will give more 'boxy' looking cells, may inhibit identification of active and inactive states of microglia.

### Progress meeting 5-8-2025
Reviewed current segmentation results for analysis pipeline. Some parameter adjustments are needed, either for detection of the original signal or post-processing of the objects to remove false detections. Also need to verify if the measurements are in pixels or to scale.

Margarita will send me more data from another project using the same marker to make sure the pipeline is generalized enough.

Margarita also has this same microglia line in mCherry, which may help the SNR and overall segmentation results. 

Discussed acquiring a finer z-step for the volumes to improve segmentation and measurements.

Images to keep:
- Control_MO [02, 04, 05, 06, 07]
- GNAS_MO_1ng [00, 01, 04]
- GNAS_MO_2ng [00, 03, 07, 08]

