# Calculate the heterozygosity rate at per-site from a VCF

The following function can be used to calculate the number and heterozygosity rate at a site level.

Largely based on [biostars-291147](https://www.biostars.org/p/291147/)

```bash
function heterozygosity_per_site {
    vcf=${1}
    bcftools query -f '%CHROM\t%POS\t%ID\t%REF\t%ALT\t[\t%SAMPLE=%GT]\n' ${vcf} | \
    awk -v OFS='\t' 'BEGIN {print "CHROM\tPOS\tREF\tALT\tn_het\tn\thet_rate"} 
    {
    n_het=gsub(/0\|1|1\|0|0\/1|1\/0|0\|2|2\|0|0\/2|2\/0|0\|3|3\|0|0\/3|3\/0|0\|4|4\|0|0\/4|4\/0/, "" ); 
    print $1, $2, $3, $4, 
    n_het,
    (NF-5),
    n_het/(NF - 5)
    }'
}

# Usage
heterozygosity_per_site samples.vcf.gz

# Restrict to region
bcftools view WI.20200512.soft-filter.vcf.gz I:1-10000 | heterozygosity_per_site -
```

__Output__

```bash
$ bcftools view my.vcf.gz I:1-10000 | heterozygosity_per_site -

I	620	.	CCTAAGA	1	330	0.0030303
I	626	.	A	9	330	0.0272727
I	626	.	A	4	330	0.0121212
I	632	.	CCTAATA	0	330	0
I	637	.	T	6	330	0.0181818
I	638	.	A	4	330	0.0121212
I	638	.	ACTAAGC	1	330	0.0030303
I	644	.	CCTAAGCCTAAGA	1	330	0.0030303
I	650	.	C	2	330	0.00606061
I	650	.	C	0	330	0
I	656	.	A	3	330	0.00909091
I	656	.	A	0	330	0
I	668	.	A	4	330	0.0121212
I	671	.	A	1	330	0.0030303
...
```
