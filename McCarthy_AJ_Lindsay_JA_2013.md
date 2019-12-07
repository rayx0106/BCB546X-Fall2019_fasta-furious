# Introduction
introduces the original paper, explains the technical details of your replication of analyses and summarizes your replication of the original results

* We replicated 2.2 Sequence analysis of S. aureus genes.

## Original Paper

The article of “Staphylococcus aureus Innate Immune Evasion is Lineage-Specific: A Bioinfomatics Study” by Alex J.McCarthy and Jodi A.Lindsay, 2013 was used for our final project. https://www.sciencedirect.com/science/article/pii/S1567134813002372?via%3Dihub. We replicated part 2.2, “Sequence analysis of S. aureus genes”.

The distribution and genetic variation of genes with characterised or hypothesized roles in inhibiting complement, chemotaxis or neutrophil function was investigated. In total, 43 genes were investigated as shown in XXXX.

The sequence of each gene in each genome was identified using BLAST searches (www.ncbi.nlm.nih.giv/blast).

The sequence of Staphylococcal superantigen-like (ssl) genes was identified firstly by using a BLAST search to identify ssl gene regions, and then secondly by comparing genomes or contigs against the MSSA476 genome, that carries all 14 ssl genes, using the online Artemis comparison tool

Sequences of genes were subsequently aligned using the ClustalW alignment and then edited by hand (Hall, 1999). The MSSA476 genome possess all core variable genes studied and was therefore used as a reference genome to assign allelic variants. Individual gene sequences from remaining genomes were compared against the MSSA476 gene sequence for SNPs and inserts and/or deletions (InDels). Each gene amino acid sequence was compared to the reference and all other sequences of that gene, and identified as variant if it was less than 80% or 90% homologous. Variants using the 90% cut-off are reported.

## Technical details

XXX Data was downloaded by the following unix code


# Downloading the first part of sequences{29}. 
```
$ cat FPAC.txt | while read p; do echo $p; efetch -db nucleotide -id $p -format fasta > $p.fasta; done;
```

