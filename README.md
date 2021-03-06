# TuningAnalysis

'TuningAnalysis' is an automated R code used to generate graphical analysis of our paper regarding Hyperparameter tuning of Decision Trees [01]. 
Current version works only with data generated by our Hyperparameter tuning project ([HpTuning](https://github.com/rgmantovani/HpTuning))
but may be easily extended. The main features available are:

* comparison of different techniques when performing hyperparameter tuning of classification algorithms;
* average performance plot with statistical tests;
* average runtime plots, presenting: training, testing and optimizatoin times;
* boxplots with the average number of iterations spent by techniques;
* hyperparameters' importance assessment through Spearman correlation and [fAnova framework](https://github.com/automl/fanova) [02].

### Installation

The installation process is via git clone. You should use the following command inside your terminal session:

```
git clone https://github.com/rgmantovani/TuningAnalysis
```

### General instructions

Classification algorithms analyzed must be in accordance with ['mlr'](https://github.com/mlr-org/mlr) R package implementation [03]. 
A complete list of the available learners may be found [here](http://mlr-org.github.io/mlr-tutorial/release/html/integrated_learners/).

Hyperparameter tuning results should be placed in the ```data/<algorithm.name>/results``` sub-directory.

If the required data to generate plots was not extracted before, then there must be some scripts to run. 
This is also checked by the automated code, and returned to the user with instructions on how to proceed.
All of the extraction scripts require the name of the algorithm as a parameter (```<algorithm.name>```).
There is no order to run these scripts, but all of them must be executed.
The files generated by these scripts will be later read and aggreageted as ```data.frame``` objects and used by the automatic analysis code.

#### A - Iterations ids

This first script will extract information regarding the convergence of the tuning techniques. 
For each single job ```j = (dataset, algorithm, technique, seed)```, 
it will identify how many evaluations were needed to generate the best solution. To run it:

```R
Rscript extractIds.R --algo=<algorithm.name> &
# example: 
# Rscript extractIds.R --algo="classif.J48" &
```

Output will be placed in the ```data/<algorithm.name>/ids``` folder, 
with one file per dataset. 

#### B - Models information

This script will extract characteristics from induced models as such as running times. 
If studied models were trees, for example, it will retrieve the tree size of each assessed tree. To run it:

```R
Rscript extractModels.R --algo=<algorithm.name> &
# example: 
# Rscript extractModels.R --algo="classif.J48" &
```

Output will be placed in the ```data/<algorithm.name>/models``` folder, 
with one file per dataset.

#### C - Hyper-parameters correlation

This script join all the hyper-parameter seetings generated in a specific data domain and calculates 
the Spearman correlation bewteen all hyper-parameter combinations two-by-two, and also between each single hyper-parameter and
and the final assessed performance. To run it:

```R
Rscript extractCorr.R --algo=<algorithm.name> &
# example: 
# Rscript extractCorr.R --algo="classif.J48" &
```
Output will be placed in the ```data/<algorithm.name>/corr``` folder, 
with one file per dataset.


#### D - FAnova hyper-parameter marginal predictions

FAnova marginal predictions are obtained by an [external project](https://github.com/automl/fanova). This our script will generate 
input files in the pattern required by the FAnova pyhton script. To run it:

```R
Rscript createFanovaInputs.R --algo=<algorithm.name> &
# example: 
# Rscript createFanovaInputs.R --algo="classif.J48" &
```
Output will be placed in the ```data/<algorithm.name>/input_fanova``` folder, 
with one file per dataset. Provide these files to the external project. It will also generate one correspondent file per dataset.
These new files, should be placed in the ```data/<algorithm.name>/fanova``` sub-directory.

### Running the code

To run the project, please, call it by the following command:
```R
 Rscript mainAnalysis.R --algo=<algorithm.name> &
 # example:
 # Rscript mainAnalysis.R --algo="classif.rpart" & 
```
It will call the analsys and output plots (as pdfs) into the ```output``` sub-directory. 

### Contact

Rafael Gomes Mantovani (rgmantovani@gmail.com) University of São Paulo - São Carlos, Brazil.

### References

[01] **Add out citation**

[02] F. Hutter, H. Hoos, K. Leyton-Brown. [An Efficient Approach for Assessing Hyperparameter Importance](http://jmlr.org/proceedings/papers/v32/hutter14.html). 
In: *Proceedings of the 31th International Conference on Machine Learning*, ICMC 2014, Beijing, China, 2014, pgs 754-762.

[03] B. Bischl, Michel Lang, Lars Kotthoff, Julia Schiffner, Jakob Richter, Erich Studerus, Giuseppe Casalicchio, Zachary Jones. 
mlr: Machine Learning in R. Journal of Machine Learning in R, v.17, n.170, 2016, pgs 1-5. URL: https://github.com/mlr-org/mlr.

<!---
### Citation
--->
<!---
If you use our code/experiments in your research, please, cite [our paper]() where this project was first used:
--->

<!---
[ADD citation]
--->

<!---
### Bibtex 
--->

<!---
```
@article{Mantovani:2017, 
  journal = {},
  title   = {},
  author  = {Rafael G. Mantovani and Tom{\'{a}}s Horv{\'{a}}th and Ricardo Cerri and
		Joaquin Vanschoren and  Andr{\'e} C. P. L. F. {de Carvalho}},
  number  = {},
  volume  = {},
  year    = {},
  pages   = {}  
}
```
--->


