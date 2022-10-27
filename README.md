# Geranium Project programs and softwares
Please download or obtain access to following before assembly and analysis

## Assembly Programs
- [Trimmomatic](https://github.com/usadellab/Trimmomatic)
- [Unicycler](https://github.com/rrwick/Unicycler)
- [SPAdes](https://github.com/ablab/spades)
- [Guppy](https://nanoporetech.com/community) - need account to download
- [Flye](https://github.com/fenderglass/Flye)
- [homopolish](https://github.com/ythuang0522/homopolish)
- [Prokka](https://github.com/tseemann/prokka)
## Metagenome Analysis Programs
- [Kraken](http://ccb.jhu.edu/software/kraken/)
- [Kaiju](https://github.com/bioinformatics-centre/kaiju)
- [Krona](https://github.com/marbl/Krona/releases)
## Whole-Genome Analysis Programs
- [Mauve](https://darlinglab.org/mauve/download.html)
- [pyani](https://github.com/widdowquinn/pyani)
## Plasmid Anylsis Programs
- [clinker](https://github.com/gamcil/clinker)
- [Proksee](https://proksee.ca/)
- [blast+](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)
- fasttree
- iqtree

## Generating core SNPs-based phylogeny

The pipeline for generation of core SNP alignment is available from 
[rknx/prok-snptree](https://github.com/rknx/prok-snptree/tree/Xhp).

The following tools and scripts were used for generating dated phylogeny
and distance matrix from raw reads:

| Function | Tools/scripts |
| --- | --- |
| Quality check for raw reads | **FᴀsᴛQC** ⇨ [src](https://github.com/s-andrews/FastQC) · [web](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) <br /> **MᴜʟᴛɪQC** ⇨ [src](https://github.com/ewels/MultiQC) · [ref](http://dx.doi.org/10.1093/bioinformatics/btw354) · [web](https://multiqc.info/) |
| Adapter identification and trimming | **ᴄᴜᴛᴀᴅᴀᴘᴛ** ⇨ [src](https://github.com/marcelm/cutadapt/) · [ref](http://dx.doi.org/10.14806/ej.17.1.200) <br /> **ᴛʀɪᴍ_ɢᴀʟᴏʀᴇ** ⇨ [src](https://github.com/FelixKrueger/TrimGalore) · [ref](https://doi.org/10.5281/zenodo.5127899) · [web](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/) |
| Genome indexing and read alignment | **ʙᴡᴀ** ⇨ [src](https://github.com/lh3/bwa) · [ref](https://doi.org/10.1093/bioinformatics/btp324) · [ref](https://doi.org/10.48550/arXiv.1303.3997) · [web](https://bio-bwa.sourceforge.net/) |
| Binary conversion and sorting | **Sᴀᴍᴛᴏᴏʟs** ⇨ [src](https://github.com/samtools/samtools) · [ref](https://doi.org/10.1093/bioinformatics/btp352) · [ref](https://doi.org/10.1093/gigascience/giab008) · [web](http://www.htslib.org/) |
| Variant calling and selection | **GATK** ⇨ [src](https://github.com/broadinstitute/gatk) · [ref](http://dx.doi.org/10.1038/ng.806) · [web](https://gatk.broadinstitute.org/) |
| SNP filtration and alignment | In-house code implemented in <br /> [rknx/prok-snptree](https://github.com/rknx/prok-snptree/tree/Xhp) |
| Dated phylogeny | **BEAST** ⇨ [src](https://github.com/beast-dev/beast-mcmc) · [ref](https://doi.org/10.1093/ve/vey016) · [web](https://beast.community/) |
| Core SNP matrix for PCA | **ʀ/ᴀᴅᴇɢᴇɴᴇᴛ** ⇨ [src](https://github.com/thibautjombart/adegenet) · [ref](https://doi.org/10.1093/bioinformatics/btr521) · [web](http://adegenet.r-forge.r-project.org/) |
| Pairwise SNP count | **FᴀsᴛᴀTᴏSNPCᴏᴜɴᴛ.sʜ** ⇨ [src](https://gist.github.com/rknx/3d3ad3b93ad963be84d7f2840486e07f) |

# Illumina assembly
## Trimmomatic
```bash
#Trimmomatic is for quality control of Illumina reads
java -jar <path to trimmomatic.jar> PE [-threads <threads] [-phred33 | -phred64] [-trimlog <logFile>] <input 1> <input 2> <paired output 1> <unpaired output 1> <paired output 2> <unpaired output 2> <step 1>
```
## Unicycler
```bash
#assembles genome from trimmomatic output. Unicycler needs SPAdes to run
for i in <input_dir>*.fastq;do N=$(basename $i .fastq); \
unicycler -s $i -o <output_dir> \
--spades_path <PATH> --no_miniasm;done
```
## Prokka
```bash
#Prokka annotates genomes
for F in <input_dir>*.fasta; do N=$(basename $F .fasta); \
prokka --outdir <PATH>$N --prefix $N --genus Xanthomonas \
--centre $N --addgenes $F; done
```

# Nanopore assembly
## Guppy
```bash
#basecalling, changes file type to fastq
cd guppy_612/ont-guppy-cpu/
ont-guppy-cpu/bin/guppy_basecaller \
-i <input_dir> \
-s <output_dir> \
-c dna_r9.4.1_450bps_fast.cfg \
--barcode_kits "SQK-RBK004" \
--trim_barcodes
```
## Flye
```bash
#flye assembles the genome with raw reads
source activate <env>
flye \
--nano-hq rawreads/genome1.fastq \
-g 4m \
-o <output_dir> \
-i 2 --plasmids
```
## homopolish
```bash
#polishes assembled genome
homopolish polish -a <input_file> -g Xanthomonas -m /homopolish/R9.4.pkl -d -o <output_dir>
```
# Metagenome Analysis
## Kraken
```bash
cd /kraken2/kraken2-2.1.2/
for i in $(ls <input_dir>*fastq.gz | grep "_R1" | cut -f 1 -d "_"); do N=$(basename $i .fastq.gz); \
./kraken2-2.1.2/kraken2 --db <kraken_db> --gzip-compressed \
--paired \
--report <output_dir>/$N.kreport \
--output <output_dir>/$N.kraken \
${i}_R1.fastq ${i}_R2.fastq;done
```
## Kaiju
```bash
cd <PATH>/kaiju/bin
./kaiju-mkbwt -n 5 -a ACDEFGHIKLMNPQRSTVWY -o <output_dir> <fasta_path>
./kaiju-mkfmi <output_dir>
for i in $(ls <raw_data_dir>*.fastq | grep "_R1" | cut -f 1 -d "_");do N=$(basename $i .fastq);\
./kaiju -t /users/PAS1552/veroman/epidemiology/kaiju/proteinsall/nodes.dmp \***
-f <output_dir>.fmi \
-i ${i}_R1.fastq -j ${i}_R2.fastq -o <kaiju_output_dir>$N.out -v -s 60 -e 10 -m 20;done
```
## Krona
```bash
#change kaiju files into kraken files
for i in <kraken_output_dir>*.out;do N=$(basename $i .out); \
./kaiju2krona -i $i -o <kraken_output_dir>/$N.krona \
-t /proteins/nodes.dmp \
-n /kaiju/proteins/names.dmp;done
#change krona files into html
cd <krona_bin_location>
for i in <kaiju_output_dir>*.krona;do N=$(basename $i .krona); \
./ktImportText -o <krona_output_dir>$N.html $i;done
```
