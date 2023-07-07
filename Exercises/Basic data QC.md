# Removing Samples and SNPs with Missing Genotypes.

plink --file GWAS --mind 0.10 --recode --out GWAS_clean_mind

Examine GWAS_clean_mind.log to see how many samples are excluded based on this criterion and fill in Box 1.

### Answer 1 sample removed.

#  You will now remove SNPs with MAFs>5% that are missing >5% of their genotypes and then remove SNPs with MAFs<5% that are missing >1% of their genotypes. SNPs which are missing genotypes can have higher error rates than those SNP markers without missing data.

plink --file GWAS_clean_mind --maf 0.05 --recode --out MAF_greater_5
plink --file GWAS_clean_mind --exclude MAF_greater_5.map --recode --out MAF_less_5

# Check variants in both data sets

plink --file MAF_greater_5 --geno 0.05 --recode --out MAF_greater_5_clean
### 2 variants removed
plink --file MAF_less_5 --geno 0.01 --recode --out MAF_less_5_clean
### 59 variants removed

# Merge these two files

plink --file MAF_greater_5_clean --merge MAF_less_5_clean.ped MAF_less_5_clean.map --recode --out GWAS_MAF_clean

# A more stringent criterion for missing data is used, samples missing >3% of their genotypes are removed.

plink --file GWAS_MAF_clean --mind 0.03 --recode --out GWAS_clean2
### No variants removed

# Error of the reported sex of an individual can occur. Information from the SNP genotypes can be used to verify the sex of individuals, by examining homozygosity (F) on the X chromosome for every individual. F is expected to be <0.2 in females and >0.8 in males. To check sex run

plink --file GWAS_clean2 --check-sex --out GWAS_sex_checking

## Use R to examine the GWAS_sex_checking.sexcheck file and determine if there are individuals whose recorded sex is inconsistent with genetic sex.
setwd('Plink_DataQC/')
sexcheck = read.table("GWAS_sex_checking.sexcheck", header=T) 
names(sexcheck)
sex_problem = sexcheck[which(sexcheck$STATUS=="PROBLEM"),] 
sex_problem
q()


### NA20530 and NA20506 were coded as a female (2) and from the genotypes appear to be males (1). In addition, 3 individuals (NA20766, NA20771 and NA20757) do not have enough information to determine if they are males or females and PLINK reports sex = 0 for the genotyped sex.Fill in the table below:

### Table 1: Sex check
<!-- FID	IID	PEDSEX	SNPSEX	STATUS	F
NA20506	NA20506	 	 	 	 
NA20530	NA20530	 	 	 	 
NA20766	NA20766	 	 	 	 
NA20771	NA20771	 	 	 	 
NA20757	NA20757	 	 	 	 
Reasons for these kinds of discrepancies, include the records are incorrect, incorrect data entry, sample swap, unreported Turner or Klinefelter syndromes. Additionally, if a sufficient number of SNPs have not been genotyped on the X chromosome it can be difficult to accurately predict the sex of an individual. In this dataset, there are only 194 X chromosomal SNPs. If you cannot validate the sex of the individual they should be removed. For this exercise, we are going to assume that when the sex was checked, we found it was incorrectly recorded (i.e. these samples were male). Therefore, this error could simply be corrected. -->

# Question 1: Why do you expect the homozygosity rate to be higher on the X chromosome in males than females?

# c. Duplicate Samples
 
 plink --file GWAS_clean2 --genome --out duplicates

### open file in R
 dups = read.table("duplicates.genome", header = T)

### We are interested in the Pi-Hat (the estimated proportion IBD sharing) value. You may notice that there is more than one duplicate (Pi-Hat=~1). Also, examine the output for pairs of individuals with high Pi-Hat values which can indicate they are related. The amount of allele sharing [Z(0), Z(1) and Z(2)] across all SNPs provides information on the type of relative pair.

### FID1 - Family ID for 1st individual; ID1 - Individual ID for 1st individual; FID2 - Family ID for 2nd individual; ID2 - Individual ID for 2nd individual; Z(0) - P(IBD=0); Z(1) - P(IBD+1); Z(2) - P(IBD=2); PI_HAT - P(IBD=2)+0.5xP(IBD=1)(proportion IBD )

