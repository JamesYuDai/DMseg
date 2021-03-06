###   DMseg: detecting differential methylation regions (DMRs) in methylome 


This is a Python package to search through methylome-side CpGs sites for DMRs between two biological conditions. The algorithm executes the following analysis steps:

1.  A linear regression model is fitted to the beta values for each CpG, using the user-input covariates: the first variable is the group label of interest.
2.  Nominal p-values from individual CpG associations are used to define the DMR: more than or equal to two consecutive CpGs with p-value <0.05 will be considered as candidate DMR. A likelihood ratio statistic (LRT) is computed for a candidate DMR.
3.  Group labels will be permuted for `B` times, step `1` and `2` are repeated for each permuation dataset. Family-wise error rate is computed using the null distribution of LRT based on permutation. 




To install the package: 

```
python -m pip install DMseg
```
To run the python package, one can first port in the package and the core function file `DMseg.py`, then call the pipeline function `DMseg_pipeline`

```
import DMseg
from DMseg import DMseg
result = DMseg.DMseg_pipeline(betafile, colDatafile, postionfile, maxgap=500, sd_cutoff=0.025, beta_diff_cutoff=0.05, zscore_cutoff=1.96, B=500, seed=1001)
```

The inputs for the function `DMseg_pipeline` are 
   * `betafile`: the file location for the n x p matrix of methylation beta values (n is the number of samples, p is the number of CpG sites)
   * `colDatafile`: the file location for the covariates for regression analysis
   * `postionfile`: the file location for the chromosomal positions of CpGs
   * `maxgap` is the maximal base pairs between two adjacent CpGs that can be considered as within the same DMR
   * `sd_cutoff` is the minimal inter-sample standard deviation for a CpG to be considered for differential methylation
   * `beta_diff_cutoff` is the minimal mean differences between the two comparison groups for a CpG to be considered for differential methylation
   * `zscore_cutoff` is the minimal z-score for a CpG to be considered for differential methylation
   * `B` is the number permutations to compute family-wise error rate 
   * `seed` is the seed for the random number generator when permuting the group labels

