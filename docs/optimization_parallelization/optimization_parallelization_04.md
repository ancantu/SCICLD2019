# Task Distribution

One of the most convenient and powerful tools that we have at TACC is [Launcher](https://github.com/TACC/launcher). Launcher is a simple way to work through a list of single-node tasks. In addition to scheduling tasks on a single node, Launcher can also spawn tasks on other nodes allocated to the job (this is how you can *really* take advantage of TACC).
This is good news for everyone that needs to complete a lot of work because every time you submit a job, your priority goes down.
If you can bundle all of your work into one large job, you will not only complete a lot of work, you will optimize your scheduling priority.

Launcher works through a text file and executes each line as a separate task. Inside each line

- Output can be piped
- Commands can be chained
- `if/else` works
- Your original environment is preserved

Lets begin by making a program that writes the task ID and the hostname it was run from.

```
for task in {1..10}
do
        echo "echo \"This is task-${task} on \$(hostname)\""
done
```

Notice that we escape the inside double-quote with a slash (`\"`).
If all the quotes are correct, your output should look like

```
echo "This is task-1 on $HOSTNAME"
...
echo "This is task-10 on $HOSTNAME"
```

`$HOSTNAME` will resolve when the commands are run again.
Now, redirect this to a file called `commandList` so we can run it with launcher.

```
for task in {1..10}
do
        echo "echo \"This is task-${task} on \$(hostname)\""
done > commandList
```
### Single node tasks

We can turn this into a launcher job by creating the SLURM submission script `launcher_test_single.sh`

```
#!/bin/bash
#SBATCH -J test_launcher
#SBATCH -o test_launcher.%j.o
#SBATCH -e test_launcher.%j.e
#SBATCH -p skx-normal
#SBATCH --mail-user=email@server.com
#SBATCH --mail-type=begin
#SBATCH --mail-type=end
#SBATCH -t 2:00:00
#SBATCH -A TRAINING-OPEN
#SBATCH -N 1
#SBATCH -n 1

module load launcher

for task in {1..10}; do
        echo "echo \"This is task-${task} on \$(hostname)\""
done > commandList

export LAUNCHER_PLUGIN_DIR=${LAUNCHER_DIR}/plugins
export LAUNCHER_RMI=SLURM
export LAUNCHER_JOB_FILE=commandList

$LAUNCHER_DIR/paramrun
```

You can also copy this file

```
$ cp /work/03076/gzynda/stampede2/ctls-public/launcher_test_single.sh .
```

Then submit the job script to your reservation

```
sbatch --reservation=LF_18_WEDNESDAY launcher_test_single.sh
```

This job will run the 10 tasks, 1 at a time (`-n 1`) on a single node (`-N 1`).
The output from this job can be viewed at

- `test_launcher.*.o` - stdout
- `test_launcher.*.e` - stderr

#### Explore (5 minutes)

- Try piping each command to a file (make sure you don't overwrite files!!)

### Running tasks on multiple nodes

Launcher also makes it easy to take any workflow and scale it out to multiple compute nodes.
Assuming you have enough tasks, you simply need to change:

- `-N` - Number of nodes to used
- `-n` - Total concurrent tasks

Lets copy our single-node submission script to the new file `launcher_test_double.sh`

```
$ cp launcher_test_single.sh launcher_test_double.sh
```

and modify the `SBATCH` header to use two nodes and two tasks.

```
#!/bin/bash
#SBATCH -J test_launcher2
#SBATCH -o test_launcher2.%j.o
#SBATCH -e test_launcher2.%j.e
#SBATCH -p skx-normal
#SBATCH --mail-user=email@server.com
#SBATCH --mail-type=begin
#SBATCH --mail-type=end
#SBATCH -t 2:00:00
#SBATCH -A TRAINING-OPEN
#SBATCH -N 2
#SBATCH -n 2

module load launcher

for task in {1..10}; do
        echo "echo \"This is task-${task} on \$(hostname)\""
done > commandList

export LAUNCHER_PLUGIN_DIR=${LAUNCHER_DIR}/plugins
export LAUNCHER_RMI=SLURM
export LAUNCHER_JOB_FILE=commandList

$LAUNCHER_DIR/paramrun
```

and then submit

```
$ sbatch --reservation=LF_18_WEDNESDAY launcher_test_double.sh
```

#### Explore (10 minutes)

- Adapt this to run our sweep of `run_tophat_yeast.sh` across two nodes (`-N 2`), and two tasks per node (`-n 4`)
  - both 1M and 500K reads
  - {4, 8, 12, 24} cores

### Workflows

Launcher blocks when it runs, so you can have multiple parallel sections in your workflow.

```
#!/bin/bash
#SBATCH -J test_launcher2
#SBATCH -o test_launcher2.%j.o
#SBATCH -e test_launcher2.%j.e
#SBATCH -p skx-normal
#SBATCH --mail-user=email@server.com
#SBATCH --mail-type=begin
#SBATCH --mail-type=end
#SBATCH -t 2:00:00
#SBATCH -A TRAINING-OPEN
#SBATCH -N 2
#SBATCH -n 2

module load launcher

# Commands for section 1
for task in {1..10}; do
        echo "echo \"This is s1-task-${task} on \$(hostname)\""
done > commandList1

export LAUNCHER_PLUGIN_DIR=${LAUNCHER_DIR}/plugins
export LAUNCHER_RMI=SLURM
export LAUNCHER_JOB_FILE=commandList1
$LAUNCHER_DIR/paramrun

# Commands for section 2
for task in {1..10}; do
        echo "echo \"This is s2-task-${task} on \$(hostname)\""
done > commandList2

export LAUNCHER_JOB_FILE=commandList1
$LAUNCHER_DIR/paramrun
```

#### Explore (10 minutes)

Use both

- /work/03076/gzynda/stampede2/ctls-public/run_cufflinks_yeast.sh
- /work/03076/gzynda/stampede2/ctls-public/run_tophat_yeast.sh

to run the tophat/cufflinks workflow on both 500K and 1M reads using launcher in a single sbatch script. The shortest job wins!

```
#!/bin/bash
#SBATCH -J test_launcher
#SBATCH -o test_launcher.%j.o
#SBATCH -e test_launcher.%j.e
#SBATCH -p skx-normal
#SBATCH --mail-user=gzynda@tacc.utexas.edu
#SBATCH --mail-type=begin
#SBATCH --mail-type=end
#SBATCH -t 2:00:00
#SBATCH -A TRAINING-OPEN
#SBATCH -N 1
#SBATCH -n 4

# Load standard module
module load launcher
ml tophat/2.1.1 bowtie/2.3.2 cufflinks/2.2.1

# Hardcode cores
CORES=8

# Input paths
PUBLIC=/work/03076/gzynda/stampede2/ctls-public
VER=Saccharomyces_cerevisiae/Ensembl/EF4
GENES=${PUBLIC}/${VER}/Annotation/Genes/genes.gtf
REF=${PUBLIC}/${VER}/Sequence/Bowtie2Index/genome

# Launcher variables
export LAUNCHER_PLUGIN_DIR=${LAUNCHER_DIR}/plugins
export LAUNCHER_RMI=SLURM

###########################################
# Tophat section
###########################################

for prefix in WT_{C,N}R_A_{500K,1M}; do
	# Define out folder
	OUT=${prefix}_n${CORES}_tophat
	[ -e $OUT ] && rm -rf $OUT
	echo "tophat2 -p ${CORES} -G $GENES -o $OUT --no-novel-juncs $REF ${PUBLIC}/${prefix}.fastq &> ${OUT}.log"
done > tophatCommands

# Run launcher section
export LAUNCHER_JOB_FILE=tophatCommands
$LAUNCHER_DIR/paramrun

###########################################
# Cufflinks section
###########################################

# four tasks, so use quarter of node
CORES=12

#### Run cufflinks
for PRE in WT_{C,N}R_A_{500K,1M}_n${CORES}; do
	OUT=${PRE}_links
	[ -e ${OUT} ] && rm -rf ${OUT}
	echo "cufflinks -p $CORES -o ${OUT} -G $GENES ${PRE}_tophat/accepted_hits.bam &> ${OUT}.log"
done > cufflinksCommands

# Run launcher section
export LAUNCHER_JOB_FILE=cufflinksCommands
$LAUNCHER_DIR/paramrun

# only two tasks, so use half of node
CORES=24

#### Merge results
for SIZE in 500K 1M; do
	MERGE=${SIZE}_n${CORES}_merge
	[ -e ${MERGE}.txt ] && rm ${MERGE}.txt
	for PRE in WT_{C,N}R_A_${SIZE}_n${CORES}; do
		echo "${PRE}_links/transcripts.gtf" >> ${MERGE}.txt
	done
	[ -e ${MERGE} ] && rm -rf ${MERGE}
	echo "cuffmerge -p $CORES -g $GENES -s ${REF}.fa -o ${MERGE} ${MERGE}.txt &> ${MERGE}.log"
done > cuffmergeCommands

# Run launcher section
export LAUNCHER_JOB_FILE=cuffmergeCommands
$LAUNCHER_DIR/paramrun

#### Cuffdiff
SUFF="_tophat/accepted_hits.bam"
for SIZE in 500K 1M; do
	DIFF=${SIZE}_n${CORES}_diff
	MERGE=${SIZE}_n${CORES}_merge
	echo "cuffdiff -L CR,NR -p $CORES ${MERGE}/merged.gtf -o ${DIFF} ${CR}${SUFF} ${NR}${SUFF} &> ${DIFF}.log"
done > cuffdiffCommands

# Run launcher section
export LAUNCHER_JOB_FILE=cuffdiffCommands
$LAUNCHER_DIR/paramrun
```

### Launcher Limitations

Launcher is great, but it has two limitations that you may eventually run into.

- It cannot dynamically add tasks to its work queue
- All commands are executed in the `sh` (Bourne) shell, not `bash`, so your commands can't be too fancy
<br>
<br>

[Back - Parallelization](optimization_parallelization_03.md)
&nbsp;&nbsp;&#151;&nbsp;&nbsp;
[Next - Optimization](optimization_parallelization_05.md)
