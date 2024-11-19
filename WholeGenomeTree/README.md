# Generating the Whole Genome Trees

(kSNP4.1)[https://pubmed.ncbi.nlm.nih.gov/37948764/] is a k-mer based SNP calling tool that can be used to generate whole genome trees. This tool is used to generate the whole genome trees for the S. epidermidis strains in this study.

## System Requirements

- Docker

This software was run on an AWS EC2 instance with 4 vCPUs and 16 GB of RAM on an Amazon Linux 2 AMI. Although we haven't explicitly tested it, there is no reason to believe that it wouldn't work on other common Linux distributions.

## Installation

Once you have docker installed, you can build the docker image using the following commands:

```bash
cd kSNP4.1
docker build -t ksnp:4.1 .
```

This should only take a few minutes to build.

## DEMO/Instructions for use

### Data

The data for the S. epidermidis strains is included in this repository. The `genomes` directory contains the genomes of the strains in the study. The `genomes/genome_w_plasmid` directory contains the genomes of the strains including the plasmid as well. The C.acc ATCC49725 genome as the outgroup and was obtained from NCBI directly.


### Download the C.acc ATCC49725 genome from NCBI

```bash
curl "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000159115.1/download?include_annotation_type=GENOME_FASTA" --output ncbi_dataset.zip
unzip ncbi_dataset.zip
cp ncbi_dataset/data/GCF_000159115.1/GCF_000159115.1_ASM15911v1_genomic.fna genomes/main_genome/C_acc_ATCC49725.fa
cp ncbi_dataset/data/GCF_000159115.1/GCF_000159115.1_ASM15911v1_genomic.fna genomes/genome_w_plasmid/C_acc_ATCC49725.fa
```

### For `full genome` (including plasmids)

```bash
cd pub_djenet_sepi
docker container run -it --rm -v $PWD:$PWD -w $PWD ksnp:4.1 bash
MakeKSNP4infile -indir genomes/genome_w_plasmid/ -outfile genome_w_plasmid.list
kSNP4 -in genome_w_plasmid.list -k 21 -ML -outdir genome_w_plasmid_kSNP_run
```

Once you're done, use `cmd + d` (or `ctrl + d`) to exit the container.

### Actual commands used (for Reproducibility)

```bash
cd kSNP4.1
make # This builds the docker image and adds to a private Amazon ECR repository

cd ..
docker container run -it --rm -v $PWD:$PWD -w $PWD 458432034220.dkr.ecr.us-west-2.amazonaws.com/ksnp4:20240112010805 bash
MakeKSNP4infile -indir genomes/genome_w_plasmid/ -outfile genome_w_plasmid.list
kSNP4 -in genome_w_plasmid.list -k 21 -ML -outdir genome_w_plasmid_kSNP_run
```

runtime: ~ 5 minutes
