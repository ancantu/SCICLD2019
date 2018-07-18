# Parallelization

According to the [Stanford CPU database](http://cpudb.stanford.edu/), processors haven't gotten faster since 2005.

![clock rates](https://github.com/CODE-at-TACC/summer-2015/raw/master/parallel/images/clock.png)

No matter how much we've spent on the latest and greatest PC, sequential (single-core) programs won't be going any faster. Even at TACC, Stampede processors ran between 2.7 and 3.5 GHz and Stampede 2 KNL processors run at 1.4 GHz.

However, transistor and core counts are increasing.

![transistors](https://github.com/CODE-at-TACC/summer-2015/raw/master/parallel/images/transistors.png)

Stampede had 16 cores in the main CPU, while Stampede 2 will have 68! We can take advantage of these resources with threaded code and concurrent task scheduling.

### Threaded Applications

The most proactive thing we can do is run our applications on multiple threads/processors when possible. This means reading the manual, finding the option for scaling execution, and usually setting it to correspond with the system core count.

| TACC System | Cores/Node |
|--|--|
| Stampede | 16 |
| Stampede 2 KNL | 68 |
| Stampede 2 SKX | 48 |
| Lonestar 5 | 24 |
| Maverick | 20 |
| Wrangler | 24 |

The version of `sort` on this system does have a parallel option, so lets give it a try using our standard workflow.

```
$ time sort -S 100M -k1,1 -k2,2n SRR2014925.bed | head
$ time sort --parallel 24 -S 100M -k1,1 -k2,2n SRR2014925.bed | head
```

We can also use a loop to see where increasing the number of threads becomes ineffective.

```
for N in {1..6}
do
   time sort --parallel $N -S 100M -k1,1 -k2,2n SRR2014925.bed > /dev/null
done 2>&1 | grep "real" | awk '{ print NR" cores\t"$0 }'
```

NOTE: `time` writes to `stderr` instead of `stdout`, so we need the `2>&1` redirection to grep for real time.

Do you see a point where extra cores becomes ineffective? Does increasing the memory limit help?

### Workflow scaling

Lets see how well our tophat2 workflow scales.
I made a convenience script that you can copy to your scratch directory.

```
$ cp /work/03076/gzynda/stampede2/ctls-public/run_tophat_yeast.sh .
```

If you take a look at the file

```
PUBLIC=/work/03076/gzynda/stampede2/ctls-public
VER=Saccharomyces_cerevisiae/Ensembl/EF4
GENES=${PUBLIC}/${VER}/Annotation/Genes/genes.gtf
REF=${PUBLIC}/${VER}/Sequence/Bowtie2Index/genome

SIZE=${1:-500K}
CORES=${2:-8}

# Load standard module
ml tophat/2.1.1 bowtie/2.3.2


for prefix in WT_{C,N}R_A_${SIZE}; do
        OUT=${prefix}_n${CORES}_tophat
        [ -e $OUT ] && rm -rf $OUT
        tophat2 -p ${CORES} -G $GENES -o $OUT --no-novel-juncs $REF ${PUBLIC}/${prefix}.fastq &> ${OUT}.log
done
```

You'll notice that it aligns yeast reads against a reference and annotation that I have in my public work directory, and it takes two parameters:

- number of reads [500K] or 1M
- number of cores [8]

You can run it 500K reads on 4 cores using the commands

```
$ bash run_tophat_yeast.sh 500K 4
```

Lets see how well this scales using the `time` command, and claiming a cell in [this google doc](https://docs.google.com/spreadsheets/d/1InOXKzfxOkoxt2-P3Z_ofARJ3Ls--r-aoi5x5IPx5l8/edit?usp=sharing) to fill out.

Do you see any interesting results?

#### Explore

- Try experimenting with or reading about thread count on other tools you know
- Try using `/bin/time -v` to see if memory scales with input size
<br>
<br>

[Back - Monitoring Jobs](optimization_parallelization_02.md)
&nbsp;&nbsp;&#151;&nbsp;&nbsp;
[Next - Task Distribution](optimization_parallelization_04.md)
