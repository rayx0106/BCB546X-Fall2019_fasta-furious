# Introduction


## Original Paper

The article of “Staphylococcus aureus Innate Immune Evasion is Lineage-Specific: A Bioinfomatics Study” by Alex J.McCarthy and Jodi A.Lindsay, 2013 was used for our final project. https://www.sciencedirect.com/science/article/pii/S1567134813002372?via%3Dihub. We replicated part 2.2, “Sequence analysis of S. aureus genes”.

The distribution and genetic variation of genes with characterised or hypothesized roles in inhibiting complement, chemotaxis or neutrophil function was investigated.

The sequence of each gene in each genome was identified using BLAST searches (www.ncbi.nlm.nih.giv/blast).

## Technical details

Raw Data was downloaded by the following unix code

### Downloading the first part of sequences(29 of 88). 

#### install homebrew with the following code
```{Unix}
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)" #for Linux
```

```{Unix}
$ cat FPAC.txt | while read p; do echo $p; efetch -db nucleotide -id $p -format fasta > $p.fasta; done;
```
### install edirect

```
brew install homebrew/science/edirect
```


### We also used biopython to download the data with the following code

```{python}
pip install biopython #install biopython
```

```{python}
from Bio.Seq import Seq
from Bio.Alphabet import IUPAC
```

```{python}
# this is a stupid way. it can be done with a for loop
import os
from Bio import SeqIO
from Bio import Entrez
Entrez.email = "mahsa.askaryhemmat@gmail.com"  # Always tell NCBI who you are
filename = "TL_HE681097.fasta"
if not os.path.isfile(filename):
    # Downloading...
    net_handle = Entrez.efetch(db="nucleotide", id="HE681097", rettype="fasta", retmode="text")
    out_handle = open(filename, "w")
    out_handle.write(net_handle.read())
    out_handle.close()
    net_handle.close()
    print("Saved")
print("Parsing...")
record = SeqIO.read(filename, "fasta")
print(record)
# We use this code download files one by one
```

### For the assembly files(59 of 88), the following code from biopython.org didn't work

```{python}
import os
from Bio import SeqIO
from Bio import Entrez
Entrez.email = "mahsa.askaryhemmat@gmail.com"  # Always tell NCBI who you are
filename = "ASM14638v1_genomic.fna"
if not os.path.isfile(filename):
    # Downloading...
    net_handle = Entrez.efetch(db="assembly", id="ASM14638v1", rettype="fasta", retmode="text")
    out_handle = open(filename, "w")
    out_handle.write(net_handle.read())
    out_handle.close()
    net_handle.close()
    print("Saved")
print("Parsing...")
record = SeqIO.read(filename, "assembly")
print(record)
```

### So, we did it manually. 

To download Assembly files, we used Batch Entrez and used the list of Accession numbers as the input file.

1. Use the web of (https://www.ncbi.nlm.nih.gov/sites/batchentrez)
2. Select "Assembly" in the Database drop-down menu
3. Click "Choose File" to submit the test file of all Genome names(https://raw.githubusercontent.com/Kakashi-sensei/BCB546X-Fall2019_fasta-furious/master/Data_directory/Assembly.txt)
4. Click "Retrieve" button
5. Save all files

### Also, we find a code way to download these files with following one(Python)

```{python}
def get_assembly_summary(id):
    """Get esummary for an entrez id"""
    from Bio import Entrez
    esummary_handle = Entrez.esummary(db="assembly", id=id, report="full")
    esummary_record = Entrez.read(esummary_handle)
    return esummary_record

def get_assemblies(term, download=True, path='assemblies'):
    """Download genbank assemblies for a given search term.
    Args:
        term: search term, usually organism name
        download: whether to download the results
        path: folder to save to
    """

    from Bio import Entrez
    #provide your own mail here
    Entrez.email = "mahsa.askaryhemmat@gmail.com"
    handle = Entrez.esearch(db="assembly", term=term, retmax='200')
    record = Entrez.read(handle)
    ids = record['IdList']
    print (f'found {len(ids)} ids')
    links = []
    for id in ids:
        #get summary
        summary = get_assembly_summary(id)
        #get ftp link
        url = summary['DocumentSummarySet']['DocumentSummary'][0]['FtpPath_RefSeq']
        if url == '':
            continue
        label = os.path.basename(url)
        #get the fasta link - change this to get other formats
        link = os.path.join(url,label+'_genomic.fna.gz')
        print (link)
        links.append(link)
        if download == True:
            #download link
            urllib.request.urlretrieve(link, f'{label}.fna.gz')
    return links
```

### Merging two fasta files
```{Unix}
cat NC_017343.fasta NC_002953.fasta > mergedtwo.fasta
```
### Then, we perform sequence analysis using NCBI BLAST 

```{Unix}
blastn -query mergedtwo.fasta -subject flp.fasta > blast.txt
```

### Aligning two sequences

```{Unix}
muscle -profile -in1 NC_017343.fasta -in2 NC_002953.3.fasta -out combinedAlignment.fasta
```
* Firstly, we use python to convert the "fasta" file into a "bed" one with this https://github.com/Kakashi-sensei/BCB546X-Fall2019_fasta-furious/blob/master/Data_directory/Tim_convert_fasta_to_bed.ipynb
But it cannot work very well.


* Then we use R to convert the "fasta" into a dataframe with the following code https://github.com/Kakashi-sensei/BCB546X-Fall2019_fasta-furious/blob/master/Data_directory/Tim_convert_fasta_to_dataframe.rmd.

* we extract the identical regions by comparing the sequence with the following code ...

### Compare the genomes or contigs against the MSSA476 genome
* We want to compare the genomes or contigs against the MSSA476 genome, that carries all 14 ssl genes, using the online Artemis comparison tool (WebACT, http://www.webact.org). But unfortunately this link provided on the paper doesn't exist. And when we compare the sequence of the genomes or contigs against the MSSA476 genome. we cannot find the identical region either.

### Aligning the sequence
* Sequences of genes were subsequently aligned using the ClustalW alignment and then edited by hand. The MSSA476 genome possess all core variable genes studied and was therefore used as a reference genome to assign allelic variants

* Individual gene sequences from remaining genomes were com- pared against the MSSA476 gene sequence for SNPs and inserts and/or deletions (InDels)

### Comparing the amino acid sequence of each gene with the reference sequence
* Each gene amino acid sequence was compared to the reference and all other sequences of that gene, and identified as variant if it was less than 80% or 90% homologous. Variants using the 90% cut-off are reported.



