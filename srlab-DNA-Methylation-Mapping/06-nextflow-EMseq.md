# Using Nextflow for EM-Seq

Author: Shelly Wannamaker  
See: [https://shellywannamaker.github.io/400th-post/](https://shellywannamaker.github.io/400th-post/)

---

## Step 1: Copy Genome Files to Klone
```bash
# show path
pwd
/gscratch/srlab/strigg/GENOMES

# copy genome
wget https://owl.fish.washington.edu/halfshell/genomic-databank/Pocillopora_meandrina_HIv1.assembly.fasta

# copy gtf file
wget https://github.com/urol-e5/timeseries_molecular/raw/d5f546705e3df40558eeaa5c18b122c79d2f4453/F-Ptua/data/Pocillopora_meandrina_HIv1.genes-validated.gtf

# copy gff file
wget https://github.com/urol-e5/timeseries_molecular/raw/d5f546705e3df40558eeaa5c18b122c79d2f4453/F-Ptua/data/Pocillopora_meandrina_HIv1.genes-validated.gff3
```

---

## Step 2: Copy WGBS Data to Klone
```bash
# open screen session
screen -r methylseq

# start interactive node
salloc -A srlab -p cpu-g2-mem2x -N 1 -c 1 --mem=16GB --time=16:00:00

# copy data
rsync --progress --verbose --archive shellytrigg@gannet.fish.washington.edu:/volume2/web/gitrepos/urol-e5/timeseries_molecular/F-Ptua/output/01.00-F-Ptua-WGBS-trimming-fastp-FastQC-MultiQC/*.gz /gscratch/scrubbed/strigg/analyses/20250421_methylseq
```

---

## Step 3: Run the MethylSeq Pipeline
```bash
# activate conda environment
mamba activate nextflow

# OR this path if needed
# mamba activate /gscratch/srlab/nextflow/bin/miniforge/envs/nextflow

nextflow run nf-core/methylseq \
  -c /gscratch/srlab/strigg/bin/uw_hyak_srlab.config \
  --input /gscratch/scrubbed/strigg/analyses/20250422_methylseq/samplesheet.csv \
  --outdir /gscratch/scrubbed/strigg/analyses/20250422_methylseq \
  --fasta /gscratch/srlab/strigg/GENOMES/Pocillopora_meandrina_HIv1.assembly.fasta \
  --em_seq \
  -resume \
  -with-report nf_report.html \
  -with-trace \
  -with-timeline nf_timeline.html \
  --skip_trimming \
  --nomeseq
```
