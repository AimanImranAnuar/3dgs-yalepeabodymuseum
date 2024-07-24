# 3D Gaussian Splatting at the Yale Peabody Museum
This repository is set up to collect all information related to my research on 3D Gaussian Splatting. It contains our code for the installation on our virtual machine, links to datasets we've taken and/or used for our testing of 3DGS on PostShot and SuGaR, and instructions for the SuGaR pipeline.

See this Google Doc for simplified instructions on the 3DGS pipeline or see below:
https://docs.google.com/document/d/1eFMp7qj4D0bKM1Ar1pMM6qKZqJ2u8CfUc7Zr9aq6vi4/edit?usp=sharing

# Overview

# 3DGS Pipeline
The below describes the pipeline I used. Note that this entire thing can be done in Linux alone, but I did not want to set up FFMPEG or COLMAP on the virtual machine so as not to introduce possible issues. Thanks to Jon Stephens for his code & tutorial on how to do this all. His tutorial can be found here. https://www.youtube.com/watch?v=UXtuigy_wYc

A video demonstrating this pipeline can be found here: *insert link*

**-1. SET-UP (INITIAL)**

The first step that only needs to be done once is to install the necessary drivers and packages and add the necessary variables to the path. Follow the steps in Jon Stephen’s guide for more detailed guidance. Commands for Linux VM driver installation can be found on the GitHub repo.

**0. IMAGE ACQUISITION (INITIAL)**

Acquire a video or pictures of your object, following photogrammetry best practice. Make sure it is not blurry, that you are able to capture distinct features and multiple angles, and try to use a camera that records EXIF (positional) data. Upload it to your computer.

**1. IMAGE PROCESSING (WINDOWS)**

In File Explorer, navigate to your User folder and then into the gaussian-splatting folder, and then create a new folder to store your data (data_folder). Then create two folders within that one, titled (input) and (output) without the parentheses. Input MUST be named “input”. If you have pictures of your object, simply put them in input. If you have a video, convert it to an image set using FFMPEG.

**2. STRUCTURE FROM MOTION (WINDOWS)**

Open Anaconda Prompt and navigate to your gaussian splatting directory and activate the Conda environment for Gaussian splatting.

*conda activate gaussian_splatting*

Staying in that directory (as it needs the script), apply structure-from-motion conversion.

*python convert.py -s data_folder*

Then zip your data folder and move it to the SuGaR directory of your Linux VM.

*scp data_zip.zip ~/SuGaR*

**3. GAUSSIAN SPLATTING (LINUX)**

Navigate to your SuGaR directory, and unzip your data folder.

*unzip data_zip.zip*

Then train your initial Gaussian splat. Make sure to include the backslashes at the end of files!

*python gaussian_splatting/train.py -s data_folder/ --iterations 7000 -m data_folder/output/*

**4. SURFACE GENERATION (LINUX)**

If no errors occur, train the optimized Gaussian splat.

*python train.py -s data_folder/ -c data_folder/output/ -r “density”/”sdf” --refinement_time “short”/“medium”/“long”*

The first parameter is how to refine: “density” for objects, “SDF” for scenes, and then you can choose how long to refine model.

**5. MODEL VIEWING (WINDOWS)**

Export the model’s mesh and ply from the Linux machine to the Windows machine.

*In Linux: zip mesh_export.zip output/refined_mesh/data/ -r*

*scp mesh_export.zip .*

*scp ply_export .*

You can view and manipulate the mesh by inserting the .obj and .mtl file into Blender and selecting “Shading” to see it in color (you may have to zoom in). You can also crop it to the area you would like.

You can view and manipulate the ply file by inserting the file into SuperSplat and decreasing the parameter “Splat Size” to 0 to see the model better.

# Contact
If you need to contact me, email me at anuaraimanimran@gmail.com
