# OIC-136_Microglia

Conda environment can be recreated with [yaml file](/CellPoseCLE.yaml)

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

### Progress meeting 5-23-2025

Met with Margarita, Jacquelyn, and Hannah to review using a Jupyter notebook, the analysis pipeline options (discussed moving forward with CellPose), and measurements for this project.

Action Items:

- add in filters to remove any objects touching the border of the image and any very small objects that may not be real cells
- Switch to using the measurement module from scikit-image to extract more intuitively named measurements
- Include shape measurements like volume, skeleton length, and solidity
  - Found a Python package [SKAN](https://skeleton-analysis.org/stable/getting_started/index.html) that may be useful for quantifying the skeleton length and getting a sholl analysis; supports 3D analysis
  - Volume and solidity can be calculated by [scikit-image](https://scikit-image.org/docs/dev/api/skimage.measure.html#skimage.measure.regionprops)
- Make a good tutorial for Margarita to follow to use the Jupyter notebook on her own
  - Reach out to Zack R. to get the proper conda envs and kernels set up to use in JupyterLab on the HPC

### Project update 6-26-2025

Added training to CellPose's cyto 3 with the goal of training a 2D-based model using individual slices and then relying on post-processing stitching to create the 3D objects. I used Napari to create ground truth labels for 4 images (variable background). Napari label layer can only create up to 256 label IDs, so I labeled all microglia with the same label value and then used skimage to relabel with unique ID label values. CellPose is incredibly picky about the structure of the images and labels. Had to use numpy's squeeze function on the split stacks and label images, then save as tiffs to then push through the CellPose network. I used a script segment from one of the StarDist notebooks to randomly shuffle and split the images and labels into test and training groups. However, this caused a mismatch between the labels and images (very concerning for stardist work, will need to verify). This resulted in a poorly trained model before I realized what the issue was.

Decided to use the CellPose GUI to load in image slices and their correct corresponding labels (previously created using Numpy and scikit-image) and save them as .npy files (the CellPose GUI will save them in the format it likes). I did this in a semi random manner to add training in small batches to avoid over training. The resulting model worked surprisingly well! I was then able to incorporate this new model into the existing [Microglia_Cellpose Notebook](/Microglia_CellPose.ipynb) and run the segmentation and quantification script.
