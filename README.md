# GRCm39.mm39.rRNA.interval_list
Ribosomal RNA interval_list for Picard Tools CollectRnaSeqMetrics for GRCm39/mm39

## Introduction
The `Picard CollectRnaSeqMetrics` command collects a variety of metrics to assess read mapping of RNA-seq data to a genome. It requires the annotations to be in `refFlat` format so that it can determine whether reads aligned to coding sequences, intergenic regions, etc. 

Then there is the rRNA interval list file. The purpose of RNA-seq is generally to capture the messenger RNA (mRNA); however, 90% of the RNA in a cell is ribosomal RNA (rRNA). To improve the sequencing depths of the mRNA, the laboratory who prepped the RNA for RNA-seq likely performed an rRNA-depleition or an mRNA enrichment step (Conesa, 2016). [Picard](https://broadinstitute.github.io/picard/) can assess how well this worked by counting bases that aligned to rRNAs while collecting its metrics; however, Picard does not necessarily know what to look for unless a ribosomal interval file is explicitly passed. Without the interval file, the Picard output metrics will incorrectly proclaim that `PCT_RIBOSOMAL_BASES` is 0 or NA. 

## Usage
Add the `--RIBOSOMAL_INTERVALS` flag and path to this interval list file to the picard command.
Example:
```
picard CollectRnaSeqMetrics \
	--INPUT in_file \
	--OUTPUT out_file \
	--REF_FLAT ref_flat_annotation_file \
	-STRAND NONE \
	--RIBOSOMAL_INTERVALS GRCm39.mm39.rRNA.interval_list
```

## Method
Generating this file required two steps:

### Create header
The index file must have a `SAM` header, which provides meta information on the included sequence records. This information was already available from a bam file (which had to be converted to sam) output by the Star alignment step. 

### Identifying the rRNA intervals
To identify the rRNA intervals for GRCm39, I used the Table Browser from Genome Browser, following the search instructions detailed by Luis Nassar [here](https://groups.google.com/a/soe.ucsc.edu/g/genome/c/WMHhIc6l068). The correct data columns had to be extracted and rearranged to generate the correct tab-separated format: sequence name, start position, end position, strand, interval name. 

## References
* Conesa, Ana, et al. "A survey of best practices for RNA-seq data analysis." Genome biology 17.1 (2016): 1-19.
