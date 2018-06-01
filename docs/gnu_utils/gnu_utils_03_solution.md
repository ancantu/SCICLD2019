
1) Print out the attributes column of a gff file
```
cut -f 9 ecoli.gff3
```

2) Find all genes in `ecoli.gff3`
```
grep "gene" ecoli.gff3         ## prints out every line that contains the word 'gene'

grep "gene" ecoli.gff3 > genes_only.gff3
grep -o "Name=[^;]\+" genes_only.gff3

grep -e "gene" ecoli.gff3 | grep -o "Name=[^;]\+"      ## prints out the name field for every line that contains the word gene
```


3) Print all the unique chromosomes in a bed file
```
cut -f 1 fileA.bed  > fileA_chr_names.bed    ## saves all chromosome names to a file
uniq fileA_chr_names.bed                   ## prints out unique chromosme names

cut -f 1 fileA.bed  | uniq
```

[Return](gnu_utils_02.md)
