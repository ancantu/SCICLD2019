# Practical Example: Human SNPs

In this session, will be trying out many of the topics you learned this week.

## What tasks do you recall from our sessions?
* VMs?
* docker containers?
* git repos?
* Jupyter notebooks?

## We’re going to practice putting all of these together!

## Tasks and Goals:
* Given 23andMe data, identify ethnigraphics haplotypes from the Y-chromosome and MT-DNA using containerized software
* Present in Jupyter notebook
  * y-haplotypes
  * alignment tree
  * mt-haplotypes
  * graph of MT-SNP

---

### 23andMe DNA service
* www.23andMe.com
* clients spit in tube
* 23andMe collects cellular debris
* Extra DNA:
   * `SNPs`: **Single Nucleotide Polymorphisms**
      * \> 90% of human DNA is identical!!!
      * We've identified regions of DNA known for being spots where the human DNA varies at often only 1 DNA nucleotide.
      * Our DNA is inherited from our parents and ancestors with them passing along the occasional mutation to their descendants, often at these SNPs, but unlike Random mutations, SNPs show **inheritance patterns** that can be correlated with both ethnicity and biological function/health.
      * **Ethnographically**, we can observe/track changes in population movement and distribution of humans by monitoring SNP patterns.
        * Some SNPs (or variance within) correlate greatly with past and present ethinicities and can be used to make suppositions about one's ancestral origins.
        * [National Geo2.0 project](https://genographic.nationalgeographic.com/tree-updates/)<br> ---
      * **Anyone used 23andMe?**
        * I'll show you where to download your own data at www.23andMe.com
        * For this exercise, will be using my SNP data partly since, as a male, I have both Y-DNA and mt-DNA data available and partly to avoid HIPAA issues (since I am here to consent to their use).
        * You're welcome to use your own DATA as we go along, but be careful with your privacy.

---

### Task 0: Get 23andMe data
* 23andMe data
* We could go over to find the data at 23andMe
  * However, this can take some time, so I'll make mine available via dropbox.
  * Let all open our VMs that we left running all night.
  * create a directory called `beck_data`.
    * ```mkdir beck_data```
  * Eventually we'll be getting my data from dropbox using:
    * ```wget https://www.dropbox.com/sh/FAKEZlsztbzucox8ktc/AAAeMk62h9LqJvSWlY88v0PLZEKAF/genome_Brian_Beck_v2_v3_Full_20190722220903.txt```
    * **The above link is purposely broken** so that my data isn't publically available. I'll be telling you verbally how to de-obfuscate the link.

---

### Task 1: Y-haplotype data

* We'll be using David Poznik's `yhaplo` software
   * git clone https://github.com/23andMe/yhaplo.git
    * **JUST FOLLOW ALONG FOR THE MOMENT**
    * I'll be showing you the basic install steps, but as a class, we'll be using github and docker in a moment.

#### STEPS
* git clone https://github.com/23andMe/yhaplo.git
* fix the PATH
* copy my 23andMe data to a new file name that yhaplo likes (\*.23andMe.txt)
* use `convert2genos.py` to convert DATA to genos format
* FINALLY I could run:
  * ```callHaplogroups.py -i converted/genome_Brian_Beck_v2_v3_Full_20190722220903.genos.txt -ta -c -hp -hpd -ds -dsd -as -asd```

**remember outputs** :
* `Y-haplotype`:
  * haplogroups.genome_Brian_Beck_v2_v3_Full_20190722220903.txt
* `Y-haplotype and SNP that determine it`:
  * paths.genome_Brian_Beck_v2_v3_Full_20190722220903.txt
* `list of SNP, haplogroups, and mutations`:
  * derived.snps.detail.genome_Brian_Beck_v2_v3_Full_20190722220903.txt

### INSTEAD, let's go get a  GIT repo that repeats all these steps to build a Docker container:
* **yes, do these steps now**
* ```git clone https://github.com/beckbw/yhaplorun.git```
* ```sudo mv /usr/bin/docker-credential-secretservice /usr/bin/docker-credential-secretservice.orig```
* ```docker login```
* ```cd yhaplorun```
* ```docker build -t beckbw/yhaplorun .```
* copy the genome_Brian_Beck_v2_v3_Full_20190722220903.txt file from `beck_data`
* ```docker run --rm -it -v ${PWD}:/tmp/work beckbw/yhaplorun /tmp/scripts/run_yhaplo.sh genome_Brian_Beck_v2_v3_Full_20190722220903.txt```
* results on in root owned directly `yhaplo`

---

### Task 2: Mitochondrial Haplotypes
* Mt-haplotype software:
* **haplogrep** : by Sebastian Schoenherr

### Source:
* git clone https://github.com/seppinho/haplogrep-cmd
 * **JUST FOLLOW ALONG FOR THE MOMENT**
 * I'll be showing you the basic install steps, but as a class, we'll be using github and docker in a moment.

#### STEPS
* create a `haplogrep` directory and get the software:
* wget https://github.com/seppinho/haplogrep-cmd/releases/download/v2.1.24/haplogrep-2.1.24.jar
  * oops! It's JAVA-based and requires GraphViz for plotting so we'll need to install those.
* `haplogrep` requires 23andMe files be converted to a `VCF` formatted file.
  * we'll use `bcftools` for converting:
  * `bcftools` requires **human reference sequences**:
   * Version 37 of the Human References is available from `Ensembl`
   * wget ftp://ftp.ensembl.org/pub/grch37/current/fasta/homo_sapiens/dna/Homo_sapiens.GRCh37.dna.primary_assembly.fa.gz
   * `bcftools` doesn't like the way the files are compressed so we'll need to uncompress them.
     * gunzip Homo_sapiens.GRCh37.dna.primary_assembly.fa.gz
  * `bcftools convert --tsv2vcf genome_Brian_Beck_v2_v3_Full_20190722220903.txt -f Homo_sapiens.GRCh37.dna.primary_assembly.fa -s genome_Brian_Beck -Ob -o genome_Brian_Beck.vcf.gz -O z`

  * **at this point the JAVA haplogrep would be run if we were doing it manually**
  * `java -jar haplogrep-2.1.24.jar --in ../beck_data/genome_Brian_Beck.vcf.gz --format vcf --out genome_Brian_Beck_haplogroups.txt --lineage > haplogrep.log &`

**remember outputs**:
* `MT-haplogroup`:
  * genome_Brian_Beck_haplogroups.txt
* `MT-haplogroup image`:
  * genome_Brian_Beck_haplogroups.dot
* Now convert `dot` to `PNG` using `GraphViz`:
  * `dot genome_Brian_Beck_haplogroups.dot -Tpng > genome_Brian_Beck_haplogroups.png`

### INSTEAD, let's go get a  GIT repo that repeats all these steps to build a Docker container:
* **yes, do these steps now**
* ```git clone https://github.com/beckbw/haplogreprun.git```
* ```cd haplogreprun```
* ```docker build -t beckbw/haplogreprun .```
* copy the genome_Brian_Beck_v2_v3_Full_20190722220903.txt file from `beck_data`
* ```docker run --rm -it -v ${PWD}:/tmp/work beckbw/haplogreprun /tmp/scripts/run_haplogrep.sh genome_Brian_Beck_v2_v3_Full_20190722220903.txt```

---

### Task3 : Display specific elements in a Jupyter Notebook
* We're going to make use of a `fork` of `Erik`\'s lesson from yesterday
  * Add in a few libraries for our Haplotype purposes
    * `biopython`, `graphviz`, `networkx`, and `ete3`
  * Add a speciality `Haplotypes_23andMe.ipynb` notebook
  * We're stil going to use `Erik`\'s instructions and **nearly** identical Dockerfile to build the Jupyter Notebook container.
   * Note how similar, **on purpose**, the processes are.

### Steps
* **yes, do these steps now**
* ```git clone https://github.com/beckbw/jupyter_notebook```
* ```cd jupyter_notebook```
* ```docker build -t beckbw/jupyter_notebook .```
 * You'll get an `_chgrp: Operation not permitted` error, because I made a mistake, but we'll work around that.
* ```docker run -p 80:8888 -v $PWD/data:/home/jupyter/work beckbw/jupyter_notebook:latest```
* as yesterday, copy-n-paste the URL string, with a change that's appropriate for your IP address

### Moving Data and Compensating for mistake:
* For each of the two previous tasks, move the files I mentioned in each section plus to the `jupyter_notebook/data` folder
  * (we're using `sudo` because of root permission on the output files we haven't dealt with yet):
  * ``` sudo cp ../yhaplorun/yhaplo/* data/```
  * ``` sudo cp ../haplogeprun/haplogreb/* data/```
  * ``` sudo cp Haplotypes_23andMe.ipynb data/```

* Now let's load the notebook and see if our data pops up.
* Let's Play-with and Discuss our notebooks for a moment.

---

### Questions: Could we have used other ways to have the data moved or to even launch and run the containers all at once?
* What approaches did `Joshua` show us that might have been effective?
* What if I went in using my Jetstream Admin privileges and deleted you VMs?
  * Could you restore functionality yourself?
    * If `yes`, could you restore it quickly?
  * For both, `Why or why not?`
  * How do you think VMs, Git Repos, and Docker containers play a role in reproducibility?

  ---

  <br>

  Top: [Course Overview](../../index.md)
