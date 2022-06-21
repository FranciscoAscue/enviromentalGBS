### make a matrix from vcf file 

```bash
bcftools view -H cv_maxmissing0.5.recode.vcf| cut -f 1 | uniq | awk '{print $0"\t"$0}' > map.txt
vcftools --vcf cv_maxmissing0.5.recode.vcf --plink --chrom-map map.txt --out cv_maxmissing0.5_2
```

#### Download plink from `https://www.cog-genomics.org/plink/`

```bash
./../plink --file cv_maxmissing0.5_2 --recode 12 --out bbd --allow-extra-chr
```


