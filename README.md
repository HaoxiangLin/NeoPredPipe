## Getting Started

This tool allows a user to process neoantigens predicted from vcf files using ANNOVAR and netMHCpan.

## Dependencies
##### Note: Should be compatible on Darwin and Linux systems, not Windows.

1. Python == 2.7 (Built using Python 2.7.13, not compatible with python 3 due to OS processes)
   - biopython == 1.70
2. ANNOVAR
   - Can be downloaded [here](http://annovar.openbioinformatics.org/en/latest/user-guide/download/).
   - ANNOVAR hg19_refGene
   - ANNOVAR hg19_refGeneMrna
4. netMHCpan
   - Using [netMHCpan-4.0](http://www.cbs.dtu.dk/cgi-bin/nph-sw_request?netMHCpan) for all tests of this pipeline.
   - Follow their steps for installation on your platform.

## Getting Started
1. Clone the repository:
```bash
git clone https://github.com/rschenck/NeoPredPipe.git
```
2. Configure the 'usr_path.ini' file for your environment.
   - All paths within the annovar header should be where you installed annovar.
   - Only one path is needed to the netMHCpan executible under netMHCpan
##### Note: You need to provide the absolute path.
3. You can see the options associated by running the following:
```bash
python ./main_netMHCpan_pipe.py --help
```
   - Which produces the following:
```bash
usage: main_netMHCpan_pipe.py [-h] [-E EPITOPES [EPITOPES ...]] [-m]
                              [-c COLREGIONS [COLREGIONS ...]] [-l] [-d]
                              [-I VCFDIR] [-H HLAFILE] [-o OUTPUTDIR]

optional arguments:
  -h, --help            show this help message and exit
  -E EPITOPES [EPITOPES ...], --epitopes EPITOPES [EPITOPES ...]
                        Epitope lengths for predictions. Default: 8 9 10
  -m                    Specifies if the vcf is a multiregion sample. Default:
                        False.
  -c COLREGIONS [COLREGIONS ...]
                        Columns of regions within vcf that are not normal within a
                        multiregion vcf file. 0 is normal in test samples. Can
                        handle different number of regions per vcf file.
  -l                    Specifies whether to delete the ANNOVAR log file.
                        Default: True. Note: Use for debugging.
  -d                    Specified whether to delete intermediate files created
                        by program. Default: True. Note: Set flag to resume
                        job.

Required arguments:
  -I VCFDIR             Input vcf file directory location. Example: -I
                        ./Example/input_vcfs/
  -H HLAFILE            HLA file for vcf patient samples.
  -o OUTPUTDIR          Output Directory Path
```

## Input files
1. VCF file. A standard vcf file with a patient identifier as the title of the .vcf.
2. An hla file with the following tab delimited format:
   - Note, patient identifier in the rows must match that preceding *.vcf
   - Headers are not required but the data should match the format in the table.
   - 'NA' is used when the HLA typing predicts the same HLA subtype for A, B, or C.
   - The program will search for the appropriate allele within netMHCpan alleles list, but care should be taken to ensure accuracy.

| Patient | HLA-A_1 | HLA-A_2 | HLA-B_1 | HLA-B_2 | HLA-C_1 | HLA-C_2 |
|  --- |  --- |  --- |  --- |  --- |  --- |  ---  |
| test1 | hla_a_31_01_02 | hla_a_02_01_80 | hla_b_40_01_02 | hla_b_50_01_01 | hla_c_03_04_20 | hla_c_06_02_01_02 |
| test2 | hla_a_01_01_01_01 | NA | hla_b_07_02_01 | NA | hla_c_01_02_01 | NA |

## Example of how to run the script using the provided example vcf files and corresponding hla types
```bash
python main_netMHCpan_pipe.py -I ./Example/input_vcfs -H ./Example/HLAtypes/hlatypes.txt -o ./ -m -c 1 2 -E 8 9 10
```