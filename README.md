# SARS-CoV-2-transformer-based-model-training-dataset
Python scripts that allow user to locally subset any given large corpus dataset to pretrain and finetune transformer-based model by filtering out ambiguous sequences, and convert to model readable csv format 

> NOTE: The corpus and affiliated metadata used for the pre-training and fine-tuning of the model used in the scripts here were downloaded from GISAID. [Click here for GISAID link](https://gisaid.org/)

## Table of Content:
1. Pre-training:
   
   a. corpus_creator_100k_seq.ipynb: This ipython script takes in 100,000 random complete SARS-CoV-2 genome sequences from GISAID genome database file and randomly splits the reads in each sequence to lengths between 0 and 512 bases long. The output of the script was written out to a csv format that is readable for the pre-training process.
   
   b. corpus_creator_one_seq.ipynb: This ipython script takes in one complete SARS-CoV-2 reference genome sequence from GISAID and randomly splits the reads in the reference genome to lengths between 0 and 512 bases long. The output of the script was written out to a csv format that is readable for the pre-training process.
   
2. Fine-tuning:
   a. 1_MSA_alignment_to_get_coordinates.ipynb: This ipython script takes in MUSCLE_MSA_refGenomes.fasta file that contains MSA alignment between the reference genome sequence in GISAID and reference genome sequence in NCBI. This step was performed to locate the true start and stop nucleotide positions of the Receptor Binding Domain (RBD) of the spike protein in GISAID reference genome sequence using the Open Reading Frame indices for SARS-CoV-2 genes for [NCBI reference genome sequence](https://www.ncbi.nlm.nih.gov/gene/1489668).
   
         Position of RBD start site in MSA between NCBI and GISAID (including gaps): 22567
         Position of RBD stop site in MSA between NCBI and GISAID (including gaps): 23235
         Position of RBD start site in GISAID  without gaps: 22517
         Position of RBD stop site in GISAID without gaps: 23185
   
   Next, the position of RBD in the MSA file between GISAID reference genome sequence and NCBI reference genome sequence is transferred to the MSA file containing all complete SARS-CoV-2 genome submissions from GISAID and position of the RBD start and stop codons are identified and trimmed from all MSA sequences to obtain only the sequences corresponding to RBD.
   
         Starting position of RBD in GISAID MSA file (with gaps): 35011
         Ending position of RBD in GISAID MSA file (with gaps): 36610
   
   b. 2_Convert_MSA_to_RBD_nuc_prot_csv.ipynb: This ipython script will take in the trimmed RBD sequences for all genome submissions present in the GISAID MSA file to select all valid submissions with sequence lengths within valid IQR ranges .
   
         Sequence Statistics before outlier removal:
         Total Sequences: 1660854
         Total Bases: 1114385340
         Minimum Length: 0 bases
         Maximum Length: 1600 bases
         Average Length: 670.97 bases

         Sequence Statistics after outlier removal:
         Total Sequences: 1659465
         Total Bases: 1113501015
         Minimum Length: 671 bases
         Maximum Length: 671 bases
         Average Length: 671.00 bases
   
   Each one of the valid RBD nucleotide sequences are converted to protein sequences and each submission is written out to a csv output containing GISAID Accession ID values from metadata file, Variant values, nucleotide sequence and protein sequences.
   
   <img width="995" alt="image" src="https://github.com/deevvan/SARS-CoV-2-transformer-based-model-training-dataset/assets/116576756/1bd0660a-885a-4777-886c-4deb825ccf4d">

   c. 3_divide_lineage_to_10_bins.ipynb: This ipython script that will take in csv file that contains valid RBD nucleotide sequences and protein sequences to compare Accession ID values with the metadata file to access Collection date value for each row and divide each Lineage group into 10 Generations unique to the lineage based on Collection date range.

   <img width="1266" alt="Screenshot 2024-10-30 at 6 02 05 PM" src="https://github.com/user-attachments/assets/5677a57d-cb09-4c25-9e55-e096de2a928d">


