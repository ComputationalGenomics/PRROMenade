# PRROMenade

To enhance current capabilities for studying metagenomes and metatranscriptomes, we developed a highly scalable read classifier, PRROMenade, that allows for both taxonomic and functional classification. PRROMenade utilizes the generalized Burrows-Wheeler transform, enhanced with a labeling step to directly assign reads to a reference hierarchy.

## Usage

### Build the index

` ./taxonomic-indexer -s <fasta file> -n <child_parent.dmp> -g <name_taxid.dmp> `
                                                                                  
For instance:

` ./taxonomic-indexer -s /home/metagenomic/mi-faser/prromenade/mifaser_formatted.faa -n /home/metagenomic/mi-faser/prromenade/mifaser_child_parent.dmp -g /home/metagenomic/mi-faser/prromenade/mifaser_name_taxid.dmp `

### Classification

` ./taxonomic-classifier  -i <folder_with_index> <read_file> -t <num_proc> -o <out-bin-file> `
  
 For instance:
 
` ./taxonomic-classifier  -i /home/metagenomic/database_folder/ /home/metagenomic/File.read.fq.gz -t 72 -o /home/metagenomic/output.bin ` 

## Citation

Please cite the following article if you use PRROMeande:

Filippo Utro, Niina Haiminen, Enrico Siragusa, Laura-Jayne Gardiner, Ed Seabolt, Ritesh Krishna, James H. Kaufman and Laxmi Parida; Scalable Functional and Pathway Characterization of Microbiomes; (manuscript submitted) 
