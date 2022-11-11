```
cd snakemake-install/
curl -L https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-Linux-x86_64.sh > mambaforge.sh
chmod +x mambaforge.sh 
./mambaforge.sh -b -p ./mambaforge.dir
source mambaforge.dir/bin/activate 
mamba create -q -y -c conda-forge -c bioconda -n snakemake snakemake snakemake-minimal
. ./mambaforge.dir/etc/profile.d/conda.sh
. ./mambaforge.dir/etc/profile.d/mamba.sh 
mamba activate snakemake
mamba install -y -c bioconda bwa
mamba install -y -c bioconda samtools

curl -L https://github.com/snakemake/snakemake-tutorial-data/archive/v5.24.1.tar.gz -o snakemake-tutorial-data.tar.gz
tar --wildcards -xf snakemake-tutorial-data.tar.gz --strip 1 "*/data" "*/environment.yaml"

cat > Snakefile <<'EOFA'
rule bwa_map:
    input:
        "data/genome.fa",
        "data/samples/A.fastq"
    output:
        "mapped_reads/A.bam"
    shell:
        "bwa mem {input} | samtools view -Sb - > {output}"
EOFA

snakemake --cores 1 mapped_reads/A.bam
```
