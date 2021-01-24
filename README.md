
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






![](Img/pipeline_tracto.jpg)





