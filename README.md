# PRROMenade

To enhance current capabilities for studying metagenomes and metatranscriptomes, we developed a highly scalable read classifier, PRROMenade, that allows for both taxonomic and functional classification. PRROMenade utilizes the generalized Burrows-Wheeler transform, enhanced with a labeling step to directly assign reads to a reference hierarchy.

## Citation

Please cite the following article if you use PRROMenade:

Filippo Utro, Niina Haiminen, Enrico Siragusa, Laura-Jayne Gardiner, Ed Seabolt, Ritesh Krishna, James H. Kaufman and Laxmi Parida: Hierarchically labeled database indexing allows scalable characterization of microbiomes. iScience 23(4), 2020: https://www.sciencedirect.com/science/article/pii/S2589004220301723

RECOMB-Seq 2020 conference presentation: https://www.youtube.com/watch?v=xxD0xr9jCQ0


## Contact

For questions or comments, feel free to contact: Laxmi Parida <parida@us.ibm.com>

## Usage

The executables released have been compiled and tested on Ubuntu 16.04.5 LTS 

### Build the index

` ./taxonomic-indexer -s <fasta file> -n <child_parent.dmp> -g <name_taxid.dmp> `
                                                                                  
For instance:

` ./taxonomic-indexer -s /home/metagenomic/database_folder/sequences.faa -n /home/metagenomic/database_folder/child_parent.dmp -g  /home/metagenomic/database_folder/name_taxid.dmp `

### Classification

` ./taxonomic-classifier  -i <folder_with_index/index_prefix> <read_file> -t <num_proc> -o <out-bin-file> `
  
 For instance:
 
` ./taxonomic-classifier  -i /home/metagenomic/database_folder/fgp_prromenade /home/metagenomic/File.read.fq.gz -t 72 -o /home/metagenomic/output.bin ` 

## Pre-computed index

A PRROMenade search index for hierarchical functional annotation of nucleotide sequences against bacterial and viral protein domains (amino acid sequences) is available at: https://developer.ibm.com/exchanges/data/all/prromenade/

This index was described and used in: 

Niina Haiminen, Filippo Utro, Ed Seabolt and Laxmi Parida: Functional profiling of COVID-19 respiratory tract microbiomes. Scientific Reports, 2021. https://www.nature.com/articles/s41598-021-85750-0

## Input files and formats

### sequences.fa[a]

The N database sequences are given in fasta format of amino acid sequences. The database sequence name (any string without `|`) is followed by its associated taxonomic identifier (taxid) and followed by a unique name (number 1...N). All fields in the sequence header are separated by `|`, like this `>sequenceDescription|taxid|name`.

Example:


```
>sequence_A|2|1
WEQQEFQKHNCIFDETFFNPPRMYGHHVVY
>sequence_B|2|2 
KYQTLMVINQPMPVDIMGVMIVRHYQCHPVTQFNCTDHMWPLGKHATVKDRHSCSPGFSPAPFPMPWVSQA
>sequence_C|4|3 
VSQAMCIHGDDLHTGQLLNKYLI
...
>sequence_AHGDA|2|N
YWKEKYEFVGCNFHIKYKTCQFEMSNSVPIGEHYLHYEPD
```

### name_taxid.dmp

The relationship between each database sequence name and its taxonomic identifier (taxid) are given in this tab-separated file. The taxonomic identifiers correspond to nodes in the taxonomy. Several database sequences can map to the same taxid. The information in this file must match that in the database sequences file above, like this `name<tab>taxid`.

Example:

```
1    2
2    2 
3    4
...
N    2
```

### child_parent.dmp

The parent of each taxonomic node is given, using the taxonomic identifiers (taxid). This file defines the taxonomy tree. Note that each node has a single parent, except the root denoted with taxid `1` has no parent. The first column is taxid of a node and the second column is the taxid of its parent. The columns are separated by tabs and `|` separators, like this `name<tab>|<tab>taxid<tab>|`.

Example:

```
2    |    1    |
3    |    1    |
4    |    2    |
```

### reads.fa[.gz]

The sequencing reads (the queries) are given in fasta of gzipped fasta format. Paired reads should be classified as two separate files and the results combined after PRROMenade classification.

Note: Minimum possible read length is three (translating to one codon) when matching to an amino acid database.

Example:

```
>read1
ACGTACGATCGATCGTACGACTTCATTTTAGCTCGATGCACGGGCGG
>read2
CAGCTGCTGACTAGCTATATATCATCTATACTTTTTTACGATGCACG
...
```

## Output format
The output will list for each read the assigned taxonomic identifier (taxid) and the length of the maximal exact match (in amino acids).
In case there are equally long maximal exact matches for multiple possible coding frames (out of the possible six, three forward and three reverse), the equally good alternative answers are separated by `;`, like this `read_name<tab>taxid:length;taxid:length;...`.
Example:
```
read1 2735:9;2730:9
read2 1858:8
read3 7242:8
...
```
