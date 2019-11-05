mkdir annotation
>With the mkdir command we created a new directory for our annotation of GAB2316C.

cp GAB_spades_output/contigs.fasta annotation
>cp copies our contigs.fasta from the GAB_spades_output directory and copies the file over to our annotation directory.

cd annotation
>We moved into our new annotation directory in order to run our annotation program prokka.

awk '/^>/{print ">GAB_" ++i; next}{print}' < contigs.fasta > contigs_names.fasta
>The awk command is needed to change the names of the contigs to a form that our annotation program, prokka, will be able to recognize. The /^>/ searches the contigs.fasta file for lines that start with > and print ">GAB_" signals the replacement of > with >GAB_. ++i will add a number after GAB_ starting with 1 and increasing by 1 with each replacement. < contigs.fasta > specifies the file that we would like to run this command on and contigs_name.fasta is the name of our new output file.

conda activate annotation
>This command activates our annotation program prokka.

prokka --outdir GAB --prefix GAB contigs_names.fasta
>This command activates and runs the prokka program to annotate our GAB2316C assembly. --outdir is where the name of output directory is specified, and this is where all the files that prokka creates will be kept. --prefix is used to specify the name we would like to give to our output files. We then specify the name of the file on which we'd like to perform our annotation, in this case it is our editted contigs.fasta file contigs_names.fasta.

cd GAB
>We use cd to change to our prokka annotation output directory, GAB, in order to analyze our annotation.

less GAB.txt
>Using the less command on GAB.txt allows us to search the file to find the number of coding sequences and the number and types of non-coding regions in GAB2316C.

grep -c "hypothetical" GAB.tsv
>grep -c was used to count the number of hypothetical proteins in GAB2316C. This was done in the tsv file as it is one of prokka's output files that gives gene products.

grep "korormicin" GAB.tsv
>The grep command was also used to search the tsv file for potential antimicrobial products. In addition to korormicin, pyrrole, quinolinone, and other known *Pseudoalteromonas* antimicrobial products were searched using grep. 
