# So you want to trim your fastq files, huh?
A guide to using trimming tool **cutadapt**

 
## Recap
**recall**: illumina sequencing uses sequencing by synthesis -> this involves **bridge amplification**

<img src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fcommercio.nyc3.digitaloceanspaces.com%2Fgoldbio-2018%2Fpages%2FPCR%2520Application%2520Figures_Bridge%2520PCR%2520copy.jpg&f=1&nofb=1&ipt=501f9422ad26c62de5c9d7e1270de03742566278020d1e13980bb9f6c6800218" width="800" alt="figure describes the process of bridge amplification">

Central to the process of sequencing by synthesis are the **adapter** that are ligated onto the ends of the DNA. These allow polymerase to replicate our DNA in bridge amplification & bind our DNA to the flow cell
Additionally, if you're only interested in a specific amplicon, the **primers** will get sequenced and included in your data as well

## However, these f*ck with our data
So, we gotta trim them (and also clean our data, which some of these other tools can also do, but that's beyond the scope of what i'm gonna do)

### There are a bunch of tools that we can use for this:
[Trimmomatic](https://github.com/usadellab/Trimmomatic), [trimFastQ](https://rdrr.io/bioc/seqTools/man/trimFastq.html), [pTrimmer](https://github.com/DMU-lilab/pTrimmer), [skewer](https://github.com/relipmoc/skewer)

 
 # [cutadapt](https://cutadapt.readthedocs.io/en/stable/guide.html)
 ---
**cutadapt** is a command line tool to trim adapters/primers/quality trim/etc
- you can use it for paired-end sequencing or single run sequencing
- you can also use it to trim x nucleotides from the head or tail
- trim nucleotides that have

## 1) install cutadapt (this step is almost word for word from the documentation, go there if you have questions)
- you can do this with conda (a package manager for python)
- a couple other ways, if you wanna do this check out the documentation linked above
- run this in the terminal (after installing conda, if you havent already)
```
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
# adds bioconda channels

conda create -n cutadapt cutadapt
# creates cutadapt environment

conda activate cutadapt
# activates environment, do this every time u wanna run cutadapt after opening a new terminal window

cutadapt --version
# confirmation cutadapt was installed successfully
```
## 2) run these commands
Download the [sample_file1](https://github.com/rieseberglab/fastq-examples/blob/9d19a6b65ce1140b71337576068b5074ba92b0ab/data/HI.4019.002.index_7.ANN0831_R1.fastq.gz) and move it into your directory you are using
```
zcat HI.4019.002.index_7.ANN0831_R1.fastq.gz | head -n 3
```
^ reads the first 3 lines of the file

output:
```
@K00271:89:HHWWNBBXX:2:1101:23277:1068 1:N:0:CAGATC
NATCGGAAGAGCACACGTCTGAACTCCAGTCACCAGATCATCTCGTATGCCGTCTTCTGCTTGAAAAAAAAAAATCTCAGACAACAAATCACAGAGTTAAGTCAGTTTACCGCACAAACTNACCAAGTCGGCGAAACAGAAGGTGGCGAC
+
```
## 3) using cutadapt
```
cutadapt -g AGATCGGAAGAGCACACGTCTGAACTCCAGTCA -o output.fastq.gz HI.4019.002.index_7.ANN0831_R1.fastq.gz
# looks for a 5' adapter & trims it and outputs it to the file you specify
```

## 4) that's it!