### Pi-hat can be inflated and individuals appear to be related to each other if you have samples from different populations. This explains why we observe pairs of individuals with Pi-hat >0.05 since threedistinctpopulationswereanalyzed. Additionally,thisphenomenoncanbeobservedifasubset(s) of samples have higher genotyping/sequencing error rates, which creates two or more “populations” and the individuals within these “populations” incorrectly appear to be related.

# Question 2: How many duplicate pairs do your find (hint: Pi-Hat = ~1)? Do pairs with a Pi-Hat = ~1 have to be duplicate samples? What is another explanation? What proportion would you expect a parent/ child to share IBD? Can you find any such relationship?

problem_pairs = dups[which(dups$PI_HAT > 0.05),] 
myvars = c("FID1", "IID1", "FID2", "IID2", "PI_HAT") 
problem_pairs[myvars]

### There are two duplicate pairs and also a trio (two parents and a child). Parent/child relationships will have a Pi_Hat value of ~0.5, but so will sibpairs. We can tell that this is a parent child relationship by examine Z(0), Z(1) and Z(2). We will retain only one sample from each duplicate pair and the parents NA12749 and NA12748. If you perform mixed-model analysis related individuals can be retained in the sample.

# d. Hardy-Weinberg Equilibrium (HWE):

plink --file GWAS_clean3 --pheno pheno.txt --pheno-name Aff --hardy

### Using R examine the file plink.hwe and look for SNPs with p-values of 10-7 or smaller.

hardy = read.table("plink.hwe", header = T) 
names(hardy)
hwe_prob = hardy[which(hardy$P < 0.0000009),] 
hwe_prob


### Population Stratification and Association Testing
You usually should not analyze data from European-Americans, Mexican-Americans and Italians together even if you control for population stratification. They can be analyzed separately and the data combined using meta-analysis.


plink --file GWAS_clean4 --genome --cluster --mds-plot 10


Although the association analysis will be performed on the entire data set, only this a subset of SNPs which are not in LD will be used to construct PCA and MDS components. We generated 10 components here.


Examining λ can aid in determining how many PC components should be included in the analysis. If there is no population stratification or other biases, then λ should equal 1 or ~1.


plink --file GWAS_clean4 --genome --cluster --pca 10 header


Eigenvectors are written to plink.eigenvec, and top eigenvalues are written to plink.eigenval. The 'header' modifier adds a header line to the .eigenvec file(s).


And then find out what λ is when we adjust for the first component:
plink --file GWAS_clean4 --pheno pheno.txt --pheno-name Aff --covar plink.eigenvec -- covar-name PC1 --logistic --adjust --out PC1


And the first and second components:
plink --file GWAS_clean4 --pheno pheno.txt --pheno-name Aff --covar plink.eigenvec -- covar-name PC1-PC2 --logistic --adjust --out PC1-PC2


and so forth for all 10 components in the .log file completing the table:


Unadj PC1   PC1-2 PC1-3 PC1-4 PC1-5 PC1-6 PC1-7 PC1-8 PC1-9 PC1-10
1.121 1.085 1.026 1.033 1.040 1.050 1.043 1.021 1.036 1.043 1.051


It is best to include two components in the analysis. Since we are analyzing three unique populations inclusion of PCs did not adequately control for substructure. If you compare the QQ plots below you can see that for this dataset the most significant SNPs were changed minimally when we adjusted for substructure but some of the moderately significant SNPs lambda became less significant after adjustment. However, in some situations the p-values can become smaller.

Why would you not want to include in your analysis individuals from different ethnic backgrounds even if you control for population substructure?


Firstly, you may not be able to adequately control for population substructure. Secondly, even if within the different populations the same genes are involved, for common variants LD structure can vary between populations, e.g., the tagSNPs in the different populations can have different allele frequencies, therefore the functional variant will not be tagged equally well in all populations and power can be reduced. It is also possible that different variants are associated, but for common variants, which are very old, usually this is not the cause. If a study involves individuals of different ancestry analysis can be performed separately and the results can be combined via meta-analysis. Studying individuals of different ancestry can be highly beneficial to fine map loci.





























