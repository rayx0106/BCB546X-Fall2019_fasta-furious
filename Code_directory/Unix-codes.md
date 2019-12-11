

# Downloading the first part of sequences(29 of 88).
## install homebrew with the following code
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)" #for Linux
```
$ cat FPAC.txt | while read p; do echo $p; efetch -db nucleotide -id $p -format fasta > $p.fasta; done;
```
## installing edirect
```
brew install homebrew/science/edirect
```
# Preparing files for BLAST
## Merging two fasta file
```
$ cat NC_017343.fasta NC_002953.fasta > mergedtwo.fasta
```
# BLAST
```
$ blastn -query mergedtwo.fasta -subject flp.fasta > blast.txt
```
# Aligning
```
$ muscle -profile -in1 NC_017343.fasta -in2 NC_002953.3.fasta -out combinedAlignment.fasta
```