
# Tractography filtering quality : comparison between state of art (FA and FOD) and newly developed entropy-based methods

This code provides tools for the evaluation of tractography filtering. The entropy-based method has been developed by Méghane DECROOCQ and available on [this repository](https://github.com/megdec/tractography-visualization).

Our objective is to compare the quality of the entropy-based method with the method usually used in tractography filtering : Fractional Anisotropy (FA) and Fiber Orientation Distributions (FOD).


## Input data

For this comparison, all the data is available in the data directory. We have data for 8 patients (code composed of two letters and three numbers). For each patient, we study 9 nerves 

| Nerve definition | Optic          | Oculomotor          | Trigeminal          | Facial and Vestibulocochlear          | Mixte          |
| ------------------ | ------------- | -------------------- | ------------------ | --------------------------------------- | ------------- |
| Abbreviation      | Chiasma     | III                          | V                        | NF                                                   | NM              |
| Diameter (mm)  | 10                | 5                          | 7                         | 3                                                     | 2                  |











In table X is presented the available nerves for each patient.

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







The input data taken by this code is a file of tractography fibers produced by a tractography software. The format of the data can be **Mrtrix tck file** or **DSIstudio txt file**. If the fibers were produced by Mrtrix, please make sure to use voxel coordinates. To convert your result to voxel coordinates, you can use the function tckconvert of Mrtrix, as described in the shell script you can find on the *Shell* folder. If you want to add a segmentation to the visualization (e.g. a tumor) it must be provided as a **binary image in nifti format**. The segmentations made using DSIstudio are compatible with the code, but any other segmentation might result in wrong placement. Sample data of tracked cranial nerves and cranial tumor segmentation is provided in the *Data* folder. 

## Filtering

A **filtering algorithm** based on the computation of the entropy of the tractography fibers is proposed. An example of use of this algorithm is given in the *main* file. To perform the filtering, you must first compute the entropy of the fibers using the *Entropy/entropy_matrix* function. 

![](Img/entropy_matrix.png)





