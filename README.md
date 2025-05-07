# Initial-gene-prediction-pipeline
This project focuses on improving the gene annotation of Cryptococcus neoformans var. grubii H99, a major fungal pathogen, using the BRAKER3 pipeline. By integrating RNA-seq evidence with ab initio predictions, we generated an updated gene model and compared it against the existing annotation from FungiDB.

The initial step for genome annotation is to download the genome and SRA files. Genome can be downloaded by using wget:

<pre> ```wget https://fungidb.org/common/downloads/Current_Release/CneoformansH99/fasta/data/FungiDB-68_CneoformansH99_Genome.fasta ``` </pre>

To download the SRA files, the script is there in SRA download script.

After downloading all the datas, the next is to check the quality of the SRA files, for that we are doing FASTQC. 

<pre> ``` #!/bin/bash

# List of SRA accession numbers
SRA_LIST="sra_accessions.txt"

# Output directories
FASTQ_DIR="/path/to/fastq_output"
FASTQC_DIR="/path/to/fastqc_output"
mkdir -p "$FASTQ_DIR" "$FASTQC_DIR"

# Load modules (adjust for your HPC environment)
module load sra-tools
module load fastqc

# Process each SRA ID
while read -r SRA_ID; do
    echo "Processing $SRA_ID..."

    # Step 1: Convert to FASTQ
    fasterq-dump "$SRA_ID" --split-files -e 6 -O "$FASTQ_DIR"

    # Step 2: Run FastQC on all resulting FASTQ files
    for fq in "$FASTQ_DIR"/"$SRA_ID"_*.fastq; do
        fastqc "$fq" -o "$FASTQC_DIR"
    done

    echo "$SRA_ID done."
done < "$SRA_LIST"

echo "All SRA samples processed with FastQC." ``` </pre>
  
