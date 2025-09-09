### Reciprocal Blast.md

1. Make a fasta file with your own sequences. Create a tab file called mysequencesdb.txt which is a tab delimited file of two columns. Here, db stands for database  All your sequence names in the first column (with a unique identifier, e.g. TA), all the squences themselves in the second one. No Headers.

2. In R:

```
fasta_file <- "mysequencesdb.fasta"
data<-read.table("mysequencesdb.txt")

writeLines(
  paste0(">", data$V1, "\n", data$V2),
  fasta_file
)

cat(readLines(fasta_file), sep = "\n")
```

3. Repeat the above to make a file with the sequences you want to blast called mybestguess.fasta (You need to tweak the code)


4. Download the blast command line at : https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ 

5. Build a blast database from your sequences following:

In the terminal:
```
makeblastdb -in mysequencesdb.fasta -dbtype nucl
```

6. Blast with the following code, to obtain a tabular result:

In the terminal:

```
blastn -query myquery.fasta  -db mysequencesdb -outfmt 7   >  blast_results.txt
```
