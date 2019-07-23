## Docker Compose
Docker compose is a tool that allows you to build, configure, and deploy
multiple containers all from a single configuration file. It is very popular
with web developers, because it allows them to separate distinct services and
work on them in isolation.

<img src="../../resources/microservice.svg" height="300">

The idea is each container provides a single service, and can be scaled up/down
as needed. Each container/service can be written in a different language, have different
dependencies, and easily be managed by different teams. This isolation is very
desirable for maintainability, but we must also be able to easily communicate between containers.
To manage container interactions we need to specify ports and mount volumes, which
can get pretty tedious as the number of containers increases. The benefit of
docker compose is it allows us define all of our services in a config file (yaml),
and spin up all of our containers at the same time.
The whole process is controlled by the config file (`docker-compose.yml`),
we'll use version 3 of the compose format in our examples, ex:

```
version: '3'
service:
  product-service:
    build: ./product
    volumes:
     - ./product:/src/app/
    ports:
     - 5001:80
  other-service:
    image: jurrutia/app:tag
    volumes:
     - ./local:/continer/loc
    ports:
     - 5000:80
    depends_on:
     - product-service
```

To find out which version of docker compose you have installed, you can run:
```
docker-compose -v
docker-compose version
```
To learn more about the commands that you can use with docker compose, you can
check the embedded help documentation:

```
docker-compose --help
```

```
docker-compose config
docker-compose build
docker-compose up
```

Compose creates a virtual network for all of the containers, so any container
in the compose file can access any other container. Making it much easier to
define and create multi-container applications.



```
docker-compose up --scale product-service=4
docker-ps
docker-compose down
```




```
git clone https://github.com/JoshuaUrrutia/docker_compose_pipeline.git
cd docker_compose_pipeline/
docker-compose config
docker-compose build
```
<!-- *Big pause (5 min)* Talk about tradeoffs here. -->
Build speed vs deployment speed vs stability vs maintainability.
Can all be competing or synergistic depending on design
```
docker-compose up
cd working/
ls
./run_containers.sh SRR8528615_C42B_siControl_R1.fastq.gz SRR8528615_C42B_siControl_R2.fastq.gz
./run_containers.sh SRR8528617_C42B_siREST_R1.fastq.gz SRR8528617_C42B_siREST_R2.fastq.gz
```
*6 min
*m1.medium (CPU: 6, Mem: 16 GB, Disk: 60 GB)

Once you're happy with the containers you can push them all to your Docker Hub so
so anyone can pull and use them:
```
docker-compose push
```

GEO:
<https://www.ncbi.nlm.nih.gov/sra?term=SRX5331845>
<https://www.ncbi.nlm.nih.gov/sra?term=SRX5331843>

So let's check REST expression as a sanity check on the experiment. Let's check the ensembl ID for
[REST](https://useast.ensembl.org/Homo_sapiens/Gene/Summary?db=core;g=ENSG00000084093;r=4:56907876-56966678).
We can see the ensembl ID is `ENSG00000084093`, so let's check REST expression
in our two alignments:
```
grep 'ENSG00000084093' star/SRR8528615_C42B_siControl/ReadsPerGene.out.tab
grep 'ENSG00000084093' star/SRR8528617_C42B_siREST/ReadsPerGene.out.tab
```

Download chromosome 4 of the human genome, and an accompanying GTF:
<https://useast.ensembl.org/info/data/ftp/index.html>


Open a web shell, and navigate to the multiQC report:
<img src="../../resources/web_desktop.png" height="300" >

Or use `scp` (secure copy) to copy the report from jestream to your machine:
```
scp $USERNAME@I$P_ADDRESS:/home/$USERNAME/docker_compose_pipeline/working/multiqc/multiqc_report_1.html .
```
