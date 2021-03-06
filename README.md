# PythonNGSTools
Scripts for NGS data processing

## HapCUT2VCF.py   
This code convert a output file from HapCUT2(https://github.com/pjedge/hapcut2) into VCF format.

Usage(python3):  
```python
python HapCUT2VCF.py  hapcut2_table  output_VCF   original_VCF(optional)  
```
Because hapcut2_table only keep phased variants. Unphased variants can be recovered if the original 
input VCF file for HapCUT2 is provided. The variants will be combined into the same output file.
Each block in HapCUT2 table is stored as haplotype block in VCF file, and marked using PS tag.
The numbering of PS starts from 1 for each run. The output tables can be concatenated before converting
to generate unique PS IDs in the whole genome.


## VCFsplit4HapCUT2.py  
This code split a VCF file into files of smaller size.  It can be used to run HapCUT2 in parallel.

Usage(python3):
```python
python VCFsplit4HapCUT2.py  vcf_input  line_number_per_file
```


## MUMmerSNPs2VCF.py  
This code converts output file from show-snps of MUMmer into VCF format.
"-x 1" option should be turned on so that reference fasta is not needed.  

Usage:  
```
show-snps -Clr -x 1  -T mum.delta.filter  >mum.delta.filterX.snps  
python3.4 MUMmerSNPs2VCF.py mum.delta.filterX.snps  mum_filterX.snps.vcf  
```

Notes for the code:  
To get the correct converted VCF files from MUMmer/snps:  
1) You need to check the reference sequence to rebuild insertion and deletion.   
Instead of reading original reference fasta file, I used "show-snps -x 1", so that the surrounding nucleotides are also reported.   
2) For the insertions, if the query sequences are reversely mapped to the references, the orders of nucleotides in query sequence are reversely reported.   
So, they needed to be concatenated in reverse order.   
3) The coordinates of insertion and deletions.   
For insertions, the coordinates in MUMmer/snps are the coordinates of nucleotides before insertions. They need to be kept as the same in VCF files.   
For deletions, the coordinates in MUMmer/snps are of the nucleotides that are deleted. The coordinates in VCF should be : first_position_of_deletion_block - 1.  



## Download_FTP_ENA.py


Download fastq files from ENA.

Usage:  
1. Search the project in ENA to get the information page of one study.  
2. Click "Read Files" tag next to "Navigation".  
3. Download the TEXT file (save as ENA_description.txt).  
4. Write the shell scripts to download fastq files (The master shell submit downloading jobs to one queue on clusters).  
```
python2.7 Download_FTP_ENA.py  ./  ENA_description.txt  
```

./ : is the current directory and can be changed into the directory to store the fastq files.  

## MUMmer helper
Some tools to split large query file, submit jobs to clusters and merge the delta output from MUMmer


