# Biomarker_Identification
Biomarker Identification for Bravo, Williams, Gkoutos and Acharjee

This repository exists to implement and duplciate the analysis by Bravo, Williams, Gkoutos and Acharjee (In Preparation 2018).

By loading R Markdown (*.Rmd) files of interest and editing directories on line 24 of the .Rmd document, code can be implemented and results duplicated (presented in the corresponding html pages).

This is an initial commit: future updates will include:

1) Editing html / markdown files and recompiling with good and efficient commenting

2) Implementing a generalized way to input matrix data

3) Creating a Docker instance to make code portable between installations of R

4) Creating better reporting functions.


Instructions:

Post-processed data needs to be a data.frame, with row names = experiments IDs, and column names = features (Genes / Proteins). An additional column name should be the target variable encoded as a 'factor' labeled "Label", indicating Y for case and N for control, respectively. All other data should be numeric, with categorial / factor variables encoded as dummy variables (aka one-hot encoding).

If importing microarray data which is pre-normalized, a desired preprocessing step may be to collapse duplicate probes and assign gene names. This can be done for HgU133 Plus Microarrays (the most common form) with the built in function preProcessHgU133PlusMicroarray. The function takes three arguments, a data frame of microarrays (called microDat), a design matrix dat frame, and a data frame of array / gene identifiers (called annoGenes). A step excluding low-variance features will also be included automatically. If using another form of microarray, supply the preProcessHgu133PlusMicroarray function with your own annoGenes file with the middle column reflecting probe names present in your microarray file.


Microarray data should be imported with the form:

          GSM388076 GSM388077 GSM388078 GSM388079 GSM388080
1007_s_at  8.093278  8.093635  9.548053  9.607639  9.503821
1053_at    6.839314  6.827462  5.992789  6.003677  5.788727
117_at     7.404180  7.545392  6.221293  6.162450  5.648036
121_at     7.395993  7.113456  8.360060  8.481419  8.361621
1255_g_at  3.037685  2.918100  3.265899  3.341523  3.140711

annoGenes data will look like the following, with Ensemble stable IDs, micorarray IDs, and gene names:

       Gene.stable.ID AFFY.HG.U133.Plus.2.probe Gene.name
276795 ENSG00000285075                                TPK1
276796 ENSG00000285363                            MTRF1LP2
276797 ENSG00000285114               234305_s_at     GSDMC
276798 ENSG00000285114               234305_s_at     GSDMC
276799 ENSG00000285114               234305_s_at     GSDMC
276800 ENSG00000285114                               GSDMC

The experimental design matrix should have the form:

  Condition    Sample
1    normal GSM388076
2    normal GSM388077
3    normal GSM388078
4    normal GSM388079
5    normal GSM388080
6    normal GSM388081

Where Sample is identical to the column names of microarray data, and the control condition is encoded with the word "normal". The case condition may be coded with anything else.

Note redundancy in microarray probes possible, and not all known genes may be assigned microarray probes. The included list was generated with BioMart software on 10/Sept/2018.


If importing other numerical data, generic preprocessing may be desired - including filtering out features with > 50% missing data; dropping features with near-zero variance (we don't want uninformative biological features); imputing other missing data based on median imputation; and transforming data to normalize with a YeoJohnson transformation. These steps can be performed with the GenericPreProcess function. For convenience, the GenericPreProcess function takes data supplied with the argument UnProcessedData and returns All2 ready for input into the algorithm. Unprocessed data should have a form identical to the Microarray data above, with the additional column of "Label" present.

Once processed, generic data should be called All2 and be in the format described above.

Preprocessing function should be uncommented and supplied with the correct arguments, on lines 375,379, or 383 as appropriate of either of the .Rmd files.


