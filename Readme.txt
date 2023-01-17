Problem Statement: 
ML-based model to classify proteins as proteins/dna/rna/carbohydrates-binding proteins based on their interaction properties
and also classify them as cancer/non-cancer by comparing against the curated cancer census genes/proteins 

Input files
1. fasta_cgc (4).fasta: cancer census genes: 768
2. PCI_1130_sequences_17may2022 (3).fasta: Carbohydrate binding proteins: 1,130 Carbohydrate-binding
3. PPI_protein_seq_25.fasta (6).txt: protein heterodimers: 880
4. protein-dna-fasta-file (4).fasta: DNA binding proteins: 3,859
5. protein-rna-fasta-file (5).fasta: RNA binding proteins​: 963

Cancer_gene_classification_pre-processing steps

Data-Preprocessing
Step 1: Check and removing redundancy of data within the file: Do it for all the files
	Note: Protein heterodimer file (protein-interacting protein file) is also not formatted, so that step needs to be done before proceeding with redundancy test
Step 2: Check and remove matches in all files against CGC (as if already listed in CGC as cancer genes, it need not be tested further) and by Uniprot id_mapping (to get complete sequence information)


Calculation of the features (Physiochemical properties) from the pre-processed CSV files (to which an additional class column was added that labeled the file as DNA/RNA/Carb/Hetero/CGC
Step 1: Import the requisite libraries 
Step 2: Open the CGC file in the Jupyter notebook and convert it into a dataframe, remove the suffix of the identifier and retain the Uniprot ID
Step 3: Concatenate all the CSV files generated after DNA-Preprocessing
Step 4: Create the Length feature by counting the length of amino acid sequence in the Sequence column
Step 5: Import the Properties file that has 130 general properties of amino acids
Step 6: Using the amino acid column names in the property file, a count of each amino acid in each sequence in the concatenated file (big_frame)
Step 7: A prefix of 'I' is given to the property index to have uniformity in the name of properties
Step 8: Two arrays are create: one having the values of first property (K0) for each amino acid and second having the count of the each amino acid in the seqeunce
Step 9: A sum product of array 1 and 2 generates the K0 value for that sequence
Step 10: Same method is now applied for all the properties
Step 11: The complied file is exported as csv	
Step 12: The sequence and identifier columns from the concatenated file with class suffix added to identifier is considered for CD-Hit at various thresholds


After CD-Hit, the final non_redundant dataset contained:
DNA-binding protein: 280
RNA-binding protein: 163
Carbohydrate binding protein:1111
Protein-interacting heterodimers: 500
CGC: 693


ML based one-vs-CGC binary classification technique to train the model for each class (DNA,RNA,Carbohydrate and protein heterodimer)
Step 1: importing necessary libraries
Step 2: Create a new dataframe for each class with its propoerties
Step 3: Add a target column which represents presence and absence of CGC as 1 and 0 respectively
Step 4: Split data to X (features) and y (label)
Step 5: Divide data to train and test (80% split)
Step 6: Build model (Random forest Classifier)
Step 7: Check for feature importance
Step 8: Derive the Classification report and confusion matrix
Step 9: Check the model performance

Repeat the steps for all the classes.
The study can be extended by checking for specfic features that distinguishing each binding proteins than only considering the general properties of amino acids.


File details: 
│   01-IITM_Project_Data_pre-processing steps to remove redundancies.ipynb
│   02-Derivation of features (physiochemical properties) from the pre-processed csv files.ipynb
│   Compiled_with_Property_and_composition.csv
│   Final_nrd_Prop_compn.csv
│   IITM_model_Carb.ipynb
│   IITM_model_DNA.ipynb
│   IITM_model_Hetero.ipynb
│   IITM_model_RNA.ipynb
│   Readme.txt
│   Sangeetha_summer_internship_report.pdf
│
└───input files
        fasta_cgc (4).fasta
        PCI_1130_sequences_17may2022 (3).fasta
        PPI_protein_seq_25.fasta (6).txt
        protein-dna-fasta-file (4).fasta
        protein-rna-fasta-file (5).fasta


