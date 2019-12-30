# LiBis: An ultrasensitive alignment method for low input bisulfite sequencing

Methylation information of cell-free DNA in cerebrospinal fluid (CSF), plasma and other body fluids has been utilized to diagnose early-stage diseases and predict therapy response. However, in common clinical settings only a very low amount of cell-free DNA can be purified, usually in the range of a few dozen nanograms. Even worse is that the cfDNA is fragmented and peaked around 120 bases. Whole genome bisulfite sequencing (WGBS), the gold standard to measure DNA methylation, on such a low amount of fragmented DNA molecules introduces a critical data analysis challenge, which is the low mapping ratio. This, in turn, generated low sequencing depth of each CpG and low coverage of genome-wide CpGs sites. The problem of insufficient informative CpGs became the bottleneck of the clinical application of cell-free DNA WGBS assays. Hence, we developed LiBis, a novel method for low input bisulfite sequencing data alignment. By dynamically clipping initially unmapped reads and remapping clipped fragments, we conservatively rescued those reads and uniquely aligned them to the genome. With much improved mapping ratio, LiBis as an integrative toolkit increases the number of informative CpGs and the precision of methylation status of each CpG. High sensitivity and cost efficiency achieved by LiBis for low input samples allow discoveries of genetic and epigenetic features for downstream analysis and biomarker identification from liquid biopsy.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

Install Python3, conda and pip
Python3 and conda can be downloaded and installed from https://www.anaconda.com/distribution/
Please make sure than Python version is at least 3.6
```
pip install -r LiBis-master/requirements.txt
```    

Install Fastqc and make sure fastqc is added to the PATH.
```      
conda install -c bioconda fastqc
```

Install bedtools
```
conda install -c bioconda bedtools
```

Install cutadapt and trim-galore
```
      conda install -c bioconda cutadapt
      conda install -c bioconda trim-galore
```

Install samtools and MOABS. Please make sure bsmap and mcall are added to the PATH. You may run "bsmap" and "mcall" to check the availability. 
```
conda install -c bioconda moabs
conda install -c bioconda samtools=1.1
```

### Installing
bsmap and mcall are included in LiBis while it may not fit to your environment. Please check whether it works or not. If it doesn't fit your environment, please install it from: https://github.com/sunnyisgalaxy/moabs
Please make sure samtools' version is 1.1.
Please add LiBis to $PATH to run the pipeline.
```      
wget https://github.com/Dangertrip/LiBis/archive/master.zip
unzip master.zip
chmod +x LiBis-master/LiBis
```


## Running the tests

Run LiBis

```    
LiBis -f 0 -n 6P1_notrim_val_1.fq.gz,6P2_notrim_val_2.fq.gz 6P1_notrim_val_1.fq.gz,6P2_notrim_val_2.fq.gz -c 0 -l s1 s2 -g hg19 -r hg19.fa -qc 1 -t 1
```    

## Docker installation

LiBis also supports the Docker installation.
```
wget https://github.com/Dangertrip/LiBis/archive/master.zip
unzip master.zip
cd LiBis-master/
docker build --tag=libis ./
docker run libis LiBis --help
```

Test case(Please put reference and sample raw data under the dictionary you input.)
```
docker run -v /path/to/yourdata:/data/ libis LiBis -f 0 -n 6P1_notrim_val_1.fq.gz,6P2_notrim_val_2.fq.gz 6P1_notrim_val_1.fq.gz,6P2_notrim_val_2.fq.gz -c 0 -l s1 s2 -g hg19 -r hg19.fa -qc 1 -t 1 
```

## Authors

Yue Yin, Jia Li, Jin Li, Minjung Lee, Sibo Zhao, Linlang Guo, Jianfang Li, Mutian Zhang, Yun Huang, Xiao-Nan Li, Deqiang Sun

## License

This project is licensed under the GNU License.

## Parameters in LiBis

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  Required. Enter a number, 0 means using parameter to
                        set up, 1 means using text file to set up
  -sf SETTINGFILE, --settingfile SETTINGFILE
                        Required. Setting txt file name. Ignore if -f is 0.
  -n [NAME [NAME ...]], --name [NAME [NAME ...]]
                        Required. Fastq file name.
  -c CLIP, --clip CLIP  Clip mode. 0 means close. 1 means open. default: 0
  -l [LABEL [LABEL ...]], --label [LABEL [LABEL ...]]
                        Required. Labels for samples
  -g GENOME, --genome GENOME
                        Required. Genome the reference belong to.(Use for
                        plotting) hg18/hg19/mm10/mm9 and so on. Plotting
                        script will not avaliable if leave it blank
  -w WINDOW, --window WINDOW
                        Window length for clipping mode, default=30
  -s STEP, --step STEP  Step size for clipping mode.
  -p PROCESS, --process PROCESS
                        Process using for one pipeline. Normally bsmap will
                        cost 8 cpu number. So total will be 8p.
  -r REF, --ref REF     Required. Reference
  -qc QUALITYCONTROL, --QualityControl QUALITYCONTROL
                        Do(1) quality control or not(0)
  -t TRIM, --trim TRIM  Do(1) trimming or not(0). Don't need to do trimming if
                        you use clip mode.
  -b BINSIZE, --binsize BINSIZE
                        Plot setting. Set the bin size for averaging
                        methylation ratio among samples, default=1000k
  -ft FILTER, --filter FILTER
                        filter for clipped reads
  -bam BAM, --bam BAM   Processed bam file for the first step. If bam files
                        are offered here, the first step of bsmap will be
                        skipped. BAM files can only be generated by BSMAP.
                        Different files should be seperated by ','. If there's
                        no bam file for part of the samples, leave the space
                        blank. For example: a.bam,b.bam,,,,f.bam
  -mcall, --mcall       Run mcall for mapped bams or not
  -plot, --plot         Generate the final report or not
  -nc, --nocheck        Skip the checking step for result folders. Using this
                        parameter may rewrite the previous results.
  -fullmode, --fullmode
                        Keep all temp files.

Update record:

## Releasing information

### Version 0.0.1
1. Remove all part fastq/bam/sam. Merge all fragments into one file to speed up the computation.
2. Extension requires the overlap between two fragments.
3. Add -bam option to allow users use their own bam file for the first stage mapping.

### version 0.0.2
1. Decided the name of the software: LiBis, which stands for Low input Bisulfite sequencing 
2. Add -mcall, -plot, -nc, -fullmode
3. normal mode of bsmap now use label as the filename for generated bam.

### version 0.0.3
1. Fixed reads name bug, remove the redundant modification of reads name like "@SRR001666.1". Keep the removal of "/1" or "/2" at the end of the reads.
2. Add left cut length and right cut length to the reads name. Now the reads name contains 4 fields at the end divided by "_":
3. The first number represent the rank of fragment from the reads(For example, if there are two fragments clipped from one reads, the second field will be 0 and 1).  
4. The second one means the mate, which file does the read come from. 
5. The third number means the left cut length
6. The forth number means the right cut length
