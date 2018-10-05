

# Project #1. “What causes antibiotic resistance?” Alignment to reference, variant calling.

## Workflow:

#### 1. getting the reference sequence of the parental (unevolved, not resistant) E. coli strain 

E.coli strain K-12 substrain MG1655 from NCBI FTP (ftp://ftp.ncbi.nlm.nih.gov/genomes/genbank/bacteria/Escherichia_coli/all_assembly_versions/GCA_000005845.2_ASM584v2) - sequence in .fasta format and annotation in .gff format

#### 2. downloading raw Illumina sequencing reads 
shotgun sequencing of an E. coli strain in .fastq format. (forward and reverse, paired end run)

#### 3. Manual inspection of sequencing files

##### command : 
`head -20 amp_res_1.fastq`
	

@SRR1363257.37 GWZHISEQ01:153:C1W31ACXX:5:1101:14027:2198 length=101
GGTTGCAGATTCGCAGTGTCGCTGTTCCAGCGCATCACATCTTTGATGTTCACGCCGTGGCGTTTAGCAATGCTTGAAAGCGAATCGCCTTTGCCCACACG
+
@?:=:;DBFADH;CAECEE@@E:FFHGAE4?C?DE<BFGEC>?>FHE4BFFIIFHIBABEECA83;>>@>@CCCDC9@@CC08<@?@BB@9:CC#######
@SRR1363257.46 GWZHISEQ01:153:C1W31ACXX:5:1101:19721:2155 length=101
GTATGAGGTTTTGCTGCATTCTCTGNGCGAATATTAACTCCNTNNNNNTTATAGTTCAAAGCAAGTACCTGTCTCTTATACACATCTCCGAGCCCACGAGC
+
@@<?=D?D==?<AFGDF+AIHEACH#22<:?E8??:9??GG#0#####000;CF=C)4.==CA@@@)=7?C7?E37;3@>;;(.;>AB#############
@SRR1363257.77 GWZHISEQ01:153:C1W31ACXX:5:1101:5069:2307 length=101
GCTTCTCTTAACTGAGGTCACCATCATGCCGTTAAGTCCCTACCTCTCTTTTGCCGGTAACTGTTCCGCCGCGATTGCCTTTTATCTGTCTCTTATACACC
+
??<DBD;4C2=<BB>:AC;<CF<CE@FE9@E1C@891CD*9:?:3D@DD4?D<DD:0;@A=AEIDDA##################################
@SRR1363257.78 GWZHISEQ01:153:C1W31ACXX:5:1101:5178:2440 length=101
GCATAAGGACGATCGCTCCAGAGTAAAATAAATACGCGCATGTGATACTCACAATACCAATGGTGAAGTTACGGGACTTAAACAAACTGAGATCAAGAATC
+
CCCFFFFFHHHHHJJJJJJJJJJFFHIJJJJJJJJJJJJJJJJJJJJJJJIJHHHHHHFDEDF;AEEEEEEDDDDDBBACDDDCDDDDCCDDDDDDCCDC3
@SRR1363257.96 GWZHISEQ01:153:C1W31ACXX:5:1101:6707:2460 length=101
TCATTAAGCCGTGGTGGATGTGCCATAGCGCACCGCAAAGTTAAGAAACCGAATATTGGGTTTAGTCTTGTTTCATAATTGTTGCAATGAAACGCGGTGAA
+
CCCFFFFFHHHHHJHIIJIIIIJJJJJJGIJJJJJIJJIIGHJJJJJIIJJDHFFFFFEDACDDDCDDDDCCDDECACCDCCCDACDDDDCCDDDDDBD@A


##### command : 
`head -20 amp_res_2.fastq`

@SRR1363257.37 GWZHISEQ01:153:C1W31ACXX:5:1101:14027:2198 length=101
GATCTAAGCTGAAGCCAGGCCAAAGTTTGACGATTGGTGCAGGCAGTAGCGCACAGCGACTGGCAAACAACAGCGATAGCATTACGTATCGTGTGCGCAAA
+
???BDB:DFHBFD@9;;+A;AFGH;ABHFHHGE@9:B:??@D>@;F?D8<<F8AA9EHHD8'..;5?A?A992(',(59CC3@C>22::A238+2>B<>B<
@SRR1363257.46 GWZHISEQ01:153:C1W31ACXX:5:1101:19721:2155 length=101
GTACTTGCTTTGNACTATAATATGCACGGAGNTAATATTCGCTCAGAGAATGCAGCAAAACCTCATACCTGTCTCTTATACACATCTGACGCTGCCGACGA
+
;@@DB?B;CFBB#2<:CB:FH<C@:<A?C::#1:86:BG9:8?8688?888EBF;783)=6-7=CC;ECD);?7;;>>AE;>(5;->AC@;B@;8?#####
@SRR1363257.77 GWZHISEQ01:153:C1W31ACXX:5:1101:5069:2307 length=101
ATAATAGGCAATCGCGTCGGAACAGTTACCGGCCAAAGAGAGGCAGGGACTTAACGGCATGATGGTGACCTCAGTTAAGAGAAGCCTGTCTCTTATACACA
+
+=?;:2,+A++AC:C:2@F6:CD:B09B?4)8@''8=))8=;=((5=4@?;@6;@?@BB;(535::>:>3(::(44:@::@3((9<32+::@(4@4+:>C3
@SRR1363257.78 GWZHISEQ01:153:C1W31ACXX:5:1101:5178:2440 length=101
ATATTAACAGTAGTATCAGTTATTTCTCTGATCTCTTTAGTCATTTGGGAGTCGACCTCAGAGAACCCGATTCTTGATCTCAGTTTGTTTAAGTCCCGTAA
+
BCCFFFFFHHHHHHIJJIJJJJJIJJJJIJGIJJJJJJIJHIHJJIJIIGGGHIJIJJJIJIJJJJJJJGHHHHHFFFFFFEEEFEEED?AACCDCCDDDB
@SRR1363257.96 GWZHISEQ01:153:C1W31ACXX:5:1101:6707:2460 length=101
GTTTCACCGCGTTTCATTGCAACAATTATGAAACAAGACTAAACCCAATATTCGGTTTCTTAACTTTGCGGTGCGCTATGGCACATCCACCACGGCTTAAT
+
CCCFFFFFHGHHHJIJJJJJJIJJJJJJIJIJJIJJIJIJJJJJJJJIJJFHIIJFIGJJJGIHHHHHGFFDDDDDDDDDDDDDDDDDDDABDDDDDDDCD

#### 4. Counting reads

##### command: 
`wc -l amp_res_1.fastq`  - counts words (option "lines")
1823504 amp_res_1.fastq
reads: 455876 (=1823504/4 - four lines per read)

##### command: 
`wc -l amp_res_2.fastq`
1823504 amp_res_2.fastq
reads: 455876

#### 5. Inspecting raw sequencing data with fastqc.

##### command [fastqc -o . amp_res_1.fastq amp_res_2.fastq]
возникли проблемы с выполнением команды - на выход выдавала два битых архива
проблему удалось решить только прямым запуском fastqc из папки программы

Количество ридов совпало с рассчитанным вручную

html-files amp_res_1_fastqc.html and amp_res_2_fastqc.html were added to the repository

##### Results:
amp_res_1_fastqc.html

|*Measure*|*Value*|
|---|---|
|Filename | amp_res_1.fastq|
|File type | Conventional base calls|
|Encoding | Sanger / Illumina 1.9|
|Total Sequences | 455876|
|Sequences flagged as poor quality|	0 |
|Sequence length | 101 |
|%GC | 50|

######Per base and per tile sequence quality is poor!

amp_res_2_fastqc.html

|*Measure*|*Value*|
|---|---|
|Filename | amp_res_2.fastq|
|File type | Conventional base calls|
|Encoding | Sanger / Illumina 1.9|
|Total Sequences | 455876|
|Sequences flagged as poor quality|	0 |
|Sequence length | 101 |
|%GC | 50|

###### Only per base sequence quality is poor

#### 6.  Filtering the reads using Trimmomatic

Encoding is Sanger/Illumina 1.9, so we'll use phred33

Run Trimmomatic in paired end mode, with following parameters:
Cut bases off the start of a read if quality below 20
Cut bases off the end of a read if quality below 20
Trim reads using a sliding window approach, with window size 10 and average quality  within the window 20. 
Drop the read if it is below length 20.

##### command:
`TrimmomaticPE -phred33 amp_res_1.fastq amp_res_2.fastq output_forward_paired.fq.gz output_forward_unpaired.fq.gz output_reverse_paired.fq.gz output_reverse_unpaired.fq.gz LEADING:20 TRAILING:20 SLIDINGWINDOW:10:20 MINLEN:20`

RESULT: TrimmomaticPE: Started with arguments:
 -phred33 amp_res_1.fastq amp_res_2.fastq output_forward_paired.fq.gz output_forward_unpaired.fq.gz output_reverse_paired.fq.gz output_reverse_unpaired.fq.gz LEADING:20 TRAILING:20 SLIDINGWINDOW:10:20 MINLEN:20
Multiple cores found: Using 4 threads
Input Read Pairs: 455876 Both Surviving: 446259 (97.89%) Forward Only Surviving: 9216 (2.02%) Reverse Only Surviving: 273 (0.06%) Dropped: 128 (0.03%)
TrimmomaticPE: Completed successfully

Manual check:
##### command: 
`wc -l output_forward_paired.fq `
output divided by four: 446259.0

##### command: 
`wc -l output_reverse_paired.fq`
output divided by four: 446259.0

FastQC analysis was repeated, per base sequence quality has improved significantly





#### 7. Aligning sequences to reference
*Indexing the reference file*
  
`bwa index GCA_000005845.2_ASM584v2_genomic.fna`

We've got 5 new files: 
bwa index GCA_000005845.2_ASM584v2_genomic.fna.amb  
bwa index GCA_000005845.2_ASM584v2_genomic.fna.ann  
bwa index GCA_000005845.2_ASM584v2_genomic.fna.bwt  
bwa index GCA_000005845.2_ASM584v2_genomic.fna.pac  
bwa index GCA_000005845.2_ASM584v2_genomic.fna.sa  

*Alignment of trimmed reads*

`bwa mem GCA_000005845.2_ASM584v2_genomic.fna ../pairing\ reads/output_forwa
rd_paired.fq ../pairing\ reads/output_reverse_paired.fq > ../alignment/alignment.sam`

output were redirected to the file "alignment.sam"

compressing of a sam-file (converting to a bam-file) 
`samtools view -S -b alignment.sam > alignment.bam`
getting basic statistics:
`samtools flagstat alignment.bam`


892776 in total (QC-passed reads + QC-failed reads)  
0 duplicates  
*_891649  mapped (99.87%:-nan%)_*  
892776 paired in sequencing  
446402 read1  
446374 read2  
888732 properly paired (99.55%:-nan%)  
890667 with itself and mate mapped  
982 singletons (0.11%:-nan%)  
0 with mate mapped to a different chr  
0 with mate mapped to a different chr (mapQ>=5)  

_99.87% of reads were mapped (891649 out of 892776)_

*Sorting and indexing BAM file*

Sorting bam file by sequence coordinate on reference:
`samtools sort alignment.bam alignment_sorted`

Indexing bam file for faster search:
`samtools index alignment_sorted.bam`

results of the alignment were visualised in IGV (Integrative Genomics Viewer)
Genomes -> Load Genome from file (choose reference)
File -> Load from File (choose sorted bam-file)

#### 8. Variant calling

we're going to see how many reads have a mutation at the same position - we need to distinguish actual mutations from the sequencing errors using an mpileup, which goes through each position and “piles up” the reads, tabulating the number of bases that match or don’t match the reference. 

Mpileup requires a sorted, indexed bam file.

`samtools mpileup -f ../raw_data/GCA_000005845.2_ASM584v2_genomic.fna alignment_sorted.bam >  my.mpileup`

To call actual variants, we will be using a program called VarScan (variant scanner). 

`varscan mpileup2snp my.mpileup --min-var-freq 0.9 --variants --output-vcf 1 > VarScan_results_90%.vcf`

Only SNPs will be reported  
Warning: No p-value threshold provided, so p-values will not be calculated  
Min coverage:	8  
Min reads2:	2  
Min var freq:	0.9  
Min avg qual:	15  
P-value thresh:	0.01  

#### 9. Variant effect prediction

Vcf file and annotation in gff format were added as "tracks" in IGV browser.
 

Three mutations were found and explored, two of them occur in genes
find whether each mutation occurs in a gene, 
whether it is missense (changes the amino acid sequence),
nonsense (introduces a frameshift or early stop codon) 
synonymous (no amino acid change). 

The following variants were identidied:

93043  
C to G  
ftsI  
Missense  
Alanine to Glycine  
  
482698  
A to T  
acrB  
Missense  
Glutamate to Leucine  

852762  
A to G  
Non-coding region  
—
  
3535147  
T to G  
envZ  
Missense  
Valine to Glycine  

4390754  
C to A  
rsgA  
Synonymous  























