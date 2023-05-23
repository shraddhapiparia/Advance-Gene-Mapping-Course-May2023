Year wise add the list of LMM tools here:

Also list the most popular tools


Chen and Abecasis (2007) implemented them via the ”FAmily based Score Test Approximation” (FASTA) in the MERLIN software package
Closely related to earlier QTDT method (Abecasis et al. 2000a;b) which implements a slightly more general/complex model
FASTA was also implemented in GenABEL, along with a similar test called GRAMMAR (Aulchenko et al. 2007)


Amin et al. (2007) proposed instead estimating the kinships based on genome-wide SNP data
Ideally we want to use G=ZZ′/m, the genetic relationship matrix (GRM) between individuals at the causal loci
Since we don’t know the causal loci, we approximate G by A, the overall GRM between individuals
Various different ways to estimate this, usually based on scaled (by allele frequency) matrix of identity-by-state (IBS) sharing

Kang et al. (2010) and Zhang et al. (2010) suggested applying the approach to apparently unrelated individuals
As a way of accounting for population substructure/stratification Also proposed applying to binary traits (case/control coded 1/0) Implemented in EMMAX and TASSEL software, respectively

Subsequently a number of other publications/software packages have implemented essentially the same model
FaST-LMM (Lippert et al. 2011)
GEMMA (Zhou and Stephens 2012)
GenABEL (GRAMMAR-Gamma) (Svishcheva et al. 2012) MMM (Pirinen et al. 2013)
MENDEL (Zhou et al. 2014)
RAREMETALWORKER
GCTA
DISSECT


Main difference between them is the precise computational tricks used to speed up the calculations
And the convenience/ease of use
See comparison in Eu-Ahsunthornwattana et al. (2014) PLoS Genetics 10(7):e1004445


BOLT-LMM (Loh et al. 2016) uses a slightly different approach, based on a Bayesian implementation of LMM formulation 1:
y = Xβ + Zu + ε
One of the first mixed model packages that worked for really large-scale
(e.g. UK Biobank) datasets
Now potentially (?) superseded by fastGWA module in GCTA
And by REGENIE (https://doi.org/10.1038/s41588-021-00870-7), which uses a slightly different formulation based on analysing the residuals following a whole-genome blockwise ridge regression
Again based on LMM formulation 1: y = Xβ + Zu + ε


For binary traits, coding cases and controls as a 1/0 quantitative trait is not optimal
Though in practice it seems to work reasonably well
LTMLM (Hayeck et al. 2015) and LEAP (Weissbrod et al. 2015) instead use an underlying liability model to improve power
Assuming known disease prevalence


Chen et al. (2016) showed that high levels of population stratification can invalidate the analysis, when applied to a case/control sample
Resulting in a mixture of inflated and deflated test statistics Developed GMMAT software to address this problem
See also CARAT software (Jiang et al. 2016, AJHG 98:243-55)


SAIGE software (Zhou et al. 2018, AJHG 50(9):1335-1341) implements a mixed model test that deals with large case-control imbalance, as you might see (for example) in UK Biobank
REGENIE also implements this same saddle point approximation (SPA) test
Along with an approximate Firth penalized likelihood-ratio test



Bulik-Sullivan et al. (2015) [Nat Genet 47:291-295] Bulik-Sullivan et al. (2015) [Nat Genet 47:1236-1241]
Clever idea that allows the variance component parameters to be estimated via a simple regression on ‘LD Scores’


GWAS have been extraordinarily successful at detecting genetic locations harboring genes associated with complex disease
But the SNPs identified do not account for the known (estimated) heritability for most disorders
Could G×G and G×E effects account for part of the ‘missing heritability’ ?
Zuk et al. (2012) PNAS 109:1193-1198


