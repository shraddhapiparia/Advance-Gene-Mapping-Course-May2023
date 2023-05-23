# Genome-Wide Association Exercise Association Analysis Controlling for Population Substructure

### Before we start testing for association, we want to know if there are outliers. Even after removing the outliers when association analysis is performed population substructure and admixture may need to be controlled. If not, we risk observing an association, which is due to a difference in genotype frequencies in cases and controls, because of population substructure/admixture and not because of linkage disequilibrium (LD) between tagSNP(s) and the functional variant(s). We are going to use multidimensional scaling (MDS) and principal components analysis (PCA) within the PLINK software to generate 10 components. Disclaimer: You usually should not analyze data from European-Americans, Mexican-Americans and Italians together even if you control for population stratification. They can be analyzed separately and the data combined using meta-analysis.

## MDS
plink --file GWAS_clean4 --genome --cluster --mds-plot 10


### Visualizing population structure using MDS is useful for identifying subpopulations, population stratification and systematic genotyping or sequencing errors, and can also be used to detect individual outliers that may need to be removed, e.g. European-Americans included in a study of African-Americans. MDS coordinates help with visualizing genetic distances and population substructure. PLINK also offers another dimension reduction, --pca, for PCA, the PC components which can also be used for visualizing data to detect outliers in the same manner which was performed using MDS. Additionally, covariates either from either MDS or PCA can be used in a regression model to aid in correcting for population substructure and admixture.

## PCA

### We will now continue performing the analysis using PLINK but will use PCA instead of MDS. We will generate PCs and determine how many PC covariates should be included in the regression model. When SNPs are tested for an association with a trait analysis can be performed, first by including no PC components, then one PC component and then two PC components and so on. Please note that as each PC component is added all the SNPs are analyzed, e.g. a complete GWAS is performed. Examining λ can aid in determining how many PC components should be included in the analysis. If there is no population stratification or other biases, then λ should equal 1 or ~1. We will use λ to determine how many PC components from our analysis will be added to the logistic regression model. First, estimate λ without adjusting for any PC components:

plink --file GWAS_clean4 --pheno pheno.txt --pheno-name Aff --logistic --adjust --out unadj

#### Generate forst 10 PCA values:
plink --file GWAS_clean4 --genome --cluster --pca 10 header

#### Then find out what λ is when we adjust for the first component:
plink --file GWAS_clean4 --pheno pheno.txt --pheno-name Aff --covar plink.eigenvec --covar-name PC1 --logistic --adjust --out PC1
#### Genomic inflation est. lambda (based on median chisq) = 1.08462.


### And the first and second components:

plink --file GWAS_clean4 --pheno pheno.txt --pheno-name Aff --covar plink.eigenvec --covar-name PC1-PC2 --logistic --adjust --out PC1-PC2

#### Genomic inflation est. lambda (based on median chisq) = 1.02628

#### Genomic inflation est. lambda (based on median chisq) for PC1-PC3 = 1.03287

## Question 1:




































