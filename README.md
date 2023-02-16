# run_BSA_analysis - bulked segregant analysis process software
---
**<font size=2 >Current Version: 1.0 Release date: December 16, 2022</font>**

The run_BSA_analysis is written in Python, which is used to bulked segregant analysis of mixed bulk files after variant calling. Only enter the mixed-bulk SNP loci information table, mixed-bulk VCF file or mixed-bulk BAMlist file, which can generate SNP loci depth plot, allele distribution frequency distribution plot, SNP loci distribution plot along chromosome, deltaSNPindex along chromosome distribution plot, and QTL interval information that breaks the threshold.
## lastest version：run_BSAanalysis  V2.0
---
- **Major updates**

  The new version adds mapping functionality and several important change：
  - **-v: input fiile**,The content of the input file has been changed, and the software no longer supports BAM format input, and has been changed to FASTQ file input.You need to enter a `list/txt` file containing a fixed format, the first column is the sample name, the second column is the sequencing library type(Paired-end`PE`，Single-read`SE`), the third column is the FASTQ1 path of PE/SE type, the fourth column is the FASTQ2 path of PE type, and the SE type does not need to be filled in:
                                              <table>
                                              <tr>
                                                <td>sample1</td>
                                                <td>PE</td>
                                                <td>sample1_1.fastq</td>
                                                <td>sample1_2.fastq</td>
                                              </tr>
                                              <tr>
                                                <td>sample2</td>
                                                <td>SE</td>
                                                <td>sample2.fastq</td>
                                                <td>  </td>
                                              </tr>
                                              <tr>
                                                <td>...</td>
                                                <td>...</td>
                                                <td>...</td>
                                                <td>...</td>
                                              </tr>
                                              </table>
  ### New Output
- **mapping dir:** Stores the result file after BWA software mapping.
- **qc dir** Stores the SOAPnuke software quality inspection result file.
## version：run_BSAanalysis  V1.1
---
- **Major updates**

  The new version adds the functions of candidate gene extraction and candidate interval INDEL marker design. Two new required parameters have been added:
  - **-R: Reference genome**,Input species reference genome sequence files, including fa, fna, and fasta formats.
  - **-gtf: Gene transfer format**,Input species Gene transfer format files.
  
  Remove the **-s: species_name** parameter in v1.0
  ### New Output
- **ann dir:** It is used to store the result file after ANNOVAR software annotates the SNP and INDEL sites after var calling.
- **Indel dir** INDEL information file extracted after variant calling.
- **Primer dir** Store INDEL primers information files within the candidate interval.
  
## version：run_BSAanalysis  V1.0
---

## Installation
---
- **Support data**

   run_BSA_analysis is designed to analyze mixed-bulk VCF files obtained by sequencing methods such as WGS, WES, RNA-seq, etc.

- **Script environment**

  [R](https://www.r-project.org/) >  = 4.2.0,
   
   - [r-ggplot2](https://faculty.washington.edu/browning/beagle/beagle.html)
    
   - [r-devtools](https://www.r-project.org/nosvn/pandoc/devtools.html)
   - [r-QTLseqr](https://github.com/bmansfeld/QTLseqr),the QTLseqr installation package is provided in the run_BSA_analysis installation package, and you can install it locally by using the following command:```bash devtools::install_local("qtlseqr.zip") ```
 
  [python3](https://www.python.org/download/releases/3.0/)

  [GATK](https://gatk.broadinstitute.org/hc/en-us)
    
  Before installing run_BSA_analysis, you need to install the above software and ensure that the software runs normally.

- **run_BSA_analysis does not require additional installation**
```bash
tar -xvf run_BSAanalysis.tar
cd /run_BSAanalysis
chmod 775 run_BSA_analysis
```
After the installation is successful, you can start the analysis using the `run_BSA_analysis` script in the installation folder.

You can use the `-h` command to check whether the script is installed successfully.
```bash
./run_BSA_analysis -h
```

## Quick start run
---

The example files are stored in the `example` under the installation folder.


 **step1. Update the config file**

Enter the locally installed software installation path and reference genome path into the `config` configuration file, and if the software is installed by `conda`, you need to fill in the software name in the path field.

 **step2. Run the script**
 
 Make sure that your R is written to an environment variable, or that the conda environment of R is activated before you use the software.
```bash 
 # test on example data 
./run_BSA_analysis -v /example/all.filtered.snp_1706.vcf -n 1706 -o ./text/ -H H1706Z -L H1706F -c /example/chr.txt  -s peanut
 ```
 After successful running, the corresponding file is generated in the result folder.
 
## Options and help
---
```bash
run_gwaslite -h
```
### Required
- **-v: input fiile**, The software provides three input file types: mixed pool SNP bit table file, mixed pool VCF file, mixed pool BAMlist file, of which VCF file and BAM file need corresponding index.
- **-n: project name**, The project name of this analysis, which is ultimately used as the result file prefix.
- **-H: Highbulk**, Highest Bulk,the highest extreme values sample name.
- **-L: Lowbulk**, Lowest bulk,the lowest extreme values sample name.
- **-c: Chr file**, a list of chromosomal names, which need to correspond to the chromosome names in the reference genome and input file. You can also perform analysis of a single chromosome or several chromosomes with secondary parameters.

    <table >
      <tr>
          <td>Chr1</td>
      </tr>
      <tr>
          <td>Chr11</td>
      </tr>
      <tr>
          <td>...</td>
      </tr>
      </table>
  
- **-s: species_name**,The reference genome required for GATK at the time of format translation. You can add your desired species reference genome to the config file, you need a dict file under the reference genome folder, if you don't have the file, you need to use GATK `CreateSequenceDictionary` function to build.
- **-o: output file path**, the output file is stored in a folder, and the script will create a `projectname_result` folder under the input path, and the result file will be stored in this folder. If you run the script continuously, the `projectname_bsa_result` folder is overwritten.

### Optional
- **-t: gatkt hread**, GATK software uses the number of threads, which default = "-Xmx8G".
- **-w: windowsize**, Size of tricube-weighted window,default=1e6.Larger windows will produce smoother data, so we recommend testing several window sizes for your data, and then deciding on the optimal size.
- **-p: popStruc**, The population type,default = "F2".
- **-q: gatkt hread**, SNP loci minimum quality value,default = 99.

### Output
- **align dir:** If you enter a BAM file, the variant calling results will be stored in this folder.
- **BSARESULT dir** It is used to store PLINK bfiles and genome-wide association analysis results.
- **table dir** The table file generated after extracting the SNP loci information.
- **SNP dir** When importing BAM files, the output files after filtering the SNP sites are stored in this folder.
- **report file** Output the report file.

## Bug reports
---
For more detailed questions or other concerns please contact radiomumm 
[zhushuliang@genomics.cn]()

