# Variant Association Tools [VAT, Wang et al (2014)] [1] 


## It was developed to perform quality control and association analysis of sequence data. It can also be used to analyze genotype data, e.g. exome chip data and imputed data. The software incorporates many rare variant association methods which include but not limited to Combined Multivariate Collapsing (CMC) [2], Burden of Rare Variants (BRV) [3], Weighted Sum Statistic (WSS) [4], Kernel Based Adaptive Cluster (KBAC) [5], Variable Threshold (VT) [6] and Sequence Kernel Association Test (SKAT) [7]. VAT inherits the intuitive command-line interface of Variant Tools (VTools) [8] with re-design and implementation of its infrastructure to accommodate the scale of dataset generated from current sequencing efforts on large populations. Features of VAT are implemented into VTools subcommand system.

#### Check total number of variants:

vtools select variant --count

#### Genotype Summary
vtools show genotypes > GenotypeSummary.txt
head GenotypeSummary.txt

#### Variant Quality Overview
vtools output variant "max(DP)" "min(DP)" "avg(DP)" "stdev(DP)" "lower_quartile(DP)" "upper_quartile(DP)" -- header

#### Count number of variants passing all filters
vtools select variant "filter=’PASS’" --count

## Data exploration

vtools update variant --from_stat ’total=#(GT)’ ’num=#(alt)’ ’het=#(het)’ ’hom=#(hom)’ ’other=#(other)’ ’minDP=min(DP_geno)’ ’maxDP=max(DP_geno)’ ’meanDP=avg(DP_geno)’ ’maf=maf()’


vtools show fields 
vtools show table variant

#### The command will calculate:
#### • total: Total number of genotypes (GT) for a variant
#### • num: Total number of alternative alleles across all samples
#### • het: Total number of heterozygote genotypes 1/0
#### • hom: Total number of homozygote genotypes 1/1
#### • other: Total number of double-homozygotes 1/2
#### • min/max/meanDP: Summaries for depth of coverage and genotype quality across samples


• maf: Minor allele frequency
• Add calculated variant level statistics to fields, which can be shown by commands vtools show fields and
vtools show table variant



## Variant annotation

vtools execute ANNOVAR geneanno

#### output the annotated variant sites to the screen
vtools output variant chr pos ref alt mut_type --limit 20 --header


#### Data Quality Control (QC) and Variant Selection
##### Ti/Tv ratio evaluations

vtools_report trans_ratio variant -n num

#### If only genotype calls having depth of coverage greater than 10 are considered:
vtools_report trans_ratio variant -n numGD10

#### Removal of low quality variant sites
#### We should not need to remove any variant site based on read depth because all variants passed the quality filter. To demonstrate removal of variant sites, let us


remove those with a total read depth {$\(\le\)$} 15. vtools select variant "DP<15" -t to_remove
vtools show tables
vtools remove variants to_remove -v0 vtools show tables


#### Filter genotype calls by quality:
vtools remove genotypes "DP_geno<10" -v0

#### Select variants by annotated functionality
vtools select variant "mut_type like ’non%’ or mut_type like ’stop%’ or region_type=’splicing’" -t v_funct
vtools show tables


The command above selects variant sites that are either nonsynonymous (by condition "mut type like ’non%’) or stop-gain/stop-loss (by condition mut type like ’stop%’) or alternative splicing (by condition region- type=’splicing’)
3367 functional variant sites are selected


# Association Tests for Quantitative Traits

## View phenotype data
vtools show samples --limit 5

## Create sub-projects for association analysis with CEU samples

vtools select variant --samples "RACE=1" -t CEU
mkdir -p ceu
cd ceu
vtools init ceu --parent ../ --variants CEU --samples "RACE=1" -- build hg19
vtools show project

## Subset data by MAFs
vtools select variant "CEU_mafGD10>=0.05" -t common_ceu 
vtools select v_funct "CEU_mafGD10<0.01" -t rare_ceu

## Annotate variants to genes
vtools use refGene

# Association testing of common/rare variants

## Common variants:

vtools associate common_ceu BMI --covariate SEX -m "LinRegBurden --alternative 2" -j1 --to_db EA_CV > EA_CV.asso.res

# Burden test for rare variants (BRV)

# Variable thresholds test for rare variants (VT)

# Association analysis of YRI samples

























































