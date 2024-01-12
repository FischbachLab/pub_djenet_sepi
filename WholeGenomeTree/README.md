# Generating the Whole Genome Trees

## Download the C.acc ATCC49725 genome from NCBI

```bash
curl "https://api.ncbi.nlm.nih.gov/datasets/v2alpha/genome/accession/GCF_000159115.1/download?include_annotation_type=GENOME_FASTA" --output ncbi_dataset.zip
unzip ncbi_dataset.zip
cp ncbi_dataset/data/GCF_000159115.1/GCF_000159115.1_ASM15911v1_genomic.fna genomes/main_genome/C_acc_ATCC49725.fa
cp ncbi_dataset/data/GCF_000159115.1/GCF_000159115.1_ASM15911v1_genomic.fna genomes/genome_w_plasmid/C_acc_ATCC49725.fa
```

## Create the kSNP4 Docker image if not already created

```bash
cd kSNP4.1/dockerfiles
docker build -t ksnp4:20240112010805 .
```

## For `chromosome` only (data not included in this repo)

```bash
cd /mnt/efs/scratch/sunit/Github/FischbachLab/s_epi
docker container run -it --rm -v $PWD:$PWD -w $PWD ksnp4:20240112010805 bash
MakeKSNP4infile -indir genomes/main_genome/ -outfile main_genome.list
kSNP4 -in main_genome.list -k 21 -ML -outdir kSNP_run
```

## For `full genome` (including plasmid)

```bash
cd /mnt/efs/scratch/sunit/Github/FischbachLab/s_epi
docker container run -it --rm -v $PWD:$PWD -w $PWD ksnp4:20240112010805 bash
MakeKSNP4infile -indir genomes/genome_w_plasmid/ -outfile genome_w_plasmid.list
kSNP4 -in genome_w_plasmid.list -k 21 -ML -outdir kSNP_run
```