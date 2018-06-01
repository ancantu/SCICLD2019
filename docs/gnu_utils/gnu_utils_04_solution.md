1) Print the first three exon records in `ecoli.gff3` in a single command
```
head -n 3 <( grep "exon" ecoli.gff3 )
grep "exon" ecoli.gff3 | head -n 3
```
2) For each record, only print the name value `Name=MJ49_RS00725;` => `MJ49_RS00725`
```
grep -o "Name=[^;]\+" ecoli.gff3 | grep -o "[^Name=]\+"
```
3) Count how many sequences are in `ecoli.fasta`
