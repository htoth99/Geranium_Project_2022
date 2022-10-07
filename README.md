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

# Illumina assembly
## Trimmomatic
```bash
#Trimmomatic is for quality control of Illumina reads
java -jar <path to trimmomatic.jar> PE [-threads <threads] [-phred33 | -phred64] [-trimlog <logFile>] <input 1> <input 2> <paired output 1> <unpaired output 1> <paired output 2> <unpaired output 2> <step 1>
```
## Unicycler
```
#assembles genome from trimmomatic output. Unicycler needs SPAdes to run
for i in <input_dir>*.fastq;do N=$(basename $i .fastq); \
unicycler -s $i -o <output_dir> \
--spades_path <PATH> --no_miniasm;done
```
## Prokka
```
#Prokka annotates genomes
for F in <input_dir>*.fasta; do N=$(basename $F .fasta); \
prokka --outdir <PATH>$N --prefix $N --genus Xanthomonas \
--centre $N --addgenes $F; done
```

# Nanopore assembly
## Guppy
```
#basecalling, changes file type to fastq
ont-guppy-cpu/bin/guppy_basecaller \
-i <input_dir> \
-s <output_dir> \
-c dna_r9.4.1_450bps_fast.cfg \
--barcode_kits "SQK-RBK004" \
--trim_barcodes
```
## Flye
```
#flye assembles the genome with raw reads
source activate <env>
flye \
--nano-hq rawreads/genome1.fastq \
-g 4m \
-o <output_dir> \
-i 2 --plasmids


