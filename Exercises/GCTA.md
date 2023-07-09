# LMM approach implemented in GCTA

gcta64 --mlma --bfile quantfamdata --pheno phenos.txt --out GCTAresults


## Plot using R commands:

### QQ plot

res3<-read.table("GCTAresults.mlma", header=T)

head(res3)

chi<-(qchisq(1-res3$p,1))

lambda=median(chi)/0.456

new3<-data.frame(res3$SNP, res3$Chr, res3$bp, res3$p)

names(new3)<-c("SNP", "CHR", "BP", "P")

head(new3)

png("qq3.png")

qq(new3$P)

dev.off()

### Manhattan plot

png("mh3.png")

manhattan(new3, pch=20, suggestiveline=F, genomewideline=F, ymin=2,
cex.x.axis=0.65, colors=c("black","dodgerblue"), cex=0.5)

dev.off()

## Compare results with FaST-LMM results

res2<-read.table("FLMMresults", header=T)

new2<-data.frame(res2$SNP, res2$Chromosome, res2$Position, res2$Pvalue)

names(new2)<-c("SNP", "CHR", "BP", "P")

merged=merge(new3,new2, by="SNP", sort=F)

head(res2)

head(new2)

head(new3)

head(merged)

png("compareGCTAFLMM.png")

plot(-log10(merged$P.x),-log10(merged$P.y))

abline(0,1)

dev.off()

