# Using Nextflow for EM-Seq

author: Shelly Wannamaker
see https://shellywanamaker.github.io/400th-post/


## Copy genome files to klone

```
# show path
pwd
/gscratch/srlab/strigg/GENOMES

# copy genome
wget https://owl.fish.washington.edu/halfshell/genomic-databank/Pocillopora_meandrina_HIv1.assembly.fasta

#copy gtf file
wget https://github.com/urol-e5/timeseries_molecular/raw/d5f546705e3df40558eeaa5c18b122c79d2f4453/F-Ptua/data/Pocillopora_meandrina_HIv1.genes-validated.gtf

# copy gff file
wget https://github.com/urol-e5/timeseries_molecular/raw/d5f546705e3df40558eeaa5c18b122c79d2f4453/F-Ptua/data/Pocillopora_meandrina_HIv1.genes-validated.gff3
```


## Copy WGBS to klone

```
# open screen session
screen -r methylseq

# start interactive node
salloc -A srlab -p cpu-g2-mem2x -N 1 -c 1 --mem=16GB --time=16:00:00

# copy data 
rsync --progress --verbose --archive shellytrigg@gannet.fish.washington.edu:/volume2/web/gitrepos/urol-e5/timeseries_molecular/F-Ptua/output/01.00-F-Ptua-WGBS-trimming-fastp-FastQC-MultiQC/*.gz /gscratch/scrubbed/strigg/analyses/20250421_methylseq
```


## Run Methlyseq pipeline

```
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
