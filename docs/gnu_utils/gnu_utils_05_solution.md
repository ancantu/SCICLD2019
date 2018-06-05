1) Replace all the tabs in `fileA.bed` with commas
```
sed -e 's/\t/,/g' fileA.bed     ## add the trailing /g for 'global' replacement (all matching occurrences)
```

2) Convert `SRR2014925_1.fastq` into a FASTA file
```
sed -n '1~4s/^@/>/p;2~4p' SRR1570041_1.fastq > SRR1570041_1.fa
```

3) Transform `ecoli.gff3` into a BED file
```
grep -v "#" ecoli.gff3 | grep -v "CDS" | awk '{ print $1, $4-1, $5-1, $9, $6, $7 }'

## need to filter out '#' lines at the beginning and 'CDS' lines which have an inconsistent number of columns
```

4) Calculate the average sequence depth for `NZ_CP013025.1` from `SRR2014925.bedgraph`
```
grep "NZ_CP013025.1" SRR2014925.bedgraph | awk 'BEGIN { SUM=0; } { SUM+=$4; print $0 } END { print "Total = "SUM/NR  }'
```



[Return](gnu_utils_05.md)
