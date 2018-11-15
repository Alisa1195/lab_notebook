## Project 3

##### 1. Exploring the dataset. FastQC

SRR292678 - paired end, insert size 470 bp

#####forward: 
reads: 5499346
length:90
GC(%): 49
#####reverse: 
reads: 5499346
length:90
GC(%): 49

SRR292862 – mate pair, insert size 2 kb

#####forward: 
reads: 5102041
length: 49
GC(%): 50
Kmer content !
#####reverse: 
reads: 5102041
length:49
GC(%): 49
Kmer content !


SRR292770 – mate pair, insert size 6 kb

#####forward: 
reads: 5102041
length: 49
GC(%): 50
#####reverse: 
reads: 5102041
length:49
GC(%): 49


Two paired files: SRR292678sub_S1_L001_R1_001.fastq and SRR292678sub_S1_L001_R2_001.fastq were merged with a following command:
`cat SRR292678sub_S1_L001_R1_001.fastq SRR292678sub_S1_L001_R2_001.fastq > reads_merged`

`jellyfish count -m 31 -s 70 ./SRR292678sub_S1_L001_R1_001.fastq`
`jellyfish histo mer_counts.jf -o mer_counts_histo.csv`


N = (M*L)/(L-K+1)
Genome_size = T/N
(N: Depth of coverage, M: Kmer peak, K: Kmer-size, L: avg read length T: Total bases)

T = 5499346*90 = 494941140
N = (65*90)/(90-31+1) = 97.5
Genome_size: 5076319 (5,1 Gb)

##### Assembling
Running SPAdes on the paired-end reads:
`spades.py --pe1-12 reads_merged.fastq -o spades_test`

##### Quality assessment (QUAST)

single-library:
contigs: 210
N50: 111860


Three libraries:  
contigs:105  
N50: 335515  


#####  locating 16S rRNA in the assembled E. coli X genome

`barrnap scaffolds.fasta`
16s_rna has been found:


NODE_81_length_5219_cov_487.144_ID_20744  
barrnap:0.7	rRNA	3533	5070	0  
Name=16S_rRNA;product=16S ribosomal RNA  

##### searching for the genome in the RefSeq database (BLAST)

LOCUS       NC_011748            5154862 bp    DNA     circular CON 22-FEB-2017  
DEFINITION  Escherichia coli 55989 chromosome, complete genome.  
ACCESSION   NC_011748   

##### Alignment of E.coli X genome to the reference (Mauve)

root alignment has 35 superintervals  
root alignment length: 5738936  
Organisms have 50.4% GC  

Searching for shiga-toxin related genes using Sequence Navigator in Mauve   
Two shiga-toxin related genes were found in the strain:  
stxB 3483605-3483874 length:269  
stxA 3483886-3484845 length:969  

##### Tracing the source of toxin genes in E. coli X

transposase from transposon Tn916 and Phage protein




