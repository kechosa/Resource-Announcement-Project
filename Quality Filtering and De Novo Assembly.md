gdown.pl https://drive.google.com/file/d/1dJHIeN-7pY9nDAz3Ia8QObJvTjbj-9HY/edit GAB_R1.fastq.gz

gdown.pl https://drive.google.com/file/d/1n_rpyjTiZjdKW4ICZDYYpf-osMIqAi71/edit GAB_R2.fastq.gz
>The gdown.pl command is used to obtain the raw reads of our selected bacterial strains from google drive and save them as fastq.gz files.

fastqc GAB_R1.fastq.gz

fastqc GAB_R2.fastq.gz
>fastqc is used on the downloaded raw read files to assess the quality of the reads. Once the assessment is complete, the created html file can be viewed on a browser using WinSCP or a similar program.

cutadapt -q 20,20 -a CTGTCTCTTATACACATCTCCGAGCCCACGAGAC -A CTGTCTCTTATACACATCTGACGCTGCCGACGA -m 50 --max-n 0 -o GAB_R1.cutadapt.fastq -p GAB_R2.cutadapt.fastq GAB_R1.fastq.gz GAB_R2.fastq.gz
>cutadapt is used to perform quality based trimming on the raw reads. -q signifies the minimum quality required, for both reads a score of 20 was chosen. -a and -A are used to signify and search for adaptor sequences in the read, with -a for the forward read and -A for the reverse read. -m is used to specify the minimum length of the reads, for these sequences 50 was used, meaning anything with less than 50 nucleotides read will trimmed. --max-n signals the number of no calls allowed, as we want a quality assembly this was set to 0. -o signifies the forward output and is where the name of the new file is given. -p is used to identify the reverse output. Following this is where we state the files on which we will use cutadapt.

conda activate de_novo
>This command signifies that we want to activate our de novo assembly programs

spades.py -k 21,51,71,91,111,127 --careful --pe1-1 GAB_R1.cutadapt.fastq --pe1-2 GAB_R2.cutadapt.fastq -o GAB_spades_output
>For our de novo assembly of the isolate we have selected the spades program. -k is used to specify the k-mer length. --careful tells spades to look carefully for errors. --pel-1 and pel-2 signify that reads are paired ends and is where we signal the files on wish we like the assembly completed. -o signifies the name of our output folder for the completed assembly.

cd GAB_spades_output
>This changes our directory to our spades assembly output

quast contigs.fasta -o Quast_contigs
>The quast command performs a quality check on the contigs.fasta file that was a result of our spades assembly. It creates a variety of files, such as .txt and .html, that can be viewed to assess the quality of the assembly.
