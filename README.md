#	CATCh

CATCh: an ensemble classifier for chimera detection in 16S rRNA sequencing studies
####  Mysara, M., Saeys, Y., et al., 2015. CATCh, an ensemble classifier for chimera detection in 16S rRNA sequencing studies. Applied and environmental microbiology, 81(5), pp.1573–84.

## Installation Requirement
Following programs need to be installed to run CATCh:

Decipher (only required when you want to run the reference-based CATCh implementation):
- DECIPHER is available as a BioConductor module, which requires the installation of R (www.cran.r-project.org). The easiest way to install DECIPHER is using the biocLite function of BioConductor in R. After launching a R-terminal, type: source("http://bioconductor.org/biocLite.R")
- biocLite("DECIPHER")
It requires the corresponding reference database needs to be downloaded from: http://decipher.cee.wisc.edu/RDP10.28_unaligned.sqlite.zip. Manual is available at http://decipher.cee.wisc.edu/Download.html.

## Included Software:
CATCh incorporates the following tools, however you do NOT need to install it separately as these software modules are included in the CATCh software:

- mothur: Available at http://www.mothur.org/wiki/Download_mothur
- WEKA 3.7: Available at http://www.cs.waikato.a.nz/ml/weka/
- RDP MultiClassifier 1.1: Available at http://rdp.cme.msu.edu/classifier/
- DECIPHER: Available at http://decipher.cee.wisc.edu/Download.html.

## Syntax:
CATCh_Complete.run {options}

### Use the Following Mandatory Options:

!!! make sure you use an underscore (and not a hyphen) to specify the option you want to set.

_f :	file.fasta: 
A fasta-formatted file containing all the reads that need to be checked for the presence of chimera. Can be a fasta-file either containing the raw sequencing reads or containing preprocessed reads (e.g. quality checking, denoising)

_m:	running mode, either denovo 'd' or reference 'r'

_n:	Name file with the redundancy in denovo mode
Name file with the redundancy in de novo mode (can be obtained in mothur via the command: unique.seqs(fasta=YourFasta.fasta)

_h:	output directory
_w:	directory where DECIPHER database is stored.
Downloaded from: http://decipher.cee.wisc.edu/RDP10.28_unaligned.sqlite.zip.


### Use the Following Non-Mandatory Options:

_o:	option to run the whole program "z" or to run specific step(s):

-"u" for Uchime
-"c" for ChimeraSlayer
-"p" for Perseus or Pintail (depending on Reference or de novo)
-"d" for decipher
-"r" for CATCh

_p:	number of processors, default 1

_r:	ChimeraSlayer reference data path, default './silva.gold.align'.


    Downloaded from: http://www.mothur.org/wiki/Silva_reference_files

_u:	UCHIME reference data path, default 'gold.fa'.


    Downloaded from: http://drive5.com/uchime/uchime_download.html

_l:	pintail reference data path, default './silva.gold.align'.


    Downloaded from: http://www.mothur.org/wiki/Silva_reference_files

_d:	direction either forward 'f' (default) or reverse 'r'

_g:	group file (required when having different samples represented in the same fasta-file.


    It consists of a tab delimited file with the read ID and the sample name. Can be directly exported from mothur.

_i:	log ID which can be set to continue a previous CATCh run

_c:	cut-off for CATCh (default 0.62 for reference, and 0.70 for denovo)


    A higher cut-off score will make the algorithm more stringent (lower number of chimera detected, with less false positives i.e. decrease in sensitivity and increase in specificity), while decreasing this cut-off will lead to a higher number of chimera predicted with an increase of false positive predictions (i.e. increased sensitivity and decrease in specificity).

## Output Files
The CATCh program generates numerous output text files in the "Tools_results"-directory:

### CATCh_Result.arff.Final_Result: 
Contains the final CATCh results after running the classifier on the output of the different individual tools. It consist of a two-column tab delimited file with the read ID (column 1) and an indication whether the read is "Chimeric" or "Non-Chimeric" (column 2). 
The output of each individual chimera prediction tool.

Logfile with the process ID, with the outputs of each tool.

### Reads predicted by CATCh having chimeras can be removed from the multi fasta file via the following commands in mothur:
system(grep –P "\tChimeric" ./Tools_Results/ CATCH_Result.arff.Final_Result > CATCH_Result.arff.Final_Result.accnos)
remove.seqs(fasta=file.fasta,accnos=CATCH_Result.arff.Final_Result.accnos)
If a name and a group file are present, add them to the previous syntax commands remove.seqs(fasta=file.fasta,accnos=CATCH_Result.arff.Final_Result.accnos,group=File.groups,name=File.names)
## Testing
Type:
perl CATCh.pl _m d _n sample.names _f sample.fasta _h /path/to/your/OutputDirectory/ 
This will produce a sample output file (default: ./Tools_Results/CATCh_Result.arff.Final_Result) which should be similar to Sample.results (provided via the downloaded zip-file, in the root-directory of CATCh).

Test and Training data
All used data have been obtained via the directions specified in the original publications of the different chimera detection tools, except for simu1 which is introduced in this work (simulated data as described in the paper).  
Simu1.fasta

A step-by-step manual on how the CATCh classifier is built can be downloaded here. Researchers who want to train and test the CATCh classifier themselves using WEKA, three WEKA-compatible files can be downloaded. The first one contains the training and testing data for the reference based CATCh implementation.  For the de novo implementation, as separate .arff file exists for training and testing CATCh de novo. All datasets have been described in the corresponding manuscript.

## Citing
If you are going to use CATCh, please cite it with the included software (Mothur, WEKA, RDP MultiClassifier 1.1 and DECIPHER).

     Mysara, M., Saeys, Y., et al., 2015. CATCh, an ensemble classifier for chimera detection in 16S rRNA sequencing studies. Applied and environmental microbiology, 81(5), pp.1573–84.
      Schloss PD, Westcott SL, Ryabin T, Hall JR, Hartmann M, Hollister EB, et al. (2009). Introducing mothur: open-source, platform-independent, community-supported software for describing and comparing microbial communities. Applied and environmental microbiology 75:7537–41.
      Hall M, National H, Frank E, Holmes G, Pfahringer B, Reutemann P, et al. (2009). The WEKA Data Mining Software: An Update. SIGKDD Explorations 11:10–18.
      Wang Q, Garrity GM, Tiedje JM, Cole Naive JR (2007), Bayesian classifier for rapid assignment of rRNA sequences into the new bacterial taxonomy. Applied and Environmental Microbiology 09/2007; 73(16):5261-7.
      ES Wright et al. (2012), DECIPHER, A Search-Based Approach to Chimera Identification for 16S rRNA Sequences. Applied and Environmental Microbiology, doi:10.1128/AEM.06516-11.

## Notice:
This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <http://www.gnu.org/licenses/>.

Contact
In case of any problems in running the program, or any possible suggestion/modification please feel free to contact the team
