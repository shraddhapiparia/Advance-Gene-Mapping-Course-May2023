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



Siemiatycki and Thomas (1981) Int J Epidemiol 10:383-387; Thompson (1991) J Clin Epidemiol 44:221-232 Phillips (1998) Genetics 149:1167-1171; Cordell (2002) Hum Molec Genet 11:2463-2468
McClay and van den Oord (2006) J Theor Biol 240:149-159; Phillips (2008) Nat Rev Genet 9:855-867 Clayton DG (2009) PLoS Genet 5(7): e1000540; Wang, Elston and Zhu (2010) Hum Hered 70:269-277


GXG GWAS tools:
Many recent publications have focussed on finding clever computational tricks to speed up exhaustive search procedure
BOOST (Wan et al. (2010) AJHG 87:325-340)
SIXPAC (Prabhu and Pe’er (2012) Genome Res 22:2230-2240) Kam-Thong et al. (2012) Hum Hered 73:220-236 (GPUs)
Fr ̊aanberg et al. (2015) PLOS Genetics 11(9):e1005502 “Discovering genetic interactions in large-scale association studies by stage-wise likelihood ratio tests”



Ma et al. (2013) PLoS Genet 9(2): e1003321
Compared 4 different tests that combine P values from pairwise (SNP x SNP) interaction tests
Showed that the truncated tests did best
Presented an application only considering gene pairs known to exhibit protein-protein interactions


Case only analysis:

Piergorsh et al. 1994; Yang et al. 1999; Weinberg and Umbach 2000


A similar idea is implemented in EPIBLASTER (Kam-Thong et al. 2011; EJHG 19:465-571)
Wu et al. (2010) (PLoS Genet 6:e1001131) also proposed a similar approach – though needs adjustment to give correct type I error rates
See also Joint Effects (JE) statistics
(Ueki and Cordell 2012; PLoS Genetics 8(4):e1002625)
All these methods test whether correlation exists (case-only) or is different in cases and controls (case/control)
Via testing a log OR for association between two loci
However, the log OR for association (λ) encapsulates a slightly different quantity between the different methods
All implemented (along with standard logistic and linear regression) in CASSI
http://www.staff.ncl.ac.uk/richard.howey/cassi/



Epistasis among HLA-DRB1, HLA-DQA1, and HLA-DQB1 in
multiple sclerosis (Lincoln et al. 2009 PNAS 106:7542-7547) HLA-C and ERAP1 in psoriasis (Strange et al. 2010)
HLA-B27 and ERAP1 in ankylosing spondylitis (Evans et al. 2011) BANK1 and BLK in SLE (Castillejo-Lopez et al. 2012)
Gusareva et al. (2014) found a reasonably convincing (partially replicating) interaction between SNPs on chromosome 6 (KHDRBS2) and 13 (CRYL1) in Alzheimer’s disease
Dai et al. (2016) [AJHG 99:352-365] identified 3 loci simultaneously interacting with established risk factors gastresophageal reflux, obesity and tobacco smoking, with respect to risk for Barrett’s esophagus

Hemani et al. 2014 (Nature 508:249-253) found 501 instances of epistatic effects on gene expression, of which 30 could be replicated in two independent samples
Many SNPs are close together, could represent haplotype effects? Or the effect of a single untyped variant?
See caveats in
Wood et al. (2014) Nature 514(7520):E3-5. PMID:25279928
Fish et al. (2016) Am J Hum Genet 99(4):817830. PMID:27640306
The Hemani et al. paper was subsequently retracted (https://www.nature.com/articles/s41586-021-03766-y)



Empirical evidence for G×E interactions
Myers et al. (2014) Hum Mol Genet 23(19): 5251-9 “Genome-wide
Interaction Studies Reveal Sex-Specific Asthma Risk Alleles”
Small et al. (2018) Nat Genet 50(4): 572-580 “Regulatory Variants at KLF14 Influence Type 2 Diabetes Risk via a Female-Specific Effect on Adipocyte Size and Body Composition”
Sung et al. (2019) Hum Molec Genet 28(15): 2615-2633 “A multi-ancestry genome-wide study incorporating gene-smoking interactions identifies multiple new loci for pulse pressure and mean arterial pressure.”

GXE interaction
Fav ́e et al. (2018) Nat Commun 9(1): 827 “Gene-by-environment Interactions in Urban Populations Modulate Risk Phenotypes”

