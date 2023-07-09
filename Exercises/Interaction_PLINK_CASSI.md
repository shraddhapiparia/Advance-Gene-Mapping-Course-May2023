# Perform association analysis and testing for interaction effects using case/control data in PLINK

## Input data format


### Command for basic association analysis in PLINK:

plink --ped simcasecon.ped --map simcasecon.map --assoc

### Command for genotype-based analysis (rather than an allele-based)

plink --ped simcasecon.ped --map simcasecon.map --model

head -1 plink.model

grep GENO plink.model

### Command for pairwise epistasis in PLINK

plink --ped simcasecon.ped --map simcasecon.map --fast-

### Formally, this tests whether the OR for association between two SNPs differs between cases and controls, which can be shown to appriximate a logistic regression based test of interaction between the SNPs. Results can be found in the file plink.epi.cc. Only pairwise interaction tests with p <= 0.0001 are reported (otherwise, for genome-wide studies, there would be too many results to report, given the large number of pairwise tests performed).

### A more powerful test for SNPs that are not in LD with one another (i.e. that are not too close to one another, in terms of their genomic location) is to additionally use the --case-only option:

plink --ped simcasecon.ped --map simcasecon.map --fast-epistasis --case-only

### A problem with the --fast-epistasis test is that it can be affected by LD between the SNPs (although only the case-only test is seriously affected). A more accurate test is to carry out logistic regresion by using the slower --epistasis command:

plink --ped simcasecon.ped --map simcasecon.map --epistasis

### Since the --epistasis option is slower, but most accurate, for genome-wide studies it might be sensible to first to screen for interactions using the --fast- epistasis command, but then confirm any findings using the --epistasis command on the smaller set of detected SNPs.


# Perform association analysis and testing for interaction effects using case/control data using CASSI

### First we need to convert our data to PLINK binary format:

plink --ped simcasecon.ped --map simcasecon.map --make-bed --out simbinary

### The logistic regression test in CASSI is essentially the same as the --epistasis test in PLINK, except that CASSI uses a likelihood ratio test rather than the asymptotically equivalent Wald (?) test used by PLINK. CASSI also has the advantage of allowing covariates into the analysis, if desired.

cassi -lr -i simbinary.bed

### Now try using the Joint Effects (JE) test, telling CASSI to use the output filename

cassiJE.out

cassi -je -o cassiJE.out -i simbinary.bed












