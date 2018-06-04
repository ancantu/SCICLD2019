1) Print the first three exon records in `ecoli.gff3` in a single command
```
head -n 3 <( grep "exon" ecoli.gff3 )    ## redirect input for the head command to be a grep for exon

grep "exon" ecoli.gff3 | head -n 3       ## Or pipe the output of grep exon into the head command
```
2) For each record, only print the name value `Name=MJ49_RS00725;` => `MJ49_RS00725`
```
grep -o "Name=[^;]\+" ecoli.gff3 | grep -o "[^Name=]\+"    ## first search for all values where Name=*something*, then search for characters that don't match "Name="
```
3) Count how many basepairs are in `ecoli.fasta`.
```
grep -v ">" ecoli.fasta | wc -c     ## we don't want to count descriptive characters, for a fasta all non-sequence lines start with ">" so first we filter those out, then count the characters
5583275
```

[Return](gnu_utils_04.md)
