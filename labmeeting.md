# Lab Meeting - January 2015
## RNA-Seq, Reproducible Research and Galaxy

---

# Agenda

* mRNA-Seq
    * Workflow
    * Tools
    	* Aligners
    	* Quantifiers
    	* Finding Differentially Expressed Genes
    	* Downstream Processing

---

# Agenda (cont.) 
* Reproducible Research
* Galaxy
* Histories
* Workflows
* Bringing your tools to Galaxy
    * Writing a wrapper for JAMM

---

# Introduction

What's the best way to analyze my data? 

## Possible requirements:
* reproducible
* shareable
* easy to use
* automated

---

# Before building a pipeline
What's the right tool for each step?

# Before running the pipeline
Do I know everything necessary about my data to
make the right decisions when configuring each tool?

---

background-image:url(images/nprot.jpg)
.footnote[Anders et al. 2013 Nat Protoc]

---

# Sequence quality checks
* FAST__QC__

# Adapters, Barcodes, ...
* flexbar
* Trimmomatic
* FastX
* TrimGalore / Cutadapt

---

# Read Mapping / Aligning
* BWA? (not splicing aware)
* Tophat 2
* STAR (fast if you have RAM: 31 GB for hs/mm)
* RUM
* GSNAP (SNP-tolerant)

---
# Feature Counting / Abundance Estimation
* HTSeq-count
* Bioconductor, R

### Isoforms
* RSEM
* express
* Bitseq

---
# Calling Differentially Expressed Genes

Given a table of feature counts for n conditions, 
which features show differential expression between conditions?

* edgeR
* DESeq2
* limma & voom
* Bitseq

---

# Downstream Analysis

* Enrichment of GO, Pathways, ...

---

# Isoform Territory

Building a decent pipeline is much harder than for DGE
* much less consensus on tools
* transcript abundance estimation is hard
* more "experimental"

---

background-image:url(images/mp.jpg)

---

# Reproducible Research
## What are we aiming for?
### What does this mean for my own research? 
#### I don't have to use Galaxy, do I?
###### No.

---

# Reproducible Research
### Data analysis and other scientific computations should be *reproducible*

> If someone asks you: Would you be able to recreate Figure 2B from your third to last paper in a day a from the raw data?

###  _Some_ reproducibility is better than none.
> While it may be painful to adjust your workflow now, it can save you at lot of anger, fear and despair in the long run

---

# Concepts
#### Data 
* No data manipulation by hand!!
* Dataset reconstruction from the raw data

#### Scripts
1. Write scripts
2. Let your scripts generate reports 
3. Turn repeatedly used code into functions 
 
#### Audit trail: version control
* git, hq, ...

#### Writing
* use version control, too
* use a repository for collaboration

???
Dropbox and Word are not the best way to do things

Step in the right direction

---
# Galaxy
"Galaxy is an open, web-based platform for data intensive biomedical research. Whether on the free public server or your own instance, you can perform, reproduce, and share complete analyses."

![Default-aligned image](images/galaxy.png)

---
# Galaxy

Concepts to represent data include

* Histories
* Workflows
* Pages
* Data Libraries, Tools, Visualizations

---
# Histories
are analyses in Galaxy that show (all)

* input
* intermediate 
* final

datasets, as well as 

* every step in the process 
* the settings used with each

History can be imported into your session and rerun as is or modified.

---
background-image:url(images/history.png)

---
# Workflows

specify the steps in a process but not the datasets. Workflows are analyses that are meant to be run, each time with different user-provided datasets

---

background-image:url(images/sketch.jpg)

---

# Bonus: Bring your own software to Galaxy
### Or how I wrote a wrapper for JAMM

Galaxy uses XML to render the interface that gets exposed to the user for each tool

```xml
<tool id="jamm1.0.6rev2" name="JAMM" version="1.0.6.2">
<description>Calls peaks on NGS data</description>
<requirements>
<!--
	<requirement type="package" version="2.5">b2g4pipe</requirement>
-->
</requirements>
<command interpreter="python">
jammwrapper.py -i
	#for $x in $replicate
	${x.input.file_name}
	#end for
	-g ${genomeSizeFile}
	-z $output
	-m $mode
	-r $resolution
	-p $processes
</command>
```
---
```xml
<inputs>
	<repeat name="replicate" title="Replicate">
		<param name="input" type="data" format="bed">
		</param>
	</repeat>
	<param name="genomeSizeFile" type="data" label="File indicating chromosome sizes" description="Chromosome size">
	</param>
	<param name="mode" type="select" label="Mode (normal or narrow)">
     <option value="normal">normal</option>
     <option value="narrow">narrow</option>
    </param>
	<param name="resolution" type="select" label="Resolution (peak or region)">
     <option value="peak">peak</option>
     <option value="region">region</option>
    </param>
         
    <param name="processes"  type="text" value="4" optional="true" help="Number of threads used by R" /> 
</inputs>

<outputs>
<data name="output" format="tabular" label="Narrowpeak tabular" />
</outputs>
```

---

background-image:url(images/jamm2.png)

---
# Do I need a wrapper?

.footnote[http://wiki.sb-roscoff.fr/ifb/index.php/Tool_Integration_Short_Tutorial]

* Input files are not passed through arguments but loaded based on predetermined fixed names
    * The wrapper can create symbolic links between the files and the predetermined fixed names in the cwd 
* Output files with conditional names are not passed through arguments
    * The wrapper can create symbolic links between the files and predetermined fixed names in the cwd 
* Using R functions
    * The wrapper can be used for libraries loading, data preprocessing and argument parsing 
* Multiple command lines
    * A simple bash wrapper can be used to pass arguments and call multiple commands

## JAMM does at least 2 of these things

---

```python
import argparse, os, shutil, subprocess, sys, tempfile
def main(): 

    #Read command line arguments
    parser = argparse.ArgumentParser()
    parser.add_argument('-i', dest = 'input', nargs='+')
    parser.add_argument('-g', dest = 'gsize')
    parser.add_argument('-z', dest = 'peakfile')
    
    parser.add_argument('-m', dest = 'mode')
    parser.add_argument('-r', dest = 'resolution')
    parser.add_argument('-p', dest = 'processes')
    #parser.add_argument('-b', dest = 'binSize')
    
    args = parser.parse_args()
    tmp_dir = tempfile.mkdtemp()
    os.chdir(tmp_dir)
    # symlink creation
    for file in args.input:
        filen =  tmp_dir + "/" + os.path.basename(os.path.splitext(file)[0])+".bed"
        os.symlink(file, filen)
        
    command = ( "/home/cmesser/galaxy/tools/jamm/JAMM.sh -s %s -g %s -o results -m %s -r %s -p %s"
     % ( tmp_dir, args.gsize, args.mode, args.resolution, args.processes ) )     proc = subprocess.Popen( command, shell=True)
    returncode = proc.wait()
    
    #mv files to a place where galaxy wrapper can find them
    mvcommand = "mv %s/results/peaks/all.peaks.narrowPeak %s" % ( tmp_dir, args.peakfile ) 
    os.system(mvcommand)

# clean up temp dir
    if os.path.exists( tmp_dir ):
        shutil.rmtree( tmp_dir )    
    
if __name__ == "__main__":
    main()
``` 

---
This presentation was created using [remark.js](https://github.com/gnab/remark)

The work done for JAMM in Galaxy can be found at https://github.com/messersc/jamm
