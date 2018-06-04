
1) Print out the attributes column of a gff file
```
cut -f 9 ecoli.gff3
```

2) Find all genes in `ecoli.gff3`
```
grep "gene" ecoli.gff3 > genes_only.gff3    ## prints out every line that contains the word 'gene', saves those lines to a genes_only.gff3 file
grep -o "Name=[^;]\+" genes_only.gff3       ## prints out Name field for all genes in genes_only file

grep -e "gene" ecoli.gff3 | grep -o "Name=[^;]\+"      ## to avoid creating a intermediate file (genes_only.gff3), you can use a pipe (more on this in the next section)
```


3) Print all the unique chromosomes in a bed file
```
cut -f 1 fileA.bed  > fileA_chr_names.bed    ## saves all chromosome names to a file
uniq fileA_chr_names.bed                   ## prints out unique chromosome names

cut -f 1 fileA.bed  | uniq       ## to avoid creating a intermediate file (fileA_chr_names.bed), you can use a pipe (more on this in the next section)
```

[Return](gnu_utils_03.md)
