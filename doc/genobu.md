# GENOBU

## De-novo Assembly

### Length distribution of nanopore reads

Count length of each read from fastq file
```
awk '(NR-2)%4==0 { print length($1)}' all_pass.fastq > length.txt
```
Plot with R lenght distribution
```
library(tidyverse)

myd = read.csv("~/projects/genobu/data/length.txt", header=F)

ggplot(myd, aes(V1)) + geom_density()
ggsave("~/projects/genobu/plots/density_buf_length.pdf")

ggplot(myd, aes(V1)) + geom_density() + xlim (0,15000)
ggsave("~/projects/genobu/plots/density_buf_length_Max15k.pdf")

```

### Run [Shasta](https://github.com/chanzuckerberg/shasta)
```
qsub -q bigmpi2@sauron.recas.ba.infn.it -l nodes=1:ppn=16 -o /lustrehome/gianluca/junk/genobu/shasta_5k.out -e /lustrehome/gianluca/junk/genobu/shasta_5k.err -N shasta-all-5k  ~/jobs/genobu/pbs-shasta_all_Min5k.sh
```

### Count total length of Assembly
```
cat Assembly.fasta | grep ">" | tr " " "\t" | cut -f 3 > length_assembly.txt

awk 'BEGIN{sum=0} {sum=sum+$1} END {print sum}' length_assembly.txt
```
