## Reciprocal Blast

## The idea 

*summarised by your favorite LLM friend*

The idea of “reciprocal BLAST” is a bit different from when you’re comparing full genomes/proteomes for orthologs. Let me break it down:

1. What you normally do with eDNA BLAST

You take your short eDNA reads or assembled contigs.

You run BLAST (usually blastn) against a reference database (e.g. NCBI nt, or a curated species database).

The “best hit” gives you a candidate species or taxon for your sequence.

This is a one-directional search: query → database.

2. Where “reciprocal BLAST” comes in

To strengthen the confidence of your taxonomic assignment, you can:

BLAST your eDNA sequence (query) against the database → record best hit (e.g., species X).

Take that best hit sequence from the database (species X reference).

BLAST it back against your full query set (or your local database).

If the best hit of species X is your original eDNA sequence, that’s a reciprocal best hit.

This reduces the chance that your eDNA read is just matching to a conserved region shared across many species.

## The Recipe

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
