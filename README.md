
# Tractography filtering quality : comparison between state of art (FA and FOD) and newly developed entropy-based methods

This code provides tools for the evaluation of tractography filtering. The entropy-based method has been developed by Méghane DECROOCQ and available on [this repository](https://github.com/megdec/tractography-visualization).

Our objective is to compare the quality of the entropy-based method with the method usually used in tractography filtering : Fractional Anisotropy (FA) and Fiber Orientation Distributions (FOD).


## Input data

For this comparison, all the data are available in the data directory. We have data for 8 patients (code composed of two letters and three numbers). For each patient, we study 9 nerves : III, V, NF and NF are separated in two parts (example : IIID for right hemisphere, IIIG for left hemisphere).

| Nerve definition | Optic          | Oculomotor          | Trigeminal          | Facial and Vestibulocochlear          | Mixte          |
| ------------------ | ------------- | -------------------- | ------------------ | --------------------------------------- | ------------- |
| Nerve abbreviation | Chiasma | III                         | V                        | NF                                                   | NM              |
| Diameter (mm)  | 10                | 5                          | 7                         | 3                                                     | 2                  |

A few nerve are not available. In table X are presented the available nerves for each patient.

|                     | AS012          | BF009          | BM013          | CE008          | GF006          | MG007          | MV011          | SF010          |
| -------------- | -------------- | -------------- | -------------- | -------------- | -------------- | -------------- | -------------- | -------------- | 
| Chiasma       | V                 | V                   | V                   | V                   | V                  | V                    | V                   |                       |
| IIID                | V                 | V                   | V                   | V                   | V                  | V                    | V                   | V                    |
| IIIG                | V                 | V                   | V                   | V                   | V                  | V                    | V                   | V                    |
| NFD              | V                 | V                   | V                   | V                   | V                  | V                    | V                   | V                    |
| NFG              | V                 | V                   | V                   | V                   | V                  | V                    | V                   | V                    |
| NMD             | V                 | V                   | V                   | V                   | V                  | V                    | V                   | V                    |
| NMG            | V                 | V                   | V                   |                       | V                  | V                    | V                   | V                    |
| VD                | V                 | V                   | V                   | V                   | V                  | V                    | V                   | V                    |
| VG                | V                 | V                   | V                   | V                   | V                  | V                    | V                   | V                    |


For each patient, we have :

- fa.mif (or fa.nii), a 3D-image containing the fractional anisotropy of the fibre
- fod.mif (of fod.nii), a 3D-image containing the spherical harmonics of the fibre orientation distributions
- eddy_corrected_dti.mif, a 3D-image corresponding to the result of the diffusion MRI
- for each nerve :
    - Ground_Truth.tck and Ground_Truth_vox.tck, corresponding to tracks files realised by the expert with the optimal parameters, respectively in millimeters and voxels. We will use it as ground truth for the evaluation
    - for each parameter and each condition of this parameter, Tracks.tck and Tracks_vox.tck, corresponding to tracks files realised with the non-optimal parameters, respectively in millimeters and voxels. (table X)

| Parameters                       | Value 1          | Value 2          | Value 3          | Value 4          | Value 5          |
| ----------------------------- | -------------- | -------------- | --------------- | ---------------- | --------------- |
| FA threshold                     | oFA               | oFA-0.03      | oFA-0.06       | oFA-0.1           | -                    | 
| ROI size                            | oROIs           | oROIs+D/5   | oROIs+2D/5  | oROIs+3D/5   | oROIs+4D/5  |
| ROI vertical translation     | oROIl-2D/5   | oROIl-D/5     | oROIl            | oROIl+D/5       | oROIl+2D/5  |
| ROI horizontal translation | oROIl-2D/5   | oROIl-D/5     | oROIl            | oROIl+D/5       | oROIl+2D/5  | 



## Compute tracks weights

In the code/Python directory, the scripts tcksample.py and tcksift2.py are made to compute the tracks weights.

For each track file (.tck), tcksample (with the option -stat_tck mean) assigns to each track the mean value of fractional anisotropy (FA map provided) corresponding to the voxel crossed by the track.

For each track file (.tck), tcksift2 assigns to each track a value of weight, according to the FOD map provided.

The weight files are .txt files with a scalar value (mainly a bit above 0) for each track. For both FA and FOD, a high value is associated with a track to remove. For each patient, nerve, parameter and condition of this paramater, we already computed the weights txt files, provided as FA_Weights.txt and FOD_Weights.txt.




## Compute dice

With the weights previously computed, we can sort the initial tracks and remove the less relevants. For both FA_Weights.txt and FOD_Weights.txt, we sort the weights in decreasing order. We consider that the less relevant tracks have a high weight.

In the code/Matlab directory, the script XXXXX computes the dice between a track file and the ground truth. We initially compute the dice between the initial track file and the ground truth. Then, we remove 1% of the less relevant tracks and recompute the dice. Again, we remove the next 1% of less relevant tracks and recompute the dice. We repete the operation until the removal of all tracks. We save the 100 dice values in the csv files Dice_FA_Filltering.csv and Dice_FOD_Filltering.csv.


## Plot dice

In the code/Python directory, the script plot_dice.py plots the evolution of the dice value for all patients, given in parameter the filter_type (FA or FOD), the nerve, the parameter and the condition on the parameter. Here is an example with the nerve IIIG, the parameter FA, and a value of 0.2.

![](img/dice_IIIG_FA_fa_0.2.png)


## Tracks visualisation

In the code/Matlab directory, the script Visualisation.m enables to generate track files from weights previously calculated. The scripts takes as inputs :
- Tracks_filename, the initial .tck file with all the tracks generated with non-optimal parameters
- Weights_filename, which is FA_Weights.txt or FOD_Weights.txt, the weights calculated before.
- Output_Dir, which is where the output files will be saved.

As ouput, we save, for each percentage of tracks remaining (default 20, 40, 60, 80 and 100), a .tck file with the X% most relevant tracks (track_X.tck) and their corresponding weights in a .txt file (track_X_weight.txt).

To visualise the tracks in Mrtrix, run the following commands in the correct directory :
```bash
mrview -load fa.nii -tractography.load Tracks.tck --tractography.tsf_load FA_Weights.txt &

mrview -load fa.nii -tractography.load Ground_Truth.tck -tractography.load FA/track_20.tck -tractography.load FA/track_40.tck -tractography.load FA/track_60.tck -tractography.load FA/track_80.tck -tractography.load FA/track_100.tck -tractography.slab -1 -tractography.lighting 1 &
```

Here is an example of visualisation for the patient MG007, Chiasma nerve, paramater FA and condition fa_0.2.

![](img/video_final.mov)









![](img/pipeline_tracto.jpg)



# Folder descrition 

### Datas
The data folder contains files and folder organise through 4 subfolder levels for Patients, Nerves, Parameters and Conditions. 

