# Lab Meeting - 12th January 2015
## RNA-Seq, Reproducible Research and Galaxy
## Clemens Messerschmidt
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

# What's the best way to analyze my data? 

## Possible requirements
* reproducible
* shareable
* easy to use
* automated

## Before building a pipeline
What's the right tool for each step?

---

background-image:url(http://www.nature.com/nprot/journal/v8/n9/images/nprot.2013.099-F1.jpg)
.footnote[Anders et al. 2013 Nat Protoc]

<!-- background-image:url(images/nprot.jpg) -->

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

background-image:url(http://3.bp.blogspot.com/-5WTELB6A4JQ/UJP22me-QvI/AAAAAAABB3M/lEk0FNWnPcs/s1600/staraligner.png)
# STAR
.footnote[doi: 10.1093/bioinformatics/bts635]

---

# Feature Counting / Abundance Estimation
* HTSeq-count
* Bioconductor tools with similar approaches


### Potential problems
![Default-aligned image](http://3.bp.blogspot.com/-UN2mxaEvKV0/UM9-SZqwkHI/AAAAAAABCV8/8HGLF9_oZKE/s1600/cuffdiff1.png)
.footnote[doi:10.1038/nbt.2450 Cufflinks2, Trapnell et al 2012, Pachter lab]

???
top row: DGE undetectable by counting reads mapping to any exon, and is underestimated if counting only constitutive exons
middle: apparent change would be detected, but in the wrong direction
bottom: ?

---
# Calling Differentially Expressed Genes

Given a table of feature counts for _n_ conditions, 
which features show differential expression between conditions?

* edgeR
* DESeq2
* limma & voom

In doubt, just use all 3 
---

# Downstream Analysis

* Enrichment of GO, Pathways, ...

.footnote[you should try out Enrichr http://amp.pharm.mssm.edu/Enrichr/]

---

# NY Genome Center pipeline
## Nicolas Robine

* STAR
* featurecount (like HTSeq, but C rather than python)
* DESeq2

---

# Isoform Territory

Building a decent pipeline is much harder than for DGE
* much less consensus on tools
* transcript abundance estimation is hard
* more "experimental"

# Transcript deconvolution
* RSEM + EBSeq
* eXpress + ?? (2012: tool promised for downstream DGE testing)
* Bitseq
    * computationally expensive
    * best performing in transcript expression estimation (doi:10.1038/nbt.2957))

For all of them, you have to (re-)align with bowtie
---

background-image:url(http://2.bp.blogspot.com/-YeFf7ioF7QY/UnKErnxs6uI/AAAAAAABFnk/JdG3UMwtBds/s640/eyras-methods.png)

---

background-image:url(images/miso.jpg)
.footnote[MISO & Sashimi, http://dx.doi.org/10.1101/002576]

???

MISO is nice for plotting your results

lacks the statistical framework to analyze replicates

---

# Reviews
Engström, P. G., Steijger, T., Sipos, B., Grant, G. R., Kahles, A., Rätsch, G., … Bertone, P. (2013). Systematic evaluation of spliced alignment programs for RNA-seq data. Nature Methods, 10(12), 1185–91. doi:10.1038/nmeth.2722

Rapaport, F., Khanin, R., Liang, Y., Pirun, M., Krek, A., Zumbo, P., … Betel, D. (2013). Comprehensive evaluation of differential gene expression analysis methods for RNA-seq data. Genome Biology, 14(9), R95. doi:10.1186/gb-2013-14-9-r95

Alamancos, G., Agirre, E., & Eyras, E. (2014). Methods to study splicing from high-throughput RNA Sequencing data. Spliceosomal Pre-mRNA Splicing, 1126. doi:10.1007/978-1-62703-980-2

Soneson, C., & Delorenzi, M. (2013). A comparison of methods for differential expression analysis of RNA-seq data. BMC Bioinformatics, 14(1), 91. doi:10.1186/1471-2105-14-91

Seyednasrollah, F., Laiho, A., & Elo, L. L. (2013). Comparison of software packages for detecting differential expression in RNA-seq studies. Briefings in Bioinformatics, bbt086–. doi:10.1093/bib/bbt086

---

background-image:url(http://www.ebc.cat/wp-content/uploads/2014/06/and-now-for-something-completely-different-1.jpg)

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

---

# Stop hosting code on your lab website

Schultheiss, Sebastian J., et al. "Persistence and availability of web services in computational biology." PLoS one 6.9 (2011): e24914. 

Of 1000 web services:
* Only 72% were still available at the published address.
* The authors could not test the functionality for 33% because there was no example data, and 13% no longer worked as expected.
* The authors could only confirm positive functionality for 45%.
* Only 274 of the 872 corresponding authors answered an email.
* Of these 78% said a service was developed by a student or temporary researcher, and many had no plan for maintenance after the researcher had moved on to a permanent position.

.footnote[http://gettinggeneticsdone.blogspot.de/2013/01/stop-hosting-data-and-code-on-your-lab.html]
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
* Archive the __exact__ version of each tool used

#### Writing
* use version control, too
* use a repository for collaboration

---
# Galaxy
"Galaxy is an open, web-based platform for data intensive biomedical research. Whether on the free public server or your own instance, you can perform, reproduce, and share complete analyses."

![Default-aligned image](images/galaxy.png)

Why the three biggest positive contributions to reproducible research are the iPython Notebook, knitr, and Galaxy: 

http://simplystatistics.org/2014/09/04/why-the-three-biggest-positive-contributions-to-reproducible-research-are-the-ipython-notebook-knitr-and-galaxy/

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

# Challenges

* Galaxy development is moving fast
    * interfaces change
    * documentation rot

* Introduction of new workflow models (dataset collections)
    * tools need updates (e.g. Tophat2)
    * one outdated tool can break your workflow

---

# Bonus: Bring your own software to Galaxy
### Or how I wrote a wrapper for JAMM

Galaxy uses a XML (tool definition) to render the interface that gets exposed to the user for each tool

```xml
<tool id="jamm" name="JAMM" version="1.0.6.2">
<description>Calls peaks on NGS data</description>
<requirements>
	<requirement type="package" version="3.1.1">R</requirement>
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

background-image:url(images/jamm.png)

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
    mvcommand = "mv results/peaks/all.peaks.narrowPeak %s" % args.peakfile
    os.system(mvcommand)

# clean up temp dir
    if os.path.exists( tmp_dir ):
        shutil.rmtree( tmp_dir )    
    
if __name__ == "__main__":
    main()
``` 

---

# The end
This presentation was created using [remark.js](https://github.com/gnab/remark)

It is available at https://github.com/messersc/talks/blob/master/labmeeting.md

The work done for JAMM in Galaxy can be found at https://github.com/messersc/jamm
