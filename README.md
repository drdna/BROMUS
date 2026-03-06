# Methods and code for characterizing EPSPS gene amplifications

## Align reads against reference genomes using minimap2 script
```bash
sbatch $script minimap2.sh S1_reference.fasta S1_reads.fastq.gz
sbatch $script minimap2.sh S1_reference.fasta S6_reads.fastq.gz
sbatch $script minimap2.sh S6_reference.fasta S6_reads.fastq.gz
```

## Extract Alignments for contigs/regions of interest
```
samtools view -b S1.S1_reference.sorted.bam "contig_4:77000000-78000000" -o S1.contig4_77k-78k.bam
```
## Visualize alignments in IGV to check for assembly errors



## Identify EPSPS contigs:
```
blastn -query EPSPS_cds.fasta -subject S1_reference.fasta -outfmt 6 | awk '$3 > 98 &&' > EPSPS.S1.BLAST
blastn -query EPSPS_cds.fasta -subject S6_reference.fasta -outfmt 6 | awk '$3 > 98 &&' > EPSPS.S6.BLAST
```

## Align S6 EPSPS contigs against S1 EPSPS contigs

1. Extract contigs of interest to speed up searching:
```
samtools faidx S1_reference.fasta contig_4 > S1_Ctg4.fasta
```
2. Use FACET to align assembled sequences and output gff alignments for IGV visualization:
```
FACET db_free S1_Ctg4.fasta S6_Ctg58.fasta -nc -g -x
```
-nc    do not clean redundant alignments (useful for detecting/visualizing repeat sequences)
-gff   output in gff format
-x     do not generate .csv report

3. Visualize alignments in IGV:

4. 
